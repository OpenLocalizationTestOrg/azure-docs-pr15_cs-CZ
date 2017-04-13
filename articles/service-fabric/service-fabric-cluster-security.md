<properties
   pageTitle="Zabezpečené clusteru služby struktury | Microsoft Azure"
   description="Popisuje scénáře zabezpečení pro službu struktury obrázku a jiné technologie provádět tyto scénáře."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/19/2016"
   ms.author="chackdan"/>

# <a name="service-fabric-cluster-security-scenarios"></a>Scénáře služby struktury clusteru zabezpečení

Služba struktury clusteru je zdroj, kterou vlastníte. Clusterů měli vždy zabezpečená neoprávněným uživatelům zabráníte připojují k vašemu clusteru, zvlášť když má výrobní pracovní vytížení ho. Ačkoli je možné vytvořit nezabezpečenou clusteru, provedete tak vám umožní anonymní uživatele o připojení definován Pokud zpřístupní koncové body správy veřejné Internetu. 

Tento článek obsahuje přehled scénáře zabezpečení pro clusterů se systémem Azure nebo samostatné a různých technologiích provádět tyto scénáře. Scénáře zabezpečení clusteru je:

- Zabezpečení mezi uzly
- Zabezpečení uzel klient
- Řízení přístupu na základě rolí (RBAC)

## <a name="node-to-node-security"></a>Zabezpečení mezi uzly
Slouží k zabezpečení komunikace mezi VMs nebo počítačích v clusteru. Zajistíte tím, že pouze počítačů, kteří jsou oprávněni připojení clusteru může účastnit hostingu aplikací a služeb v clusteru.

![Diagram komunikace mezi uzly][Node-to-Node]

Clusterů se systémem Azure nebo samostatný clusterů se systémem Windows můžete použít [Certifikát zabezpečení](https://msdn.microsoft.com/library/ff649801.aspx) nebo [Zabezpečení systému Windows](https://msdn.microsoft.com/library/ff649396.aspx) u počítačů s Windows Server.
### <a name="node-to-node-certificate-security"></a>Mezi uzly certifikátu zabezpečení
Služba struktury používá certifikáty serveru X.509 zadané jako součást konfigurace typ uzlu při vytváření clusteru. Rychlý přehled toho, jaké tato poukázky a jak lze získat nebo byla vytvořená je k dispozici na konci tohoto článku.

Certifikát zabezpečení nakonfigurovaný při vytváření clusteru prostřednictvím portálu Azure, správce prostředků Azure šablon nebo šablonu JSON samostatně. Můžete zadat primární certifikát a volitelné sekundární certifikát, který se používá pro efekty přechodu certifikát. Primární a sekundární certifikáty, které zadáte třeba liší od správce klienta a jen pro čtení klienta certifikáty, které zadáte [Uzel Klient](#client-to-node-security)cenného papíru.

Azure číst [Nastavení obrázku s použitím správce prostředků Azure šablony](service-fabric-cluster-creation-via-arm.md) se dozvíte, jak nakonfigurovat certifikátu zabezpečení v clusteru.

Samostatný systému Windows Server přečíst [zabezpečené samostatného obrázku v systému Windows pomocí X.509 certifikátů](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Zabezpečení systému windows mezi uzly
Samostatný systému Windows Server přečíst [zabezpečené samostatného obrázku v systému Windows pomocí zabezpečení systému Windows](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Zabezpečení uzel klient
Klientských a slouží k zabezpečení komunikace mezi klientem a jednotlivých uzlech clusteru. Tento typ zabezpečení ověří a zabezpečuje komunikaci klienta, která zajišťuje přístup jenom uživatelé clusteru a nasazené na clusteru. Klienti jsou jedinečně identifikovány pomocí své přihlašovací údaje zabezpečení systému Windows nebo jejich certifikát zabezpečovací přihlašovací údaje.

![Diagram uzel klient komunikace][Client-to-Node]

Clusterů se systémem Azure nebo samostatný clusterů se systémem Windows pomocí [Certifikátu zabezpečení](https://msdn.microsoft.com/library/ff649801.aspx) nebo [Zabezpečení systému Windows](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Uzel Klient certifikátu zabezpečení
 Uzel Klient certifikátu zabezpečení nakonfigurovaný při vytváření clusteru prostřednictvím portálu Azure správce prostředků šablony nebo šablonu samostatného JSON zadáním certifikát správce klienta a/nebo certifikát klienta uživatele.  Správce klienta a uživatele klienta certifikáty, které zadáte třeba liší od primárních a sekundárních certifikáty, které zadáte [mezi uzly](#node-to-node-security)cenného papíru.

Klienti připojují k clusteru pomocí certifikátu Správce mají úplný přístup k možnostem správy.  Klienti připojují k clusteru pomocí certifikátu klienta jen pro čtení uživatele mít pouze pro čtení přístup k možnostem správy. Jinými slovy tato poukázky slouží k roli základy řízení přístupu (RBAC) popsané dál v tomto článku.

Azure číst [Nastavení obrázku s použitím správce prostředků Azure šablony](service-fabric-cluster-creation-via-arm.md) se dozvíte, jak nakonfigurovat certifikátu zabezpečení v clusteru.

Samostatný systému Windows Server přečíst [zabezpečené samostatného obrázku v systému Windows pomocí X.509 certifikátů](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Uzel Klient Azure Active Directory (AAD) zabezpečení na Azure
Clusterů se systémem Azure můžete taky zabezpečený přístup k koncové body správy pomocí Azure Active Directory (AAD). Informace o tom, jak vytvořit potřebné artefakty AAD, naplnění během vytváření clusteru a připojení k těmto clusterů následně najdete v článku [Nastavení obrázku tak, že pomocí šablony správce prostředků Azure](service-fabric-cluster-creation-via-arm.md) .

## <a name="security-recommendations"></a>Doporučení týkající se zabezpečení
Azure clusterů doporučujeme použít AAD zabezpečení ověření klienty a certifikáty mezi uzly cenného papíru.

Pro samostatnou systému Windows Server clusterů se doporučuje používat zabezpečení systému Windows se skupinou spravovat účty (GMA), pokud máte Windows serveru 2012 R2 a služby Active Directory. V opačném případě pořád pomocí zabezpečení systému Windows u účtů Windows.

## <a name="role-based-access-control-rbac"></a>Řízení přístupu (RBAC) na základě rolí
Řízení přístupu umožňuje správci clusteru omezit přístup k určité clusteru operace pro různé skupiny uživatelů, lepší zabezpečení clusteru. Dva typy ovládacích prvků různých access podporuje klientům připojení ke clusteru: role správce a role uživatele.

Správci mají úplný přístup k možnostem správy (včetně možností pro čtení i zápis). Uživatelé, ve výchozím nastavení mít pouze pro čtení přístup k možnostem správy (například dotazů) a možnost řešení aplikací a služeb.

Můžete zadat klienta role správce a uživatele při vytváření clusteru poskytnutím oddělené identity (certifikáty, AAD atd.) pro jednotlivá pole. Další informace na výchozí nastavení řízení přístupu a jak lze změnit výchozí nastavení najdete v tématu [řízení přístupu pro klienty služby struktury na základě rolí](service-fabric-cluster-security-roles.md).


## <a name="x509-certificates-and-service-fabric"></a>Certifikáty X.509 a struktury služby
Digitálních certifikátů X.509 se běžně používají ověření klienty a servery a postup pro šifrování a digitální podpis zpráv. Podrobné informace o tato poukázky přejděte k [práci s certifikáty](http://msdn.microsoft.com/library/ms731899.aspx).

Několik důležitých věcí k zamyšlení:

- Certifikáty v clusterů spuštění pracovního vytížení výrobní by měly být vytvořený s využitím správně nakonfigurované certifikát službu systému Windows Server nebo získané schválené [Certifikační autorita (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
- Nechcete-li použít libovolnou dočasné nebo otestovat certifikáty ve výrobním, které jsou vytvořené pomocí nástrojů, jako jsou MakeCert.exe.
- Pomocí certifikátu podepsaného svým držitelem, ale měli učinit pouze test clusterů ale ne v výrobní.

### <a name="server-x509-certificates"></a>Certifikáty X.509 serveru

Certifikáty serveru mít primární úkolu ověřování serveru (uzly) klientům nebo ověřování serveru (uzly) k serveru (uzly). Jednou z počáteční kredibility při klienta nebo uzel ověří uzel je ke kontrole hodnoty běžné název v poli Předmět. Tento běžné název nebo jiné certifikáty předmět alternativní názvy musí být v seznamu Povolené běžné názvy.

Následující článek popisuje, jak získat certifikáty s předmětem alternativní názvy (SAN): [jak přidat alternativní název subjektu zabezpečené LDAP certifikát](http://support.microsoft.com/kb/931351).

Pole Subject může obsahovat více hodnot, každá s předponou inicializace označíte typ hodnoty. Nejčastěji inicializace je "CN" pro běžné název. například "CN = www.contoso.com". Je také možné pro pole Subject prázdné. Pokud je zadána volitelné pole alternativní název subjektu, musí obsahovat běžné název certifikátu a jednu položku za alternativní název subjektu. Toto jsou zadány jako hodnoty název DNS.

Hodnota pole zamýšlené účely certifikátu by měl obsahovat odpovídající hodnotě, například "Server ověření" nebo "klienta".

### <a name="client-x509-certificates"></a>Certifikáty X.509 klienta

Klientské certifikáty nejsou obvykle vystaven certifikát od nezávislé certifikační autority. Místo toho osobní úložiště aktuální umístění uživatele obvykle obsahuje certifikátů tam umístí kořenová autorita s zamýšlený účel "Ověření klienta". Klient můžete používejte tyto certifikát požaduje vzájemné ověření.

>[AZURE.NOTE] Všechny operace správy clusteru služby struktury vyžadují certifikáty serveru. Klientské certifikáty nelze použít pro správu.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Další kroky

Tento článek obsahuje informace o zabezpečení clusteru. Další, [vytvořte cluster v Azure pomocí šablony správce prostředků](service-fabric-cluster-creation-via-arm.md) nebo přes [Azure portálu](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
