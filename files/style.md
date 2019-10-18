
# Programming style

- Style applies to software!
- The goal is clarity above all else, though conciseness is often considered desirable.
- Very analogous to writing prose.
- Expect to iteratively edit/refine/improve as you would text.

## Two specific suggestions (among many)

- Gather `import` statements at the top.
-- Analogous to listing ingredients at top of recipe.
- Document as you write.
-- There will never be a time in your life when you say, "I have nothing else to do, I think I'll go back and document some of my old work."
-- This ties directly into the Jupyter philosophy of documenting your research.
- Again, expect to refine to achieve greater simplicity and concision.
- There are style guides (e.g., https://www.python.org/dev/peps/pep-0008/) but they tend to focus on syntax (how many spaces to indent, etc.), not on clarity, which is more difficult to prescribe.

## Naming

Good variable names, yes, but there's a better, more nuanced principle:

**The larger a variable's scope, the more descriptive a name it should have**.

Example:


```python
my_list = [7, 19, -3]
sum = 0
for v in my_list:
    sum += v
```

If `v` is used only in this loop, a single-letter variable is fine.  Calling it `value` or `the_current_value` will not really aid comprehension.  But what `v` were a global variable?


```python
v = 37 # ??
```

- The difference here is really one of scope.  A variable used only within a small section of code (a loop, a short function) gains a lot of meaning simply from the context it sits in, particularly if the code is following a well-known idiom.
- Of course, if the meaning is not obvious from the context, then a better/longer name is warranted regardless of the scope.
- Functions have large scope, since they can be called from anywhere, hence should have descriptive names.  Consider if not:


```python
def xa(xb, xc, xd):
    xe = str(xb) + "/" + str(xc) + "/" + str(xd)
    print(xe)
```

A meaningful name really helps:


```python
def print_date(xb, xc, xd):
    xe = str(xb) + "/" + str(xc) + "/" + str(xd)
    print(xe)
```

But that's not really enough.  The arguments, i.e., the calling signature, are essential parts of a function's "name."


```python
def print_date(year, month, day):
    joined = str(year) + "/" + str(month) + "/" + str(day)
    print(joined)
```

The scope for `joined` is small.  A single-letter variable would not really diminish clarity here.


```python
def print_date(year, month, day):
    s = str(year) + "/" + str(month) + "/" + str(day)
    print(s)
```

In an editing pass, we might notice that `s` serves little purpose.


```python
def print_date(year, month, day):
    print(str(year) + "/" + str(month) + "/" + str(day))
```

## Documentation
- Document first and foremost for yourself.
-- Don't think of it as a burden/duty to help others, think of it as a selfish benefit.
- "Docstrings" tie help into Python's help system.


```python
def print_date(year, month, day):
    "Prints the date in Y/M/D format."
    print(str(year) + "/" + str(month) + "/" + str(day))
```


```python
help(print_date)
```

Use triple quotes for multiline strings.


```python
def print_date(year, month, day):
    """Prints the date
    in Y/M/D format."""
    print(str(year) + "/" + str(month) + "/" + str(day))
```

## Assertions

- An assertion raises an exception if a condition is not satisfied.
- Appropriate for catching programming errors, not so much for user entry errors.


```python
def calc_bulk_density(mass, volume):
    "Return dry bulk density = powder mass / powder volume."""
    assert volume > 0
    return mass/volume
```


```python
calc_bulk_density(2.5, -3)
```

Assertions are particularly useful for checking that arguments have the expected type and meet whatever other criteria are expected of them.  Without assertions, Python blithely charges on.


```python
print_date(2.7, False, [19, "what the?!"])
```


```python
def print_date(year, month, day):
    "Prints the date in Y/M/D format.  Arguments should be integers."
    assert type(year) == type(month) == type(day) == int
    print(str(year) + "/" + str(month) + "/" + str(day))
```


```python
print_date(2.7, False, [19, "what the?!"])
```

You can add a custom message to assertions.


```python
def print_date(year, month, day):
    "Prints the date in Y/M/D format.  Arguments should be integers."
    assert type(year) == type(month) == type(day) == int, "bad argument type, expecting an int"
    assert year > 0, "bad year"
    assert 1 <= month <= 12, "bad month"
    print(str(year) + "/" + str(month) + "/" + str(day))
```


```python
print_date(2.7, False, [19, "what the?!"])
```

## Exercise
The following code prints the sum of the squares of a list of numbers.  See if you can simplify it to make it clearer.  The focus here is not on variable names, but on the structure of the code.  Bonus points if you can rewrite the code using a one-line list comprehension.


```python
my_list = [3, 5, -19]
answer = None # don't know it yet
length_of_list = len(my_list)
list_of_squares = []
for index in range(0, length_of_list):
    value = my_list[index]
    square = value*value
    list_of_squares.append(square)
the_sum = 0
for square_value in list_of_squares:
    the_sum = the_sum + square_value
answer = the_sum
print(answer)
```


```python
my_list = [3, 5, -19]
print(sum([v*v for v in my_list]))
```

Consider: if you weren't told in advance what the above code did, and had to discover for yourself, which version would you rather read?
