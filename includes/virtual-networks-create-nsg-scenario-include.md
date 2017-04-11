## <a name="scenario"></a>Scénář

Tento dokument líp ukazují, jak vytvořit NSGs, budou používat scénář níže.

![Scénář VNet](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

V tomto scénáři vytvoříte NSG pro každou podsítě virtuální sítě **TestVNet** podle níže uvedeného popisu: 

- **NSG FrontEnd**. Front-end NSG použije se podsítě *FrontEnd* a obsahují dvě pravidla:  
    - **rdp pravidlo**. Pokud chcete toto pravidlo bude přenosy RDP podsítě *FrontEnd* .
    - **pravidla na webu**. Toto pravidlo bude přenosy protokolu HTTP podsítě *FrontEnd* .
- **NSG back-end**. Back-end NSG použije se podsítě *back-end* a obsahují dvě pravidla: 
    - **pravidlo sql**. Pokud chcete toto pravidlo umožňuje SQL přenos jenom z podsítě *FrontEnd* .
    - **pravidla na webu**. Pokud chcete toto pravidlo zakazuje že všechny internetové přenosy vazby z podsítě *back-end* .

Kombinace jednotlivá pravidla vytvořit scénáři DMZ profesionálové podsítě back-end můžete přijímat, jen příchozích pro SQL z podsítě front-end kde má přístup k Internetu, zatímco podsítě front-end můžete komunikovat prostřednictvím Internetu a přijímat příchozí žádosti HTTP pouze.
 
