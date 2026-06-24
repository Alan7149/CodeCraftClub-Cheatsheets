# 🍶 Flask Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Flask (Python micro-framework) quick reference.

---

## Setup

```bash
pip install flask
export FLASK_APP=app.py      # Windows: set FLASK_APP=app.py
flask run                     # dev server :5000
flask run --debug
```

## Minimal App

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, World!"

if __name__ == "__main__":
    app.run(debug=True)
```

## Routing

```python
@app.route("/users/<int:user_id>")
def user(user_id):
    return f"User {user_id}"

@app.route("/post/<slug>")           # string by default
def post(slug):
    return slug

# HTTP methods
@app.route("/items", methods=["GET", "POST"])
def items():
    if request.method == "POST":
        return "created", 201
    return "list"
```

## Request Data

```python
from flask import request

request.args.get("q")            # query string ?q=
request.form.get("name")         # form field
request.json                     # JSON body (dict)
request.get_json()
request.files["upload"]          # uploaded file
request.headers.get("Authorization")
request.cookies.get("session")
```

## Responses

```python
from flask import jsonify, redirect, url_for, render_template, abort

return jsonify({"ok": True})              # JSON
return jsonify({"error": "x"}), 404       # with status
return redirect(url_for("home"))
return render_template("index.html", name="Alan")
abort(403)                                 # raise HTTP error
```

## Templates (Jinja2)

```python
return render_template("list.html", posts=posts)
```

```html
{% extends "base.html" %}
{% block content %}
  {% for post in posts %}
    <h2>{{ post.title }}</h2>
  {% else %}
    <p>No posts</p>
  {% endfor %}
  {% if user %}Hi {{ user.name }}{% endif %}
{% endblock %}
```

## Blueprints (modular routes)

```python
# blog/routes.py
from flask import Blueprint
blog = Blueprint("blog", __name__, url_prefix="/blog")

@blog.route("/")
def index():
    return "blog home"

# app.py
app.register_blueprint(blog)
```

## SQLAlchemy (ORM)

```python
from flask_sqlalchemy import SQLAlchemy
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///app.db"
db = SQLAlchemy(app)

class Post(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(120), nullable=False)

# queries
Post.query.all()
Post.query.filter_by(title="Hi").first()
Post.query.get(1)
db.session.add(post); db.session.commit()
db.session.delete(post); db.session.commit()
```

## Error Handling & Middleware

```python
@app.errorhandler(404)
def not_found(e):
    return jsonify({"error": "not found"}), 404

@app.before_request
def before():
    pass

@app.after_request
def after(response):
    return response
```

## Config & Extensions

```python
app.config["SECRET_KEY"] = "change-me"
app.config.from_pyfile("config.py")

# Popular: flask-cors, flask-login, flask-migrate, flask-jwt-extended
from flask_cors import CORS
CORS(app)
```

---

[🔝 Back to README](../README.md)
