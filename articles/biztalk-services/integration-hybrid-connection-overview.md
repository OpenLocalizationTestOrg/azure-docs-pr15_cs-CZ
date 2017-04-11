<properties
    pageTitle="Přehled připojení hybridní | Microsoft Azure"
    description="Informace o připojení hybridní, zabezpečení, porty TCP a podporované konfigurace. MABS, WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ccompy"/>


# <a name="hybrid-connections-overview"></a>Přehled hybridní připojení
Úvod k připojení hybridní podporované konfigurace seznamy a seznamy požadované porty TCP.


## <a name="what-is-a-hybrid-connection"></a>Co je hybridní připojení

Hybridní připojení je funkce služby Azure BizTalk. Hybridní připojení zadejte snadno a pohodlný způsob, jak připojit Web Apps funkce v aplikaci služby Azure (dřív to bylo weby) a mobilní aplikace v aplikaci služby Azure (dříve Mobilní služba) pro místní zdroje za bráně firewall.

![Hybridní připojení][HCImage]

Výhody hybridní připojení patří:

- Web Apps a mobilní aplikace mají přístup k existující místních dat a services bezpečnému.
- Více webových aplikací nebo mobilní aplikace můžete sdílet hybridní připojení pro přístup k místní zdroje.
- Minimální porty TCP vyžadovaných přístup k vaší síti.
- Použití hybridního připojení aplikace přístup k jenom určité místního prostředek, který je publikován hybridní připojení.
- Můžete připojit k žádné místní prostředek, který používá statické TCP portu, například SQL serveru, MySQL, rozhraní API webových HTTP a většina vlastní webové služby.

    > [AZURE.NOTE] Aktuálně nepodporuje protokolu TCP služby, které používají dynamické porty (například FTP pasivní nebo rozšířené pasivní režimu). Nepodporuje také LDAP. LDAP použití statického přístavu TCP ale můžou taky použít UDP. Výsledkem je není podporovaná.

- Můžete používat se všechny rámce podporován aplikací Web Apps (.NET PHP, Java, Python, Node.js) a mobilní aplikace (Node.js, .NET).
- Web Apps a mobilní aplikace můžete získat přístup k místní zdroje stejným způsobem jako kdybyste webu nebo mobilní aplikaci se nachází v místní síti. Například stejné připojení řetězec použít místní lze také na Azure.


Hybridní připojení taky poskytují podnikovým správcům s ovládací prvek a přehled o podnikové prostředky používána v hybridním aplikací, včetně:

- Nastavení zásad skupiny pomocí správci můžou povolit připojení hybridní v síti a taky určit prostředky, které jsou přístupné aplikacemi hybridní.
- Protokoly událostí a auditování podnikové síti se systémem poskytují přehled zdrojů používána v hybridním připojení.


## <a name="example-scenarios"></a>Příklady scénářů

Hybridní připojení podporují následující kombinace framework a aplikace:

- .NET framework přístup k SQL serveru
- .NET framework přístup ke službám protokolu HTTP/HTTPS s webový klient
- PHP přístup k serveru SQL Server MySQL
- Java přístup k serveru SQL Server MySQL a Oracle
- Java přístup ke službám protokolu HTTP/HTTPS

Pokud chcete použít hybridní připojení pro přístup k místním SQL serveru, zvažte následující skutečnosti:

- Instance Express s názvem SQL musí být nakonfigurované pro použití statického porty. Ve výchozím nastavení SQL Express s názvem instance použít dynamické porty.
- Výchozí instance Express SQL použití statického přístavu, ale musí být povolený TCP. Ve výchozím nastavení není povolený TCP.
- Při použití clusterů nebo dostupnost skupiny `MultiSubnetFailover=true` režim není aktuálně podporován.
- `ApplicationIntent=ReadOnly` Aktuálně nepodporuje.
- Ověřování serveru SQL, může být potřeba jako prostředek začátku do konce se tak mohli ověřovat nepodporuje aplikaci Azure a místních SQL server.


## <a name="security-and-ports"></a>Zabezpečení a porty

Hybridní připojení umožňuje zabezpečená připojení z Azure aplikací a místní hybridní Connection Manager připojení hybridní povolení přidružení sdílené podpis aplikace Access (zabezpečení). Klíče samostatné připojení vzniká aplikace a místní hybridní Connection Manager. Klávesy připojení můžete chránění a odvolat nezávisle na sobě.

Hybridní připojení poskytovat bezproblémové a zabezpečené rozložení kláves aplikace a místní hybridní Connection Manager.

V tématu [Vytvoření a Správa připojení hybridní](integration-hybrid-connection-create-manage.md).

*Povolení aplikace je nezávislý na hybridní připojení*. Lze způsobem příslušné ověření. Metoda se tak mohli ověřovat závisí na začátku do konce se tak mohli ověřovat metody podporované různých Azure cloudu a místní součásti. Například Azure aplikace má přístup k místním SQL serveru. V tomto scénáři pravděpodobně SQL ověření, že se tak mohli ověřovat metodu, která je podporována začátku do konce.

#### <a name="tcp-ports"></a>Porty TCP
Hybridní připojení vyžadují pouze odchozí připojení TCP a HTTP z privátní sítě. Pokud chcete otevřít jakýkoli brány firewall nebo změnit konfiguraci sítě obvod umožňuje příchozí připojení do sítě nepotřebujete.

Hybridní připojení používá tyto porty protokolu TCP:

Port | Proč je nutné
--- | ---
9350 - 9354 | Tyto porty se používají pro přenos dat. Správce relay Service Bus sond port 9350 zjistěte, jestli TCP připojení není k dispozici. Pokud je k dispozici, pak předpokládá port 9352 je také k dispozici. Přenosy dat se podíváme na port 9352. <br/><br/>Povolte odchozí připojení na tyto porty.
5671 | Při použití port 9352 pro přenos dat port 5671 slouží jako řídicí kanál. <br/><br/>Povolte odchozí připojení k tomuto portu.
80, 443 | Tyto porty se používají pro některé požadavky dat Azure. Pokud porty 9352 a 5671 nejsou použitelné, je *pak* porty 80 a 443 také náhradních porty používané pro přenos dat a řídicí kanál.<br/><br/>Povolte odchozí připojení na tyto porty. <br/><br/>**Poznámka:** Se nedoporučuje používat jako náhradní porty místo jiné porty TCP. Nastavit informace HTTP/WebSocket slouží jako protokol místo nativní TCP pro datových kanálů. Může vést k nižší výkon.



## <a name="next-steps"></a>Další kroky

[Vytvoření a Správa připojení hybridní](integration-hybrid-connection-create-manage.md)<br/>
[Připojení k místní zdroje Azure Web Apps](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Připojení ke místního serveru SQL Azure webovou aplikaci](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>


## <a name="see-also"></a>Viz taky

[Rozhraní REST API pro správu služeb BizTalk na Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalk služby: edice grafu](biztalk-editions-feature-chart.md)<br/>
[Vytvoření BizTalk služby Azure portálu](biztalk-provision-services.md)<br/>
[BizTalk služby: Řídicí panel, sledování a měřítko karty](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
