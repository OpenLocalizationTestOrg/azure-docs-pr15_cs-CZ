<properties
   pageTitle="Zabezpečené cluster v systému Windows pomocí zabezpečení systému Windows | Microsoft Azure"
   description="Zjistěte, jak nakonfigurovat mezi uzly a uzel klient zabezpečení samostatného obrázku v systému Windows pomocí zabezpečení systému Windows."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="secure-a-standalone-cluster-on-windows-using-windows-security"></a>Zabezpečené samostatného obrázku v systému Windows pomocí zabezpečení systému Windows

Abyste zabránili neoprávněnému přístupu do služby struktury clusteru je nutné zabezpečit, zvlášť když má výrobní pracovní vytížení ho. Tento článek popisuje, jak konfigurovat mezi uzly a uzel klient zabezpečení pomocí zabezpečení systému Windows v souboru *ClusterConfig.JSON* a odpovídá kroku konfigurovat zabezpečení [vytvoření samostatného obrázku v systému Windows](service-fabric-cluster-creation-for-windows-server.md). Další informace o použití služby struktury zabezpečení systému Windows najdete v článku [scénáře zabezpečení obrázku](service-fabric-cluster-security.md).

>[AZURE.NOTE]
Měli byste zvážit výběr zabezpečení cenného papíru mezi uzly opatrně, protože upgrade bez obrázku z jednoho zabezpečení volba do jiného. Změna výběru zabezpečení vyžadují opětovného vytvoření úplné obrázku.

## <a name="configure-windows-security"></a>Konfigurace zabezpečení systému Windows
Ukázkový *ClusterConfig.Windows.JSON* konfigurační soubor stáhnout s [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) samostatného obrázku balíček obsahuje šablony pro konfiguraci zabezpečení systému Windows.  V části **Vlastnosti** nakonfigurovaný zabezpečení systému Windows:

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

|**Konfigurace nastavení**|**Popis**|
|-----------------------|--------------------------|
|ClusterCredentialType|Zabezpečení systému Windows povolena nastavením parametru **ClusterCredentialType** na *Windows*.|
|ServerCredentialType|Zabezpečení systému Windows pro klienty aktivované nastavením parametru **ServerCredentialType** na *Windows*. Tento údaj označuje, zda klienti clusteru a vlastního clusteru, běží v doméně služby Active Directory.|
|WindowsIdentities|Obsahuje identit obrázku a klienta.|
|ClusterIdentity|Konfiguruje mezi uzly zabezpečení. Seznam oddělený čárkami skupiny spravovat účty služby nebo názvy.|
|ClientIdentities|Konfiguruje uzel klient zabezpečení. Pole klientských uživatelské účty.|
|Identity|Identita klienta uživatel domény.|
|IsAdmin|True Určuje, jestli domény má uživatel přístup správce klienta, NEPRAVDA pro klientský přístup uživatelů.|

[Uzlu uzel zabezpečení](service-fabric-cluster-security.md#node-to-node-security) nakonfigurovaný tak, že nastavení pomocí **ClusterIdentity**. Abyste mohli vytvořit vztah důvěryhodnosti mezi uzly, budou musí být upozorněni od sebe. Můžete to provést dvěma způsoby: specifikovat skupiny spravovat účet služby, který obsahuje všechny uzly clusteru nebo identit uzel domény všech uzlů v clusteru. Důrazně doporučujeme použít [Skupinu spravovat účet služby (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) přístup, zejména u větších clusterů (více než 10 uzly) nebo clusterů, které by mohly zvětšit nebo zmenšit.
Tento přístup umožňuje uzly přidat nebo odebrat z gMSA, aniž by bylo změny manifest obrázku. Tento přístup nevyžaduje vytvoření skupiny doménu, pro které Správce clusteru obdrželi přístupová práva k přidání a odebrání členů. Další informace najdete v tématu [Začínáme s účty služby spravovaných skupiny](http://technet.microsoft.com/library/jj128431.aspx).

[Klient uzel zabezpečení](service-fabric-cluster-security.md#client-to-node-security) se konfigurují **ClientIdentities**. Abyste mohli vytvořit vztah důvěryhodnosti mezi klientem a clusteru, musíte nakonfigurovat clusteru vědět, který klient identit, kterým můžete důvěřovat. Lze provést dvěma způsoby: Zadejte uživatele skupiny domény, které můžete připojit nebo určit uživatele uzel domény, které se mohou připojit. Služba struktury podporuje dva typy ovládacích prvků různých přístup pro klienty připojených k obrázku služby struktury: správce a uživatele. Řízení přístupu poskytuje možnost Správce clusteru omezit přístup k určité typy operací obrázku pro různé skupiny uživatelů, lepší zabezpečení clusteru.  Správci mají úplný přístup k možnostem správy (včetně možností pro čtení i zápis). Uživatelé, ve výchozím nastavení mít pouze pro čtení přístup k možnostem správy (například dotazů) a možnost řešení aplikací a služeb.

Následující části **zabezpečení** příklad nakonfiguruje zabezpečení systému Windows a určuje, že počítačích v *ServiceFabric/clusterA.contoso.com* jsou součástí clusteru a *CONTOSO\usera* má přístup správce klienta:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
        "IsAdmin": true
        }]
    }
},
```

## <a name="next-steps"></a>Další kroky

Po konfiguraci zabezpečení systému Windows v souboru *ClusterConfig.JSON* , životopisu proces vytváření obrázku v [vytvoření samostatného obrázku v systému Windows](service-fabric-cluster-creation-for-windows-server.md).

Další informace o tom mezi uzly zabezpečení, uzel klient zabezpečení a řízení přístupu na základě rolí v tématu [scénáře zabezpečení obrázku](service-fabric-cluster-security.md).

Příklady připojení pomocí prostředí PowerShell nebo FabricClient najdete v článku [připojení k zabezpečené obrázku](service-fabric-connect-to-secure-cluster.md) .
