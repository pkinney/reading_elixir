class: center, middle

# Reading Elixir

---

.left-column[
## What is it?
]
.right-column[
- Functional language
- Built on top of Erlang (and the Erlang VM)
 - Direct access to much of Erlang
 - OTP included
- Immutable to the max
- "Looks like Ruby, faster than Node.js"
]

---

.left-column[
## What is it?
## **Organization**
]
.right-column[
### Modules

```elixir
defmodule MyModule do
  @module_attribute "hi"
  ...
end
```

### Functions

```elixir
def MyModule do
  def my_function(a, b \\ 0) do
    do_add(a, b)
  end

  defp do_add(a, b), do: a + b
end
```
]

---

.left-column[
## What is it?
## **Organization**
]
.right-column[
### Project Structure

```bash
> mix new hello
...
> tree hello
hello
├── README.md
├── config
│   └── config.exs
├── lib
│   └── hello.ex
├── mix.exs
└── test
    ├── hello_test.exs
    └── test_helper.exs
```

```bash
> mix deps.get
> mix test
```
]
---

.left-column[
## What is it?
## **Organization**
]
.right-column[
### mix.exs

```elixir
defmodule Hello.Mixfile do
  use Mix.Project

  def project do
    [app: :hello,
     version: "0.0.1",
     elixir: "~> 1.2",
     build_embedded: Mix.env == :prod,
     start_permanent: Mix.env == :prod,
     deps: deps]
  end

  def application do
    [applications: [:logger]]
  end

  defp deps do
    [{:polyline, "~> 0.1.0"},
     {:timex, "~> 2.1.3"},
     {:exvcr, "~> 0.7", only: :test}]
  end
end
```
]

---

.left-column[
## What is it?
## Organization
## **Types**
]
.right-column[
### Value Types

```elixir
1405006117752879898543142606244511569936384000000000 # 42! (integer)
1.4142135623730951 # √2 (double)
:atom # like Ruby's symbols
(1..4) # [1, 2, 3, 4]
~r{[aeiou]} # Regex.run ~r{[aeiou]}, "caterpillar" => ["a"]
```

### System Types

```elixir
#PID<0.57.0> # self() give my PID
#Reference<0.0.8.181> # make_ref()
```
]

---

.left-column[
## What is it?
## Organization
## **Types**
]
.right-column[
### Atom

```elixir
:one
:or_two
```

### List

```elixir
[1, 2, 3] === [1 | [2, 3]]
[1, 2] ++ [3, 4] === [1, 2, 3, 4]
```

### Keyword List

```elixir
[ name: "John", city: "Dallas", likes: "Eeples & Baneenees" ]
[ {:name, "John"}, {:city, "Dallas"}, {:likes, "Eeples & Baneenees"} ]

[42, name: "John", city: "Dallas"]
[42, {:name, "John"}, {:city, "Dallas"}]
```
]

---

.left-column[
## What is it?
## Organization
## **Types**
]
.right-column[
### Maps

```elixir
a = %{ "foo" => "bar", {:ok, 3} => "baz" }
a["foo"] === "bar"
Map.get(a, "foo") === "bar"

b = %{ :foo => "bar", :baz => 42 }
b === %{ foo: "bar", baz: 42 }
b.foo === "bar"

c = %{ String.downcase("HeLlO") => "world" }
c["hello"] === "world"
```

### Updating Maps

```elixir
m = %{ a: 1, b: 2, c: 3 }
m1 = %{ m | b: "two", c: "three" }
m1 === %{a: 1, b: "two", c: "three"}
```
]

---

.left-column[
## What is it?
## Organization
## **Types**
]
.right-column[
### Structs

```elixir
defmodule User do
  defstruct first_name: "", last_name: "", role: :user
end

u = %User{first_name: "John", last_name: "Sample"}
u.first_name === "John"
u.role === :user
u.__struct__ === User
```
]

---

.left-column[
## What is it?
## Organization
## **Types**
]
.right-column[
### Strings

```elixir
name = "john"
"Hello, #{String.capitalize name}!"
"Hello, " <> String.capitalize(name) <> "!"

~w{one two three} === ["one", "two", "three"]
```

### Char List

```elixir
is_binary("Hello") === true
is_list('Hello') === true

'Hello, ' ++ to_char_list(String.capitalize(name)) ++ '!'
```
]

---

.left-column[
## What is it?
## Organization
## Types
## **Functions**
]
.right-column[
### Anonymous Functions

```elixir
square = fn n -> n*n end
square.(3) === 9

add = &(&1 + &2)
add.(1, 2) === 3

dcase = &String.downcase/1
dcase.("HeLLO") === "hello"
```
]

---

.left-column[
## What is it?
## Organization
## Types
## **Functions**
]
.right-column[
### Pattern Matching

```elixir
square = fn
  1 -> "1, duh!"
  n -> n*n
end
square.(1) === "1, duh"
square.(2) === 4
```

```elixir
divide = fn
  0, _ -> 0
  _, 0 -> :nan
  a, 1 -> a
  a, b -> a / b
end
divide.(1, 0) === :nan
divide.(2, 1) === 2
divide.(12, 4) === 3.0
```
]

---

.left-column[
## What is it?
## Organization
## Types
## **Functions**
]
.right-column[
### Module Functions

```elixir
defmodule MyMath do
  def sub(a, b \\ 0)
  def sub(a, 0), do: a
  def sub(a, b) when a === b, do: 0
  def sub(a, b) when is_integer(a) and is_integer(b) do
    a - b
  end
end

MyMath.sub(3) === 3
MyMath.sub(1, 0) === 1
MyMath.sub(2, 2) === 0
MyMath.sub(2, :sandwich)
** (FunctionClauseError) no function clause matching in MyMath.sub/2
    iex:21: MyMath.sub(2, :sandwich)
```
]

---
.left-column[
## What is it?
## Organization
## Types
## **Functions**
]
.right-column[
### List Recursion with Functions

```elixir
defmodule MyMath do
  def sum([]), do: 0
  def sum([a | tail]) do
    a + sum(tail)
  end
end

MyMath.sum([1, 2, 3]) === 6
MyMath.sum(:sandwich)
** (FunctionClauseError) no function clause matching in MyMath.sum/1
    iex:25: MyMath.sum(:sandwich)
```
]

---

.left-column[
## What is it?
## Organization
## Types
## Functions
## Flow Control
## Enum
## Matching
]
.right-column[
### Things we didn't cover

- with
]
