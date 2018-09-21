# qw-lang (quockerwodger)

## Basics

### Numbers

```python
# Two ways to declare integer named x
int x = 0
x = 0

uint x = 0
x = 0 # Incorrect: this is not a uint but int type

```

### Lists

```python
# Create a list of integers
list{int} l = [3,1,5,3,2,6]

# Create a list of strings
list{string} s = []

# Create a mixed list, two ways
list{mixed} m = [1, "abc", 2, "xyz", 3]
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

### classes

```python
class Foo:
  def __init__(self, x, y=None):
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

```
