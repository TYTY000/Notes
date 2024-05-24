Like Haskell, (Stream needed)
map -> map
forEach -> forM_

filter -> filter
reduce -> foldr
```java
List.of(1,2,3).stream()
    .reduce(0, (x,y) -> x+y)
```

```haskell
foldr (+) 0 [1,2,3]
```
  
In Java, because the operator is required to be associative, the implementation of `reduce` is free to choose any order of combination, including partial combinations from within the sequence.
```java
List.of(5, 8, 3, 1).stream()
    .reduce(Math::max)
// computes max(max(max(5,8),3),1) and returns an Optional<Integer> value containing 8
```

```haskell
maximum [5,8,3,1]
```