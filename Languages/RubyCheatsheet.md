# 💎 Ruby Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Ruby quick reference.

---

## Hello World & Running

```ruby
puts "Hello, World!"
```

```bash
ruby script.rb        # run a file
irb                   # interactive REPL
gem install rails     # install a gem
```

## Variables & Types

```ruby
name = "Alan"          # String
age = 21                # Integer
pi = 3.14               # Float
active = true           # boolean
nothing = nil           # NilClass
SYMBOL = :id            # Symbol (immutable identifier)
CONSTANT = 100          # constants start uppercase

age.class               # Integer
"42".to_i; 42.to_s; "3.5".to_f
```

## Strings

```ruby
s = "Code Craft Club"
s.length; s.upcase; s.downcase; s.reverse
s.split(" "); s.include?("Craft"); s.gsub("Club", "Crew")
s[0]; s[0..3]                      # "Code"
"#{name} is #{age}"                 # interpolation
"abc".chars; "a,b".split(",")
```

## Numbers

```ruby
7 / 2          # 3 (integer division)
7 % 2          # 1
2 ** 10        # 1024
10.times { |i| puts i }
(1..5).to_a    # [1,2,3,4,5]
5.even?; 5.odd?; -3.abs
```

## Collections

```ruby
# Array
a = [1, 2, 3]
a.push(4); a << 5; a.pop; a.first; a.last
a.map { |x| x * 2 }; a.select { |x| x > 1 }; a.reject { |x| x < 2 }
a.reduce(:+); a.sort; a.reverse; a.include?(2)
a.each { |x| puts x }

# Hash
h = { name: "Alan", age: 21 }
h[:name]; h[:age] = 22
h.keys; h.values; h.each { |k, v| puts "#{k}: #{v}" }
h.fetch(:email, "n/a")
```

## Control Flow

```ruby
if age >= 18
  puts "adult"
elsif age > 12
  puts "teen"
else
  puts "kid"
end

puts "adult" if age >= 18          # modifier form
status = age >= 18 ? "adult" : "minor"

case day
when "Sat", "Sun" then puts "weekend"
else puts "weekday"
end

3.times { |i| puts i }
[1, 2, 3].each { |x| puts x }
i = 0
while i < 3 do i += 1 end
```

## Methods

```ruby
def greet(name, greeting = "Hi")
  "#{greeting}, #{name}!"          # implicit return
end

def total(*nums)                    # splat / variadic
  nums.sum
end

square = ->(x) { x * x }            # lambda
square.call(5); square.(5)
```

## Blocks, Procs & Yield

```ruby
def repeat(n)
  n.times { |i| yield i }
end
repeat(3) { |i| puts i }

[1, 2, 3].map { |x| x * 2 }          # one-line block
[1, 2, 3].each do |x|                # multi-line block
  puts x
end
```

## Classes & OOP

```ruby
class Animal
  attr_accessor :name               # getter + setter

  def initialize(name)
    @name = name                     # instance variable
  end

  def speak
    "#{@name} makes a sound"
  end
end

class Dog < Animal                   # inheritance
  def speak
    "#{@name} barks"
  end
end

d = Dog.new("Rex")
d.speak; d.is_a?(Animal)             # true

# Module (mixin)
module Greetable
  def hello; "Hi from #{name}"; end
end
```

## Error Handling

```ruby
begin
  risky
rescue ArgumentError => e
  puts e.message
rescue => e
  puts "other: #{e}"
ensure
  puts "always runs"
end

raise ArgumentError, "bad input"
```

---

[🔝 Back to README](../README.md)
