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
13. Type inferrance

## Basics

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
test2 = x + y # ERROR on different types, must use cast
test2 = (uint)x + y # Works with cast

```
### tuples (immutable)

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

### dicts

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

### functions
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
```

### classes

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
