1.Isolation 
2.system / resource management 
3.completely faithful simulation

orders of magnitude slower .

**guest kernel in user mode**
1. run guest kernel in user mode
2. trap into virtual machine monitor which has complete control of hardware.
3. run in supervisor mode and return. ( emulate instruction execution)

**guest pagetable**
theoretically: gva -(map)-> gpa -(vmm map)-> hpa
reality:
vmm "shadow" pagetable = guest pgtbl + vmm map
gva -> hpa

**device**
1.emulation(slavishly mimic)

2.virtual devide controlled by vmm ( model for vmm )

3.pass-through a real device(NIC)

**hardware support for virtual machine : VT_X**
hardware get it's non-root registers for guest OS, needn't to go through expensive traps into vmm .
EPT: 2-level pte which allow guest to access pertain page in vmm.