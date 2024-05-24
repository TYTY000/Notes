  
This reading talked about three major ways to achieve safety from race conditions on shared mutable state:

- [](http://web.mit.edu/6.031/www/sp21/classes/21-thread-safety/#@confinement_not_sharing)Confinement: not sharing the variables or data.
- Immutability: sharing, but keeping the data immutable and variables unreassignable.
- Threadsafe data types: storing the shared mutable data in a single threadsafe datatype.

[](http://web.mit.edu/6.031/www/sp21/classes/21-thread-safety/#@these_ideas_connect)These ideas connect to our three key properties of good software as follows:

- [](http://web.mit.edu/6.031/www/sp21/classes/21-thread-safety/#@safe_bugs_trying)**Safe from bugs.** We’re trying to eliminate a major class of concurrency bugs, race conditions, and eliminate them by design, not just by accident of timing.
    
- [](http://web.mit.edu/6.031/www/sp21/classes/21-thread-safety/#@easy_understand_applying)**Easy to understand.** Applying these general, simple design patterns is far more understandable than a complex argument about which thread interleavings are possible and which are not.
    
- [](http://web.mit.edu/6.031/www/sp21/classes/21-thread-safety/#@ready_change_writing)**Ready for change.** We’re writing down these justifications explicitly in a thread safety argument, so that maintenance programmers know what the code depends on for its thread safety.