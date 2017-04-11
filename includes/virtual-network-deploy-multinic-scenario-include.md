## <a name="scenario"></a>Scénář

Tento dokument provede jednotlivými nasazení, které používá více nic v VMs ve scénáři konkrétní. V tomto scénáři máte IaaS dvouvrstvý pracovního vytížení hostované v Azure. Jednotlivé vrstvy nasazenou v samostatném podsítě v síti virtuální (VNet). Vrstvy front-end se skládá z několika webových serverů, seskupeny do Vyrovnávání zatížení nastavení vysoké dostupnosti. Vrstvy back-end se skládá z několika databázový server. Tyto servery databáze má být nasazené s dvěma nic, jedno pro přístup k databázi, druhý pro správu. Scénáře také sítě skupin zabezpečení (NSGs) přenosy, které mohou každý podsítě a NIC v nasazení. Následující obrázek ukazuje základní architektura tento scénář.  

![Scénář MultiNIC](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

