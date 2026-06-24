# 🔷 ASP.NET Core Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · ASP.NET Core (C#) Web API quick reference.

---

## Setup & Run

```bash
dotnet new webapi -n MyApi    # create Web API project
cd MyApi
dotnet run                     # run (https://localhost:5001)
dotnet watch run               # hot reload
dotnet add package <name>      # add a NuGet package
dotnet build / dotnet publish
```

## Program.cs (Minimal Hosting)

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();
builder.Services.AddScoped<IPostService, PostService>();
builder.Services.AddDbContext<AppDbContext>(opt =>
    opt.UseSqlServer(builder.Configuration.GetConnectionString("Default")));

var app = builder.Build();

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();
app.Run();
```

## Minimal API Endpoints

```csharp
app.MapGet("/", () => "Hello World");
app.MapGet("/posts/{id}", (int id) => Results.Ok(new { id }));
app.MapPost("/posts", (Post p) => Results.Created($"/posts/{p.Id}", p));
app.MapPut("/posts/{id}", (int id, Post p) => Results.NoContent());
app.MapDelete("/posts/{id}", (int id) => Results.Ok());
```

## Controller-Based API

```csharp
[ApiController]
[Route("api/[controller]")]
public class PostsController : ControllerBase
{
    private readonly IPostService _service;
    public PostsController(IPostService service) => _service = service;

    [HttpGet]
    public ActionResult<IEnumerable<Post>> GetAll() => Ok(_service.All());

    [HttpGet("{id}")]
    public ActionResult<Post> Get(int id)
    {
        var post = _service.Find(id);
        return post is null ? NotFound() : Ok(post);
    }

    [HttpPost]
    public ActionResult<Post> Create([FromBody] Post post)
    {
        _service.Add(post);
        return CreatedAtAction(nameof(Get), new { id = post.Id }, post);
    }

    [HttpPut("{id}")]
    public IActionResult Update(int id, Post post) { ... return NoContent(); }

    [HttpDelete("{id}")]
    public IActionResult Delete(int id) { ... return Ok(); }

    // query string: ?q=term
    [HttpGet("search")]
    public IActionResult Search([FromQuery] string q) => Ok(_service.Search(q));
}
```

## Entity Framework Core

```csharp
public class Post
{
    public int Id { get; set; }
    public string Title { get; set; } = "";
    public bool Published { get; set; }
}

public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions options) : base(options) {}
    public DbSet<Post> Posts => Set<Post>();
}

// Queries (LINQ)
await _db.Posts.ToListAsync();
await _db.Posts.FindAsync(id);
await _db.Posts.Where(p => p.Published).OrderBy(p => p.Title).ToListAsync();
_db.Posts.Add(post); await _db.SaveChangesAsync();
_db.Posts.Remove(post); await _db.SaveChangesAsync();
```

```bash
dotnet ef migrations add Init
dotnet ef database update
```

## Dependency Injection Lifetimes

```csharp
builder.Services.AddSingleton<IService, Service>();  // one for app lifetime
builder.Services.AddScoped<IService, Service>();     // one per request
builder.Services.AddTransient<IService, Service>();  // new every time
```

## Model Validation

```csharp
public class Post
{
    [Required] public string Title { get; set; } = "";
    [StringLength(500)] public string Body { get; set; } = "";
    [EmailAddress] public string Email { get; set; } = "";
}
// [ApiController] auto-returns 400 on invalid model
```

## Configuration (appsettings.json)

```json
{
  "ConnectionStrings": { "Default": "Server=.;Database=app;" },
  "Jwt": { "Key": "secret" }
}
```

```csharp
var key = builder.Configuration["Jwt:Key"];
```

## Common Results

```csharp
return Ok(data);              // 200
return Created(uri, data);    // 201
return NoContent();           // 204
return BadRequest("msg");     // 400
return Unauthorized();        // 401
return NotFound();            // 404
```

---

[🔝 Back to README](../README.md)
