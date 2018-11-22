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
@param: data The data object's id is used in place of the uuid.uuid1()

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
The type-based approach to provenance collection provides a generic ProvenanceType class
that defines the properties of a provenance-aware workflow component. It provides
a wrapper that meets the provenance requirements, while leaving the computational
behaviour of the component unchanged. Types may be developed as PatternType and ContextualType to represent respectively complex
computational patterns and to capture specific metadata contextualisations associated to the produce output data.

### BULK_SIZE
int(x=0) -> int or long
int(x, base=10) -> int or long

Convert a number or string to an integer, or return 0 if no arguments
are given.  If x is floating point, the conversion truncates towards zero.
If x is outside the integer range, the function returns a long instead.

If x is not a number or if base is given, then x must be a string or
Unicode object representing an integer literal in the given base.  The
literal can be preceded by '+' or '-' and be surrounded by whitespace.
The base defaults to 10.  Valid bases are 0 and 2-36.  Base 0 means to
interpret the base from the string as an integer literal.
>>> int('0b100', base=0)
4
### PROV_EXPORT_URL
str(object='') -> string

Return a nice string representation of the object.
If the argument is a string, the return value is the same object.
### PROV_PATH
str(object='') -> string

Return a nice string representation of the object.
If the argument is a string, the return value is the same object.
### REPOS_URL
str(object='') -> string

Return a nice string representation of the object.
If the argument is a string, the return value is the same object.
### SAVE_MODE_FILE
str(object='') -> string

Return a nice string representation of the object.
If the argument is a string, the return value is the same object.
### SAVE_MODE_SENSOR
str(object='') -> string

Return a nice string representation of the object.
If the argument is a string, the return value is the same object.
### SAVE_MODE_SERVICE
str(object='') -> string

Return a nice string representation of the object.
If the argument is a string, the return value is the same object.
### send_prov_to_sensor
bool(x) -> bool

Returns True when the argument x is true, False otherwise.
The builtins True and False are the only two instances of the class bool.
The class bool is a subclass of the class int, and cannot be subclassed.
### getProvStateObjectId
```python
ProvenanceType.getProvStateObjectId(self, name)
```
Documentation for a function.
More details.

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

### INPUT_NAME
str(object='') -> string

Return a nice string representation of the object.
If the argument is a string, the return value is the same object.
### REPOS_URL
str(object='') -> string

Return a nice string representation of the object.
If the argument is a string, the return value is the same object.
