# DefaultDict

This is a package for a defaultdict that deep copies its default value.

This can be used to implement a simple counter object, similar to the example code this is based on.

```python
counter = DefaultDict()

for item in ['spam', 'egg', 'spam', 'spam', 'bacon', 'spam']:
    counter[item] += 1

print("Menu contents:")
for item in counter:
    print("  {}x {}".format(counter[item], item))
```

Output:
```
Menu contents:
  1x bacon
  1x egg
  4x spam
```

Unlike [collections.defaultdict](https://docs.python.org/3/library/collections.html#collections.defaultdict), the default value is an object - not a factory. This allows for easier creation of nested data structures. For example, the following is valid and encouraged:

```python
# shapes_by_color[color][shape] = count
shapes_by_color = DefaultDict(
    default=DefaultDict(
        {'total': 0},
        default=0
    )
)

with open('test/shapes.log', 'r') as fp:
    for line in fp:
        color, shape = line.split()
        shapes_by_color[color][shape] += 1
        shapes_by_color[color]['total'] += 1

for color, shapes in shapes_by_color.items():
    print("{}:".format(color))
    for shape, count in shapes.items():
        if shape == 'total':
            continue
        print("  {}x {}".format(count, shape))
    print("  Total: {}".format(count))
```

Output:
```
green:
  2x triangle
  1x circle
  Total: 3
red:
  1x octagon
  2x rhombus
  Total: 3
blue:
  1x triangle
  1x square
  Total: 2
```

Note that the default value is a reference, and is affected by external changes. This only affects new uses of the default value, not previous uses.
```python
spam = DefaultDict(default=['Hello'])
print(spam['eggs'])

spam.default.append('world')
print(spam['sausage'])

print(spam)
```

Output:
```
['Hello']
['Hello', 'world']
DictWithDefault(init={'sausage': ['Hello', 'world'], 'eggs': ['Hello']}, default=['Hello', 'world'])
```
