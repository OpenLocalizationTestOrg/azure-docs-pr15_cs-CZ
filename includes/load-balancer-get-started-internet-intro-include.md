Řešení pro vyrovnávání zatížení Azure je řešení pro vyrovnávání zatížení vrstvě 4 (TCP, UDP). Služba Vyrovnávání zatížení sítě distribuuje příchozí komunikaci mezi zdravé služby instance služby cloud nebo virtuálních počítačů v sadě Vyrovnávání zatížení poskytuje vysokou dostupnost. Azure služba Vyrovnávání zatížení sítě můžete zobrazit tyto služby na více portech a více adres IP.

Můžete nakonfigurovat Vyrovnávání zatížení pro:

* Načtěte Zůstatek příchozího internetového provozu pro virtuální počítače (VMs). Služba Vyrovnávání zatížení sítě v tomto scénáři označovány jako [Internetové služby Vyrovnávání zatížení](../articles/load-balancer/load-balancer-internet-overview.md).
* Přenos zůstatku zatížení mezi VMs v virtual network (VNet), mezi VMs v cloudu služby nebo mezi místního počítače a VMs v prostorách mezi virtuální sítě. Služba Vyrovnávání zatížení sítě v tomto scénáři označovány jako [vnitřní vyrovnávání zátěže (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Externí přenosu pro konkrétní instance VM.
