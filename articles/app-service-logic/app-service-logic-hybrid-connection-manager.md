<properties 
    pageTitle="Použití hybridního Connection Manager | Microsoft Azure" 
    description="Instalace a konfigurace Connection Manager hybridní a připojení ke spojnicím místní v aplikacích pro použití logických operátorů" 
    services="app-service\logic" 
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/>

# <a name="connect-to-on-premises-connectors-using-the-hybrid-connection-manager"></a>Připojení ke spojnicím místní použití hybridního Connection Manager

>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2014 12 01 náhled schématu verze. Použití logických operátorů aplikace všeobecně dostupná (GA) používá bránu pro místní připojení. Další informace o nové [brány](app-service-logic-gateway-connection.md) a [Logika aplikace GA](https://azure.microsoft.com/documentation/services/logic-apps/).

Použít místní systém, použije aplikace použití logických operátorů Connection Manager hybridní. Některé spojnice můžete připojit k místní systém, třeba SQL Server, SAP, SharePoint a tak dál. 

Na dosah – stačí hybridní připojení správce (HCM)-jednou Instalační služby systému, které máte nainstalované na serveru IIS ve vaší síti, za brány firewall. Použití předávací Bus služby Azure, HCM ověří místní systém pomocí konektoru v Azure. 

> [AZURE.NOTE] Hybridní Connection Manager se jenom když se připojujete k místní zdroje za brány firewall. Pokud se nepřipojujete k místní systém, nemusíte Connection Manager hybridní.

Začněte tím, budete potřebovat:

- Azure služby Bus relay názvů přidružení zabezpečení připojovací řetězec. Viz [Služby Bus ceny](https://azure.microsoft.com/pricing/details/service-bus/) rozhodnout, jaký osy obsahuje relé.
- Místní systém přihlašovací údaje, včetně uživatelské jméno a heslo. Například pokud se připojujete k místním SQL serveru, musíte SQL Server přihlašovací účet a heslo.
- Místní informace o serveru, včetně port číslo a název serveru. Například pokud se připojujete k místním SQL serveru, musíte název serveru SQL Server a číslo portu TCP.

## <a name="get-the-service-bus-connection-string"></a>Získání připojovacího řetězce Bus služby

Na portálu Azure zkopírujte kořenové služby Bus přidružení zabezpečení připojovací řetězec. Tento připojovací řetězec připojí Azure spojnice k místní systém. 

1. [Azure klasické portál](http://go.microsoft.com/fwlink/p/?LinkID=213885)vyberte služby Bus názvů a vyberte **Informace o připojení**:

    ![][SB_ConnectInfo]

2. Zkopírujte připojovací řetězec přidružení zabezpečení:

    ![][SB_SAS]

## <a name="install-the-hybrid-connection-manager"></a>Instalace hybridní Connection Manager

1. [Azure portál](http://go.microsoft.com/fwlink/p/?LinkID=525040)vyberte spojnici, kterou jste vytvořili. Aby se otevřela, můžete vybrat **Procházet**, vyberte **Rozhraní API aplikace**a vyberte spojnice nebo rozhraní API aplikace. 
<br/><br/>
V **Hybridním připojení**je **neúplné**nastavení:
<br/>
![][2] 

2. Vyberte **hybridní připojení**. Připojovací řetězec Bus služby, které jste předtím zadali koncovém.
3. Zkopírujte **primární konfigurační řetězec**:
<br/>
![][PrimaryConfigString]

4. V části **Místní hybridní Connection Manager**můžete stáhnout správce připojení hybridní nebo instalace přímo na portálu. 
<br/><br/>
Přímo na portálu, přejděte na serveru IIS místní přejděte na portál a vyberte **Stáhnout a konfigurovat**.
<br/><br/>
Ke stažení Connection Manager hybridní, přejděte do místního serveru IIS a přejděte do **aplikace ClickOnce** (http://hybridclickonce.azurewebsites.net/install/Microsoft.Azure.BizTalk.Hybrid.ClickOnce.application). Spustí se instalace automaticky, můžete ho spusťte.

5. V okně **Nastavení posluchače** zadejte **Řetězec primární konfigurace** dříve vložený (v kroku 3) a vyberte **nainstalovat**.

Po dokončení nastavení, zobrazí následující:
<br/>
![][3] 

Teď můžete přejít ke spojnici připojit znovu, stav připojení hybridní při **Připojit**. Bude pravděpodobně nutné spojnice zavřete a znova otevřete:
<br/>
![][4] 

> [AZURE.NOTE] Přepnutí na vedlejší připojovací řetězec, znova spusťte instalační program hybridní připojení a zadejte **Řetězec sekundární konfigurace**.


## <a name="tcp-ports-and-security"></a>Porty TCP a zabezpečení

Při vytváření hybridních připojení na webu se vytvoří na serveru IIS místní místního. Serveru IIS může být DMZ. Požadavky na port TCP na serveru IIS patří:

TCP Port | Proč
--- | ---
 | Žádné příchozí porty TCP jsou potřeba.
9350 - 9354 | Tyto porty se používají pro přenos dat. Správce relay Service Bus sond port 9350 zjistěte, jestli TCP připojení není k dispozici. Pokud je k dispozici, pak předpokládá port 9352 je také k dispozici. Přenosy dat se podíváme na port 9352. <br/><br/>Povolte odchozí připojení na tyto porty.
5671 | Při použití port 9352 pro přenos dat port 5671 slouží jako kanál ovládacího prvku. <br/><br/>Povolte odchozí připojení k tomuto portu. 
80, 443 | Pokud porty 9352 a 5671 nejsou použitelné, je potřeba *potom* porty 80 a 443 náhradní porty používané pro přenos dat a řídicí kanál.<br/><br/>Povolte odchozí připojení na tyto porty.
Místní systém port | Na místní systém otevřete port používaný systém. Třeba SQL Server obvykle používá port 1433. Otevřete tento port TCP.

[Hostitelské za bránou Firewall s Bus služby](http://msdn.microsoft.com/library/azure/ee706729.aspx)

## <a name="troubleshooting"></a>Řešení potíží

![][HCMFlow]

### <a name="on-premises-troubleshooting"></a>Poradce při potížích v místním

1. Na serveru IIS potvrďte role webového serveru IIS je nainstalovaný a začít všechny služby IIS.
2. Na serveru IIS potvrďte, že je připojení správcem hybridní nainstalovanou a spuštěnou:
 - Ve Správci služby IIS (inetmgr), webu ***MicrosoftAzureBizTalkHybridListener*** by měl být uveden a běžet. 
 - Tento web používá ***HybridListenerAppPool*** , které se spouští prostřednictvím místního *síťového* místního předdefinované uživatelského účtu. Tento fond aplikací má taky zahajovat.
3. Na serveru IIS spojnice je potvrzení nainstalovanou a spuštěnou: 
 - Vytvoří se web spojnice. Například pokud jste vytvořili spojnici SQL, je ***MicrosoftSqlConnector_nnn*** webu. Ve Správci služby IIS (inetmgr), potvrďte uvedené a spustit tento web. 
 - Tento web používá vlastní fondu aplikací služby IIS s názvem ***HybridAppPoolnnn***. Tento fond aplikací spustí prostřednictvím místního *síťového* místního předdefinované uživatelského účtu. Tento web a fond aplikací má obě zahajovat. 
 - Přejděte na místní spojnici. Například pokud váš web spojnice používá port 6569, vyhledejte http://localhost:6569. Výchozí dokument není nakonfigurovaný tak, aby `HTTP Error 403.14 - Forbidden error` očekává.
4. V bráně firewall potvrďte otevřeno porty TCP uvedené v tomto tématu.
5. Podívejte se na zdrojový nebo cílový systému:
 - Některé místní systémy vyžadují další závislost soubory. Například pokud se připojujete k místním SAP, musí být některé další soubory SAP nainstalovaný na serveru IIS.
 - Zkontrolujte připojení k systému s přihlašovací účet. TCP port používaný systém například musí být otevřený, jako je port 1433 pro systém SQL Server. Přihlašovací účet, který jste zadali v portálu Azure musí mít přístup k systému.
6. Pro všechny chyby na serveru IIS v protokolech událostí. 
7. Vyčištění a znovu nainstalujte Connection Manager hybridní: 
 - Ve službě IIS ručně odstraňte web spojnici a jeho fondu aplikací. 
 - Spusťte hybridní Connection Manager a potvrďte, že zadávání správné **Primární konfigurační řetězec** spojnice.



### <a name="in-the-azure-classic-portal"></a>Na portálu Azure klasické

1. Potvrďte, že oboru služby Bus má **aktivního** stavu.
2. Když vytvoříte na spojnici, zadejte připojovací řetězec přidružení služby Bus zabezpečení. Není třeba vkládat ACS připojovací řetězec.


## <a name="faq"></a>NEJČASTĚJŠÍ DOTAZY

**OTÁZKA**: existují dva hybridní připojení manažeři. Jaký je rozdíl? 

**Answer (odpověď)**: existuje technologie [Hybridní připojení](../biztalk-services/integration-hybrid-connection-overview.md) , která používá především Web Apps (dřív to bylo weby) a mobilní aplikace (dřív to bylo mobilních služeb) se připojit k místní. Tento správce připojení hybridní je vlastní [Nastavení](../biztalk-services/integration-hybrid-connection-create-manage.md) a použití služby Azure BizTalk (pozadí). Podporuje pouze protokoly TCP a HTTP.

Pomocí aplikace služby Azure spojnic máme také Connection Manager hybridní.  Toto hybridní Connection Manager znamená *údajů služby Azure BizTalk (pozadí)* a podporuje víc než protokoly TCP a HTTP. Podívejte se na [spojnic a rozhraní API aplikace seznam aplikací](app-service-logic-connectors-list.md).

Obě Bus služby Azure umožňuje připojit k místní systém.

**OTÁZKA**: když můžu vytvořit vlastní rozhraní API aplikace, dá se pomocí správce aplikace služby hybridní Connection Manager připojit k místní? 

**Answer (odpověď)**: neúčastní tradiční smysl. Můžete použít předdefinované spojnice, nastavení Správce připojení aplikace služeb hybridní se připojit k místní systém. Tato spojnice pak pomocí svou vlastní aplikaci API, případně použití logických operátorů aplikace. V současné době nejde vyvíjet nebo vytvořit vlastní hybridní rozhraní API aplikace (třeba SQL konektor nebo soubor).

Pokud váš vlastní rozhraní API používá port TCP a HTTP, můžete [Připojení hybridní](../biztalk-services/integration-hybrid-connection-overview.md) a jeho hybridní Connection Manager. V tomto scénáři je použita služby Azure BizTalk. [Připojit k místní SQL Server z web appu](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md) můžou pomoct.  


## <a name="read-more"></a>Další informace

[Sledování aplikací logiky](app-service-logic-monitor-your-logic-apps.md)<br/>
[Služby Bus ceny](https://azure.microsoft.com/pricing/details/service-bus/)



[SB_ConnectInfo]: ./media/app-service-logic-hybrid-connection-manager/SB_ConnectInfo.png
[SB_SAS]: ./media/app-service-logic-hybrid-connection-manager/SB_SAS.png
[PrimaryConfigString]: ./media/app-service-logic-hybrid-connection-manager/PrimaryConfigString.png
[HCMFlow]: ./media/app-service-logic-hybrid-connection-manager/HCMFlow.png
[2]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupIncomplete.jpg
[3]: ./media/app-service-logic-hybrid-connection-manager/HybridSetup.jpg
[4]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupComplete.jpg

 
