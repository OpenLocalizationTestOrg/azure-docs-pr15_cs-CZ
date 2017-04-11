## <a name="scenario"></a>Scénář

Tento dokument líp ukazují, jak vytvořit VNet a podsítí, budou používat scénář níže.

![Scénář VNet](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

V tomto scénáři vytvoříte VNet s názvem **TestVNet** s rezervovaná CIDR blokem **192.168.0.0./16**. Vaše VNet bude obsahovat následující podsítí: 

- **FrontEnd**, pomocí **192.168.1.0/24** jako jeho blokování CIDR.
- **Back-end**, pomocí **192.168.2.0/24** jako jeho blokování CIDR.

 