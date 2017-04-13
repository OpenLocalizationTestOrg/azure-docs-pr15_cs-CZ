<properties 
    pageTitle="Přesunutí zdroje do nové skupiny prostředků | Microsoft Azure" 
    description="Správce prostředků Azure používejte k přecházení zdrojů do nové skupiny prostředků nebo předplatného." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/21/2016" 
    ms.author="tomfitz"/>

# <a name="move-resources-to-new-resource-group-or-subscription"></a>Přesunutí zdroje do nové skupiny prostředků nebo předplatného

V tomto tématu se dozvíte, jak přesunout prostředky k novému předplatnému nebo nové skupiny prostředků v rámci stejného předplatného. Můžete portálu prostředí PowerShell, Azure rozhraní příkazového řádku a rozhraní REST API přesunutí zdroje. Operace přesunout v tomto tématu jsou k dispozici bez pomoc od Azure podpory.

Při rozhodování, které se obvykle přesuňte prostředky:

- Z důvodu fakturační zdroj třeba live v jiné předplatné.
- Zdroj už sdílí stejné životního cyklu jako prostředky, které byly dříve seskupeny s. Chcete-li přesunout na jiné místo do nové skupiny prostředků tak daný zdroj můžete spravovat samostatně z jiných zdrojů.

Při přesunu zdroje se skupinou zdrojové a cílové skupině uzamčené během operace. Psaní a odstraňování blokované ve skupinách až do dokončení přesunout.

Nemůžete změnit umístění zdroje. Přesunutí zdroje pouze přesune do nové skupiny prostředků. Nová skupina zdrojů může mít do jiného umístění, ale, který nelze změnit umístění zdroje.

> [AZURE.NOTE] Tento článek popisuje, jak přesunout zdrojů v existující Azure účet nabízení. Pokud Opravdu chcete změnit účet Azure nabízející (například upgrade z přislíbený předem zaplatit) a pokračujte v práci s existujících zdrojů, najdete v článku [přepínání předplatného Azure na jinou nabídku](billing-how-to-switch-azure-offer.md). 

## <a name="checklist-before-moving-resources"></a>Kontrolní seznam teprve pak přejděte zdroje

Existuje několik důležitých kroků k provedení teprve pak přejděte zdroje. Ověřením tyto podmínky se můžete vyhnout chyby.

1. Služba musí povolit možnost přesunout prostředky. Prohlédněte si seznam pod informace o které [služby umožňují přesunutí zdroje](#services-that-enable-move).
1. Předplatná zdrojové a cílové musí existovat ve stejném [klientovi služby Active Directory](./active-directory/active-directory-howto-tenant.md). Pokud chcete přesunout do nového klienta, zavolejte na linku podpory.
2. Určení předplatné musí registrované pro zprostředkovatele prostředků zdroje, která je přesouvána. V opačném případě dojít k chybě oznamující, že **předplatné není registrovaný u typu zdroje**. Můžete narazit potíže při přesunu zdroj k novému předplatnému, ale, že předplatné je použito nikdy tento typ zdroje. Se dozvíte, jak chcete zkontrolovat stav registraci a zaregistrovat zdroje poskytovatelů, najdete v článku [poskytovatelé zdroje a typy](../resource-manager-supported-services.md#resource-providers-and-types).
4. Pokud přesunujete aplikaci služby aplikace, zkontrolování [omezení aplikace služeb](#app-service-limitations).
4. Pokud přesunujete prostředků přidružených k obnovení služeb, zkontrolování [služby Recovery omezení](#recovery-services-limitations)
5. Pokud přesunujete zdrojů pomocí klasického modelu, si přečtete [klasické nasazení omezení](#classic-deployment-limitations).

## <a name="when-to-call-support"></a>Pokud chcete zavolat na linku podpory

Můžete přesouvat většina prostředky prostřednictvím samoobslužné operace zobrazené v tomto tématu. Použijte operace samoobslužných funkcí, které mají:

- Přesunutí zdroje správce prostředků.
- Přesunutí klasické zdroje podle [omezení klasické nasazení](#classic-deployment-limitations). 

Pokud je potřeba zavolat na linku podpory:

- Přesunutí zdroje nový účet Azure (a služby Active Directory klienta).
- Přesunutí klasické zdroje, ale máte potíže s omezeními.

## <a name="services-that-enable-move"></a>Služeb, které umožňují přesunout

Nyní jsou služeb, které umožňují přesunutí do nové skupiny prostředků i předplatného:

- Rozhraní API správy
- Aplikace služby aplikace (web apps) – najdete v článku [omezení služeb aplikací](#app-service-limitations)
- Automatizace
- Dávkové
- CDN
- Cloudových služeb – najdete v článku [Omezení nasazení klasické](#classic-deployment-limitations)
- Data Factory
- DNS
- DocumentDB
- HDInsight clusterů
- IoT rozbočovače
- Klíčové trezoru 
- Mediální služby
- Mobilní zapojení
- Oznámení o rozbočovače
- Provozní přehledy
- Redis mezipaměti
- Plánovač
- Hledání
- Služba Bus
- Úložiště
- Úložiště (classic) – najdete v článku [Omezení nasazení Classic](#classic-deployment-limitations)
- Databáze SQL server – databáze a serveru musí být umístěny ve stejné skupině zdroje. Obsah přesouvaných SQL server, všechny své databáze se přesouvají.
- Virtuálních počítačích - však slouží nepodporují přesunutí k novému předplatnému, pokud jeho certifikáty jsou uloženy v trezoru klíč
- Virtuálních počítačích (klasický) – najdete v článku [Omezení nasazení klasické](#classic-deployment-limitations)
- Virtuální sítě

## <a name="services-that-do-not-enable-move"></a>Služby, které neumožňují přesunout

Jsou služby, které aktuálně neumožňují přesunutí zdroje:

- Aplikace brány
- Přehledy aplikace
- Expresní směrování
- Obnovení služby trezoru - taky není přesunout výpočetním, sítě a úložiště prostředky přidružené trezoru služby Recovery naleznete v tématu [obnovení služby omezení](#recovery-services-limitations).
- Virtuálních počítačích s certifikátem uložené v trezoru klíč
- Sady měřítko virtuálních počítačích
- Virtuální sítí (klasický) – najdete v článku [Omezení nasazení klasické](#classic-deployment-limitations)
- Brána VPN

## <a name="app-service-limitations"></a>Omezení služeb aplikací

Při práci s aplikacemi jiných aplikací, nelze přesunout jenom plán aplikaci služby. Přesunout aplikaci služby aplikace, jsou možností:

- Přesunutí plán služeb aplikací a dalších zdrojů aplikace služby v dané skupině zdrojů do nové skupiny prostředků, které ještě neobsahují aplikaci služby zdrojů. Tento požadavek znamená, že musíte přesunout nebo zkopírovat i aplikaci služby zdroje informací, které nejsou přidružené plán služeb aplikací. 
- Přesunout do skupiny různých zdrojů aplikace, ale zachovat všechny plány aplikaci služby v původní skupina zdroje.

Pokud původní skupina zdroje obsahuje také přehledy aplikace zdroje, přesouvat nemůžete daný zdroj, protože aplikace přehledy neumožňuje aktuálně operaci přesunout. Jestliže zahrnete zdrojů aplikace přehledy při přesunu aplikaci služby aplikace, se celý přesunout nezdaří. Však přehledy aplikace a plán služeb aplikací nemusí být umístěny ve stejné skupině zdrojů aplikace aplikace budou fungovat správně.

Pokud například skupina zdroje obsahuje:

- **web – a** který je spojený s **plán a** a **aplikace přehledy a**
- **web-b** , který je spojený s **plánem b** a **aplikace přehledy b**

Jsou vaše možnosti:

- Přesunutí **webu a**, **plánu a**, **web-b**a **plán b**
- Přesunutí **webu a** a **web b**
- Přesunutí **webu a**
- Přesunutí **webové b**

Všechny ostatní kombinace zahrnují posunete typ zdroje, který nelze přejít (aplikace přehledy) nebo nechte za typ zdroje, který nelze zůstat při přesunu plán aplikace služeb (libovolný typ zdroje aplikaci služby).

Pokud web appu je umístěn v jiné skupině zdrojů než svůj plán služeb aplikací, ale chcete přesunout obojí můžete nové skupiny prostředků, je nutné provést tento přesun ve dvou krocích. Příklad:

- **web – a** nachází ve **skupině web**
- **plán a** nachází ve **skupině plán**
- Chcete, aby **web a** a **plán a** být umístěny v **kombinaci skupiny**

K provedení této přesunout, proveďte operace dvou samostatných přesunout v následujícím pořadí:

1. Přesunutí **web a** do **skupiny plán**
2. Přesunutí **webu a** a **plán a** **kombinované**skupiny.

Certifikát služby aplikace můžete přesunout do nové skupiny prostředků nebo předplatné bez jakýchkoli problémů. Ale pokud webovou aplikaci certifikát SSL, které jste si koupili externě odeslat do aplikace, musí odstraníte certifikát teprve pak přejděte web appu. Provedením následujících kroků:

1. Nahrané certifikát odstranit z web appu
2. Přesunutí web appu
3. Nahrání certifikátu do web appu

## <a name="recovery-services-limitations"></a>Obnovení služby omezení

Přesunutí není povolený úložiště, síť nebo výpočet zdroje použili k nastavení havárie obnovení s obnovení webu Azure. 

Předpokládejme například, nastavili replikace místního počítače k účtu úložiště (Storage1) a chcete chráněné počítače přijít po převzetí Azure jako virtuálního počítače (VM1) připojené k síti virtuální (Network1). Přesouvat nemůžete některou z těchto Azure zdrojů – Storage1 VM1 a Network1 - přes skupiny zdrojů ve stejném předplatném nebo přes předplatná.

## <a name="classic-deployment-limitations"></a>Omezení klasické nasazení

Možnosti pro přesouvání prostředků nasazených pomocí klasického modelu se liší v závislosti na tom, jestli jsou přesouvání prostředků v rámci předplatného nebo k novému předplatnému. 

### <a name="same-subscription"></a>Stejné předplatného

Při přesouvání zdrojů z jedné skupině zdrojů do jiné skupiny zdrojů ve stejném předplatném, platí následující omezení:

- Nebylo možné přesunout virtuálních sítí (klasické).
- Virtuálních počítačích (klasický) musí přesune pomocí cloudovou službu. 
- Cloudová služba pouze přesouvat při přesunutí včetně všech jeho virtuálních počítačích.
- Jedinou cloudové služby přesouvat najednou.
- Najednou lze přesunout jen s jedním klientem úložiště (klasické).
- Úložiště účtu (classic) nelze přesunout v rámci jedné operace s virtuálního počítače nebo do cloudové služby.

K přesunutí klasické zdrojů do nové skupiny prostředků ve stejném předplatném, použijte operace standardních přesunu prostřednictvím [portálu](#use-portal), [Azure Powershellu](#use-powershell), [Rozhraní příkazového řádku Azure](#use-azure-cli)nebo [Rozhraní REST API](#use-rest-api). Použijete stejné operace při použití pro přesouvání správce prostředků prostředků.

### <a name="new-subscription"></a>Nové předplatné

Při přesunu prostředky k novému předplatnému, platí následující omezení:

- V stejnou operaci musí přesouvat všechny classic zdroje v předplatného.
- Předplatné cílové nesmí obsahovat další klasické zdroje.
- Přesunutí můžete požadované pouze prostřednictvím samostatné rozhraní REST API pro klasické přesune. Při přesunu klasické prostředky k novému předplatnému nefungují standardní příkazy přesunout správce prostředků.

Přesunout klasické prostředky k novému předplatnému, je nutné použít ZBÝVAJÍCÍ operacích, které jsou specifické pro klasické zdroje. Proveďte následující kroky pro přesun klasické prostředky k novému předplatnému.

1. Zaškrtněte, pokud předplatné zdroj se může účastnit přesunout více předplatného. Použijte následující operace:

         POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
    
     V hlavním textu žádosti o patří:

         {
           "role": "source"
         }

     Odpověď na operaci ověření probíhá v tomto formátu:

         {
           "status": "{status}",
           "reasons": [
             "reason1",
             "reason2"
           ]
         }

2. Zaškrtněte, pokud předplatné cíl může účastnit přesunout více předplatného. Použijte následující operace:

         POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01

     V hlavním textu žádosti o patří:

         {
           "role": "target"
         }

     Odpověď je ve stejném formátu jako předplatné ověření zdroje.

3. Pokud obou předplatná předat ověření, přesuňte všechny klasické zdrojů jedno předplatné na jiné předplatné s následující operace:

         POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01

    V hlavním textu žádosti o patří:

         {
           "target": "/subscriptions/{target-subscription-id}"
         }

Operace může používat několik minut. 

## <a name="use-portal"></a>Pomocí portálu

Přesunout zdrojů do nové skupiny prostředků ve **stejném předplatném**, vyberte skupina zdroje obsahující tyto prostředky a pak klikněte na tlačítko **Přesunout** .

![Přesunutí zdroje](./media/resource-group-move-resources/edit-rg-icon.png)

Nebo přejdete prostředky k **novému předplatnému**vyberte skupina zdroje obsahující tyto prostředky a potom vyberte ikonu předplatné upravit.

![Přesunutí zdroje](./media/resource-group-move-resources/change-subscription.png)

Vyberte zdroje, které chcete přesunout a cílové skupině zdroje. Potvrďte, že budete muset aktualizovat skripty pro tyto materiály a klikněte na **OK**. Pokud jste vybrali na ikonu úpravy předplatné v předchozím kroku, je nutné zaškrtnout cílového odběru.

![Výběr cíle](./media/resource-group-move-resources/select-destination.png)

V části **oznámení**uvidíte, jestli je spuštěný operaci přesunout.

![Zobrazení stavu přesunout](./media/resource-group-move-resources/show-status.png)

Po dokončení, budete upozorněni výsledku.

![zobrazení výsledků přesunout](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>Pomocí prostředí PowerShell

Přesunout existujících zdrojů do jiného pole Skupina zdroje nebo předplatného, pomocí příkazu **Přesunout AzureRmResource** .

První příklad ukazuje, jak přesunout jeden zdroj do nové skupiny prostředků.

    $resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId

Druhý příklad ukazuje, jak přesunout více zdrojů do nové skupiny prostředků.

    $webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
    $plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId

Přesunout k novému předplatnému, zadat hodnotu parametru **DestinationSubscriptionId** .

Zobrazí se výzva k potvrzení, že chcete přesunout zadané zdroje.

    Confirm
    Are you sure you want to move these resources to the resource group
    '/subscriptions/{guid}/resourceGroups/newRG' the resources:

    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

## <a name="use-azure-cli"></a>Použití Azure rozhraní příkazového řádku

Existujících zdrojů přesunout na jiné pole Skupina zdroje nebo předplatného, použijte příkaz **Přesunout azure zdroje** . Je třeba zadat ID zdroje zdroje, které chcete přesunout. Můžete získat ID zdroje s Tento příkaz:

    azure resource list -g sourceGroup --json

Vracející v tomto formátu:

    [
      {
        "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
        "name": "storagedemo",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "southcentralus",
        "tags": {},
        "kind": "Storage",
        "sku": {
          "name": "Standard_RAGRS",
          "tier": "Standard"
        }
      }
    ]

Následující příklad ukazuje, jak přesunout účet úložiště do nové skupiny prostředků. V parametru **-i** poskytují že hodnoty oddělené seznam číslo id zdroje je přesunout.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
    
Zobrazí se výzva k potvrzení, že chcete přesunout je daný zdroj.

## <a name="use-rest-api"></a>Použití rozhraní REST API

Budete chtít přesunout existujících zdrojů do jiného pole Skupina zdroje nebo předplatného, spusťte:

    POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version} 

V hlavním textu žádosti o zadáte cílové skupině zdroje a prostředky přejít. Další informace o ZBÝVAJÍCÍ práci přesunout najdete v článku [Přesunutí zdroje](https://msdn.microsoft.com/library/azure/mt218710.aspx).


## <a name="next-steps"></a>Další kroky
- Další informace o rutinách Powershellu pro správu předplatného, najdete v článku [použití Azure pomocí Správce prostředků](powershell-azure-resource-manager.md).
- Další informace o příkazech Azure rozhraní příkazového řádku pro správu předplatného najdete v tématu [použití rozhraní příkazového řádku Azure pomocí Správce prostředků](xplat-cli-azure-resource-manager.md).
- Další informace o portálu funkcí pro správu předplatného najdete v tématu [použití Azure portálu pro přidávání a používání zdrojů](./azure-portal/resource-group-portal.md).
- Další informace o použití logického organizace svých prostředcích najdete v tématu [použití značek k uspořádání zdroje](resource-group-using-tags.md).
