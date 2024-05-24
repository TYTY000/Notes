**Always override `hashCode` when you override `equals`.**


- [](http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@behavioral_equality_same)Behavioral equality is the same as observational equality.
- `equals()` must be overridden to compare abstract values.
- `hashCode()` must be overriden to map the abstract value to an integer.

[](http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@mutable_types)**For mutable types**:

- [](http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@behavioral_equality_different)Behavioral equality is different from observational equality.
- `equals()` should generally not be overriden, but inherit the implementation from `Object` that compares references, just like `==`.
- `hashCode()` should likewise not be overridden, but inherit `Object`’s implementation that maps the reference into an integer.

The best implementation? That is a hard question because it depends on the usage pattern.

A for nearly all cases reasonable good implementation was proposed in _Josh Bloch_'s **_Effective Java_** in Item 8 (second edition). The best thing is to look it up there because the author explains there why the approach is good.

### A short version

1. Create a `int result` and assign a **non-zero** value.
    
2. For _every field_ `f` tested in the `equals()` method, calculate a hash code `c` by:
    
    - If the field f is a `boolean`: calculate `(f ? 0 : 1)`;
    - If the field f is a `byte`, `char`, `short` or `int`: calculate `(int)f`;
    - If the field f is a `long`: calculate `(int)(f ^ (f >>> 32))`;
    - If the field f is a `float`: calculate `Float.floatToIntBits(f)`;
    - If the field f is a `double`: calculate `Double.doubleToLongBits(f)` and handle the return value like every long value;
    - If the field f is an _object_: Use the result of the `hashCode()` method or 0 if `f == null`;
    - If the field f is an _array_: see every field as separate element and calculate the hash value in a _recursive fashion_ and combine the values as described next.
3. Combine the hash value `c` with `result`:
    
    ```java
    result = 37 * result + c
    ```
    
4. Return `result`
    

This should result in a proper distribution of hash values for most use situations.

