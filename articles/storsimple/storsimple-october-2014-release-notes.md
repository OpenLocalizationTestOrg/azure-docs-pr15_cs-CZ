<properties 
    pageTitle="Aktualizace 8000 StorSimple 0,1 poznámky k verzi | Microsoft Azure"
    description="Tento článek popisuje nové funkce a opravy, otevřené problémy a k dispozici řešení pro říjen 2014 verzi Microsoft Azure StorSimple (aktualizace 0,1)."
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
    ms.date="09/21/2016"
    ms.author="alkohli" />

# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>Aktualizace řady 8000 StorSimple 0,1 poznámky – říjen 2014  

## <a name="overview"></a>Základní informace

Následující poznámky k verzi popsána kritické otevřené problémy StorSimple 8000 řady aktualizace 0,1 vydání z října 2014. Obsahují také seznam StorSimple software a firmware aktualizace zahrnuté v této verzi. Toto je první vydání po StorSimple 8000 řady vydanou verzi byl zavedený obecně červenec 2014 a odpovídá verzi softwaru 6.3.9600.17312.  

Doporučujeme vyhledání a použití všechny dostupné aktualizace hned po instalaci zařízení. Můžete také zapnout automatické aktualizace si stáhněte a nainstalujte aktualizace s vysokou prioritou od Microsoftu hned po jejich vydání. Další informace najdete v tématu Jak [Aktualizovat StorSimple zařízení](storsimple-update-device.md).  

Zkontrolujte informace obsažené v poznámky k verzi před nasazením aktualizace ve vašem StorSimple řešení.  

>[AZURE.IMPORTANT]
> 
-   Umožňuje StorSimple Správce služby a ne prostředí Windows PowerShell pro StorSimple instalovat aktualizace z října.  
-   Aktualizace obvykle trvá asi 3 hodiny dokončete.  
-   Říjen verzi StorSimple neobsahuje nějaké aktualizace StorSimple virtuální zařízení. Můžete také použít všechny dostupné aktualizace Windows, včetně poslední bezpečnostní opravy, ale neuvidíte změny ve verzi pro virtuální zařízení.  

Zkontrolujte, jestli že jsou splněné následující požadavky před aktualizace StorSimple zařízení.  

- Ujistěte se, jestli jsou oba řadiče zařízení spuštěný před vyhledat aktualizace. Pokud některý z řadiče nespustí, se nepovede naskenovaný obrázek. Ověřte řadiče v pořádku stavu, přejděte v části stránky **údržby** **Stav hardwaru** . Pokud jsou součástí, které **potřebujete pozornost**, obraťte se na Microsoft Support soubory protokolů.  
- Pevné IP adresy pro oba řadiče 0 a 1 řadiče domény se zajistila směrovatelné a můžete připojit k Internetu, které jsou použity pro přípravu aktualizace zařízení. [Rutina testovat připojení](https://technet.microsoft.com/library/hh849808.aspx) slouží k použití příkazu ping adresu známé mimo sítě například outlook.com, můžete ověřit, že správce má připojení k externí sítě.  
- Ujistěte se, že požadované výstupní porty jsou dostupné na vašem zařízení StorSimple pro odchozí komunikaci. Další informace najdete v tématu [požadavky pro zařízení s StorSimple připojení sítě](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
- Pokud verze software zařízení není starší než 6.3.9600.17312 (aktualizace z října 2014), zakážete porty Data 2 a 3 dat, pokud je povoleno, před spuštěním aktualizace. Pokud necháte Data 2 nebo 3 dat porty povolené po instalaci aktualizace, může způsobit ovladači zařízení přejděte do režimu obnovení. Upozorňujeme, že když zakážete síťová rozhraní všechny související svazky budou do režimu offline a vstupu a výstupu bude přerušení po celou dobu aktualizace.  

## <a name="whats-new-in-the-october-release"></a>Co je nového ve verzi dne

Tato aktualizace obsahuje tato vylepšení:

- Teď můžete StorSimple Správce služby UI ke správě řadiče zařízení. Správy, které obsahují akce znovu spustit, vypnout nebo zapnout řadiči. Další informace přejděte na [Správa StorSimple řadiče zařízení](storsimple-manage-device-controller.md).  
- Plánujete přidělení šířky pásma sítě WAN podle kombinace den týdne a čas den. Umožňuje lepšímu využití šířky pásma WAN dobu největšího. Různé šířky pásma šablon jsou povoleny kontejnerů různých hlasitost. Další informace přejděte na [Správa šablon StorSimple šířky pásma](storsimple-manage-bandwidth-templates.md).  
- Můžete nakonfigurovat e-mailová oznámení včasným upozornění správci a některé z existujících nebo případně nadcházející problémů. Další informace přejděte na [Konfigurovat nastavení upozornění](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-the-october-release"></a>Problémy s pevnou ve verzi dne


Následující tabulka obsahuje souhrn problémy, které byly pevně v tuto aktualizaci.  

| Ne. | Funkce | Problém | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
|-----|---------|-------|---------------------------------|--------------------------------|
| 1 | Rozhraní sítě | V předchozí verzi síťových rozhraní DATA 2 a 3 dat byly vyměnit softwaru. To byl odstraněn tato aktualizace. Vymažte nastavení a zakažte tyto síťová rozhraní před instalací aktualizace. Po instalaci aktualizace, budete muset překonfigurovat těchto rozhraní. | Ano | Ne |
| 2 | Podpora balíčku | V předchozí verzi, pokud jste spustili rutiny prostředí Windows PowerShell **Export HcsSupportPackage** k načtení protokolech Baseboard Management řadiči (BMC) nezdařil se následující upozornění: "operace úspěšně v tomto řadiči, ale se nezdařilo v důsledku následujících chyb řadiči mezi dvěma účastníky. Zkontrolujte, zda je funkční druhé a zda se můžete připojit aktuální uzel partnerovi." Tento problém je teď pevné. | Ano | Ne |
| 3 | Převzetí zařízení | V předchozí verzi došlo možnost dat nekonzistence Pokud úlohy **Seznamte se s zálohování** způsobila nezdařenou přepojení zařízení. Tento problém je teď pevné. | Ano | Ne |
| 4 | Převzetí zařízení | V předchozí verzi po přepojení zařízení zálohování bylo vidět, ale kontejneru přidružené hlasitost nebyl nalezen na cílové zařízení. Tento problém je teď pevné. | Ano | Ne |
| 5 | Převzetí zařízení | V předchozí verzi jste udělali chybu ve výčtu cloudu zálohování během registru obnovení, které potenciálně umožňují dat nekonzistence kdyby problémy s připojením cloudu. | Ano | Ne |
| 6 | Aktualizaci firmwaru | V předchozí verzi úlohy aktualizaci firmwaru zařízení se nepodařilo a zobrazí chybu, které je uvedeno rutin nebyly rozpoznat, a jako výsledek nepodařilo aktualizovat. Správce potom přechodem do režimu obnovení. Tento problém je teď pevné. | Ano | Ne |
| 7 | Instalace | Chyby způsobená zařízení není s správně obrázky během instalace mít teď opravit. | Ano | Ne |
| 8 | Obnovení Factory | Teď můžete volitelně přeskočit firmware vyhledat factory obnovit. Jedná se o změnu z předchozí verze. | Ano | Ne |
| 9 | Obnovení Factory | V předchozí verzi rutina obnovit factory jste při práci s firmware verze kontroly provedené jenom pro některé součásti hardwaru. Kontroly pro další firmware provedené po prvním restartování procesu, což může způsobit selhání obnovit. Tento opravný nástroj zajišťuje, že všechny kontroly firmware provádějí při továrny obnovit rutina po spuštění a před první systém restartováním počítače. | Ano | Ne |
| 10 | Úložiště účtu klíčové otočení v prostoru | **Vyvolat HcsmServiceDataEncryptionKeyChange** rutina slouží k otočení klávesy účtu úložiště zobrazí výzvu k zadání šifrovací klíč dat služby. Jedná se o změnu z předchozí verze, ve kterém šifrovací klíč dat služby předané jako parametr vložené. | Ano | Ne |
| 11 | Navrácení 24 hodin | Během obnovení havárie vyčištění na zařízení zdroj z toho důvodu předchozího, působí navrácení selhání. To byl odstraněn v této verzi. | Ano | Ne |

## <a name="known-issues-in-the-october-release"></a>Známé problémy v říjnu vydání

Následující tabulka obsahuje souhrn známé problémy v této verzi.

| Ne. | Funkce | Problém | Komentáře a řešení | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Obnovení Factory | V některých případech při provádění factory resetovat zařízení StorSimple může být pomalé a zobrazit tato zpráva: **Obnovit factory probíhá (fáze 8)**. Tím se stane, když probíhá rutinu stiskněte kombinaci kláves CTRL + C. | Není stisknutím klávesy CTRL + C po zahájení factory obnovit. Pokud už jste tento stav, obraťte se prosím Microsoft Support k dalším krokům. | Ano | Ne |
| 2 | Obnovení Factory | Proveďte není factory obnovit StorSimple zařízení, které se změní z GA na října 2014 uvolněte. | Tuto operaci funkční pouze v případě opravný nástroj je nainstalovaný. Kontaktujte Microsoft Support získat vyžaduje opravu. | Ano | Ne | 
| 3 | Disk kvora | Výjimečně Pokud většinou disků v EBOD přílohu 8600 zařízení jsou to od ní odpojilo výsledkem bez disku kvora fondu úložiště bude offline. Zůstane v režimu offline, i když disků připojeni. | Je třeba Restartujte zařízení. Pokud potíže potrvají, obraťte se prosím Microsoft Support k dalším krokům. | Ano | Ne |
| 4 | Selhání snímek cloudu | Výjimečně může selhat cloudu snímek s chybou **maximální limit záložní dosažení**. K tomu dojde, pokud překročit 255 online klony na stejné zařízení ze stejného objemem původní, který byl odstraněn. | | Ano | Ano |
| 5 | ID nesprávné zařízení | Při provádění náhradní řadiče domény řadiče domény 0 může zobrazit jako řadiči 1. Během řadiče náhradní při načtení obrázek z uzel mezi dvěma účastníky ID řadiče zobrazit původně jako ID zařízení mezi dvěma účastníky. Výjimečně toto chování mohou také projeví po restartování systému. |Není nutná žádná akce uživatele. Tato situace vyřeší samotné po dokončení náhradní řadiče domény. | Ano | Ne |
| 6 | Zařízení sledování grafy | Ve službě StorSimple správce sledování grafy zařízení nefunguje Basic nebo ověřování NTLM je povolen v konfiguraci proxy serveru pro zařízení. | Změna konfigurace proxy web pro zařízení registrovaný u StorSimple Správce služby tak, že ověření nastavíte na žádný. K tomuto účelu spustit prostředí Windows PowerShell pro rutinu StorSimple Set-HcsWebProxy. | Ano | Ano |
| 7 | Účty úložiště | Odstranění účtu úložiště pomocí služba úložiště je nepodporované funkce. To vede k situaci, ve kterém nelze uživatelská data načíst. | | Ano | Ano |
| 8 | Převzetí zařízení | Více předávání služeb při selhání kontejneru hlasitosti na stejné zařízení zdroje pro různé cílové zařízení není podporovaná. | Přepnutí z jednoho nefunkční zařízení několika zařízeních pokusy hlasitost kontejnerů na první selhání zařízení dojít ke ztrátě dat vlastnictví. Po selhání tyto hlasitost kontejnery zobrazit nebo s odlišným chováním při zobrazení v portálu Azure klasické. | Ano | Ne |
| 9 | Instalace | Během StorSimple adaptér pro instalaci služby SharePoint budete muset zadat IP zařízení v pořadí k úspěšnému dokončení instalace.    | | Ano | Ne |
| 10 | Proxy serveru webové | Pokud konfigurace proxy serveru webové HTTPS jako zadaný protokol, bude to mít vliv komunikace služby zařízení a zařízení budou přesměrovány offline. Podpora balíčků vygeneruje také v procesu jinými významné zdroje na vašem zařízení. | Zajistěte adresu URL proxy HTTP jako zadaný protokol. Další informace o tom, jak [Konfigurovat proxy serveru webové pro svoje zařízení](storsimple-configure-web-proxy.md). | Ano | Ne |
| 11 | Proxy serveru webové | Pokud konfigurace a povolíte proxy serveru webové na registrovaných zařízení, je potřeba restartovat aktivní řadiče na vašem zařízení. | | Ano | Ne |
| 12 | Vysoký cloudové latence a vysoký pracovní zátěž vstupu a výstupu | Pokud vaše zařízení StorSimple zjistí kombinací čekacích velmi vysoké cloudu dob (pořadí sekund) a maximum pracovní zátěž vstupu a výstupu, svazky zařízení přejděte do jejich funkčnost bude omezena stavu a vstupně-výstupních se nemusí podařit, zobrazí se chyba "zařízení není připraveno". | Je třeba ručně restartujte zařízení řadiče a provedení přepojení zařízení k obnovení ze tato situace. | Ano | Ne |

## <a name="physical-device-updates-in-the-october-release"></a>Aktualizace fyzické zařízení v dne

Když se zařízením fyzické, použijí se tyto aktualizace, verzi softwaru se změní na 6.3.9600.17312. Pokud není zadáno jinak, tyto poznámky k verzi platí pro všechny modely StorSimple zařízení. Další informace o těchto aktualizací najdete v části [softwaru fyzické zařízení října 2014 aktualizace pro Microsoft Azure StorSimple zařízení](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Sériově připojené řadiče přidružení SCSI (zabezpečení) a aktualizace firmwaru v října aktualizací

Tato verze aktualizovat ovladač a firmware řadiči přidružení zabezpečení zařízení fyzicky. Další informace o řadiče přidružení zabezpečení aktualizovat najdete v tématu [října 2014 aktualizace pro řadiče přidružení LSI zabezpečení v aplikaci Microsoft Azure StorSimple zařízení](http://support.microsoft.com/kb/2987020).   

Tato verze se týká rovněž aktualizaci firmwaru kumulativní prostředních spolehlivost problémy s hardwarové součásti zařízení. Další informace o aktualizaci firmwaru najdete v článku [aktualizace pro Microsoft Azure StorSimple zařízení firmware října 2014](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-the-october-release"></a>Aktualizace virtuální zařízení v dne

Tato verze neobsahuje nějaké aktualizace virtuální zařízení. Použili jste tuto aktualizaci nezmění verzi softwaru virtuální zařízení.
 
