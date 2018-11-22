# dispel4py

The core packages for Dispel4Py.

# dispel4py.provenance

## clean_empty
```python
clean_empty(d)
```
Given a dictionary in input, removes all the properties that are set to None.
It workes recursevly through lists and nested documents


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


## write
```python
write(self, name, data)
```
Redefines the native write function of the dispel4py SimpleFunctionPE to take into account
provenance payload when transfering data.

## getDestination_prov
```python
getDestination_prov(self, data)
```
When provenance is activated it redefines the native dispel4py.new.process getDestination function to take into account provenance information
when redirecting grouped operations.

## commandChain
```python
commandChain(commands, env, queue=None)
```
Utility function to execute a chain of system commands on the hosting oeprating system.
The current environment variable can be passed as parameter env.
The queue parameter is used to store the stdoutdata, stderrdata of each process in message

## getUniqueId
```python
getUniqueId(data=None)
```
Utility function to generate a unique id to be assigned to the provenance entities. As default it uses a combination
of hostname process id and the result of the uuid.uuid1() method.
When data is passed the object id is used in place of the uuid.uuid1()

## num
```python
num(s)
```
Utility function that checks the type of the object passed as s parameter.
If the parameter is a string representing a number it will be returned as float or int

## ProvenanceType
```python
ProvenanceType(self)
```
The type-based approach to provenance collection provides a generic ProvenanceType
that defines the properties of a provenance-aware workflow component. It provides
a wrapper that meets the provenance requirements, while leaving the computational
behaviour of the component unchanged. Types may be developed as PatternType and ContextualType to represent respectively complex
computational patterns and to capture specific metadata contextualisations associated to the produce output data.

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

