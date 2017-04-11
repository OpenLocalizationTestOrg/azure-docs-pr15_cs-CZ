<properties
   pageTitle="Použití logických operátorů aplikace instalace místní data brány | Microsoft Azure"
   description="Informace o tom, jak nainstalovat bránu místních dat pro použití v aplikaci použití logických operátorů."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="install-the-on-premises-data-gateway-for-logic-apps"></a>Instalace brána dat místní logiky aplikací pro

## <a name="installation-and-configuration"></a>Instalace a konfigurace

### <a name="prerequisites"></a>Zjistit předpoklady pro

Minimální:

* .NET 4.5 framework
* 64bitová verze systému Windows 7 nebo Windows Server 2008 R2 (nebo novější)

Doporučujeme:

* 8 core CPU
* 8 GB paměti
* 64bitová verze Windows 2012 R2 (nebo novější)

Související důležité informace:

* Nelze nainstalovat bránu řadiče domény.
* Brána neměli nainstalovat na počítač, například přenosný počítač, který může být vypnou a zůstanou vypnuté, režimu spánku, nebo nejste připojení k Internetu, protože brány nedají spustit jednu z těchto případů. Kromě toho brány výkonu může dojít k prostřednictvím bezdrátové sítě.

### <a name="install-a-gateway"></a>Instalace brány

Můžete získat [Instalační program pro místní brána pro dat tady](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

Určit **místní brána dat** jako způsob, přihlaste pomocí svého pracovního nebo školního účtu a potom buď konfigurace Nová brána migrovat, obnovit nebo převzít řízení existující brány.

* Konfigurace brány pro it a **obnovení klíč**zadejte **název** a klikněte nebo klepněte na **Konfigurovat**.

    Zadat obnovení klíč, který obsahuje nejméně osm znaků a udržovat její bezpečné místo. Pokud chcete migrovat, obnovení nebo převzít řízení jeho brány, musíte tento klíč.

* Migrace, obnovení nebo převzít řízení existující brány, poskytnutí obnovení klávesy, která byla zadané při vytváření brány.

### <a name="restart-the-gateway"></a>Restartujte brány

Brány spustí jako služba Windows, který, stejně jako u jakékoli jiné služby systému Windows, můžete spustit a zastavit několika způsoby. Můžete třeba otevřete příkazový řádek se zvýšenými oprávněními, na kterém běží brána počítač a znovu spusťte některou z následujících příkazů:

* Zastavit službu, spusťte tento příkaz:

    `net stop PBIEgwService`

* Spusťte službu, spusťte tento příkaz:

    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>Konfigurace brány firewall nebo proxy serveru

Informace o tom, jak zadejte proxy informace pro brány najdete v tématu [Konfigurace nastavení proxy serveru](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-proxy/).

Můžete ověřit, zda bránu firewall nebo proxy server, blokována bránou připojení spuštěním následujícího příkazu z příkazovém řádku prostředí PowerShell. Testovat připojení Bus služby Azure. Tomto pouze test připojení k síti a nemá všechno, co dělat s cloudovou službu serveru nebo brány. Je dobré zjistit, jestli váš počítač skutečně dostali k Internetu.

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

Výsledky by měla vypadat podobně jako tento příklad. Pokud **TcpTestSucceeded** není pravdivá, které mohou být blokovány bránu firewall.

```
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Pokud chcete být vyčerpávající, nahraďte hodnoty **název_počítače** a **Port** datové typy v části [konfigurovat porty](#configure-ports) dál v tomto tématu.

Taky blokována bránou firewall připojení, která Bus služby Azure umožňuje Azure datacentrech. Pokud to je případ, je vhodné povolených (odblokování) všechny adresy IP pro oblast pro tyto datacentrech. Můžete získat seznam [Azure IP adresy](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="configure-ports"></a>Konfigurace porty

Brány vytvoří odchozí připojení Bus služby Azure. Ho informuje uživatele o odchozí porty: TCP 443 (výchozí), 5671, 5672, 9350 s popisem 9354. Brány nevyžaduje příchozí porty.

Další informace o [hybridním řešení](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| NÁZVY DOMÉN | ODCHOZÍ PORTY | POPIS |
| ----- | ------ | ------ |
| *. analysis.windows.net | 443 | PROTOKOL HTTPS |
| *. login.windows.net | 443 | PROTOKOL HTTPS |
| *. servicebus.windows.net |5671 5672 | Rozšířené Protocol (AMQP) pro zpracování zpráv |
| *. servicebus.windows.net | 443, 9350 9354 | Posluchačů na Relay Service Bus prostřednictvím protokolu TCP (vyžaduje 443 pro pořízení tokenu řízení přístupu) |
| *. frontend.clouddatahub.net | 443 | PROTOKOL HTTPS |
| *. core.windows.net | 443 | PROTOKOL HTTPS |
| Login.microsoftonline.com | 443 | PROTOKOL HTTPS |
| *. msftncsi.com | 443 | Slouží k testování připojení k Internetu, pokud brána nedostupná pro službu Power BI. |

Pokud potřebujete bílé seznam IP adres místo domén, můžete stáhnout a použít [seznam rozsahů IP Datacentra Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653). V některých případech připojení Bus služby Azure budou s adresou IP místo plně kvalifikované názvy domén.

### <a name="sign-in-account"></a>Přihlašovací účet

Uživatelé se přihlaste pomocí buď pracovní nebo školní účet. Toto je váš účet organizace. Pokud registraci k Office 365 nabídky a neměli zadat e-mailu skutečnou práci, může vypadat jeff@contoso.onmicrosoft.com. Váš účet do cloudové služby, je uložena v klientovi v Azure Active Directory (AAD). Ve většině případů účtu AAD Přípony se budou shodovat e-mailovou adresu.

### <a name="windows-service-account"></a>Účet služby Windows

Místní data bránu pro účely NT SERVICE\PBIEgwService přihlašovací pověření služby systému Windows. Ve výchozím nastavení má vpravo od protokolu jako služba. Toto je v kontextu počítače, na kterém instalujete brány.

Toto není účet používá pro připojení k místním zdrojům dat nebo pracovního nebo školního účtu, se kterým jste přihlášení do cloudových služeb.

##<a name="frequently-asked-questions"></a>Nejčastější dotazy

### <a name="general"></a>Obecné

**Otázka**: jaké zdroje dat brány podporuje?<br/>
**Odpověď**: k této psaní, SQL Server.

**Otázka**: je potřeba bránu pro zdroje dat v cloudu, jako jsou databáze SQL Azure? <br/>
**Odpověď**: Ne. Bránu se připojí k místním jenom pro zdroje dat.

**Otázka**: co je aktuální služby Windows s názvem?<br/>
**Answer (odpověď)**: služby v brány se nazývá služba Power BI organizace brány.

**Otázka**: jsou všechny příchozí připojení k bráně z cloudu? <br/>
**Odpověď**: Ne. Brány používá odchozí připojení Bus služby Azure.

**Otázka**: Co když mám zablokovat odchozí připojení? Co je potřeba otevřít? <br/>
**Answer (odpověď)**: v tématu porty a hosts, které používá bránu.


**Otázka**: znamená brány muset nainstalovat na stejném počítači jako zdroj dat? <br/>
**Odpověď**: Ne. Bránu se připojí ke zdroji dat pomocí informace o připojení, která byla k dispozici. Považujte klientské aplikace v tomto smyslu brány. Ho bude stačí se připojit k název serveru, který byl k dispozici.


**Otázka**: co je zpoždění při spouštění dotazů ke zdroji dat z brány? Jaký je nejlepší architekturu? <br/>
**Answer (odpověď)**: snížit latence sítě, nainstalujte brány co nejblíž zdroji dat nejblíže. Pokud instalujete brány na zdroje skutečnými daty minimalizuje latence zavádí. Zvažte datacentrech stejně. Například pokud služby používá datovém centru západní USA a máte použitý ve OM Azure SQL Server, budete chcete mít taky OM Azure západ USA. Tím minimalizovat latence a výstupním náklady na OM Azure.


**Otázka**: jsou všechny požadavky pro šířku pásma sítě? <br/>
**Answer (odpověď)**: je vhodné mít dobrý výkon pro připojení k síti. Každé prostředí se liší, množství dat odesílaného ovlivní a výsledky. ExpressRoute může pomoci zajistit úroveň výkon mezi místním prostředím a datacentrech Azure.

Aplikace jiných výrobců nástroj Test rychlosti Azure umožňuje Nápověda odhadnout, co je vaše výkon.


**Otázka**: můžete spustit službu Windows brány s účtem služby Azure Active Directory? <br/>
**Odpověď**: Ne. Služba Windows musí mít platný účet systému Windows. Ve výchozím nastavení se spustí s IDENTIFIKÁTOREM služby NT SERVICE\PBIEgwService.


**Otázka**: jak výsledky posílají zpátky do cloudu? <br/>
**Answer (odpověď)**: je to prostřednictvím Bus služby Azure. Další informace najdete v tématu jak to funguje.


**Otázka**: kde jsou moje přihlašovací údaje uloženy? <br/>
**Answer (odpověď)**: přihlašovací údaje zadané pro zdroj dat jsou uloženy zašifrovaných ve službě cloud brány. Přihlašovací údaje jsou dešifrovat na brány na pracovišti.

### <a name="high-availabilitydisaster-recovery"></a>Obnovení vysoké dostupnost/havárie

**Otázka**: jsou všechny plány pro podpora scénářů dostupnost s brány? <br/>
**Answer (odpověď)**: Toto je na plán, ale nemůžeme časové osy ještě nemáte.


**Otázka**: jaké možnosti máte k dispozici pro obnovení havárie? <br/>
**Answer (odpověď)**: obnovovacího klíče slouží k obnovení nebo přesunutí brány. Při instalaci brány zadejte obnovovacího klíče.


**Otázka**: jaké jsou výhody obnovovací klíč? <br/>
**Answer (odpověď)**: poskytuje způsob, jak migrovat nebo obnovit nastavení brány po selhání.

### <a name="troubleshooting"></a>Řešení potíží

**Otázka**: kde jsou protokoly brány? <br/>
**Answer (odpověď)**: najdete v článku nástroje dál v tomto tématu.


**Otázka**: Jak můžu se podívat, co dotazů je poslané na místní zdroj dat? <br/>
**Answer (odpověď)**: můžete povolit trasování dotazu, který bude obsahovat dotazů odesílaného. Nezapomeňte ho změnit zpátky na původní hodnoty až mít Poradce při potížích. Nechte dotazu trasování povoleno způsobí protokolech větší.

Můžete taky prohlédnout nástroje, které zdroj dat obsahuje dotazy trasování. Například můžete rozšířit události nebo SQL nástroje pro SQL Server a služby Analysis Services.

## <a name="how-the-gateway-works"></a>Jak funguje brány

Při interakci uživatele s tedy prvek, který je připojen ke zdroji dat místní:

1. Cloudové služby vytvoří dotaz, spolu s šifrované pověření pro zdroj dat a odešle dotaz do fronty brány zpracuje.
1. Služba analyzuje dotaz a posune žádost Bus služby Azure.
1. Brána dat místní zjišťuje Bus služby Azure pro až do žádosti.
1. Brána získává dotaz, dešifruje přihlašovací údaje a připojuje k zdroje dat pomocí těchto přihlašovacích údajů.
1. Brány odešle dotazu na zdroj dat pro spuštění.
1. Výsledky jsou odeslány ze zdroje dat zpět brány a potom do cloudové služby Služba použije výsledky.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="update-to-the-latest-version"></a>Aktualizaci na nejnovější verzi

Velké množství problémů může ukazovat při verze brány není aktuální.  Je vhodné obecné a ujistěte se, že se nacházíte na nejnovější verzi.  Pokud jste to ještě aktualizovali brány za měsíc nebo delší, můžete zvažte možnost instalace nejnovější verze brány a zjistěte, pokud jste reprodukování problému.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users-"></a>Chyba: Nejde přidat uživatele do skupiny. (-2147463168 PBIEgwService výkonu protokolu uživatele)

Pokud se pokoušíte nainstalovat bránu na doménu, která není podporována, může se zobrazit tato chyba. Je potřeba nasadit brány na počítač, který není řadiče domény.

## <a name="tools"></a>Nástroje

### <a name="collecting-logs-from-the-gateway-configurator"></a>Shromáždění protokolů z Konfigurátor brány

Můžete shromáždit několik protokoly brány. Vždy zahájíte s protokoly.

#### <a name="installer-logs"></a>Instalační program protokoly

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Konfigurace protokolování

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Protokoly organizace brány služby

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Protokoly událostí

Brána pro správu dat a PowerBIGateway protokoly jsou obsaženy v části **aplikace protokoly a služeb**.

### <a name="fiddler-trace"></a>Sledování Fiddler

[Fiddler](http://www.telerik.com/fiddler) je bezplatná nástroj z Telerik, která sleduje přenosy protokolu HTTP.  Můžete zobrazit na pozadí a dále s Power BI služby v klientském počítači. Může se zobrazit chyby a další související informace.

## <a name="next-steps"></a>Další kroky
- [Vytvoření připojení k místní k aplikacím použití logických operátorů](app-service-logic-gateway-connection.md)
- [Funkce integrace Enterprise](app-service-logic-enterprise-integration-overview.md)
- [Použití logických operátorů aplikace spojnic](../connectors/apis-list.md)