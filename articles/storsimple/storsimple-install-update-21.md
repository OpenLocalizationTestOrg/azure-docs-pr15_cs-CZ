<properties
   pageTitle="Instalace aktualizací 2.2 na zařízení StorSimple | Microsoft Azure"
   description="Vysvětluje, jak nainstalovat StorSimple 8000 řady aktualizace 2.2 na vašem zařízení StorSimple 8000 řady."
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
   ms.date="08/02/2016"
   ms.author="alkohli" />

# <a name="install-update-22-on-your-storsimple-device"></a>Instalace aktualizací 2.2 na StorSimple zařízení

## <a name="overview"></a>Základní informace

Tento kurz vysvětluje, jak nainstalovat aktualizace 2.2 na zařízení s StorSimple starší verzí software prostřednictvím portálu Azure klasické a metodou hotfix. Metoda hotfix se používá při bránu v síťovém rozhraní než DATA 0 StorSimple zařízení a snažíte se aktualizace z verze 1 software před aktualizace.

Aktualizace 2.2 obsahuje software zařízení, WMI a iSCSI aktualizace. Pokud aktualizujete z verze 2.1, pouze aktualizace softwaru zařízení muset použije. Pokud aktualizujete z verze 2 před aktualizace, budete taky požádáni použít LSI ovladač Spaceport, Storport a aktualizace firmwaru disku. Zařízení softwaru, WMI, iSCSI, LSI ovladač, Spaceport a Storport opravy bez přerušení aktualizací a mohou být použity prostřednictvím portálu Azure klasické. Aktualizace firmwaru disku vyloučit aktualizací a lze použít pouze prostřednictvím rozhraní prostředí Windows PowerShell zařízení. 

> [AZURE.IMPORTANT]

> - Nastavení ručního a automatického před kontroly se dá před nainstalovat stav zařízení z hlediska hardwaru stavu a síťové připojení. Tyto předběžné ověřování pouze v případě použití aktualizací z portálu Microsoft Azure klasické.
> - Doporučujeme instalace softwaru a ovladač aktualizace prostřednictvím stránek portálu Azure klasické. Má jenom přejdete do rozhraní prostředí Windows PowerShell zařízení (aktualizace) Pokud Kontrola brány před aktualizace selže na portálu. V závislosti na verzi, kterou aktualizujete z aktualizace může trvat 1,5-2,5 hodin nainstalovat. Aktualizace režimu údržby musí být nainstalovaný prostřednictvím rozhraní prostředí Windows PowerShell zařízení. Vyloučit aktualizací jsou aktualizace režimu údržby tyto povede prostoj pro svoje zařízení.
> - Pokud Správce nepovinných snímek StorSimple, upgradovat můžete mít snímek Manager verze 2.2 aktualizace před aktualizace zařízení.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-22-via-the-azure-classic-portal"></a>Nainstalujte aktualizaci 2.2 prostřednictvím portálu Azure klasické

Proveďte následující kroky k aktualizaci zařízení [Aktualizovat 2.2](storsimple-update21-release-notes.md).


> [AZURE.NOTE]
Chcete-li použít aktualizace 2 nebo novější (včetně aktualizace 2.1), budou moct získat další diagnostické informace ze zařízení Microsoft. Výsledkem je když náš tým operace identifikuje zařízení, která jsou problémy, jsme mohou lépe shromáždění informací ze zařízení a Diagnostika problémů s. Po přijetí aktualizace 2 nebo novější, dovolujete, abychom vám podporují aktivní.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Ověřte, že zařízení běží **StorSimple 8000 řady aktualizace 2.2 (6.3.9600.17708)**. **Poslední aktualizace data** je také třeba změnit. 

    Pokud aktualizujete z verze před aktualizace 2, zobrazí se také, že jsou k dispozici aktualizace režimu údržby (Tato zpráva může nadále zobrazovat až po dobu 24 hodin po instalaci aktualizace).

    Údržby režimu aktualizace jsou vyloučit aktualizace, které za následek prostoje zařízení a lze použít pouze prostřednictvím rozhraní prostředí Windows PowerShell zařízení. V některých případech při spuštění 1.2 aktualizaci firmwaru disku už pravděpodobně aktuální, přičemž nemusíte nainstalujte všechny aktualizace režimu údržbu.

    Pokud aktualizujete z aktualizace 2, zařízení by měla aktuální. Můžete přeskočte zbývající kroky.

13. Stáhněte si aktualizace režimu údržbu pomocí kroků uvedených v tématu [stažení hotfix](#to-download-hotfixes) moct vyhledat a stáhněte si KB3121899, které instalace aktualizace firmwaru disku (ostatní by již být instalovat aktualizace nyní).

13. Postupujte podle kroků uvedených v [nainstalovat a ověřte údržbu režimu hotfix](#to-install-and-verify-maintenance-mode-hotfixes) instalovat aktualizace režimu údržbu. 

  

## <a name="install-update-22-as-a-hotfix"></a>Nainstalujte aktualizaci 2.2 oprava

Tento postup použijte, pokud se vám nepovede zaškrtnutí brány při pokusu o instalaci aktualizací pomocí portálu Azure klasické. Kontrola selže a máte brány přiřazená síťové 0-DATA zařízení běží verzi softwaru před aktualizace 1.

Verze software, které lze upgradovat metodou hotfix jsou:

- Aktualizace 0,1, 0,2, 0,3
- Aktualizace 1, 1.1, 1.2
- Aktualizace 2, 2.1 

> [AZURE.IMPORTANT]
>
> - Pokud vaše zařízení se systémem vydání (GA), kontaktujte prosím [Podporu společnosti Microsoft](storsimple-contact-microsoft-support.md) na pomůžou při aktualizaci.

Metoda hotfix zahrnuje následující tři kroky:

- Stáhněte si hotfix z katalog služby Microsoft Update.
- Instalace a ověřte hotfix normální režim.
- Instalace a ověřte hotfix režimu údržby (pouze při aktualizaci z před aktualizace 2).

#### <a name="download-updates-for-a-device-running-update-21-software"></a>Stáhněte si aktualizace pro zařízení softwarem 2.1 aktualizace

**Pokud vaše zařízení běží aktualizovat 2.1**, budete muset stahovat jenom zařízení aktualizace KB3179904. Pouze instalace binární soubor začíná "všechny hcsmdssoftwareudpate". Nelze nainstalovat Cis a update agent strategie minimálního stahování začíná `all-cismdsagentupdatebundle`. Chyba při k tomu nevyzve bude výsledkem chyba. Jedná se o bez přerušení aktualizaci, nebude přerušení vstupu a výstupu a zařízení nebudou mít všechny výpadek služeb.


#### <a name="download-updates-for-a-device-running-update-2-software"></a>Stáhněte si aktualizace pro zařízení softwarem aktualizace 2

**Pokud vaše zařízení běží aktualizace 2**, je třeba stáhnout a nainstalovat následující hotfix v předepsaném pořadí:

| Pořadí  | ZNALOSTNÍ BÁZI KNOWLEDGE BASE        | Popis                    | Typ aktualizace  | Instalace času |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3179904 | Aktualizace softwaru & #42;  |  Běžná <br></br>Bez přerušení     | ~ 45 minut |
| 2.      | KB3146621 | balíček iSCSI | Běžná <br></br>Bez přerušení  | ~ 20 minut |
| 3.      | KB3103616 | WMI balíčku |  Běžná <br></br>Bez přerušení      | ~ 12 minut |


 & #42;  Aktualizace *osobní zprávu, software se skládá ze dvou binární soubory: aktualizace softwaru zařízení začíná `all-hcsmdssoftwareupdate` a Cis a strategie minimálního stahování agent začíná `all-cismdsagentupdatebundle`. Aktualizace softwaru zařízení musí být nainstalovaný před agent Cis a strategie minimálního stahování. I po restartování aktivní řadiče prostřednictvím `Restart-HcsController` aktualizovat rutina po použití agent Cis a strategie minimálního stahování (a před použitím zbývající aktualizuje).* 

#### <a name="download-updates-for-a-device-running-pre-update-2-software"></a>Stáhněte si aktualizace pro zařízení 2 softwarem před aktualizace

**Pokud vaše zařízení běží verze 0,2, 0,3, 1.0 a 1.1**, je třeba stáhnout a nainstalovat LSI ovladačů aktualizace kromě software, iSCSI a WMI. Pokud používáte aktualizovat 1.2 nebo 2 je už nainstalovaný tuto aktualizaci. 
 
| Pořadí  | ZNALOSTNÍ BÁZI KNOWLEDGE BASE        | Popis                    | Typ aktualizace  | Instalace času |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3121900 | Ovladač LSI a firmware             |  Běžná <br></br>Bez přerušení      | ~ 20 minut |


<br></br>
**Pokud vaše zařízení běží verze 0,2, 0,3, 1.0, 1.1 a 1.2**, třeba stáhnout a nainstalovat Spaceport a oprava Storport. Pokud používáte aktualizace 2 jsou už nainstalované tyto.

| Pořadí  | ZNALOSTNÍ BÁZI KNOWLEDGE BASE        | Popis                    | Typ aktualizace  | Instalace času |
|--------|-----------|-------------------------|------------- |-------------|
| 5.      | KB3090322 | Oprava spaceport </br> Windows serveru 2012 R2 |  Běžná <br></br>Bez přerušení      | ~ 20 minut |
| 6.      | KB3080728 | Oprava Storport </br> Windows serveru 2012 R2 |  Běžná <br></br>Bez přerušení      | ~ 20 minut |



<br></br>
Taky budete muset nainstalovat aktualizaci firmwaru disku. Můžete ověřit, jestli potřebujete aktualizace firmwaru disku spuštěním `Get-HcsFirmwareVersion` rutiny. Pokud používáte tyto verze firmwaru: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, není potřeba nainstalovat tyto aktualizace a pak.


| Pořadí  | ZNALOSTNÍ BÁZI KNOWLEDGE BASE        | Popis                    | Typ aktualizace  | Instalace času |
|--------|-----------|-------------------------|------------- |-------------|
| 7.      | KB3121899 | Firmware disku              |  Údržby <br></br>Vyloučit      | ~ 30 minut |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Tento postup je nutné provést jenom jednou použít 2.2 aktualizace. Portál Azure klasické slouží k použití následné aktualizace.
> - Pokud aktualizujete z aktualizace 2, celkové instalaci je tomu vašemu nejbližší 1,5 hodin.
> - Před použitím tohoto postupu můžete použít aktualizovat, aby byli řadiče zařízení online a hardware součásti jsou funkční.

Provedení následujících kroků můžete stáhnout a nainstalovat hotfix.

[AZURE.INCLUDE [storsimple-install-update21-hotfix](../../includes/storsimple-install-update21-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Další kroky

Další informace o [aktualizaci 2.1 vydání](storsimple-update21-release-notes.md).
