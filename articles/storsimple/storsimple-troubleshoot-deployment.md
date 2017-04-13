<properties 
   pageTitle="Poradce při potížích nasazení StorSimple | Microsoft Azure"
   description="Popisuje, jak Diagnostika a oprava chyby, ke kterým dochází při první instalaci StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="troubleshoot-storsimple-device-deployment-issues"></a>Poradce při potížích nasazení StorSimple zařízení

## <a name="overview"></a>Základní informace

Tento článek poskytuje užitečné řešení problémů s nasazením Microsoft Azure StorSimple. Popisuje běžné problémy s možných příčin a doporučené kroky k řešení problémů, ke kterým může dojít při konfiguraci StorSimple. Tyto informace platí pro zařízení fyzicky místní StorSimple a StorSimple virtuální zařízení.

> [AZURE.NOTE] Zařízení konfigurace problémech, které může vystaven může dojít při nasazení zařízení poprvé nebo se mohou vyskytnout, později, když zařízení je funkční. Tento článek se zaměřuje na první nasazení potíží. Poradce při potížích s provozní zařízení, přejděte na [článek Poradce při potížích provozní zařízení](storsimple-troubleshoot-operational-device.md).

Taky Tento článek popisuje nástrojů pro řešení potíží s StorSimple nasazení a poskytuje podrobné příklad Poradce při potížích.

## <a name="first-time-deployment-issues"></a>Potíže při prvním nasazení

Pokud narazíte na chyby při instalaci zařízení poprvé, zvažte následující skutečnosti:

- Pokud odstraňujete fyzické zařízení, ujistěte se, že má hardware nainstalovali a nakonfigurovali podle [nainstalovat zařízení StorSimple 8100](storsimple-8100-hardware-installation.md) nebo [StorSimple 8600 zařízení](storsimple-8600-hardware-installation.md).
- Zkontrolujte požadavky na nasazení. Ujistěte se, že máte všechny informace uvedené v [Kontrolní seznam konfigurace nasazení](storsimple-deployment-walkthrough.md#deployment-configuration-checklist).
- Prohlédněte si poznámky k verzi StorSimple zobrazíte, pokud je popsán problém. Poznámky k verzi zahrnout zástupná instalace známé problémy. 

Při nasazení zařízení nejčastější problémy, které uživatelé mohou vyskytnout nominální při spuštění Průvodce nastavením a při registraci zařízení pomocí Windows Powershellu pro StorSimple. (Pomocí Windows Powershellu pro StorSimple zaregistrovat a nakonfigurujte StorSimple zařízení. Další informace o registrace zařízení najdete v tématu [Krok 3: nakonfigurovat a zaregistrovat zařízení přes Windows PowerShell pro StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple)).

V následujících částech můžete vyřešit problémy, které dojít při konfiguraci zařízení StorSimple poprvé.

## <a name="first-time-setup-wizard-process"></a>Průvodce instalací první

Podle těchto kroků souhrn Průvodce instalací. Podrobné informace o nastavení najdete v článku [StorSimple zařízení v místním nasazení](storsimple-deployment-walkthrough.md).

1. Spusťte rutinu [Vyvolat HcsSetupWizard](https://technet.microsoft.com/library/dn688135.aspx) spustíte Průvodce nastavením, který vás provede zbývající kroky. 
2. Konfigurace sítě: Průvodce nastavením je možné konfigurovat nastavení sítě pro rozhraní 0 sítě dat na zařízení s StorSimple. Tato nastavení patřit následující úkoly:
  - Virtuální IP (VIP) podsítě masky a brána – rutinu [Set-HcsNetInterface](https://technet.microsoft.com/library/dn688161.aspx) probíhá na pozadí. Konfiguruje IP adresu, masku podsítě a bránu pro rozhraní 0 sítě dat na zařízení s StorSimple.
  - Primární server DNS – rutinu [Set-HcsDnsClientServerAddress](https://technet.microsoft.com/library/dn688172.aspx) se spustí na pozadí. Konfiguruje nastavení serveru DNS pro StorSimple řešení.
  - Server NTP – rutinu [Set-HcsNtpClientServerAddress](https://technet.microsoft.com/library/dn688138.aspx) se spustí na pozadí. Konfiguruje nastavení serveru NTP StorSimple řešení.
  - Volitelné web proxy – rutinu [Set-HcsWebProxy](https://technet.microsoft.com/library/dn688154.aspx) se spustí na pozadí. Nastaví a umožňuje konfigurací proxy web StorSimple řešení.
3. Nastavení hesla: dalším krokem je nastavit správce zařízení a StorSimple snímek správce hesel. Pokud používáte Update 1, pak se nebude muset nastavit StorSimple snímek správce hesel.
  - Heslo správce zařízení se používá pro přihlášení k zařízení. Výchozí heslo zařízení je **hesla1**.
  - Při konfiguraci zařízení na používání StorSimple snímek správce StorSimple snímek správce heslo požaduje. Je potřeba nejdřív nastavit heslo v Průvodci nastavením a pak můžete nastavit a změňte výběr z službu StorSimple správce. Toto heslo ověří zařízení pomocí StorSimple snímek správce.
 
    > [AZURE.IMPORTANT] Hesla se shromažďují před registrací, ale projeví až po úspěšném zaregistrovat zařízení. Dojde k chybě při použití hesla, budete vyzváni k znovu zadat heslo, dokud se shromažďují vyžadovaná hesla (splňující požadavky na složitost).

4. Zaregistrovat zařízení: posledním krokem je zaregistrovat zařízení se službou StorSimple správce spuštěné v Microsoft Azure. Registrace, musíte získat [klíč registrace služby](storsimple-manage-service.md#get-the-service-registration-key) z portálu Microsoft Azure klasické a zadejte v Průvodci nastavením. Po úspěšné registraci zařízení šifrovací klíč dat služby se vám poskytovány. Nezapomeňte si tento šifrovací klíč bezpečné místo, protože ho budete vyzváni k registraci všech dalších zařízeních se službou.

## <a name="common-errors-during-device-deployment"></a>Běžné chyby při nasazení zařízení

Následující tabulka uvádí, že se můžete setkat při běžných chyb můžete:

- Konfigurace nastavení požadovaných sítě.
- Konfigurace nastavení proxy volitelné web.
- Nastavení Správce zařízení a StorSimple snímek správce hesel. 
- Zaregistrujte zařízení. 

## <a name="errors-during-the-required-network-settings"></a>Chyby při nastavení požadovaných sítě

| Ne.| Chybová zpráva | Možné příčiny | Doporučené akce |
| ---| ------------- | --------------- | ------------------ |
| 1  | Vyvolat HcsSetupWizard: Tohoto příkazu lze spustit pouze řadiči aktivní. | Konfigurace provádění pasivní řadiči.| Spusťte tento příkaz z active řadiče. Další informace přečtěte si část [rozpoznání aktivní řadiče na vašem zařízení](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).|
| 2 | Vyvolat HcsSetupWizard: Není připraveno zařízení. | Jsou problémy s připojení k síti na DATECH 0.| Zkontrolujte připojení k síti fyzické dat 0.|
| 3 | Vyvolání HcsSetupWizard: Je konflikt adresy IP pomocí jiného systému v síti (výjimky z HRESULT: 0x80070263). | IP pro DATA 0 byla už nepoužívá jiný systém. | Zadejte nový IP, který nepoužívá v.|
| 4 | Vyvolání HcsSetupWizard: Prostředek clusteru se nezdařila. (Výjimky z HRESULT: 0x800713AE). | Duplikování VIP. Předaném IP se už používá.| Zadejte nový IP, který nepoužívá v.|
| 5 | Vyvolat HcsSetupWizard: Adresa neplatné IPv4. | IP adresu podle nesprávný formát.| Zkontrolujte formát a zadejte svoji IP adresu znovu. Další informace najdete v tématu [Adres protokolu Ipv4][1]. |
| 6 | Vyvolání HcsSetupWizard: Neplatné IPv6 address. | IP adresu podle nesprávný formát.| Zkontrolujte formát a zadejte svoji IP adresu znovu. Další informace najdete v tématu [Adresování Ipv6][2].|
| 7 | Vyvolání HcsSetupWizard: Žádné další koncové body nejsou k dispozici z mapování koncového bodu. (Výjimky z HRESULT: 0x800706D9) | Funkce clusterů nefunguje. | [Kontaktování podpory společnosti Microsoft](storsimple-contact-microsoft-support.md) k dalším krokům.

## <a name="errors-during-the-optional-web-proxy-settings"></a>Chyby při nastavení proxy volitelné web

| Ne.| Chybová zpráva | Možné příčiny | Doporučené akce |
| ---| ------------- | --------------- | ------------------ |
| 1  | Vyvolat HcsSetupWizard: Neplatný parametr (výjimky z HRESULT: 0x80070057) | Parametrů přidělený nastavení proxy serveru není platný.| Identifikátor URI není k dispozici ve správném formátu. Použití v tomto formátu: http://*<IP address or FQDN of the web proxy server>*:*<TCP port number>* |
| 2 | Vyvolat HcsSetupWizard: Server RPC není k dispozici (výjimky z HRESULT: 0x800706ba) | Příčinou je jedním z následujících akcí:<ol><li>Clusteru je nahoru.</li><li>Trpný řadiče nemůžu komunikovat s active řadiče a na příkaz spustit z pasivní řadiče.</li></ol> | V závislosti na příčina:<ol><li>[Kontaktování podpory společnosti Microsoft](storsimple-contact-microsoft-support.md) a ujistěte se, že clusteru je nahoru.</li><li>Spusťte příkaz z active řadiče. Pokud budete chtít spustit příkaz z pasivní řadiče, bude nutné ověřit pasivní řadiče mohli komunikovat s active řadiče. Je třeba [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) Pokud toto připojení se přeruší.</li></ol> |
| 3 | Vyvolání HcsSetupWizard: Volání RPC selhalo (výjimky z HRESULT: 0x800706be) | Clusteru je vypnutý. | [Kontaktování podpory společnosti Microsoft](storsimple-contact-microsoft-support.md) a ujistěte se, že clusteru je nahoru.|
| 4 | Vyvolat HcsSetupWizard: Prostředek nebyl nalezen clusteru (výjimky z HRESULT: 0x8007138f) | Prostředek clusteru nebyl nalezen. K tomu může dojít při instalaci není správný. | Budete muset resetujte výchozí hodnoty. [Kontaktování podpory společnosti Microsoft](storsimple-contact-microsoft-support.md) k vytvoření obrázku zdroje.|
| 5 | Vyvolat HcsSetupWizard: Prostředek nejsou online clusteru (výjimky z HRESULT: 0x8007138c)| Prostředky clusteru nejsou online. | [Kontaktování podpory společnosti Microsoft](storsimple-contact-microsoft-support.md) k dalším krokům.|

## <a name="errors-related-to-device-administrator-and-storsimple-snapshot-manager-passwords"></a>Chyb souvisejících s Správce zařízení a StorSimple snímek správce hesel

Výchozí heslo správce zařízení je **hesla1**. Toto heslo vyprší po prvním přihlášení; Proto musíte ho změnit pomocí Průvodce nastavením. Při zaregistrovat zařízení poprvé, je nutné zadat nové heslo správce zařízení. 

Pokud používáte StorSimple snímek správce spuštěná na hostiteli systému Windows Server pro správu zařízení, musíte taky zadat heslo správce snímek StorSimple při registraci první. 

Ujistěte se, že hesla splňovat následující kritéria:

- Heslo správce zařízení by mělo být 8 až 15 znaků.
- Heslo správce StorSimple snímku by mělo být 14 nebo 15 znaků.
- Hesla by měl obsahovat 3 z následujících typů 4 znaky: malá, velká písmena, číselné a speciální. 
- Svoje heslo nemůžou být stejné jako poslední 24 hesla.

Kromě toho mějte na paměti hesla vypršela každý rok, který bude možné měnit až po úspěšném zaregistrovat zařízení. Pokud z nějakého důvodu selže registrace, hesla se nezmění. Další informace o Správce zařízení a StorSimple snímek správce hesel přejděte na článek [použití službu StorSimple správce chcete změnit StorSimple hesla](storsimple-change-passwords.md).

Můžete narazit jednu nebo více z následujících chyb při nastavování správce zařízení a StorSimple snímek správce hesel.

| Ne.| Chybová zpráva | Doporučené akce |
| ---| ------------- | ------------------ | 
| 1  | Heslo vyšší než maximální délka. |Použijte heslo, které splňují tyto požadavky:<ul><li>Heslo správce zařízení musí být 8 až 15 znaků.</li><li>Heslo správce snímek StorSimple musí být 14 nebo 15 znaků.</li></ul> | 
| 2 | Heslo nevyhovovala požadovanou délku. | Použijte heslo, které splňují tyto požadavky:<ul><li>Heslo správce zařízení musí být 8 až 15 znaků.</li><li>Heslo správce snímek StorSimple musí být 14 nebo 15 znaků.</lu></ul> |
| 3 | Heslo musí obsahovat malá písmena. | Hesla musí obsahovat 3 z následujících typů 4 znaky: malá, velká písmena, číselné a speciální. Ujistěte se, že heslo splňuje tyto požadavky. |
| 4 | Heslo musí obsahovat číslice. | Hesla musí obsahovat 3 z následujících typů 4 znaky: malá, velká písmena, číselné a speciální. Ujistěte se, že heslo splňuje tyto požadavky. |
| 5 | Heslo musí obsahovat speciální znaky. | Hesla musí obsahovat 3 z následujících typů 4 znaky: malá, velká písmena, číselné a speciální. Ujistěte se, že heslo splňuje tyto požadavky. |
| 6 | Heslo musí obsahovat 3 z následujících typů 4 znaky: velká, malá písmena, číselné a jinak. | Svoje heslo neobsahuje požadované typy znaků. Ujistěte se, že heslo splňuje tyto požadavky. |
| 7 | Parametr neodpovídá potvrzení. | Ujistěte se, že hesla splňuje požadavky na všechny a, že je zadaná správně. |
| 8 | Svoje heslo neodpovídá výchozí. | Výchozí heslo je *hesla1*. Budete muset po prvním přihlášení změnit toto heslo. |
| 9 | Heslo, které jste zadali heslo zařízení neodpovídá. Zadejte heslo znovu. | Kontrola heslo a zadejte znovu heslo. |

Hesla se shromažďují před zařízení je registrovaná, ale platí pouze po úspěšném registraci. Pracovní postup obnovení hesla vyžaduje zařízení k registraci. 

> [AZURE.IMPORTANT] Obecně když se nepovede pokus o použití hesla, software opakovaně pokusí shromáždit heslo, dokud nebude úspěšná. Výjimečně nelze použít heslo. V takovém případě můžete zaregistrovat zařízení a pokračovat, ale nebude změnit heslo. Obdržíte žádný údaj, která se změní heslo – správce snímek StorSimple heslo nebo heslo správce zařízení. Pokud dojde k této situaci, doporučujeme změnit obě hesla.

Můžete resetovat hesla v Azure klasické portálu prostřednictvím službu StorSimple správce. Další informace najdete v tématu: 

- [Změna hesla správce zařízení](storsimple-change-passwords.md#change-the-device-administrator-password).
- [Změna hesla správce StorSimple snímek](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

## <a name="errors-during-device-registration"></a>Chyby při registraci zařízení

Používáte službu StorSimple správce spuštěné v Microsoft Azure zaregistrovat zařízení. Jeden nebo více z následujících problémů může dojít při registraci zařízení.

| Ne.| Chybová zpráva | Možné příčiny | Doporučené akce |
| ---| ------------- | --------------- | ------------------ |
| 1  | 350027 k chybě zaregistrovat zařízení s StorSimple správce. |  | Počkejte několik minut a potom akci opakujte. Pokud problém přetrvává, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md).|
| 2  | Chyba 350013: Došlo k chybě při registraci zařízení. Může to být kvůli nesprávné služby registračního klíče. | | Registrace klíčem správnou službu znovu zaregistrujte zařízení. Další informace najdete v tématu [získat klíči služby registrace.](storsimple-manage-service.md#get-the-service-registration-key) |
| 3 | Chyba 350063: Ověřovací služba StorSimple správce předaný ale registrace se nezdařila. Akci opakujte za určitou dobu. | Tato chyba znamená, že termínu ověřování s ACS ale selže volání přihlásit ke službě. Může to být výsledek jev Občasná sítě. | Pokud potíže potrvají, obraťte se na [Podporu společnosti Microsoft](storsimple-contact-microsoft-support.md). |
| 4 | Chyba 350049: Služba není dostupný při registraci. | Když při volání na službu, výjimka webové neobdrží. V některých případech to získat pevnou s opakování operace později. | Zkontrolujte svoji IP adresu a název DNS a akci opakujte. Pokud potíže potrvají, [kontaktovat Microsoft Support.](storsimple-contact-microsoft-support.md) | 
| 5 | Chyba 350031: Zařízení už registrované. | | Není potřeba žádná akce. |
| 6 | Chyba 350016: Registrace zařízení se nezdařila. | |Ujistěte se, že správnost registračního klíče. |
| 7 | Vyvolání HcsSetupWizard: Došlo k chybě při registraci zařízení; může to být kvůli nesprávné IP address or nebo název DNS. Zkontrolujte nastavení sítě a zkuste to znovu. Pokud problém přetrvává, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md). (Chyba 350050) | Zajistit, aby vaše zařízení můžete pomocí příkazu ping externí sítě. Pokud nemáte připojení k externí síti, může se tato chyba nepovede registraci. Tuto chybu může být kombinací jednu nebo více z následujících akcí:<ul><li>Nesprávný IP</li><li>Nesprávný podsítě</li><li>Nesprávný brány</li><li>Nesprávná nastavení DNS</li></ul> | Následující postup v [Podrobný Poradce při potížích příklad](#step-by-step-storsimple-troubleshooting-example). |
| 8 | Vyvolání HcsSetupWizard: Aktuální operaci nelze kvůli chyba interním služby [0x1FBE2]. Akci opakujte později. Pokud potíže potrvají, obraťte se prosím Microsoft Support. | Toto je obecné chyby vyvolání pro všechny uživatele neviditelné chyby ze služby nebo agent. Nejběžnější důvod, proč může být selže ACS ověřování. Možná příčina selhání je, že jsou problémy s konfigurací serveru NTP a není správně nastavený čas na zařízení. | Opravte čas (v případě potíží) a pak opakujte registrace. Pokud chcete nastavit časové pásmo použijete příkaz Set HcsSystem - podle časového pásma, velké první písmeno každého slova podle časového pásma (například "standardní Tichomoří").  Pokud potíže potrvají, [kontakt podpory společnosti Microsoft](storsimple-contact-microsoft-support.md) pro další kroky. |
| 9 | Upozornění: Nelze aktivovat zařízení. Správce zařízení a hesla správce snímek StorSimple nebyly změněny. | Když se registrace nepovede, Správce zařízení a hesla správce snímek StorSimple se nezmění. |

## <a name="tools-for-troubleshooting-storsimple-deployments"></a>Nástroje pro řešení potíží s StorSimple nasazení

StorSimple obsahuje několik nástrojů, které můžete použít k řešení StorSimple řešení. Jedná se o:

- Podpora balíčků a zařízení protokoly 
- Rutiny pro speciálně pro řešení potíží 

## <a name="support-packages-and-device-logs-available-for-troubleshooting"></a>Podporují balíčků a zařízení protokoly k dispozici pro řešení potíží

Podpora balíček obsahuje příslušné protokoly, které mohou pomoci týmu Microsoft Support k řešení problémů s zařízení. Prostředí Windows PowerShell pro StorSimple umožňuje generovat šifrované podpory balíček, který můžete sdílet s pracovníky podpory.

### <a name="to-view-the-logs-or-the-contents-of-the-support-package"></a>Chcete-li zobrazit protokoly nebo obsah balíček podpory

1. Umožňuje generovat balíčku podpory podle popisu v prostředí Windows PowerShell pro StorSimple [vytvořit a spravovat balíčku podpory](storsimple-create-manage-support-package.md).

2. Stažení [dešifrování skript](https://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) místně ve vašem klientském počítači.

3. Pomocí tohoto [Podrobný postup](storsimple-create-manage-support-package.md#edit-a-support-package) otevření a dešifrování balíček podpory.

4. Protokoly balíčku dešifrovaný podpory jsou ve formátu trasování událostí pro Windows nebo etvx. Provedením následujících kroků můžete tyto soubory zobrazit v prohlížeči událostí systému Windows:
  1. Zadejte příkaz **eventvwr** na klienta Windows. Tím se spustí prohlížeč událostí.
  2. V podokně **akcí** klikněte na **Otevřít uložený protokol** a přejděte na souborů protokolu ve formátu etvx/trasování událostí pro Windows (balíček podpora). Teď můžete zobrazit soubor. Po otevření souboru kliknete pravým tlačítkem a uložit soubor jako text.
   
    > [AZURE.IMPORTANT] Taky můžete použít rutinu **Get-WinEvent** k otevření souborů v prostředí Windows PowerShell. Další informace najdete v článku [Get-WinEvent](https://technet.microsoft.com/library/hh849682.aspx) v dokumentaci rutiny prostředí Windows PowerShell.

5. Protokoly při otevření v prohlížeči událostí, hledejte následující protokolů, které obsahují problémům souvisejícím s konfigurací zařízení:

  - hcs_pfconfig/funkční protokolu
  - hcs_pfconfig/konfigurace

6. Protokoly najděte řetězců souvisejících rutiny s názvem pomocí Průvodce nastavením. Seznam těchto rutin najdete v článku [Průvodce instalací první](#first-time-setup-wizard-process) . 

7. Pokud si nejste schopni zjistit příčinu problému, můžete [kontaktovat podporu společnosti Microsoft](storsimple-contact-microsoft-support.md) pro další kroky. Pomocí postupu v seznamu [vytvořit žádost o podporu](storsimple-contact-microsoft-support.md#create-a-support-request) při Microsoft Support požádejte o pomoc.

## <a name="cmdlets-available-for-troubleshooting"></a>Rutiny pro správu k dispozici pro řešení potíží

Pomocí těchto rutin prostředí Windows PowerShell zjišťování chyb připojení.

- `Get-NetAdapter`: Tahle rutina slouží ke zjištění stavu rozhraní sítě. 

- `Test-Connection`: Tahle rutina slouží ke kontrole připojení k síti ve i mimo ni sítě.

- `Test-HcsmConnection`: Tahle rutina slouží ke kontrole připojení úspěšně registrovaných zařízení.

Pokud používáte aktualizace 1 na zařízení s StorSimple, jsou dostupné taky tyto diagnostické rutiny.

- `Sync-HcsTime`: Tahle rutina slouží k zobrazení času zařízení a vynutit čas synchronizovat se serverem NPT.

- `Enable-HcsPing`a `Disable-HcsPing`: pomocí těchto rutin umožňuje hosts ping síťová rozhraní na zařízení s StorSimple. Ve výchozím nastavení rozhraní sítě StorSimple nebudete reagovat požadavky ping.

- `Trace-HcsRoute`: Tahle rutina slouží jako nástroj pro trasování směrování. Odešle pakety každému směrovači na cestě k cíli průběhu určitého časového období a potom vypočítá výsledky podle paketů vrácených každého směrování. Protože `Trace-HcsRoute` ukazuje úroveň ztráty paketu v zadaném směrovači nebo odkaz, zdůraznit které směrovači nebo odkazy může způsobovat problémy se sítí. 

- `Get-HcsRoutingTable`: Tahle rutina slouží k zobrazení místní tabulky směrování IP.

## <a name="troubleshoot-with-the-get-netadapter-cmdlet"></a>Poradce při potížích s rutinu Get-NetAdapter

Při konfiguraci síťových rozhraní pro nasazení první zařízení stav hardwaru není k dispozici v StorSimple Správce služby UI, protože zařízení není zatím registrovaný u služby. Kromě toho na stránce se stavem Hardware nemusí neodráží vždy správně stav zařízení, zejména pokud jsou problémy, které ovlivňují synchronizace služby. V těchto situacích můžete použít `Get-NetAdapter` rutina určit stav sítě rozhraní.

### <a name="to-see-a-list-of-all-the-network-adapters-on-your-device"></a>Chcete zobrazit všechny síťové adaptéry na vašem zařízení

1. Spusťte Windows PowerShell pro StorSimple a potom zadejte `Get-NetAdapter`. 

2. Výstup použít `Get-NetAdapter` rutina a následující Rady, které pochopit stav sítě rozhraní.
  - Pokud je rozhraní správný a zapnutí, stav **Index_rozhraní** se zobrazí jako **nahoru**.
  - Pokud rozhraní pořádku, ale není připojený fyzicky (podle síťový kabel), **Index_rozhraní** zobrazují jako **Zakázané**.
  - Pokud je rozhraní správný, ale není povolený, stav **Index_rozhraní** se zobrazí jako **NotPresent**.
  - Pokud rozhraní neexistuje, se nezobrazoval v tomto seznamu. StorSimple Správce služby UI, bude mít tento rozhraní ve stavu selhání.

Další informace o tom, jak používat tuto rutinu přejděte do [GetNetAdapter](https://technet.microsoft.com/library/jj130867.aspx) reference pro rutiny prostředí Windows PowerShell. 

V následujících částech zobrazit příklady výstup `Get-NetAdapter` rutiny. 

 V těchto vzorcích řadiče 0 byla pasivní řadiče a nakonfigurovanou následujícím způsobem:

- DATA 0, DATA 1, DATA 2 a DATA 3 sítě, rozhraní existoval na zařízení.
- DATA 4 a 5 dat síťové karty nebyly nalezeny; proto nejsou uvedené v výstupu.
- Byla povolena DATA 0.

Řadiči 1 byl aktivní řadiče a nakonfigurovanou následujícím způsobem:

- DATA 0, DATA 1, 2 dat, DATA 3, 4 dat a dat 5 sítě, rozhraní existoval na zařízení.
- Byla povolena DATA 0.

**Ukázka výstup – řadiče 0**

Následující obrázek je výstup řadiče 0 (pasivní řadiče). Nejste připojení dat 1, DATA 2 a DATA 3. DATA 4 a 5 dat nejsou zobrazeny, protože neexistují na zařízení. 

     Controller0>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter #2     17       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter        14       NotPresent
     Ethernet 2           HCS VNIC                                    13       Up
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up


**Ukázka výstup – řadiči 1**

Následující obrázek je výstup řadiči 1 (aktivní řadiče). Pouze DATA 0 nakonfigurovaný síťové na zařízení a pracovat.

     Controller1>Get-NetAdapter
     Name                 InterfaceDescription                        ifIndex  Status
     ------               --------------------                        -------  ----------
     DATA3                Mellanox ConnectX-3 Ethernet Adapter        18       NotPresent
     DATA2                Mellanox ConnectX-3 Ethernet Adapter #2     19       NotPresent
     DATA1                Intel(R) 82574L Gigabit Network Co...#2     16       NotPresent
     DATA0                Intel(R) 82574L Gigabit Network Conn...     15       Up
     Ethernet 2           HCS VNIC                                    13       Up
     DATA5                Intel(R) Gigabit ET Dual Port Server...     14       NotPresent
     DATA4                Intel(R) Gigabit ET Dual Port Serv...#2     17       NotPresent

 
## <a name="troubleshoot-with-the-test-connection-cmdlet"></a>Poradce při potížích s rutinu testovat připojení

Můžete použít `Test-Connection` rutina zkontrolujte, zda StorSimple zařízení můžete připojit k externí síti. Pokud všechny sociální sítě parametry, včetně DNS, budou správně nakonfigurované v Průvodci nastavením, můžete `Test-Connection` rutina ping adresu známé mimo sítě, jako je outlook.com. 

Povolíte ping vyřešit problémy s připojením s tuto rutinu Pokud ping zakázané.

Viz následující příklady výstup `Test-Connection` rutiny. 

> [AZURE.NOTE] V prvním vzorku zařízení nakonfigurována nesprávné DNS. Ve druhém výběru DNS není správná.
 
**Ukázka výstup – nesprávné DNS**

V následujícím příkladu je žádný výstup pro adresy IPV4 a IPV6, což znamená, že nedojde k vyřešení DNS. To znamená, že je bez připojení k externí síti a správné DNS musí být zadán. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com
     HCSNODE0      outlook.com

**Ukázka výstup – správné DNS**

V následujícím příkladu DNS vrátí adresu IPV4 označující, že DNS správně nakonfigurované. To znamená, že je připojení k externí sítě. 

     Source        Destination     IPV4Address      IPV6Address
     ------        -----------     -----------      -----------
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194
     HCSNODE0      outlook.com     132.245.92.194

## <a name="troubleshoot-with-the-test-hcsmconnection-cmdlet"></a>Poradce při potížích s rutinu Test HcsmConnection

Použití `Test-HcsmConnection` rutiny pro zařízení, které je už připojené k a registrovaný u StorSimple Správce služby. Tato rutina vám pomůže zkontrolovat propojení mezi registrovaných zařízení a odpovídající StorSimple Správce služeb. Tento příkaz můžete spustit na prostředí Windows PowerShell pro StorSimple. 

### <a name="to-run-the-test-hcsmconnection-cmdlet"></a>Spusťte rutinu Test HcsmConnection

1. Ujistěte se, že je registrovaná zařízení.

2. Zkontrolujte stav zařízení. Pokud zařízení je nebo byl deaktivovaný, v režimu údržby nebo v režimu offline, může se zobrazit následující chyby: 

   - ErrorCode.CiSDeviceDecommissioned – znamená to, že zařízení deaktivuje.
   - ErrorCode.DeviceNotReady – znamená to, že zařízení je v režimu údržby.
   - ErrorCode.DeviceNotReady – to znamená, že zařízení není online.

3. Zkontrolujte, že je spuštěná služba StorSimple správce (použít rutinu [Get-ClusterResource](https://technet.microsoft.com/library/ee461004.aspx) ). Pokud není spuštěná služba, může se zobrazit následující chyby:

   - ErrorCode.CiSApplianceAgentNotOnline
   - ErrorCode.CisPowershellScriptHcsError – to znamená, že jste udělali výjimky při spuštění Get-ClusterResource.

4. Zaškrtněte políčko token služby Řízení přístupu (ACS). Pokud je výjimku web, může to být výsledek brány problém, chybějící ověřování proxy, nesprávné DNS nebo neúspěšné ověření. Může se zobrazit následující chyby:

   - ErrorCode.CiSApplianceGateway – to znamená výjimku HttpStatusCode.BadGateway: službu překládání názvů nelze přeložit název hostitele. 
   - ErrorCode.CiSApplianceProxy – to znamená výjimku HttpStatusCode.ProxyAuthenticationRequired (HTTP stavový kód 407): nemůže ověřit klienta na proxy serveru. 
   - ErrorCode.CiSApplianceDNSError – to znamená WebExceptionStatus.NameResolutionFailure výjimku: službu překládání názvů nelze přeložit název hostitele.
   - ErrorCode.CiSApplianceACSError – to znamená služby vrácena chyba ověření, že je připojení.
   
    Pokud to není výjimku webu, vyhledejte ErrorCode.CiSApplianceFailure. Tento údaj označuje, že zařízení se nezdařila.

5. Zkontrolujte připojení služby cloudu. Pokud službu výjimku web, může se zobrazit následující chyby:

  - ErrorCode.CiSApplianceGateway – to znamená výjimku HttpStatusCode.BadGateway: intermediate proxy serveru dostali chybný požadavek z jiného proxy nebo z původního serveru.
  - ErrorCode.CiSApplianceProxy – to znamená výjimku HttpStatusCode.ProxyAuthenticationRequired (HTTP stavový kód 407): nemůže ověřit klienta na proxy serveru. 
  - ErrorCode.CiSApplianceDNSError – to znamená WebExceptionStatus.NameResolutionFailure výjimku: službu překládání názvů nelze přeložit název hostitele.
  - ErrorCode.CiSApplianceACSError – to znamená služby vrácena chyba ověření, že je připojení.
  
    Pokud to není výjimku webu, vyhledejte ErrorCode.CiSApplianceSaasServiceError. Tento údaj označuje problém se službou StorSimple správce.
 
6. Zkontrolujte připojení Bus služby Azure. ErrorCode.CiSApplianceServiceBusError označuje, že zařízení nemůže připojit k Bus služby.
 
Soubory protokolu CiSCommandletLog0Curr.errlog a CiSAgentsvc0Curr.errlog bude mít víc informací, například Podrobnosti o výjimce. 

Další informace o tom, jak použít rutinu, přejděte na [Test HcsmConnection](https://technet.microsoft.com/library/dn715782.aspx) v prostředí Windows PowerShell odkazovat si přečtěte následující dokumentaci.

> [AZURE.IMPORTANT] Můžete použít tuto rutinu pro aktivní a pasivní správce. 
 
Viz následující příklady výstup `Test-HcsmConnection` rutiny. 

**Ukázka výstup – úspěšně registrovaných zařízení spuštěna StorSimple verze (červenec 2014)**

První vzorek tvoří ze zařízení, které máte úspěšně zaregistrované v rámci službu StorSimple správce a má žádné problémy s připojením. 

     Controller1>Test-HcsmConnection -verbose
     Checking device state  ... Success.
     Device registered successfully
     Checking device authentication.  ... This operation will take few minutes to complete....
     Checking device authentication.  ... Success.
     Checking connectivity from device to StorSimple Manager service.  ... This operation will take few minutes to complete....
     Checking connectivity from device to StorSimple Manager service.  ... Success.
     Checking connectivity from StorSimple Manager service to StorSimple device. .... Success.
     Controller1>

**Ukázkový výstup – úspěšně registrovaných zařízení se serverem StorSimple aktualizace 1**

Pokud používáte aktualizace 1 na zařízení s StorSimple, nebudete muset spustit s přepínačem podrobného.

      Controller1>Test-HcsmConnection
       
      Checking device registration state  ... Success
      Device registered successfully
       
      Checking primary NTP server [time.windows.com] ... Success
       
      Checking web proxy  ... NOT SET
       
      Checking primary IPv4 DNS server [10.222.118.154] ... Success
      Checking primary IPv6 DNS server  ... NOT SET
      Checking secondary IPv4 DNS server [10.222.120.24] ... Success
      Checking secondary IPv6 DNS server  ... NOT SET
       
      Checking device online  ... Success
 
      Checking device authentication  ... This will take a few minutes.
      Checking device authentication  ... Success
       
      Checking connectivity from device to service  ... This will take a few minutes.
       
      Checking connectivity from device to service  ... Success
       
      Checking connectivity from service to device  ... Success
       
      Checking connectivity to Microsoft Update servers  ... Success
      Controller1>

**Ukázka výstup – offline zařízení spuštěna StorSimple verze (červenec 2014)**

V tomto příkladu je ze zařízení, které má stav **Offline** v portálu Azure klasické.

     Checking device state: Success 
     Device is registered successfully 
     Checking connectivity from device to SaaS.. Failure

Zařízení nelze se připojit pomocí konfigurace proxy aktuální web. Může to být problém s konfigurací proxy web nebo problém s připojením k síti. V tomto případě nezapomeňte, že nastavení proxy serveru webové jsou správné a web servery proxy online a dostupný. 

## <a name="troubleshoot-with-the-sync-hcstime-cmdlet"></a>Poradce při potížích s rutinu HcsTime synchronizací

Tahle rutina slouží k zobrazení času zařízení. Pokud je zařízení čas posun se serverem NPT, můžete tuto rutinu platnost synchronizace času se serverem NTP. Pokud posun mezi zařízení a NTP server je větší než 5 minut, zobrazí se upozornění. Posun vyšší než 15 minut, zařízení budou přesměrovány offline. K vynucení synchronizace času můžete dál používat tuto rutinu. Ale pokud posun delší než 15 hodin, pak nebudete moct platnost synchronizace, které se zobrazí chybová zpráva a čas.

**Ukázkový výstup – synchronizace vynuceného času pomocí synchronizace HcsTime**
 
     Controller0>Sync-HcsTime
     The current device time is 4/24/2015 4:05:40 PM UTC.
 
     Time difference between NTP server and appliance is 00.0824069 seconds. Do you want to resync time with NTP server?
     [Y] Yes [N] No (Default is "Y"): Y
     Controller0>

## <a name="troubleshoot-with-the-enable-hcsping-and-disable-hcsping-cmdlets"></a>Poradce při potížích s rutiny Enable-HcsPing a zakázat HcsPing

Pomocí těchto rutin zajistit, aby síťová rozhraní na vašem zařízení reagovat na požadavky ping ICMP. Ve výchozím nastavení rozhraní sítě StorSimple nebudete reagovat požadavky ping. Použití tuto rutinu je nejjednodušší způsob, jak zjistit, zda zařízení online a dostupný.  

**Ukázkový výstup – povolit HcsPing a zakázat HcsPing**

     Controller0>
     Controller0>Enable-HcsPing
     Successfully enabled ping.
     Controller0>
     Controller0>
     Controller0>Disable-HcsPing
     Successfully disabled ping.
     Controller0>

## <a name="troubleshoot-with-the-trace-hcsroute-cmdlet"></a>Poradce při potížích s rutinu HcsRoute sledování

Tahle rutina slouží jako nástroj pro trasování směrování. Odešle pakety každému směrovači na cestě k cíli průběhu určitého časového období a potom vypočítá výsledky podle paketů vrácených každého směrování. Protože rutinu ukazuje úroveň ztráty paketu v každém směrovači nebo odkaz, můžete určit které směrovači nebo odkazy může způsobovat problémy se sítí.

**Ukázkový výstup znázorňující, jak sledovat trasu paket s HcsRoute sledování**

     Controller0>Trace-HcsRoute -Target 10.126.174.25
     
     Tracing route to contoso.com [10.126.174.25]
     over a maximum of 30 hops:
       0  HCSNode0 [10.126.173.90]
       1  contoso.com [10.126.174.25]
      
     Computing statistics for 25 seconds...
                 Source to Here   This Node/Link
     Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  Address
       0                                           HCSNode0 [10.126.173.90]
                                     0/ 100 =  0%   |
       1    0ms     0/ 100 =  0%     0/ 100 =  0%  contoso.com
      [10.126.174.25]
      
     Trace complete.

## <a name="troubleshoot-with-the-get-hcsroutingtable-cmdlet"></a>Poradce při potížích s rutinu Get-HcsRoutingTable

Tahle rutina slouží k zobrazení směrovací tabulky pro vaše zařízení StorSimple. Směrovací tabulku, která je sady pravidel, které pomáhají určit, kde budete přesměrováni datové pakety cestování přes síť Internet Protocol (IP). 

Směrování tabulka obsahuje rozhraní a brány, který přesměrovává data do určité sítě. Dále poskytuje metriky, což je maker rozhodnutí pro trasu k dosažení konkrétní cíle. Nižší metriky, vyšší předvoleb. 

Například pokud máte 2 síťová rozhraní, DATA 2 a 3 dat, připojení k Internetu. Pokud směrování metriky pro DATA 2 a 3 dat jsou 15 a 261, 2 dat s nižší metriky je upřednostňované rozhraní používané pro přístup k Internetu.

Pokud používáte aktualizace 1 na zařízení s StorSimple, 0 síťové rozhraní dat obsahuje nejvyšší předvoleb pro přenos cloudu. To znamená, že i v případě, že jsou tu uvedené ostatní rozhraní povolena cloudu, přenosy cloudu směrován regresi datových 0. 

Pokud narazíte `Get-HcsRoutingTable` rutina bez zadání parametrů (viz následující příklad), rutinu výstup IPv4 a IPv6 směrovací tabulky. Můžete taky můžete zadat `Get-HcsRoutingTable -IPv4` nebo `Get-HcsRoutingTable -IPv6` získat relevantní směrování tabulku.

      Controller0>
      Controller0>Get-HcsRoutingTable
      ===========================================================================
      Interface List
       14...00 50 cc 79 63 40 ......Intel(R) 82574L Gigabit Network Connection
       12...02 9a 0a 5b 98 1f ......Microsoft Failover Cluster Virtual Adapter
       13...28 18 78 bc 4b 85 ......HCS VNIC
        1...........................Software Loopback Interface 1
       21...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
       22...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #3
      ===========================================================================
       
      IPv4 Route Table
      ===========================================================================
      Active Routes:
      Network Destination        Netmask          Gateway       Interface  Metric
                0.0.0.0          0.0.0.0  192.168.111.100  192.168.111.101     15
              127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
              127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
        127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
            169.254.0.0      255.255.0.0         On-link     169.254.1.235    261
          169.254.1.235  255.255.255.255         On-link     169.254.1.235    261
        169.254.255.255  255.255.255.255         On-link     169.254.1.235    261
          192.168.111.0    255.255.255.0         On-link   192.168.111.101    266
        192.168.111.101  255.255.255.255         On-link   192.168.111.101    266
        192.168.111.255  255.255.255.255         On-link   192.168.111.101    266
              224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
              224.0.0.0        240.0.0.0         On-link     169.254.1.235    261
              224.0.0.0        240.0.0.0         On-link   192.168.111.101    266
        255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        255.255.255.255  255.255.255.255         On-link     169.254.1.235    261
        255.255.255.255  255.255.255.255         On-link   192.168.111.101    266
      ===========================================================================
      Persistent Routes:
        Network Address          Netmask  Gateway Address  Metric
                0.0.0.0          0.0.0.0  192.168.111.100       5
      ===========================================================================
       
      IPv6 Route Table
      ===========================================================================
      Active Routes:
       If Metric Network Destination      Gateway
        1    306 ::1/128                  On-link
       13    276 fd99:4c5b:5525:d80b::/64 On-link
       13    276 fd99:4c5b:5525:d80b::1/128
                                          On-link
       13    276 fd99:4c5b:5525:d80b::3/128
                                          On-link
       13    276 fe80::/64                On-link
       12    261 fe80::/64                On-link
       13    276 fe80::17a:4eba:7c80:727f/128
                                          On-link
       12    261 fe80::fc97:1a53:e81a:3454/128
                                          On-link
        1    306 ff00::/8                 On-link
       13    276 ff00::/8                 On-link
       12    261 ff00::/8                 On-link
       14    266 ff00::/8                 On-link
      ===========================================================================
      Persistent Routes:
        None
       
      Controller0>
 
## <a name="step-by-step-storsimple-troubleshooting-example"></a>Podrobný StorSimple Poradce při potížích příklad

Následující příklad ukazuje podrobný Poradce při potížích s StorSimple nasazení. Scénáře příkladu registrace zařízení nezdaří a zobrazí se chybová zpráva, že nastavení sítě nebo název DNS není správná.

Chybová zpráva je:

     Invoke-HcsSetupWizard: An error has occurred while registering the device. This could be due to incorrect IP address or DNS name. Please check your network settings and try again. If the problems persist, contact Microsoft Support.
     +CategoryInfo: Not specified
     +FullyQualifiedErrorID: CiSClientCommunicationErros, Microsoft.HCS.Management.PowerShell.Cmdlets.InvokeHcsSetupWizardCommand

Chyba může být způsobeno některým z následujících akcí:

- Instalace nesprávné hardwaru
- Chyba síťové složka
- Nesprávný IP adresu, masku podsítě, brány, primární DNS server nebo proxy serveru webové
- Nesprávný registračního klíče
- Nastavení nesprávné brány firewall

### <a name="to-locate-and-fix-the-device-registration-problem"></a>Najít a vyřešit problém registrace zařízení

1. Kontrola konfigurace zařízení: aktivní řadiči spustit `Invoke-HcsSetupWizard`.

     > [AZURE.NOTE] Průvodce spuštěním řadiči aktivní. Pokud chcete ověřit, zda jste připojeni k aktivní řadiči, podívejte se nápis prezentovány v konzole sériové. Nápis označuje, zda jste připojeni k řadiče 0 nebo řadiči 1, a jestli správce aktivní nebo pasivní. Další informace přejděte na [identifikovat aktivní řadiče na vašem zařízení](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).
 
2. Ujistěte se, že zařízení správně zapojené: Zkontrolujte síťové kabely na zpět ploché zařízení. Kabeláž závisí na modelu zařízení. Další informace přejděte na [nainstalovat zařízení StorSimple 8100](storsimple-8100-hardware-installation.md) nebo [StorSimple 8600 zařízení](storsimple-8600-hardware-installation.md).

     > [AZURE.NOTE] Pokud používáte 10 porty sítě, musíte použijte dodaná adaptéry QSFP SFP a SFP kabely. Další informace najdete v tématu [seznam kabely, přepínačích a vysílače doporučené dodavatelem OEM Mellanox porty](http://www.mellanox.com/page/cables?mtag=cable_overview).
 
3. Ověření stavu rozhraní sítě:

   - Použijte rutinu Get-NetAdapter zjišťování stavu rozhraní sítě pro DATA 0. 
   - Pokud odkaz nefunguje, stav **Index_rozhraní** se označuje, že rozhraní dolů. Pak potřebujete zkontrolovat síťové připojení port k zařízení a chcete přepnout. Bude taky muset vyloučit chybná kabely. 
   - Pokud máte podezření, že DATA 0 port aktivní řadiče selhalo, kde můžete potvrdit to tak, že při připojování k DATŮM 0 port řadiči 1. Potvrďte to Odpojit síťový kabel z zpět zařízení z řadiče 0, připojte kabel k řadiči 1 a pak znova spusťte rutinu Get-NetAdapter. 
   Pokud DATA 0 port na řadiči se nezdaří, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) k dalším krokům. Možná budete muset nahradit správce systému.
 
4. Ověřte připojení k přechodu:
   - Zkontrolujte, že DATA 0 síťová rozhraní na řadiče 0 a 1 řadiče v primární přílohu nejsou na stejné podsítě. 
   - Podívejte se v centrální nebo směrovači. Obvykle byste měli být připojeni obou řadiče stejné centrální nebo směrovači. 
   - Zkontrolujte, že přepínače, které používáte pro připojení dat 0 pro oba řadiče ve stejném vLAN.
   
5. Odstraňte všechny chyby uživatele:

  - Spuštění Průvodce nastavením znovu (Spustit **Vyvolat HcsSetupWizard**) a zadejte hodnoty znovu a zkontrolujte, že nejsou žádné chyby. 
  - Ověření registrace klíč použitý. Stejný klíč registrace mohou sloužit k připojení ke službě StorSimple správce víc zařízení. Pomocí postupu v [získat klíč registrace služby](storsimple-manage-service.md#get-the-service-registration-key) zajistit, že používáte správné registračního klíče.

    > [AZURE.IMPORTANT] Pokud máte víc služby, bude nutné ověřit, že registrace příslušné služby používá se zaregistrovat zařízení. Pokud zařízení zaregistrovali ke službě nepovedlo StorSimple správce, musíte se [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) k dalším krokům. Možná bude potřeba provést factory resetovat zařízení (který může dojít ke ztrátě dat) a potom je připojíte k určení.

6. Zkontrolujte, jestli máte připojení k externí síti získáte pomocí rutiny testovat připojení. Další informace přejděte na [Poradce při potížích s rutinu testovat připojení](#troubleshoot-with-the-test-connection-cmdlet).

7. Vyhledat bránu firewall rušení. Pokud jste ověřili, že virtuální IP (VIP), podsítě, brány a DNS správnost nastavení všech a dál vidět problémy s připojením a pak je možné, že brány firewall blokuje komunikaci mezi zařízení a externí sítě. Je třeba zajistit, že porty 80 a 443 jsou dostupné na vašem zařízení StorSimple pro odchozí komunikaci. Další informace najdete v tématu [požadavky pro zařízení s StorSimple připojení sítě](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

8. Podívejte se na protokoly. Přejděte na [podporu balíčků a zařízení protokoly k dispozici pro řešení potíží](#support-packages-and-device-logs-available-for-troubleshooting).

9. Pokud se předchozích kroků problém nevyřeší, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) , aby vám.

## <a name="next-steps"></a>Další kroky
[Zjistěte, jak řešit problémy s provozní zařízení](storsimple-troubleshoot-operational-device.md).

<!--Link references-->

[1]: https://technet.microsoft.com/library/dd379547(v=ws.10).aspx
[2]: https://technet.microsoft.com/library/dd392266(v=ws.10).aspx 
