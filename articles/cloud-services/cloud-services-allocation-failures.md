<properties
    pageTitle="Poradce při potížích cloudové služby přidělení | Microsoft Azure"
    description="Poradce při potížích přidělení při nasazení Cloudovým službám v Azure"
    services="azure-service-management, cloud-services"
    documentationCenter=""
    authors="simonxjx"
    manager="felixwu"
    editor=""
    tags="top-support-issue"/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-six"/>



# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Poradce při potížích přidělení při nasazení Cloudovým službám v Azure

## <a name="summary"></a>Souhrn
Při nasazení instance do cloudové služby nebo přidat nový web nebo instancí pracovního role, Microsoft Azure přidělí pro využití prostředků. Při provádění tyto operace ještě před dosažením omezení Azure předplatné může občas dojít k chybám. Tento článek popisuje některé běžné chyby přidělení příčiny a navrhne možné remediation. Informace mohou být také užitečné při plán nasazení služeb.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Pozadí – jak funguje přidělení
Servery v Azure datacentrech jsou rozdělit na clusterů. Nová žádost o cloudu přidělení služby je pokus o ve více clusterů. Při nasazení jeho první instanci ke službě cloudového (v pracovní nebo výrobní), který cloudových služeb se Připne clusteru. Další nasazení ke cloudové službě dojde ve stejném clusteru. V tomto článku budeme označovat to jako "připnuté clusteru". Diagram 1 pod znázorňuje velikost písmen normální rozdělení, která je pokus o ve více clusterů; Diagram 2 znázorňuje velikost písmen přidělení, který má připnuté clusteru 2, protože je to, kde je hostovaný existující CS_1 služby cloudu.

![Pole přidělení v diagramu](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Proč se stane přidělení selhání
Při požadavku na přidělení se Připne clusteru, není vyšší pravděpodobnost selhání najdete bezplatné zdroje informací od fondu zdrojů k dispozici se omezí na clusteru. Kromě toho pokud žádosti o přidělení se Připne clusteru, ale typ zdroje, které požadujete nepodporuje tomuto clusteru, žádosti o se nepovede i v případě clusteru má bezplatné zdroje. Diagram 3 pod znázorňuje velikost písmen, kde připojené přidělení selhala, protože clusteru pouze candidate nemá bezplatné zdroje informací. Diagram 4 znázorňuje velikost písmen, kde připojené přidělení selhala, protože clusteru pouze candidate nepodporuje na požadovanou velikost OM, i když clusteru má bezplatné zdroje informací.

![Chyba při připojené přidělení](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Poradce při potížích přidělení služby cloudu
### <a name="error-message"></a>Chybová zpráva
Může zobrazit tato chybová zpráva:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Běžné problémy
Zde jsou běžné scénáře přidělení, které způsobují požadavku na přidělení připnuté do jednoho obrázku.

- Nasazení pracovní pozici – Pokud do cloudové služby má nasazení v obou úsek, pak celé cloudovou službu se Připne konkrétní obrázku.  To znamená, že pokud nasazení již existuje v výrobní úsek, klikněte na nové pracovní nasazení lze pouze rozdělit ve stejném clusteru jako úsek výrobní. Pokud clusteru se blíží kapacitu, může být žádost.

- Změna měřítka – přidání nové instance existující cloudové služby musí přidělit ve stejném clusteru.  Malá měřítka žádosti o můžete obvykle rozdělí, ale ne vždy. Pokud clusteru se blíží kapacitu, může být žádost.

- Skupina spřažení - nového nasazení prázdné cloudové služby můžete přidělují struktury v libovolné obrázku v dané oblasti pokud cloudovou službu se Připne spřažení skupiny. Nasazení do stejné skupiny spřažení se pokusí stejné clusteru. Pokud clusteru se blíží kapacitu, může být žádost.

- Skupina spřažení vNet - starší virtuálních sítí byly stejným skupinám spřažení místo regiony a cloudovým službám v těchto virtuální sítí by připnuté clusteru spřažení skupiny. Nasazení na tento typ virtuální sítě se pokusí připnuté clusteru. Pokud clusteru se blíží kapacitu, může být žádost.

## <a name="solutions"></a>Řešení

1. Přeinstalujte nové cloudové služby: Toto řešení je pravděpodobné, že většina úspěšné jako umožňuje platformu můžete vybírat všech clusterů v dané oblasti.

    - Nasazení pracovní zátěž nové cloudové služby  

    - Aktualizovat záznamy CNAME nebo záznam tak, aby ukazovaly přenosy nové cloudové služby

    - Po nula přenos bude starého webu, můžete odstranit staré cloudové služby. Toto řešení měly ukládat nula výpadek služeb.

2. Odstranění výrobní a přípravu sloty: Toto řešení stávající název DNS zachová, ale bude mít za následek prostoj aplikace.

    - Odstranění produkční a pracovní průřezy existující cloudové služby, takže cloudové služby je prázdná a potom

    - Vytvořte nové nasazení v existující cloudové službě. Znova se pokusí o přidělení na všech clusterů v oblasti. Zajistěte, aby že se skupina spřažení není stejným cloudovou službu.

3. User - toto řešení zachová existující IP adresy, ale bude mít za následek prostoj aplikace.  

    - Vytvoření ReservedIP pro existující nasazení pomocí prostředí Powershell

    ```
    New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
    ```

    - Postupujte podle #2 z horní, jak zajistit zadáte nový ReservedIP v CSCFG služby.

4. Odebrání skupiny pro spřažení nové nasazení - doporučuje se už spřažení skupiny. Postupujte podle kroků 1 # výše uvedeného nasadit do nového cloudové služby. Zajištění Cloudová služba není ve skupině spřažení.

5. Převeďte místní virtuální sítě – přečtěte si téma [jak migrovat ze skupin spřažení k síti místní virtuální (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
