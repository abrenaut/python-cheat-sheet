# The Python Way

From Raymond Hettinger's [Transforming Code into Beautiful, Idiomatic Python](https://www.youtube.com/watch?v=OSGv2VnC0go)

## Loops

### Looping over a range of numbers

> Whenever you're manipulating indices directly, you're probably doing it wrong

_[range(start, stop[, step])](https://docs.python.org/3/library/stdtypes.html#typesseq-range)_

```python
for i in [0, 1, 2, 3, 4, 5]:
    print i**2

# the python way
# range() takes a small amount of memory because it calculates individual items as needed
for i in range(6):
    print i**2
```

### Looping over a collection

```python
colors = ['red', 'green', 'blue', 'yellow']

for i in range(len(colors)):
    print colors[i]

# the python way
for color in colors:
    print color
```

### Looping backwards

_[reversed(seq)](https://docs.python.org/3/library/functions.html#reversed)_

```python
colors = ['red', 'green', 'blue', 'yellow']

for i in range(len(colors)-1, -1, -1):
    print colors[i]

# the python way
for color in reversed(colors):
    print color
```

### Looping over a collection and indices

_[enumerate(iterable, start=0)](https://docs.python.org/3/library/functions.html#enumerate)_

```python
colors = ['red', 'green', 'blue', 'yellow']

for i in range(len(colors)):
    print i, '-->', colors[i]

# the python way
for i, color in enumerate(colors):
    print i, '-->', color
```

### Looping over two collections

_[zip(*iterables)](https://docs.python.org/3.3/library/functions.html#zip)_: Makes an iterator that aggregates elements from each of the iterables.

```python
names = ['raymond', 'rachel', 'matthew']
colors = ['red', 'green', 'blue', 'yellow']

n = min(len(names), len(colors))
for i in range(n):
    print names[i], '-->', colors[i]

# the python way
for name, color in zip(name, colors):
    print name, '-->', color
```

### Looping in sorted order

_[sorted(iterable[, key][, reverse])](https://docs.python.org/3/library/functions.html#sorted)_

```python
colors = ['red', 'green', 'blue', 'yellow']

for color in sorted(colors):
    print color

# reverse order
for color in sorted(colors, reverse=True):
    print color

# custom order
for color in sorted(colors, key=len):
    print color
```

### Call a function until a sentinel value

> As soon as you've made something iterable, it works with all of the Python toolkit

_[iter(object[, sentinel])](https://docs.python.org/3/library/functions.html#iter)_
_[functools.partial(func, *args, **keywords)](https://docs.python.org/3/library/functools.html#functools.partial)_

```python
# sentinel value the traditional way
blocks = []

while True:
    block = f.read(32)
    if block = '':
        break
    blocks.append(block)

# the second argument of the iter function is a sentinel value
# in order to make it work, the first function has to be a function with no arguments, hence the partial
blocks = []
for block in iter(partial(f.read, 32), ''):
    blocks.append(block)
```

### Distinguishing multiple exit points in loops

> The for loop else should have been called nobreak

```python
def find(seq, target):
    found = False
    for i, value in enumerate(seq):
        if value == tgt:
            found = True
            break
        if not found:
            return -1
        return i

def find(seq, target):
    for i, value in enumerate(seq):
        if value == tgt:
            break
    else:
        return -1
    return i
```

## Dictionaries

### Looping over dictionary keys

```python
d = {'matthew': 'blue', 'rachel': 'green', 'raymond': 'red'}

for k in d:
    print k

for k in d.keys():
    if k.startswith('r'):
        del d[k]
```

### Looping over a dictionary keys and values

```python
d = {'matthew': 'blue', 'rachel': 'green', 'raymond': 'red'}

for k in d:
    print k, '-->', d[k]

# the python way
for k, v in d.items():
    print k, '-->', v
```

### Construct a dictionary from pairs

```python
names = ['raymond', 'rachel', 'matthew']
colors = ['red', 'green', 'blue', 'yellow']

d = dict(zip(names, colors))
```

### Counting with dictionaries

_[class collections.defaultdict([default_factory[, ...]])](https://docs.python.org/3.3/library/collections.html#collections.defaultdict)_

```python
colors = ['red', 'green', 'red', 'blue', 'green', 'red']

d = {}
for color in colors:
    if color not in d:
        d[color] = 0
    d[color] += 1

d = {}
for color in colors:
    d[color] = d.get(color, 0) + 1

# the python way
d = defaultdict(int)
for color in colors:
    d[color] += 1
```

### Grouping with dictionaries

```python
names = ['raymond', 'rachel', 'matthew', 'roger', 'betty', 'melissa', 'judith', 'charlie']

d = {}
for name in names:
    key = len(name)
    if key not in d:
        d[key] = []
    d[key].append(name)

d = {}
for name in names:
    key = len(name)
    d.setdefault(key, []).append(name)

# the python way
d = defaultdict(list)
for name in names:
    key = len(name)
    d[key].append(name)
```

### Remove and return a (key, value) pair from a dictionary

> popitem() is atomic so it can be used bewteen threads

_[popitem()](https://docs.python.org/3.0/library/stdtypes.html#dict.popitem)_

```python
d = {'matthew': 'blue', 'rachel': 'green', 'raymond': 'red'}

while d:
    key, value = d.popitem()
    print key, '-->', value
```

### Linking dictionaries

_[ChainMap](https://docs.python.org/3/library/collections.html#collections.ChainMap)_

```python
defaults = {'color': 'red', 'parser': 'guest'}
parser = argparse.ArgumentParser()
parser.add_argument('-u', '--user')
parser.add_argument('-c', '--color')
namespace = parser.parse_args([])
command_line_args = {k: v for k, v in vars(namespace).items() if v}

d = default.copy()
d.update(os.environ)
d.update(command_line_args)

# the python way
d = ChainMap(command_line_args, os.environ, defaults)
```

## Clarity

### Keyword arguments

```python
twitter_search('@obama', False, 20, True)

# the python way
twitter_search('@obama', retweets=False, numtweets=20, popular=True)
```

### Named tuples

```python
doctest.testmod()
# output: (0, 4)

TestResults = namedtuple('TestResults', ['failed', 'attempted'])

doctest.testmod()
# output: TestResults(failed=0, attempted=4)
```

### Unpacking sequences

```python
p = 'Raymond', 'Hettinger', 0x30, 'python@example.com'

fname = p[0]
lname = p[1]
age = p[2]
email = p[3]

# the python way
fname, lname, age, email = p
```

### Updating multiple state variables

```python
def fibonacci(n):
    x = 0
    y = 1
    for i in range(n):
        print x
        t = y
        y = x + y
        x = t

# the python way
def fibonnaci(n):
    x, y = 0, 1
    for i in range(n):
        print x
        x, y = y, x + y
```

## Efficiency

### Concatening strings

```python
names = ['raymond', 'rachel', 'matthew', 'roger', 'betty', 'melissa', 'judith', 'charlie']

s = names[0]
for name in name[1:]:
    s += ', ' + name
print s

# the python way
', '.join(names)
```

### Updating sequences

```python
names = ['raymond', 'rachel', 'matthew', 'roger', 'betty', 'melissa', 'judith', 'charlie']

# whenever you use this you should be using a deque instead
del names[0]
names.pop(0)
names.insert(0, 'mark')

names = deque(['raymond', 'rachel', 'matthew', 'roger', 'betty', 'melissa', 'judith', 'charlie'])
del names[0]
names.popleft()
names.appendleft('mark')
```

## Decorators and Context Managers

_[@functools.lru_cache(maxsize=128, typed=False)](https://docs.python.org/dev/library/functools.html#functools.lru_cache)_

```python
def web_lookup(url, saved={}):
    if url in saved:
        return saved[url]
    page = urllib.urlopen(url).read()
    saved[url] = page
    return page

# the python way
@lru_cache
def web_lookup(url):
    return urllib.urlopen(url).read()
```

## Factor-out temporary contexts

> Anytime your setup and teardown logic get repeated in your code you want a context manager to improve it

```python
old_context = getcontext().copy()
getcontext().prec = 50
print Decimal(355) / Decimal(113)
setcontext(old_context)

# the python way
with localcontext(Context(prec=50)):
    print Decimal(355) / Decimal(113)

try:
    os.remove('somefile.tmp')
except OSError:
    pass

# the python way
@contextlib.contextmanager
def ignored(*exceptions):
    try:
        yield
    except exceptions:
        pass

with ignored(OSError):
    os.remove('somefile.tmp')
```

## Concise Expressive One-Liners

> One logical line of code equals one sentence in English