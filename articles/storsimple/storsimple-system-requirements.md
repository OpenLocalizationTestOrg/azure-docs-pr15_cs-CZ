<properties
   pageTitle="Požadavky na systém pro StorSimple | Microsoft Azure"
   description="Popisuje software, sítí a dostupnost požadavky a osvědčené postupy pro řešení Microsoft Azure StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/31/2016"
   ms.author="alkohli"/>

# <a name="storsimple-software-high-availability-and-networking-requirements"></a>StorSimple software, dostupnost a požadavky na sociální sítě

## <a name="overview"></a>Základní informace

Vítá vás Microsoft Azure StorSimple. Tento článek popisuje důležité systémové požadavky a osvědčené postupy pro zařízení s StorSimple a pro klienty úložiště přístupu k zařízení. Doporučujeme, abyste si prošli informace pečlivě před nasazení systému StorSimple a potom zpátky na ni odkázat podle potřeby při nasazení a další operace.

Systémové požadavky:

- **Požadavky na software pro klienty úložiště** - jsou popsány podporované operační systémy a všech dalších věcí uvedených operačních systémech.
- Informace o porty, které musí být otevřený v bráně firewall umožňující iSCSI, cloudu nebo přenosy Správa **sítě požadavky pro zařízení StorSimple** - obsahuje.
- **Požadavky na dostupnost pro StorSimple** - popisuje požadavky na dostupnost a osvědčené postupy pro počítač StorSimple Host (hostitel) a zařízení. 


## <a name="software-requirements-for-storage-clients"></a>Požadavky na software pro klienty úložiště

Následující požadavky na software jsou pro přístup zařízení StorSimple úložiště klientech.

| Podporované operační systémy | Verze požadovaná | Další požadavky/poznámky |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008 R2 SP1, 2012, 2012R2 |StorSimple iSCSI svazky jsou podporovány pro použití na pouze následující typy disku Windows:<ul><li>Jednoduchý hlasitosti na základní disku</li><li>Jednoduchý a zrcadlený hlasitosti na dynamický</li></ul>Pokud používáte multilicenční iSCSI StorSimple jsou podporovány systému Windows Server 2012 tenké zřizování a ODX funkce.<br><br>StorSimple můžete vytvořit tence zřizování a plně zřizování svazky. Částečně zřizování svazky je možné vytvořit.<br><br>Přeformátování tence zřizování hlasitost může trvat velmi dlouho. Doporučujeme odstraníte hlasitost a potom vytvoříte novou místo přeformátování. Ale pokud chcete přeformátovat svazku:<ul><li>Spusťte tento příkaz před přeformátovat vyhnout zpožděním obnovit místa: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Po dokončení formátování pomocí následujícího příkazu znovu povolit obnovit místo:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>Instalujte systému Windows Server 2012, jak je uvedeno ve [znalostní bázi Knowledge Base 2878635](https://support.microsoft.com/kb/2870270) s vaším počítačem systému Windows Server.</li></ul></li></ul></ul> Pokud správce snímek StorSimple nebo StorSimple adaptér konfigurujete pro SharePoint, přejděte na [požadavky na Software pro volitelné součásti](#software-requirements-for-optional-components).|
| VMWare ESX | 5.5 a 6.0 | Podporovaná pomocí služby VMWare vSphere jako iSCSI klienta. Funkce VAAI blok je podporována s VMware vSphere na StorSimple zařízeních.
| Linux RHEL/CentOS | 5, 6 a 7 | Podpora klientů iSCSI Linux s otevřít iSCSI vyzývající verze 5, 6 a 7. |
| Linux | SUSE Linux 11 | |
 > [AZURE.NOTE] IBM AIX aktuálně nepodporuje s StorSimple.

## <a name="software-requirements-for-optional-components"></a>Požadavky na software pro volitelné součásti

Následující požadavky na software jsou volitelné StorSimple součástí (StorSimple snímek správce a StorSimple adaptér pro SharePoint).

| Součásti | Host (hostitel) platformy | Další požadavky/poznámky |
| --------------------------- | ---------------- | ------------- |
| Správce StorSimple snímku | Windows Server 2008 R2 SP1 2012, 2012R2 | Použití aplikace StorSimple snímek Manager v systému Windows Server a je nutné pro zálohování a obnovení zrcadlený dynamických disků konzistenci aplikací záloh.<br> Správce snímek StorSimple je podporována pouze v systému Windows Server 2008 R2 SP1 (64bitová verze), Windows 2012 R2 a Windows Server 2012.<ul><li>Pokud používáte okno Server 2012, musíte nainstalovat .NET 3.5 – 4.5 před instalací StorSimple snímek správce.</li><li>Pokud používáte Windows Server 2008 R2 SP1, musíte nainstalovat Windows Management Framework 3.0 před instalací StorSimple snímek správce.</li></ul> |
| StorSimple adaptér pro službu SharePoint | Windows Server 2008 R2 SP1 2012, 2012R2 |<ul><li>StorSimple adaptér pro službu SharePoint je podporována pouze na Sharepointu 2010 a Sharepointu 2013.</li><li>Pole Kód RBS vyžaduje SQL Server Enterprise Edition verze 2008 R2 nebo 2012.</li></ul>|

## <a name="networking-requirements-for-your-storsimple-device"></a>Požadavky pro zařízení s StorSimple připojení sítě

Zařízení s StorSimple je uzamčený zařízení. Porty potřebovat otevřít v bráně firewall umožňující iSCSI cloudu a správu. V následující tabulce jsou uvedeny porty, které je potřeba otevřít v bráně firewall. V této tabulce *v* příchozích a *odchozích* odkazuje na směr, odkud příchozí žádosti klienta o přístup k zařízení. *Nebo *odchozí* * odkazuje na směr, ve kterém zařízení StorSimple odešle data externě, za nasazení: například odchozí k Internetu.

| Port číslo<sup>1, 2</sup> | Či oddálení | Obor port | Povinné | Poznámky |
|------------------------|-----------|------------|----------|-------|
|TCP 80 (HTTP)<sup>3</sup>|  Rezervovat |  WAN | Ne |<ul><li>Port pro odchozí spojení se používá pro přístup k Internetu vyhledat aktualizace.</li><li>Webový odchozí proxy server je uživatel, která dokáže nahradit.</li><li>Povolte aktualizace systému, musí být tento port také otevřený řadiče pevnou IP adresy.</li></ul> |
|TCP 443 (HTTPS)<sup>3</sup>| Rezervovat | WAN | Ano |<ul><li>Port pro odchozí spojení se používá pro přístup k datům v cloudu.</li><li>Webový odchozí proxy server je uživatel, která dokáže nahradit.</li><li>Povolte aktualizace systému, musí být tento port také otevřený řadiče pevnou IP adresy.</li><li>Tento port slouží také v obou řadiči pro uvolnění paměti.</li></ul>|
|UDP 53 (DNS) | Rezervovat | WAN | V některých případech; v části poznámky. |Tento port jenom v případě, že používáte internetové DNS server požaduje. |
| UDP 123 (NTP) | Rezervovat | WAN | V některých případech; v části poznámky. |Tento port je požadován jenom v případě, že používáte server Internetové NTP. |
| TCP 9354 | Rezervovat | WAN | Ano |Odchozí port používá zařízení StorSimple komunikovat s službu StorSimple správce. |
| 3260 (iSCSI) | V | MÍSTNÍ SÍTĚ | Ne | Tento port slouží k přístupu k datům prostřednictvím iSCSI.|
| 5985 | V | MÍSTNÍ SÍTĚ | Ne | Příchozí port slouží správcem snímek StorSimple komunikovat s StorSimple zařízení.<br>Tento port slouží také při můžete vzdáleně připojovat k prostředí Windows PowerShell pro StorSimple přes HTTP. |
| 5986 | V | MÍSTNÍ SÍTĚ | Ne | Tento port se používá při můžete vzdáleně připojovat k prostředí Windows PowerShell pro StorSimple přes protokol HTTPS. |

<sup>1</sup> žádné příchozí porty muset otevřít na veřejné Internetu.

<sup>2</sup> Pokud více porty provést konfigurace brány, pořadí odchozí přenosy směrované závisí na základě port směrování pořadí podle [Port směrování](#routing-metric)pod.

<sup>3</sup> řadiče pevnou IP adresy na zařízení s StorSimple musí být směrovatelné a připojit k Internetu. Pevné IP adresách, které se používají pro údržbu aktualizace zařízení. Pokud řadiče zařízení nemůže připojit k Internetu přes pevné IP adresy, nebudete moct provést aktualizaci StorSimple zařízení.

> [AZURE.IMPORTANT] Zajistit, aby bránu firewall upravit nebo dešifrování všechny přenosy SSL mezi StorSimple zařízení a Azure.

### <a name="url-patterns-for-firewall-rules"></a>Adresa URL vzory pravidla brány firewall

Správci sítě můžete nakonfigurovat často Upřesnit firewall pravidla založená na adresu URL vzory k filtrování příchozí a odchozí přenosy. Zařízení s StorSimple a službu StorSimple správce závisí na jiné aplikace Microsoft například Bus služby Azure, Azure Active Directory, řízení přístupu, úložiště účty a servery služby Microsoft Update. Adresa URL vzory související s těmito aplikacemi mohou sloužit k konfigurace pravidel brány firewall. Je důležité pochopit, že můžete změnit adresu URL vzory související s těmito aplikacemi. To zase vyžaduje správce sítě ke sledování a aktualizace pravidel brány firewall pro StorSimple jako a v případě potřeby.

Doporučujeme nastavit pravidla brány firewall pro odchozí přenosy dat podle StorSimple liberally pevnou IP adres ve většině případů. Však níže uvedených informací můžete nastavit pravidla Upřesnit brány firewall, které jsou potřeba k vytvoření zabezpečené prostředí.

> [AZURE.NOTE] Zařízení (zdrojový) IP adresy by měl nastavit vždy povolené síťové rozhraní. Cíl IP adresy by měl nastavit [Azure datacentra rozsahy IP adres](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).

#### <a name="url-patterns-for-azure-portal"></a>Adresa URL vzory Azure portálu
| Vzor adresy URL                                                      | Součásti nebo funkce                                           | IP adresy zařízení                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | Služba StorSimple správce<br>Služba řízení přístupu<br>Bus služby Azure| Rozhraní povolena cloudu sítě        |
|`https://*.backup.windowsazure.com`|Registrace zařízení| Pouze DATA 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Odvolání certifikátů |Rozhraní povolena cloudu sítě |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Účty Azure úložiště a sledování | Rozhraní povolena cloudu sítě        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Servery služby Microsoft Update<br>                             | Řadiče domény jenom pevnou IP adresy               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Řadiče domény jenom pevnou IP adresy   |
| `https://*.partners.extranet.microsoft.com/*`                    | Podpora balíčku                                                  | Rozhraní povolena cloudu sítě        |

#### <a name="url-patterns-for-azure-government-portal"></a>Adresa URL vzory pro státní správu Azure portálu
| Vzor adresy URL                                                      | Součásti nebo funkce                                           | IP adresy zařízení                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`   | Služba StorSimple správce<br>Služba řízení přístupu<br>Bus služby Azure| Rozhraní povolena cloudu sítě        |
|`https://*.backup.windowsazure.us`|Registrace zařízení| Pouze DATA 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Odvolání certifikátů |Rozhraní povolena cloudu sítě |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Účty Azure úložiště a sledování | Rozhraní povolena cloudu sítě        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Servery služby Microsoft Update<br>                             | Řadiče domény jenom pevnou IP adresy               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Řadiče domény jenom pevnou IP adresy   |
| `https://*.partners.extranet.microsoft.com/*`                    | Podpora balíčku                                                  | Rozhraní povolena cloudu sítě        |

### <a name="routing-metric"></a>Metriky

Metriky směrování souvisí s rozhraní a brána směrování data do určité sítě. Metriky používá směrovací protokol pro výpočet nejlepší cestu do zadaného umístění, pokud dozví, že se stejným cílem existovat více cest. Nižší metriky, vyšší předvoleb.

V souvislosti s StorSimple Pokud více brány a síťových rozhraní není nakonfigurován komunikace kanálu směrování metriky bude uplatnit určete relativní pořadí, ve kterém se zobrazí rozhraní používat. Směrování metriky nemohli měnit uživatele. Můžete však použít `Get-HcsRoutingTable` rutina vytisknout směrování tabulky (a metriky) na vašem zařízení StorSimple. Další informace o rutiny Get-HcsRoutingTable v [Poradce při potížích s StorSimple nasazení](storsimple-troubleshoot-deployment.md).

Směrování metrických algoritmů se liší v závislosti na verzi softwaru v StorSimple zařízení.

**Uvolnění před aktualizace 1**

Jedná se o spolehlivější před aktualizace 1 například GA 0,1, 0,2 nebo 0,3 vydání. Pořadí směrování metrice vypadá takto:

   *Poslední nakonfigurované 10 GbE síťové > Další 10 GbE síťové > poslední nakonfigurované 1 GbE síťové > rozhraní sítě jiných GbE 1*


**Uvolnění počáteční od 1 aktualizace a před aktualizace 2**

Jedná se o spolehlivější třeba 1, 1.1 nebo 1.2. Pořadí směrování metrice je určeno následujícím způsobem:

   *DATA 0 > poslední nakonfigurované 10 GbE síťové > Další 10 GbE síťové > poslední nakonfigurované 1 GbE síťové > rozhraní sítě jiných GbE 1*

   V části Update 1 je volání metriky dat 0 nejnižší; proto všechny cloudu – přenosy směrovaná přes DATA 0. Poznamenejte si to pokud existuje více než jeden cloudu s podporou síťové na zařízení s StorSimple.


**Uvolnění počínaje aktualizace 2**

Aktualizace 2 má několika vylepšením týkající se sítí a směrování metriky se takto změnila. Chování můžete vysvětlení následujícím způsobem.

- Sadu hodnot předem byly přiřazeny síťová rozhraní.   

- Zvažte tabulka s příkladem – ukázáno v následujícím příkladu se hodnoty přiřazené k různá síťová rozhraní když jsou cloudové povoleny nebo v zakázaném cloudu, ale nakonfigurované brány. Poznámka: hodnoty přiřazené tady jsou příklady hodnot pouze.


  	| Rozhraní sítě | U kterých je povolené cloudu | Shluk zakázáno s brány |
  	|-----|---------------|---------------------------|
  	| Data 0  | 1            | -                        |
  	| Dat 1  | 2            | 20                       |
  	| Data 2  | 3            | 30                       |
  	| Dat 3  | 4            | 40                       |
  	| Dat 4  | 5            | 50                       |
  	| Dat 5  | 6            | 60                       |


- Pořadí, ve kterém bude přenosy cloudu směrovaná přes rozhraní sítě je:

    *Data 0 > dat 1 > datum 2 > dat 3 > dat 4 > dat 5*

    Lze vysvětlit v následujícím příkladu.

    Zvažte StorSimple zařízení s dvou rozhraní povolena cloudu sítě Data 0 a dat 5. Data 1 až 4 dat jsou cloudové zakázáno, ale máte nakonfigurované brány. Pořadí, ve kterém bude směrovat přenosy zařízení budou:

    *Data 0 (1) > dat 5 (6) > dat 1 (20) > Data 2 (30) > dat 3 (40) > dat 4 (50)*

    *kde čísla v závorkách označují odpovídajících směrování metriky.*

    Když se nepovede Data 0, bude získat až 5 dat směrovat přenosy cloudu. Předpokladu, že brány je nakonfigurovaný na jiné síti kdyby Data 0 a 5 dat se nezdaří, bude přenosy cloudu absolvovat Data 1.


- Pokud dojde k chybě rozhraní povolena cloudu sítě, pak jsou k dispozici 3 opakování s 30 druhý zpoždění se připojit k rozhraní. Pokud se nezdaří všechna opakování, přenos směrovány na další k dispozici u kterých je povolené cloudu rozhraní stanovený směrovací tabulky. Pokud všechny cloudu s podporou síťová rozhraní selhání, pak zařízení se nepovede nad ostatní řadiči (Restart není v tomto případě).

- Pokud dojde k selhání VIP pro rozhraní povolena iSCSI sítě, bude 3 opakování s 2 sekund zpoždění. Toto chování má strávených stejné z předchozí verze. Všechna síťová rozhraní iSCSI nezdaří, selhání řadiče domény se projeví (připojí restartovat počítač).


- Upozornění je také umocněné na zařízení s StorSimple po selhání VIP. Další informace najdete v článku [upozornění snadné](storsimple-manage-alerts.md).

- Z hlediska opakování iSCSI přednost před cloudu.

    Příklad: StorSimple A zařízení má dvě síťová rozhraní povolena, Data 0 a Data 1. Cloud s podporou vzhledem k tomu Data 1 je obou cloudu a iSCSI s podporou se data 0. Žádná další síťová rozhraní na tomto zařízení můžou používat cloudové nebo iSCSI.

    -Li Data 1 se nezdaří, je uveden v poslední sítě rozhraní iSCSI, výsledkem bude selhání řadiče dat 1 na jiné zařízení.


### <a name="networking-best-practices"></a>Doporučené postupy sítě

Kromě toho výše sítě požadavky pro optimální výkon StorSimple řešení přejděte prosím dodržovat následující doporučené postupy:

- Zkontrolujte, že StorSimple zařízení nainstalovaný vyhrazené šířky pásma 40 MB / (zabere nejméně) vždy k dispozici. Tento šířky pásma nesmí sdílet (nebo rozdělení by měl být zaručena použitím zásady QoS) s jinými aplikacemi.

- Zkontrolujte, zda je vždy dostupné síťové připojení k Internetu. Připojení k Internetu Občasná nebo nedůvěryhodných na zařízeních, včetně jakékoliv, bez připojení k Internetu povede Nepodporovaná konfigurace.

- Izolujte přenosy iSCSI a cloudu vyhrazené síťová rozhraní na vašem zařízení pro přístup pomocí protokolu iSCSI nebo cloudu. Další informace najdete v tématu Jak [změnit síťová rozhraní](storsimple-modify-device-config.md#modify-network-interfaces) na zařízení s StorSimple.

- Nepoužívejte konfigurace odkaz agregace ovládací prvek Protocol (LACP) pro síťová rozhraní. Toto je Nepodporovaná konfigurace.


## <a name="high-availability-requirements-for-storsimple"></a>Požadavky na dostupnost pro StorSimple

Hardwarové platformy, která je součástí řešení StorSimple má dostupnost a spolehlivost funkcí, které jsou zdrojem základ pro infrastrukturu vysoce dostupné, chybám úložiště ve vaší datacentra. Můžou ale nastat požadavky a doporučené postupy, které se musí odpovídat k zajištění dostupnosti StorSimple řešení. Před nasazením StorSimple pečlivě zkontrolujte následující požadavky a osvědčené postupy pro StorSimple zařízení a počítače připojený hostitel.

Další informace o sledování a udržování hardwarové součásti StorSimple zařízení klikněte na [použít službu StorSimple správce ke sledování hardwarové součásti a stav](storsimple-monitor-hardware-status.md) a [výměna součástí StorSimple hardwaru](storsimple-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Požadavky na dostupnost a postupy pro zařízení s StorSimple

Přečtěte si následující informace pečlivě zajistit dostupnost StorSimple zařízení.

#### <a name="pcms"></a>PCMs

StorSimple zařízení patří nadbytečné, za provozu power a chlazení moduly (PCMs). Každý PCM má dost kapacitu k poskytování služby pro celý šasi. Abyste měli jistotu dostupnost, musí být nainstalovaný obou PCMs.

- Vaše PCMs připojení ke zdrojům různých power poskytnout dostupnost, pokud se nezdaří prameni power.
- Když PCM nepovede, žádost o náhradu okamžitě.
- Odebrání selhalo PCM pouze v případě, že máte nahradit a je připraven případně ji nainstalujte.
- Neodebírat obou PCMs současně. Modul PCM zahrnuje modulu záložní battery. Odebrání obě PCMs způsobí vypnutí bez battery ochrany a stav zařízení se neuloží. Další informace o battery přejděte na [zachovat modulu záložní battery](storsimple-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Moduly řadiče domény

StorSimple zařízení patří nadbytečné, za provozu řadiče moduly. Moduly řadiče pracovat aktivní/pasivní způsobem. Kdykoli dané jednoho řadiče domény modulu je aktivní a službu poskytuje, při pasivní ostatní řadiče moduly. Modul pasivní řadiče zapnutý a zahájí činnost, pokud modul aktivní řadiče selže nebo je odebrat. Každý modul řadiče má dost kapacitu k poskytování služby pro celý šasi. Modulů řadiče domény musí být nainstalovaný zajistit dostupnost.

- Ujistěte se, nainstalované modulů řadiče vždy.

- Když modul řadiče nepovede, požádat o náhradu okamžitě.

- Odebrání modulu řadiče selhalo pouze v případě, že máte nahradit a je připraven případně ji nainstalujte. Odebrání modulu pro delší dobu ovlivní vzduchu a tedy chlazení systému.

- Ujistěte se, že síťové připojení k modulů zařízení jsou stejné a rozhraní připojení sítě mají stejné síťové konfiguraci.

- Pokud modul řadiče selže nebo potřebuje náhradní, ujistěte se, že ostatní řadiče moduly ve aktivního stavu před nahrazením modul řadiče selhalo. Abyste ověřili, že řadiči je aktivní, přejděte na [identifikovat aktivní řadiče na vašem zařízení](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Neodebírat modulů řadiče ve stejnou dobu. Pokud probíhá selhání řadiče domény, vypněte modul úsporném řadiče nebo odebrání skříni.

- Po selhání řadiče domény počkejte nejméně pět minut před odebráním buď modul řadiče.

#### <a name="network-interfaces"></a>Rozhraní sítě

StorSimple zařízení řadiče moduly každý mají čtyři 1 Gigabit a dvě 10 Ethernetu síťová rozhraní.

- Ujistěte se, že síťové připojení k modulů zařízení jsou stejné a síti rozhraní, rozhraní řadiče domény modulu připojeni k nastavení stejné síti.

- Pokud je to možné, nasazení síťových připojení přes odlišné přepínače zajistit dostupnost služby v případě selhání zařízení sítě.

- Po odpojení pouze nebo poslední zbývající iSCSI s podporou rozhraní (plus pár IP adresy přiřazené), nejprve zakázat rozhraní a odpojte kabely. Pokud rozhraní je odpojený, pak způsobí aktivní řadiče přenesou do pasivní správce. Pokud pasivní řadiče taky jeho odpovídající rozhraní odpojit, obě řadiče bude restartujte tisknutím před spuštěním u jednoho řadiče.

- Nejméně dvě datová rozhraní připojení k síti v jednotlivých modulech řadiče domény.

- Pokud jste povolili dvou 10 GbE rozhraní nasazení můžou být přes odlišné přepínače.

- Pokud je to možné, použijte MPIO na serverech zajistit, aby servery nevadí vám odkazu, síť nebo selhání rozhraní.

Další informace o sítích zařízení dostupnost a výkonu, přejděte na [nainstalovat zařízení StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) nebo [StorSimple 8600 zařízení](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>Disky SSD a pevných disků

StorSimple zařízení patří plné stavu disků (disky SSD) a jednotky pevného disku (pevných disků), které jsou chráněné pomocí zobrazuje zrcadlově mezery. Zrcadlený mezerami zajistíte, že bude moct tolerovat selhání disky SSD nebo pevných disků zařízení.

- Ujistěte se, jestli jsou nainstalované názevprojektu SSD a pevný disk.

- Neúspěšné SSD nebo pevného disku, žádost o náhradu okamžitě.

- Pokud SSD nebo pevný disk selže nebo vyžaduje náhradní, ujistěte se, odebrat jenom SSD nebo pevného disku, který vyžaduje náhradní.

- Odebrání víc SSD nebo pevný disk ze systému kdykoli v čase.
Ztrátu systémových selhání a potenciální dat může způsobit selhání 2 nebo více disků určitého typu (pevného disku, SSD) nebo po sobě jdoucí selhání uvnitř rámečku čas (krátký). Pokud k tomu dojde, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) , aby vám.

- Během náhradní sledování **Stavu hardwaru** na stránce **správy** jednotek v disky SSD a pevných disků. Zelené zaškrtnutí stav označuje, že disků správný nebo OK, že červený vykřičník přejděte označuje selhalo SSD nebo pevný disk.

- Doporučujeme konfigurace snímky cloudu pro všechny svazky, které potřebujete pro ochranu pro nečekané selhání systému.

#### <a name="ebod-enclosure"></a>EBOD přílohu

Model zařízení StorSimple 8600 obsahuje přílohu rozšířené skupina disků (EBOD) kromě primární přílohu. EBOD obsahuje EBOD řadiče a jednotky pevného disku (pevných disků), které jsou chráněné pomocí zobrazuje zrcadlově mezery. Zrcadlený mezerami zajistíte, že bude moct tolerovat selhání pevných disků jeden nebo víc zařízení. Přílohu EBOD je připojen k primární přílohu prostřednictvím nadbytečné kabely přidružení zabezpečení.

- Ujistěte se, že modulů EBOD přílohu řadiče kabely přidružení zabezpečení i všechny jednotky pevného disku nainstalovaných vždy.

- Když modul EBOD přílohu řadiče nepovede, požádat o náhradu okamžitě.

- Pokud modul EBOD přílohu řadiče nepovede, přesvědčte se, že řadiče domény modulu aktivní před nahradit modulu selhalo. Abyste ověřili, že řadiči je aktivní, přejděte na [identifikovat aktivní řadiče na vašem zařízení](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Během EBOD řadiče domény modulu nahrazení, nepřetržitě sledovat stav součásti ve službě StorSimple správce přístupem **údržbu** > **Stav hardwaru**.

- Pokud kabelu přidružení zabezpečení selže nebo vyžaduje náhradní (Microsoft Support by měl zapojit k rozhodnutí), ujistěte se, odebrat jenom kabel přidružení zabezpečení, který vyžaduje náhradní.

- Neodebírat souběžně obou kabely přidružení zabezpečení systému kdykoli v čase.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Dostupnost doporučení pro počítače Host (hostitel)

Pečlivě zkontrolujte tyto doporučené postupy pro zajištění vysoké dostupnosti připojené k zařízení StorSimple tabulkami hosts.

- Konfigurace StorSimple s [konfigurací clusteru serveru dvěma uzly soubor][1]. Odebráním jednoho místa selhání a budovy v redundance na straně hostitele celé řešení zpřístupní vysoce.

- Použití stále k dispozici (CA) sdílí s Windows Server 2012 (SMB 3.0) umožňující dostupnost při selhání řadiče úložiště. Další informace o konfiguraci soubor serveru clusterů a stále k dispozici sdílení s Windows Server 2012 podívejte se do této [videoukázku](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Další kroky

- [Další informace o limitech StorSimple systému](storsimple-limits.md).
- [Naučte se nasadit StorSimple řešení](storsimple-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
