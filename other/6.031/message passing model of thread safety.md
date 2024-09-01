## Thread safety arguments with message passing

[](http://web.mit.edu/6.031/www/sp21/classes/23-queues/#@thread_safety_argument)A thread safety argument with message passing might rely on:

- [](http://web.mit.edu/6.031/www/sp21/classes/23-queues/#@existing_threadsafe_data)**Existing threadsafe data types** for the synchronized queue. This queue is definitely shared and definitely mutable, so we must ensure it is safe for concurrency.
    
- [](http://web.mit.edu/6.031/www/sp21/classes/23-queues/#@immutability_messages_data)**Immutability** of messages or data that might be accessible to multiple threads at the same time.
    
- [](http://web.mit.edu/6.031/www/sp21/classes/23-queues/#@confinement_data_individual)**Confinement** of data to individual producer/consumer threads. Local variables used by one producer or consumer are not visible to other threads, which only communicate with one another using messages in the queue.
    
- [](http://web.mit.edu/6.031/www/sp21/classes/23-queues/#@confinement_mutable_messages)**Confinement** of mutable messages or data that are sent over the queue but will only be accessible to one thread at a time. This argument must be carefully articulated and implemented. Suppose one thread has some mutable data to send to another thread. If the first thread drops all references to the data like a hot potato as soon as it puts them onto a queue for delivery to the other thread, then only one thread will have access to those data at a time, precluding concurrent access.