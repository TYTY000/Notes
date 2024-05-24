Here is a summary of the functions that work with `malloc`:

`void *malloc (size_t size)`

Allocate a block of size bytes. See [Basic Memory Allocation](https://www.gnu.org/software/libc/manual/html_node/Basic-Allocation.html).

`void free (void *addr)`

Free a block previously allocated by `malloc`. See [Freeing Memory Allocated with `malloc`](https://www.gnu.org/software/libc/manual/html_node/Freeing-after-Malloc.html).

`void *realloc (void *addr, size_t size)`

Make a block previously allocated by `malloc` larger or smaller, possibly by copying it to a new location. See [Changing the Size of a Block](https://www.gnu.org/software/libc/manual/html_node/Changing-Block-Size.html).

`void *reallocarray (void *ptr, size_t nmemb, size_t size)`

Change the size of a block previously allocated by `malloc` to `nmemb * size` bytes as with `realloc`. See [Changing the Size of a Block](https://www.gnu.org/software/libc/manual/html_node/Changing-Block-Size.html).

`void *calloc (size_t count, size_t eltsize)`

Allocate a block of count * eltsize bytes using `malloc`, and set its contents to zero. See [Allocating Cleared Space](https://www.gnu.org/software/libc/manual/html_node/Allocating-Cleared-Space.html).

`void *valloc (size_t size)`

Allocate a block of size bytes, starting on a page boundary. See [Allocating Aligned Memory Blocks](https://www.gnu.org/software/libc/manual/html_node/Aligned-Memory-Blocks.html).

`void *aligned_alloc (size_t size, size_t alignment)`

Allocate a block of size bytes, starting on an address that is a multiple of alignment. See [Allocating Aligned Memory Blocks](https://www.gnu.org/software/libc/manual/html_node/Aligned-Memory-Blocks.html).

`int posix_memalign (void **memptr, size_t alignment, size_t size)`

Allocate a block of size bytes, starting on an address that is a multiple of alignment. See [Allocating Aligned Memory Blocks](https://www.gnu.org/software/libc/manual/html_node/Aligned-Memory-Blocks.html).

`void *memalign (size_t size, size_t boundary)`

Allocate a block of size bytes, starting on an address that is a multiple of boundary. See [Allocating Aligned Memory Blocks](https://www.gnu.org/software/libc/manual/html_node/Aligned-Memory-Blocks.html).

`int mallopt (int param, int value)`

Adjust a tunable parameter. See [Malloc Tunable Parameters](https://www.gnu.org/software/libc/manual/html_node/Malloc-Tunable-Parameters.html).

`int mcheck (void (*abortfn) (void))`

Tell `malloc` to perform occasional consistency checks on dynamically allocated memory, and to call abortfn when an inconsistency is found. See [Heap Consistency Checking](https://www.gnu.org/software/libc/manual/html_node/Heap-Consistency-Checking.html).

`struct mallinfo2 mallinfo2 (void)`

Return information about the current dynamic memory usage. See [Statistics for Memory Allocation with `malloc`](https://www.gnu.org/software/libc/manual/html_node/Statistics-of-Malloc.html).