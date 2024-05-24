![[Pasted image 20230830151004.png]]
- Thread.stop
- Kernel.sleep
- (instance method) wakeup & run 
- Thread.exit
- (instance method) terminate, kill
- terminate! / exit! / kill! 不执行ensure
- raise exception( if cannot handle, become terminated with exception /nil state) state
- Killing a thread is dangerous, killing a thread with ! methods is even more dangerous, leaving files, sockets, and other resources open.

-  Thread.list