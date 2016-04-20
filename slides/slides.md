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
## Types
## Functions
## Flow Control
## Enum
## Pattern Matching
## **Not Covered**
]
.right-column[
### Structs

- with
]

---

# Questions?
