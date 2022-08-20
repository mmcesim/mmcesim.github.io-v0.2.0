---
layout: default
title: Function
permalink: /doc/alg/function
parent: ALG Language
grand_parent: Documentation
nav_order: 2
has_children: false
---

# Function of ALG Language
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

***

## Syntax Basics

The initiative of proposing a new programming language for algorithm
implementation is based on the multi-backend design of mmCEsim.
The language is specially designed so that it can be exported to C++
(with Armadillo), Python (with NumPy) and Matlab/Octave easily.

Every line of ALG language calls a function.
Let's first have a look at its basic structure before we cover its details.

```
ret1::type1 ret2 = FUNC param1 param2::type2 key1=value1 key2=value2::type3
```

It may look like an assembly language at the first glance,
due to all parameters are separated by space.
But it is actually much more convenient.
Here are some basic rules:
- All tokens are separated by space.
- The function line is mainly composed of three parts:
  **return values**, **function name** and **parameters**,
  in the left to right direction.
- Some functions may not have return values, and you may also omit the return values.
  If there are return values, there is a `=` between return values and function names.
- Function name is the first word on the right of `=` (if there are return values)
  or the first word of line (if there is no return value).
- Like Python, parameters can be passed in by two ways:
  1. **value in position**: Like `param1` and `param2` in the above example.
    Parameters in different positions correspond to different usages in the function.
    This is the only way in C++.
  2. **key and value**: Parameters can also be specified using key and its
    corresponding value. `value1` and `value2` are passed in using this method.
    It should be noted that there should be no space around the `=` between key and value.

  There are some special cases that parameters are viewed as a whole,
  for example [`COMMENT`](#comment) and [`CALC`](#calc).
- If a parameter contains space or special characters, you need to use the
  double quotes like `"param with space"` and escape special characters as in
  C++ and Python.
- You may optionally specify the type of return value and parameters with `::`
  after the value. For example, in the above example `dtype1`, `dtype2` and `dtype3`
  are type specifications for `ret1`, `param2` and `value2` respectively.
  For more information about data type, please refer to [data type of ALG language](type).

{: .caution }
There should be no space around the `=` between key and value for parameters.
For example, `key=val` is valid while `key = val` is forbidden.

***

## CALC

## COMMENT
Print a line of comment in the exported code.

## ELSE

## ELIF

## END

## INIT
Initialize a variable.

## FOR

## FOREVER

## LOOP

## PRINT

## WHILE