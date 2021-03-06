# qw-lang (quockerwodger)

## Features

Quockerwodger feature proposal:
1. Compiled language with a high-level syntax
2. Follows many of Python's philosophies
3. Syntax & semantics are based off of Python (writes like Python to the extent that some Python code will work unchanged)
4. Generates safe C code that is compiled to an executable
5. Fast compilation times due to how the C code is generated
6. No garbage collection
7. Memory safety
8. Thread safety
9. C library interoperability
10. Compiles to small executables
11. Static build option
12. Debug and production build options
13. Type inference


## Types

* Signed integers: __int__ (32bit), __int64__
* Unsigned integers: __uint__ (32bit), __uint64__
* Floats: __float__ (7 decimal digits of precision)
* Doubles: __double__ (15 decimal digits of precision)
* Tuples: __tuple__
* Lists: __list__, __list{TYPE}__
* Strings: __string__
* Dictionaries: __dict__
* Structures: __struct__
* Classes: __class__
* Boolean: __bool__
* Binary data: __bytes__
* Mixed types: __mixed__


## Type Inference Defaults

* Signed 32bit integers:
  ```python
  x = 0
  ```
* Signed 64bit integers: None
* Unsigned integers: None
* Floats:
  ```python
  x = 1.1
  ```
* Doubles: None
* Tuples:
  ```python
  x = (1,2,3)
  ```
* Lists:
  ```python
  x = [1,2,3]
  x = [uint(1), int(2), 3]
  ```
* Strings:
  ```python
  x = 'foo'
  x = "foo"
  ```
* Boolean:
  ```python
  x = True
  x = False
  ```
* Dictionaries:
  ```python
  x = {'a':1, 'b':2}
  ```

## Examples

### Numbers

```python
# Two ways to declare signed integer named x
int x = 0
x2 = 0

# Declare unsigned integer
uint y = 0
# Incorrect: this works but is not a unsigned int but signed integer type
y2 = 0

# float and double
float f = 33.1233
double d = 1.3210123

test = x + x2
test2 = x + y # Compile time error on different types, must use cast
test2 = uint(x) + y # Works with cast

```
### Tuples (immutable)

```python
t1 = (1,2,3) # integers
t2 = ("a", "b", 3, uint(2)) # mixed types

print(t1[-1]) # => 3
print(t2[1])  # => b
```

### Lists

```python
# Create a list of integers
list{int} l = [3,1,5,3,2]
l.append(6)

# Create a list of strings
list{string} s = []

# Create a mixed list, two ways
list{mixed} m = [1, string("abc"), uint(2), "xyz", 3]
m = [] # This works too. Default is mixed type if not specified

print(m[0])   # => 1
print(m[-1])  # => 3
print(m[1:])  # => ["abc", 2, "xyz", 3]

for i in m:
  print("{} {}", i, type(i)) # => 1 int

index = m.index('abc')
print(index)    # => 1
print(m[index]) # => 'abc'

print(len(m))   # => 5
l.sort()
print(l)        # => [1, 2, 3, 3, 5, 6]
l.reverse()
print(l)        # => [6, 5, 3, 3, 2, 1]

if 6 in l:
  print("6 is in list 'l'")

```

### Dictionaries

```python
dict{string, int} d = {}
dict{int, int} i = {1:1, 2:3, 0:3,}
dict{mixed, mixed} m = {0:'hello', 'world':1, 'foo':'bar'}
x = {'a1':0, 'a2':10, 3:'thirty'}

print(i[1])       # => 1
print(m['world']) # => 1
print(m['foo'])   # => bar
print(type(m))    # => dict

for k, v in m.items():
  print(k, v) # => 0 hello ...

del(m['foo']) # deletes key foo
```

### Structs
```python

struct Foo:
    string name = "Foo"
    uint index = 0

Foo f = Foo()
print(f.name)   # => Foo
print(f.index)  # => 0


list{Foo} foos = []
foos.append(f)  # Makes a pointer to f

del(f.name)     # Error
del(f)          # Works
print(f)        # Error

print(foos[0])  # Works since f will be block scoped in underlying c code with a real parent object to each
```

### Functions
```python
def foo():
  return 1

x = foo() # x is an integer

string bar(string suffix=None):
  if suffix:
    return 'prefix_'+suffix
  return 'prefix_'

s = bar(suffix='test')
print(s) # => prefix_test

struct Baz:
    int x = 0
    int y = 1

Baz foo(int a, int b):
    Baz s = Baz()
    s.x = a
    s.b = b
    return s

Baz s = foo(10, 12)

```

### Classes

```python
class Foo:
  def __init__(self, int x, int y=None):
    self.x = x
    self.y = y
    self.baz = False

  def bar(self):
    print(self.x+2)
    if self.y:
      print(self.y+3)

class Bar(Foo):
  pass

Bar b = Bar(10)
b.x += 3
b.bar() # => 15

x = getattr(b, 'x', None)
z = getattr(b, 'z', None)
print(x) # => 13
print(z) # => None
```

## Building

```bash
# Produces production 'hello' executable
quock build hello.qw
```

```bash
# Build debug 'hello' executable
quock debug hello.qw
```

