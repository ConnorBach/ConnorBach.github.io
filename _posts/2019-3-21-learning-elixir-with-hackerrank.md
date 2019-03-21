---
layout: post
title: "Learning Elixir"
---
## Elixir 

I've been reading a lot about Elixir recently and wanted to give the language a spin. Elixir is a functional
language that runs on the Erlang VM. With syntax that draws inspiration from Ruby, Elixir provides a smoother transition
to the functional paradigm than other languages. In addition, frameworks like Phoenix and Nerves give Elixir many targets, including
the web and embedded systems respectively.

With the recent release of [Phoenix Liveview](https://dockyard.com/blog/2018/12/12/phoenix-liveview-interactive-real-time-apps-no-need-to-write-javascript), a framework that provides real-time page updates with server rendered HTML, I thought it was time to dive into learning Elixir.

## Solving Programming Challenges 

I had played around with Haskell before but Elixir has been my first deep dive into functional concepts.
To get a good feel for the language I decided to head over to [Hackerrank](hackerrank.com) and tackle some of their
functional programming challenges. You can view any of my solutions on Github [here](https://github.com/ConnorBach/elixir_problems).

## Basic Functional Concepts in Elixir

I'll demonstrate some of the cool features that Elixir offers by walking you through a few methods.

### Piping, Ranges, and Anonymous functions

Here is an simple example, a function that prints Hello World n times:

```
def hello(n) do
    (1 .. n) |> Enum.each(fn _ -> IO.puts("Hello World") end)
end
```

This function has three major Elixir concepts: ranges, anonymous functions, and piping. The first part of the line generates a range
from 1 to n. Ranges represent a sequence of numbers and can be used for iteration. The next symbol is the pipe operator. Its role is to
make function composition more readable. Instead of f(g(x)) you would write x |> g() |> f(). The final call is the Enum.each function. It
takes the range and an anonymous function as input, executing the function once for every item in the range.

### Reduce and Pattern Matching

One of my favorite functions to write was one that calculates the area of an N-sided polygon when given a list of its vertices.

```
def area(points) do
	temp = Enum.reduce(points, {0, Enum.at(points, -1)}, fn point, acc -> 
		{area, prevPoint} = acc
		{x1, y1} = point
		{x2, y2} = prevPoint
		curArea = x1 * y2 - x2 * y1
		{curArea + area, point}
	end)
	|> elem(0) 
	abs(0.5 * temp)
end
```

In this example we use the reduce function to iterate over the points and calculate the area. At each iteration we need
to have access to both the current point and the previous point. To that end we pass the previous point along in the accumulator
of the reduce function, in addition to the current area. The initial value of the accumulator is the last point in the list, giving us
the ability to calculate the final side of the polygon without an additional step. The pattern matching of Elixir also comes in handy here, 
allowing us to destructure the point tuples into easily usable variables.

### IO

In my brief Haskell adventures, IO always seemed to give me trouble. I had no such problems in Elixir and once I got the hang of the
IO methods it became very easy to parse input any way I liked. 

This example shows the IO code for the area function that I mentioned above.

```
Example input:
4
1 2
3 4
5 6
7 8

{numPoints,_} = IO.gets("") |> Integer.parse()
Enum.map(1..numPoints, fn _ -> 
    IO.read(:stdio, :line)
    |> String.split(" ")
    |> Enum.map(fn x -> elem(Integer.parse(x), 0) end)
    |> (fn x -> {Enum.at(x,0), Enum.at(x, 1)} end).()
end)
|> Solution.area()
|> IO.inspect()
```

By mapping over the number of points to read we can grab and parse each line individually and package them into tuples.
The output of this map is a list that can be piped right into our area method!

## Final Thoughts

I really enjoyed tackling problems in Elixir and plan to continue learning the language in the future. I'd like to use Elixir to build
a web project by utilizing the Phoenix framework to see what it is like in a more realistic situation. Overall I was surprised by the
flexibility and ease of use that the language provides. I am excited to see how Elixir continues to develop!
