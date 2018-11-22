# dispel4py

The core packages for Dispel4Py.

# dispel4py.provenance

## total_size
```python
total_size(o, handlers={}, verbose=False)
```
Returns the approximate memory footprint an object and all of its contents.

Automatically finds the contents of the following builtin containers and
their subclasses:  tuple, list, deque, dict, set and frozenset.
To search other containers, add handlers to iterate over their contents:

    handlers = {SomeContainerClass: iter,
                OtherContainerClass: OtherContainerClass.get_elements}


## ProvenanceType
```python
ProvenanceType(self)
```

## get_source
```python
get_source(object, spacing=10, collapse=1)
```
Print methods and doc strings.

Takes module, class, list, dictionary, or string.
## ProvenanceRecorder
```python
ProvenanceRecorder(self, name='ProvenanceRecorder', toW3C=False)
```

