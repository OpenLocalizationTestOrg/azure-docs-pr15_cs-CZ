<properties
   pageTitle="Instalace aktualizací 2 na zařízení StorSimple | Microsoft Azure"
   description="Vysvětluje, jak nainstalovat StorSimple 8000 řady aktualizace 2 na vašem zařízení StorSimple 8000 řady."
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

# <a name="install-update-2-on-your-storsimple-device"></a>Instalace aktualizací 2 na StorSimple zařízení

## <a name="overview"></a>Základní informace

Tento kurz vysvětluje, jak nainstalovat aktualizace 2 na zařízení s StorSimple starší verzí software prostřednictvím portálu Azure klasické. Kurz zahrnuje kroky potřebné k aktualizaci brány je nakonfigurovaný na síťovou než DATA 0 StorSimple zařízení a snažíte se aktualizace z verze 1 software před aktualizace.

2 přináší aktualizace softwaru zařízení, LSI ovladač aktualizace a aktualizace firmwaru disku. Software zařízení a LSI aktualizace jsou bez přerušení aktualizace a mohou být použity prostřednictvím portálu Azure klasické. Aktualizace firmwaru disku vyloučit aktualizací a lze použít pouze prostřednictvím rozhraní prostředí Windows PowerShell zařízení.

> [AZURE.IMPORTANT]

> -  Nevidíte aktualizace 2 okamžitě, protože jsme proveďte fázích aktualizace. Vyhledat aktualizace během několika dnů znovu jako tuto aktualizaci budou k dispozici brzy bude k dispozici.
> - Nastavení ručního a automatického před kontroly se dá před nainstalovat stav zařízení z hlediska hardwaru stavu a síťové připojení. Tyto předběžné ověřování pouze v případě použití aktualizací z portálu Microsoft Azure klasické.
> - Doporučujeme instalace softwaru a ovladač aktualizace prostřednictvím stránek portálu Azure klasické. Má jenom přejdete do rozhraní prostředí Windows PowerShell zařízení (aktualizace) Pokud Kontrola brány před aktualizace selže na portálu. Aktualizace může trvat 4-7 hodin nainstalovat (včetně aktualizací Windows). Aktualizace režimu údržby musí být nainstalovaný prostřednictvím rozhraní prostředí Windows PowerShell zařízení. Vyloučit aktualizací jsou aktualizace režimu údržby tyto povede prostoj pro svoje zařízení.
> - Pokud Správce nepovinných snímek StorSimple, upgradovat můžete mít snímek Manager verze aktualizace 2 před aktualizace zařízení.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-the-azure-classic-portal"></a>Nainstalujte aktualizaci 2 prostřednictvím portálu Azure klasické

Proveďte následující kroky k aktualizaci zařízení [aktualizace 2](storsimple-update2-release-notes.md).


> [AZURE.NOTE]
Aktualizace 2 umožňuje společnosti Microsoft pro získání dalších diagnostické informace ze zařízení. Výsledkem je když náš tým operace identifikuje zařízení, která jsou problémy, jsme mohou lépe shromáždění informací ze zařízení a Diagnostika problémů s. Po přijetí aktualizace 2, dovolujete, abychom vám podporují aktivní.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Ověřte, že zařízení běží **StorSimple 8000 řady aktualizace 2 (6.3.9600.17673)**. **Poslední aktualizace data** je také třeba změnit. Zobrazí se vám také, že jsou k dispozici aktualizace režimu údržby (Tato zpráva může nadále zobrazovat až po dobu 24 hodin po instalaci aktualizace).

    Údržby režimu aktualizace jsou vyloučit aktualizace, které za následek prostoje zařízení a lze použít pouze prostřednictvím rozhraní prostředí Windows PowerShell zařízení. V některých případech při spuštění 1.2 aktualizaci firmwaru disku už pravděpodobně aktuální, přičemž nemusíte nainstalujte všechny aktualizace režimu údržbu.

13. Stáhněte si aktualizace režimu údržbu pomocí kroků uvedených v tématu [stažení hotfix](#to-download-hotfixes) moct vyhledat a stáhněte si KB3121899, které instalace aktualizace firmwaru disku (ostatní by již být instalovat aktualizace nyní).

13. Postupujte podle kroků uvedených v [nainstalovat a ověřte údržbu režimu hotfix](#to-install-and-verify-maintenance-mode-hotfixes) instalovat aktualizace režimu údržbu.


## <a name="install-update-2-as-a-hotfix"></a>Nainstalujte aktualizaci 2 oprava

Tento postup použijte, pokud se vám nepovede zaškrtnutí brány při pokusu o instalaci aktualizací pomocí portálu Azure klasické. Kontrola selže a máte brány přiřazená síťové 0-DATA zařízení běží verzi softwaru před aktualizace 1.

Verze softwaru, které lze upgradovat metodou hotfix jsou aktualizace 0,1, aktualizace 0,2 a aktualizace 0,3, aktualizace 1, aktualizace 1.1 a 1.2 aktualizace. Metoda hotfix zahrnuje následující tři kroky:

- Stáhněte si hotfix z katalog služby Microsoft Update.
- Instalace a ověřte hotfix normální režim.
- Instalace a ověřte hotfix režimu údržbu.

Oprava nainstalovat aktualizace 2, stáhněte si a nainstalujte následující hotfix:

| Pořadí  | ZNALOSTNÍ BÁZI KNOWLEDGE BASE        | Popis                    | Typ aktualizace  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3121901 | Aktualizace softwaru         |  Běžná     |
| 2      | KB3121900 | Ovladač LSI              |  Běžná     |
| 3      | KB3080728 | Oprava Storport </br> Windows serveru 2012 R2 |  Běžná     |
| 4      | KB3090322 | Oprava spaceport </br> Windows serveru 2012 R2 |  Běžná     |
| 5      | KB3121899 | Firmware disku           | Údržby  |


> [AZURE.IMPORTANT]
>
> - Pokud vaše zařízení se systémem vydání (GA), kontaktujte prosím [Podporu společnosti Microsoft](storsimple-contact-microsoft-support.md) na pomůžou při aktualizaci.
> - Tento postup je nutné provést jenom jednou použít aktualizace 2. Portál Azure klasické slouží k použití následné aktualizace.
> - Každý instalaci může trvat asi o 20 minut dokončete. Celková instalaci je tomu vašemu nejbližší 2 hodin.
> - Před použitím tohoto postupu pro tuto aktualizaci, ujistěte se, že jsou oba řadiče zařízení online a hardwarové součásti správný.

Provedení následujících kroků můžete použít tuto aktualizaci oprava.

[AZURE.INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]



## <a name="next-steps"></a>Další kroky

Další informace o [aktualizaci 2 vydání](storsimple-update2-release-notes.md).
