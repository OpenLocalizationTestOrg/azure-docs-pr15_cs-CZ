<properties 
   pageTitle="Aktualizace 8000 StorSimple 0,3 poznámky k verzi | Microsoft Azure"
   description="Tento článek popisuje nové funkce a opravy, otevřené problémy a k dispozici řešení pro únor 2015 verzi Microsoft Azure StorSimple (aktualizace z 0,3)."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>StorSimple 8000 řady aktualizace 0,3 poznámky: únor 2015

## <a name="overview"></a>Základní informace

Následující poznámky k verzi popsána kritické otevřené problémy StorSimple 8000 řady aktualizace 0,3 vydání z února 2015. Obsahují také seznam StorSimple software a firmware aktualizace zahrnuté v této verzi. Toto je třetí verze po StorSimple 8000 řady vydanou verzi byl zavedený obecně červenec 2014.
  
Tato aktualizace z ledna aktualizace verzi softwaru zařízení nezmění. Ho nadále verze 6.3.9600.17312. Můžete potvrdit, že aktualizace nainstalována kontrolou datum **Poslední aktualizace** . Pokud je datum 2/10/2015 nebo novější, byla aktualizace nainstalována úspěšně.  

Doporučujeme vyhledání a použití všechny dostupné aktualizace hned po instalaci StorSimple zařízení. Můžete také zapnout automatické aktualizace si stáhněte a nainstalujte aktualizace s vysokou prioritou od Microsoftu hned po jejich vydání. Další informace najdete v tématu [aktualizace StorSimple zařízení](storsimple-update-device.md).  

Zkontrolujte informace obsažené v poznámky k verzi před nasazením aktualizace ve vašem StorSimple řešení.  

>[AZURE.IMPORTANT]   
>
> - Umožňuje StorSimple Správce služby a ne prostředí Windows PowerShell pro StorSimple instalace Únorová aktualizace.   
> - Trvá asi za hodinu nainstalovat tuto aktualizaci. Ale pokud instalujete kumulativními aktualizacemi, může trvat asi 3 hodiny dokončete.  
> - Únor verzi StorSimple neobsahuje nějaké aktualizace StorSimple virtuální zařízení. Můžete také použít všechny dostupné aktualizace Windows virtuální zařízení, včetně poslední bezpečnostní opravy, ale neuvidíte změny ve verzi pro virtuální zařízení.  

Ujistěte se, zda jsou splněny následující požadavky před aktualizace StorSimple zařízení.  

- Ujistěte se, jestli jsou oba řadiče zařízení spuštěný před vyhledat aktualizace. Pokud některý z řadiče nespustí, se nepovede naskenovaný obrázek. Ověřte řadiče v pořádku stavu, přejděte v části stránky **údržby** **Stav hardwaru** . Pokud jsou součástí, které **potřebujete pozornost**, obraťte se na Microsoft Support soubory protokolů.
- Zkontrolujte, zda pevné IP adresy pro řadiče 0 a 1 řadiče směrovat a můžete připojit k Internetu, které jsou použity pro přípravu aktualizace zařízení. [Rutina testovat připojení](https://technet.microsoft.com/library/hh849808.aspx) slouží k použití příkazu ping adresu známé mimo sítě, jako je outlook.com, můžete ověřit, že správce má připojení k externí sítě.
- Ujistěte se, že porty 80 a 443 jsou dostupné na vašem zařízení StorSimple pro odchozí komunikaci. Další informace najdete v tématu [požadavky pro zařízení s StorSimple připojení sítě](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
- Pokud verze software zařízení není starší než 6.3.9600.17312 (aktualizace z října 2014), zakážete porty Data 2 a 3 dat, pokud je povoleno, před spuštěním aktualizace. Nechte Data 2 nebo 3 dat porty povolené po instalaci aktualizace může způsobit ovladači zařízení přejděte do režimu obnovení. Upozorňujeme, že když zakážete síťová rozhraní všechny související svazky budou do režimu offline a vstupně-výstupních bude přerušení po celou dobu aktualizace.  
  
## <a name="whats-new-in-the-february-release"></a>Co je nového ve verzi dne

Tato aktualizace obsahuje opravě u výrobce resetovat problém, ke kterým došlo na zařízení, která vám to upgrade z GA uvolněte verzi října 2014. Další informace najdete v tématu [problémy s pevnou v této verzi](#issues-fixed-in-the-february-release).   

Tato aktualizace neobsahuje nové funkce a funkce.  

## <a name="issues-fixed-in-the-february-release"></a>Problémy s pevnou ve verzi dne

Následující tabulka popisuje problém, který byl odstranit tuto aktualizaci.  
 
| Ne. | Funkce | Problém | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
|-----|---------|-------|---------------------------------|-------------------------------|
| 1 | Obnovení Factory | Zkuste provést factory obnovit na zařízení, který původně měli GA verzi (verze 6.3.9600.17215) nainstalovaný, ale aktualizovaný října verzi (verze 6.3.9600.17312). U výrobce resetujte dojde k chybě a nestabilní zařízení. | Ano | Ne |


## <a name="known-issues-in-the-february-release"></a>Známé problémy ve verzi dne

Následující tabulka obsahuje souhrn známé problémy v této verzi.
 
| Ne. | Funkce | Problém | Komentáře a řešení | Platí pro fyzické zařízení  | Platí pro virtuální zařízení |
|-----|---------|-------|----------------------------|-----------------------------|--------------------------|
| 1 | Obnovení Factory | V některých případech při provádění factory resetovat zařízení StorSimple může být pomalé a zobrazit tato zpráva: **Obnovit factory probíhá (fáze 8)**. Tím se stane, když probíhá rutinu stiskněte kombinaci kláves CTRL + C. | Není stisknutím klávesy CTRL + C po zahájení factory obnovit. Pokud už jste tento stav, obraťte se prosím Microsoft Support k dalším krokům. | Ano | Ne |
| 2 | Disk kvora | Výjimečně Pokud většinou disků v přílohu EBOD 8600device odpojeni výsledkem bez disku kvora fondu úložiště bude offline. Zůstane v režimu offline, i když disků připojeni. | Je třeba Restartujte zařízení. Pokud potíže potrvají, obraťte se prosím Microsoft Support k dalším krokům. | Ano | Ne |
| 3 | Selhání snímek cloudu | Výjimečně může selhat cloudu snímek s chybou **maximální limit záložní dosažení**. K tomu dojde, pokud překročit 255 online klony na stejné zařízení ze stejného objemem původní, který byl odstraněn. |  | Ano | Ano |
| 4 | ID nesprávné zařízení | Při provádění náhradní řadiče domény řadiče domény 0 může zobrazit jako řadiči 1. Během řadiče náhradní při načtení obrázek z uzel mezi dvěma účastníky ID řadiče zobrazit původně jako ID zařízení mezi dvěma účastníky. Výjimečně toto chování mohou také projeví po restartování systému. | Není nutná žádná akce uživatele. Tato situace vyřeší samotné po dokončení náhradní řadiče domény. | Ano | Ne |
| 5 | Zařízení sledování grafy | Ve službě StorSimple správce sledování grafy zařízení nefunguje Basic nebo ověřování NTLM je povolen v konfiguraci proxy serveru pro zařízení. | Změna konfigurace proxy web pro zařízení registrovaný u StorSimple Správce služby tak, že ověření nastavíte na žádný. K tomuto účelu spustit prostředí Windows PowerShell pro rutinu StorSimple Set-HcsWebProxy. | Ano | Ano |
| 6 | Účty úložiště | Odstranění účtu úložiště pomocí služba úložiště je nepodporované funkce. To vede k situaci, ve kterém nelze uživatelská data načíst. |  | Ano | Ano |
| 7 | Převzetí zařízení | Více předávání služeb při selhání kontejneru hlasitosti na stejné zařízení zdroje pro různé cílové zařízení není podporovaná.  Přepnutí z jednoho nefunkční zařízení několika zařízeních pokusy hlasitost kontejnerů na první selhání zařízení dojít ke ztrátě dat vlastnictví. Po selhání tyto hlasitost kontejnery zobrazit nebo s odlišným chováním při zobrazení v portálu Azure klasické. |   | Ano | Ne |
| 8 | Instalace | Během StorSimple adaptér pro instalaci služby SharePoint budete muset zadat IP zařízení v pořadí k úspěšnému dokončení instalace. |  | Ano | Ne |
| 9 | Proxy serveru webové | Pokud konfigurace proxy serveru webové HTTPS jako zadaný protokol, bude to mít vliv komunikace služby zařízení a zařízení budou přesměrovány offline. Podpora balíčků vygeneruje také v procesu jinými významné zdroje na vašem zařízení. | Zajistěte adresu URL proxy HTTP jako zadaný protokol. Další informace o tom, jak [Konfigurovat proxy serveru webové pro svoje zařízení](storsimple-configure-web-proxy.md). | Ano | Ne |
| 10 | Proxy serveru webové | Pokud konfigurace a povolíte proxy serveru webové na registrovaných zařízení, je potřeba restartovat aktivní řadiče na vašem zařízení. |  | Ano | Ne |
| 11 | Vysoký cloudové latence a vysoký pracovní zátěž vstupu a výstupu | Pokud vaše zařízení StorSimple zjistí kombinací čekacích velmi vysoké cloudu dob (pořadí sekund) a maximum pracovní zátěž vstupu a výstupu, svazky zařízení přejděte do jejich funkčnost bude omezena stavu a vstupně-výstupních se nemusí podařit, zobrazí se chyba "zařízení není připraveno". | Je třeba ručně restartujte zařízení řadiče a provedení přepojení zařízení k obnovení ze tato situace. | Ano | Ne |

## <a name="physical-device-updates-in-the-february-release"></a>Uvolněte fyzické zařízení aktualizace z února

Tuto aktualizaci se dá vyřešit problém obnovit factory, ke kterým došlo na zařízení, která vám to upgrade z GA uvolněte verzi října 2014. Neobsahuje žádné další aktualizace StorSimple zařízení.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Sériově připojené řadiče přidružení SCSI (zabezpečení) a firmware aktualizace z února aktualizací

Tato verze neobsahuje nějaké aktualizace řadiče sériově připojené SCSI (přidružení zabezpečení) nebo firmware. Ovladač aktualizace proběhla v říjnu 2014 vydání.  

## <a name="virtual-device-updates-in-the-february-release"></a>Virtuální zařízení aktualizace z února vydání

Tato verze neobsahuje nějaké aktualizace virtuální zařízení. Použili jste tuto aktualizaci nezmění verzi softwaru virtuální zařízení.
 
