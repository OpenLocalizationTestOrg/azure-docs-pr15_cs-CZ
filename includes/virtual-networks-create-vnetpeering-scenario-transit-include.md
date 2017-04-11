## <a name="service-chaining---transit-through-peered-vnet"></a>Služba zřetězení - dopravní prostřednictvím peered VNet

Přestože použití systému směruje usnadňuje přenosy automaticky pro nasazení, jsou případy, ve kterých chcete určit směrování paketů prostřednictvím virtuální zařízení.
V tomto scénáři dvou VNets v jsou předplatné, HubVNet a VNet1 podle popisu v diagramu pod. Můžete nasadit sítě virtuální Appliance(NVA) v VNet HubVNet. Po vytvoření VNet prozkoumávání mezi HubVNet a VNet1, můžete nastavit uživatele definované a určit přesměrování na NVA v HubVNet.

![Při přenosu šifrovaná NVA](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [AZURE.NOTE] Pro zjednodušení předpokládá, že všechny VNets tady jsou ve stejném předplatném. Fungují ale i pro scénář křížově předplatného.

Vlastnost klíče povolení směrování přenosu je parametr "Povolit přesměrování přenosy". Díky přijímání a odesílání přenos z / NVA peered VNet.  
