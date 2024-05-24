fundamental： cache + out-of-order + isolation 

```
Flush & Reload
1  char buf[8192];
2  // flush
3  clflush buf[0]
4  clflush buf[4096]
5  
6  <some expensive instrction like devide>
7
8  r1 = <a kernel virtual address>
9  r2 = *r1
10 r2 = r2 & 1
11 r2 = r2 * 4096
12 r3 = buf[r2]
13
14 <handle the page fault from line 9>
15 
16 // reload
17 a = rdtsc
18 r0 = buf[0]
19 b = rdtsc
20 r1 = buf[4096]
21 c = rdtsc
22 if b-a < c-b:
23   low big was probably 0
```
