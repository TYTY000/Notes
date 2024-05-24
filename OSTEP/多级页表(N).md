if (TLB Hit)
	//access(TLB_addr)
	TLB_offset = (VA&OFFSET_MASK)
	PA = TLB_entry_PFN | TLB_offset
	Register = access (PA)
else     //查第一个页表
	PDIndex1 =( VPN &PD_MASK )>>PD_SHIFT; 
	PDEAddr = PDIndex1 * sizeof(PDE)+ PDBR;
	PDE = access (PDEAddr);
	do   //i=1
		check_valid(PDE[i]); //failed-->seg fault
		if i>=2 check_protection(PDE[i]);
		PTIndex[i] = (VPN&PT_MASK[i])>> PT_SHIFT;
		PTEAddr[i] = ( PFN  << SHIFT ) + (PTIndex * sizeof (PTE));
		PTE[i] = access(PTEAddr[i]);
	while(i<=N);
