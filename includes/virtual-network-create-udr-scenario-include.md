## <a name="scenario"></a>Scénář

Tento dokument líp ukazují, jak vytvořit UDRs, budou používat scénář níže.

![POPIS OBRÁZKU](./media/virtual-network-create-udr-scenario-include/figure1.png)

V tomto scénáři vytvoříte jeden UDR *přední konce podsítě* a jiné UDR *zpět ukončit podsítě* podle níže uvedeného popisu: 

- **UDR FrontEnd**. Front-end UDR použije se podsítě *FrontEnd* a obsahují jednu postupu:  
    - **RouteToBackend**. Tento postup vám pošle všechny přenosy back-end připravenost **FW1** virtuálního počítače.
- **UDR back-end**. Back-end UDR použije se podsítě *back-end* a obsahují jednu postupu: 
    - **RouteToFrontend**. Tento postup vám pošle všechny přenosy front-end připravenost **FW1** virtuálního počítače.

Kombinace těchto postupů zajistí, že budou všechny přenosy určené z jednoho podsítě do jiného směrovány **FW1** virtuální zařízení, které se používá jako virtuální zařízení. Budete potřebovat zapnout IP přesměrování pro tuto OM, zajistit, že ho obdrží přenosy určené pro ostatní VMs.
