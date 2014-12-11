PEP 457 - Syntax For Positional-Only Parameters
===============

## Introduction
This PEP (Python Enhancement Proposal) proposes a change in how the syntax for parameter passing is
defined in Python, in a way such that documentation can be expressed more consistently with the actual implementation, and such that the language allows positional-only parameters without hard-to-read implementations.

One of the important ideas of the proposal is that:
> currently in the documentation there's no way to tell whether a function takes positional-only parameters. 

Note that while it is possible to simulate positional-only parameters, one has to implement the argument parsing and the function signature does not tell what arguments are accepted.

## Proposal info
* https://www.python.org/dev/peps/pep-0457/
* Last updated 2013-10-09
* Larry Hastings <larry at hastings.org>
* Status: draft

## Motivation
In Python there are different semantics for function parameters declaration. Sometimes this can be confusing for the programmer, for example:

## Example
One of the examples in the proposal is the function

the `range()` function is documented with the signatures
```python
range(stop)
range(start, stop[, step])
```
which with the new syntax would be
```python
range([start,] stop, [step,] /)
```
Note that this syntax is ambiguous, so the leftmost optional parameter would be preferred.

## Motivation (copy-pase)
> * Documentation can clearly, unambiguously, and consistently express exactly how the arguments for a function will be interpreted.
* The syntax is reserved for future use, in case the community decides someday to add positional-only parameters to the language.
* Argument Clinic can use a variant of the syntax as part of its input when defining the arguments for built-in functions.


## Proposed syntax
Current
```python
def name(positional_or_keyword_parameters, *, keyword_only_parameters):
```

New
```python
def name(positional_only_parameters, /, positional_or_keyword_parameters,
         *, keyword_only_parameters):
```

New, optional parameters grouped with `[]`
```python
def foo([a, b,] c, [d,] /):
```

Example:
```python
def addch([y, x,] ch, [attr,] /):
```
instead of (as in current documentation)
```python
addch(ch[, attr])
addch(y, x, ch[, attr])
```

Note that the right optional argument group is chosen if 2 arguments are given; the groups are determined by the number of arguments and in case of ambiguity the leftmost argument is used.
