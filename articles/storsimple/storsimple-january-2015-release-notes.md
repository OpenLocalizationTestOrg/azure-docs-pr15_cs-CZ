<properties 
   pageTitle="Aktualizace 8000 StorSimple 0,2 poznámky k verzi | Microsoft Azure"
   description="Popisuje nové funkce a opravy, otevřené problémy a k dispozici řešení z ledna 2015 verzi Microsoft Azure StorSimple (aktualizace 0,2)."
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
   ms.date="05/16/2016"
   ms.author="v-sharos" />


# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>Aktualizace řady 8000 StorSimple 0,2 poznámky - ledna 2015

## <a name="overview"></a>Základní informace

Následující poznámky k verzi popsána kritické otevřené problémy ledna 2015 verzi aplikace Microsoft Azure StorSimple. Obsahují také seznam StorSimple software a firmware aktualizace zahrnuté v této verzi. Toto je druhý vydání po StorSimple 8000 řady vydanou verzi byl zavedený obecně červenec 2014.
  
Tuto aktualizaci nezmění verzi softwaru fyzické zařízení z října aktualizace. Ho nadále verze 6.3.9600.17312. Obrázek použitý obrázek virtuální zařízení změnil v této verzi. Proto všechny nové virtuální zařízení vytvořených po 1/20/2015 se zobrazí ve verzi jako 6.3.9600.17361.  

Zkontrolujte následující informace obsažené v poznámky k verzi pro aktualizaci ledna 2015.

> [AZURE.IMPORTANT]  
>
>- Tato aktualizace není k dispozici prostřednictvím služby Windows Update a nejde nainstalovat, jako je další aktualizace. Zařízení s nebudou tuto aktualizaci, i když jste nainstalovali aktualizace tak, že na portálu Azure klasické. Tuto aktualizaci se vztahuje pouze na virtuální zařízení vytvořené po 20 ledna 2015. 
> 
>- Leden verzi StorSimple neobsahuje nějaké aktualizace zařízení fyzicky StorSimple. Můžete také použít všechny dostupné aktualizace Windows virtuální zařízení, včetně poslední bezpečnostní opravy, ale neuvidíte změny ve verzi pro zařízení fyzicky StorSimple.

## <a name="whats-new-in-the-january-release"></a>Co je nového ve verzi leden

Tato aktualizace obsahuje oprava související svazky přechodu do offline režimu virtuálního zařízení. (Viz [problémy s pevnou v této verzi](#issues-fixed-in-the-january-release)).  

Aktualizace neobsahuje nové funkce a funkce.  

## <a name="issues-fixed-in-the-january-release"></a>Problémy s pevnou ve verzi leden

Následující tabulka popisuje problém, který byl odstranit tuto aktualizaci.

|Ne.|Funkce|Problém|Platí pro fyzické zařízení|Platí pro virtuální zařízení 
|---|-------|-----|--------------------------|-------------------------
|1|Svazky přechodu do offline režimu|Když čekacích vysoký cloudové dob potrvají několik minut, přejdete na hostiteli offline svazky StorSimple virtuální zařízení. Tento opravný nástroj zvyšuje mezní cloudu čekacím tím minimalizace situací, které způsobují svazky do offline režimu u hostitele.|Ne|Ano  

## <a name="known-issues-in-the-january-release"></a>Známé problémy v od vydání

Následující tabulka obsahuje souhrn známé problémy v této verzi.
 
|Ne.|Funkce|Problém|Komentáře a řešení|Platí pro fyzické zařízení|Platí pro virtuální zařízení 
|---|-------|-----|-------------------|--------------------------|-------------------------
|1| Obnovení Factory|  V některých případech při provádění factory resetovat zařízení StorSimple může být pomalé a zobrazit tato zpráva: **Obnovit factory probíhá (fáze 8).** Tím se stane, když probíhá rutinu stiskněte kombinaci kláves CTRL + C.| Není stisknutím klávesy CTRL + C po zahájení factory obnovit. Pokud už jste tento stav, obraťte se prosím Microsoft Support k dalším krokům.|Ano|Ne|
|2|Disk kvora| Výjimečně Pokud většinou disků v EBOD přílohu 8600 zařízení jsou to od ní odpojilo výsledkem bez disku kvora fondu úložiště bude offline. Zůstane v režimu offline, i když disků připojeni.|Je třeba Restartujte zařízení. Pokud potíže potrvají, obraťte se prosím Microsoft Support k dalším krokům.|Ano |Ne
|3|Selhání snímek cloudu|Výjimečně může selhat cloudu snímek s chybou **maximální limit záložní dosažení**. K tomu dojde, pokud překročit 255 online klony na stejné zařízení ze stejného objemem původní, který byl odstraněn.||Ano|Ano
|4|ID nesprávné zařízení|Při provádění náhradní řadiče domény řadiče domény 0 může zobrazit jako řadiči 1. Během řadiče náhradní při načtení obrázek z uzel mezi dvěma účastníky ID řadiče zobrazit původně jako ID zařízení mezi dvěma účastníky.  Výjimečně toto chování mohou také projeví po restartování systému.|Není nutná žádná akce uživatele. Tato situace vyřeší samotné po dokončení náhradní řadiče domény.|Ano|Ne 
|5| Zařízení sledování grafy|Ve službě StorSimple správce sledování grafy zařízení nefunguje Basic nebo ověřování NTLM je povolen v konfiguraci proxy serveru pro zařízení.|Změna konfigurace proxy web pro zařízení registrovaný u StorSimple Správce služby tak, že ověření nastavíte na žádný. K tomuto účelu spustit prostředí Windows PowerShell pro rutinu StorSimple Set-HcsWebProxy.|Ano|Ano
|6| Účty úložiště|Odstranění účtu úložiště pomocí služba úložiště je nepodporované funkce. To vede k situaci, ve kterém nelze uživatelská data načíst.|| Ano |  Ano
|7|Převzetí zařízení| Více předávání služeb při selhání kontejneru hlasitosti na stejné zařízení zdroje pro různé cílové zařízení není podporovaná.| Přepnutí z jednoho nefunkční zařízení několika zařízeních pokusy hlasitost kontejnerů na první selhání zařízení dojít ke ztrátě dat vlastnictví. Po selhání tyto hlasitost kontejnery zobrazit nebo s odlišným chováním při zobrazení v portálu Azure klasické.|Ano|Ne
|8| Instalace|Během StorSimple adaptér pro instalaci služby SharePoint budete muset zadat IP zařízení v pořadí k úspěšnému dokončení instalace.||Ano|Ne
|9| Proxy serveru webové|Pokud konfigurace proxy serveru webové HTTPS jako zadaný protokol, bude to mít vliv komunikace služby zařízení a zařízení budou přesměrovány offline. Podpora balíčků vygeneruje také v procesu jinými významné zdroje na vašem zařízení.|Zajistěte adresu URL proxy HTTP jako zadaný protokol. Přečtěte si další informace o tom, jak [Konfigurovat proxy serveru webové pro svoje zařízení](storsimple-configure-web-proxy.md).|Ano |Ne
|10|Proxy serveru webové|  Pokud konfigurace a povolíte proxy serveru webové na registrovaných zařízení, je potřeba restartovat aktivní řadiče na vašem zařízení.|| Ano |Ne
|11|Vysoký cloudové latence a vysoký pracovní zátěž vstupu a výstupu|Pokud vaše zařízení StorSimple zjistí kombinací čekacích velmi vysoké cloudu dob (pořadí sekund) a maximum pracovní zátěž vstupu a výstupu, svazky zařízení přejděte do jejich funkčnost bude omezena stavu a vstupně-výstupních se nemusí podařit, zobrazí se chyba "zařízení není připraveno".|Je třeba ručně restartujte zařízení řadiče a provedení přepojení zařízení k obnovení ze tato situace.|Ano|Ne

## <a name="physical-device-updates-in-the-january-release"></a>Aktualizace fyzické zařízení v leden

Tato aktualizace neobsahuje nějaké další změny StorSimple zařízení.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Sériově připojené řadiče přidružení SCSI (zabezpečení) a aktualizace firmwaru v leden aktualizací

Tato verze neobsahuje nějaké aktualizace řadiče sériově připojené SCSI (přidružení zabezpečení) nebo firmware. Ovladač aktualizace proběhla v říjnu 2014 vydání. 

## <a name="virtual-device-updates-in-the-january-release"></a>Aktualizace virtuální zařízení v leden

Tato verze obsahuje aktualizované obrázek virtuální zařízení. Virtuální zařízení vytvořené po 20 ledna 2015 se zobrazí jako 6.3.9600.17361 verzi softwaru.

 
