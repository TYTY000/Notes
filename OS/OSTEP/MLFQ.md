Multi-Level-Frequency-Queue
• Rule 1: If Priority(A) > Priority(B), A runs (B doesn’t).  优先级的意义
• Rule 2: If Priority(A) = Priority(B), A & B run in RR.  同级优先处置
• Rule 3: When a job enters the system, it is placed at the highest  
priority (the topmost queue).  新增
• Rule 4: Once a job uses up its time allotment at a given level (regardless of how many times it has given up the CPU), its priority is  
reduced (i.e., it moves down one queue).  降级条件
• Rule 5: After some time period S, move all the jobs in the system  
to the topmost queue.  刷新机制

是对短期周转和长期运行的最好权衡