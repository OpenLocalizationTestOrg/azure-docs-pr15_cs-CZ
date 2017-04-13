<properties
   pageTitle="Instalace aktualizací 1.2 na zařízení StorSimple | Microsoft Azure"
   description="Vysvětluje, jak nainstalovat StorSimple 8000 řady aktualizace 1.2 na vašem zařízení StorSimple 8000 řady."
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
   ms.date="08/22/2016"
   ms.author="alkohli" />

# <a name="install-update-12-on-your-storsimple-device"></a>Instalace aktualizací 1.2 na StorSimple zařízení

## <a name="overview"></a>Základní informace

Tento kurz vysvětluje, jak nainstalovat aktualizace 1.2 na StorSimple zařízení, na kterém běží verzi softwaru před aktualizace 1. Kurz zahrnuje další kroky k aktualizaci při konfiguraci brány na síťovou než DATA 0 StorSimple zařízení.

Aktualizace 1.2 obsahuje aktualizace softwaru zařízení, aktualizace ovladačů LSI a aktualizace firmwaru disku. Software a aktualizace ovladačů LSI bez přerušení aktualizací a mohou být použity prostřednictvím portálu Azure klasické. Aktualizace firmwaru disku vyloučit aktualizací a lze použít pouze prostřednictvím rozhraní prostředí Windows PowerShell zařízení.

Podle toho, jakou verzi zařízení pracuje, můžete určit, pokud se použije 1.2 aktualizace. Můžete zkontrolovat, že verzi softwaru zařízení tak, že přejdete na **první pohled dával** část zařízení **řídicího panelu**.

</br>

| Pokud verzi softwaru...   | Co se stane v portálu?                              |
|---------------------------------|--------------------------------------------------------------|
| Uvolnění - GA                    | Pokud jste se verzí (GA), nebudou mít vliv tuto aktualizaci. Ujistěte se, [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) a aktualizovat zařízení.|
| Aktualizace 0,1                      | Portál platí 1.2 aktualizace.                                |
| Aktualizace 0,2                      | Portál platí 1.2 aktualizace.                                |
| Aktualizace 0,3                      | Portál platí 1.2 aktualizace.                                |
| Aktualizace 1                        | Tuto aktualizaci nebudou k dispozici.                           |
| Aktualizace 1.1                      | Tuto aktualizaci nebudou k dispozici.                           |

</br>

> [AZURE.IMPORTANT]

> -  Nevidíte aktualizace 1.2 okamžitě, protože jsme proveďte fázích aktualizace. Vyhledat aktualizace během několika dnů znovu jako tuto aktualizaci budou k dispozici brzy bude k dispozici.
> - Tato aktualizace obsahuje sadu kontrol ručního a automatického před stav zařízení z hlediska hardwaru stavu a síťové připojení. Tyto předběžné ověřování pouze v případě použití aktualizací z portálu Microsoft Azure klasické.
> - Doporučujeme instalace softwaru a ovladač aktualizace prostřednictvím stránek portálu Azure klasické. Má jenom přejdete do rozhraní prostředí Windows PowerShell zařízení (aktualizace) Pokud Kontrola brány před aktualizace selže na portálu. Aktualizace může trvat 5 až 10 hodin nainstalovat (včetně aktualizací Windows). Aktualizace režimu údržby musí být nainstalovaný prostřednictvím rozhraní prostředí Windows PowerShell zařízení. Vyloučit aktualizací jsou aktualizace režimu údržby tyto povede prostoj pro svoje zařízení.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Nainstalujte aktualizaci 1.2 prostřednictvím portálu Azure klasické

Proveďte následující kroky k aktualizaci zařízení [Aktualizovat 1.2](storsimple-update1-release-notes.md). Tento postup použijte, pouze pokud máte brány nakonfigurovaný na dat 0 síťové na vašem zařízení.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Ověřte, že zařízení běží **StorSimple 8000 řady aktualizace 1.2 (6.3.9600.17584)**. **Poslední aktualizace data** je také třeba změnit. Zobrazí se vám také, že jsou k dispozici aktualizace režimu údržby (Tato zpráva může nadále zobrazovat až po dobu 24 hodin po instalaci aktualizace).

    Údržby režimu aktualizace jsou vyloučit aktualizace, které za následek prostoje zařízení a lze použít pouze prostřednictvím rozhraní prostředí Windows PowerShell zařízení.

    ![Stránky údržby] (./media/storsimple-install-update-1/InstallUpdate12_10M.png "Stránky údržby")

13. Stáhněte si aktualizace režimu údržbu pomocí kroků uvedených v tématu [stažení hotfix]( #to-download-hotfixes) moct vyhledat a stáhněte si KB3063416, které instalace aktualizace firmwaru disku (ostatní by již být instalovat aktualizace nyní).

13. Postupujte podle kroků uvedených v [nainstalovat a ověřte údržbu režimu hotfix](#to-install-and-verify-maintenance-mode-hotfixes) instalovat aktualizace režimu údržbu.

14. Na portálu Azure klasické přejděte na stránku **údržby** a v dolní části stránky, klikněte na **Zkontrolovat aktualizace** pro všechny aktualizace Windows a potom klikněte na **Instalovat aktualizace**. Dokončení po použití aktualizací úspěšně nainstalovaný.



## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Instalace aktualizací 1.2 na zařízení, které má brána nakonfigurován pro rozhraní 0 sítě bez dat

Tento postup byste měli použít jenom v případě, že při pokusu o instalaci aktualizací pomocí portálu Azure klasické se nepovede zaškrtnutí brány. Kontrola selže a máte brány přiřazená síťové 0-DATA zařízení běží verzi softwaru před aktualizace 1. Pokud vaše zařízení nemá brány na síťové 0-DATA, můžete aktualizovat zařízení přímo z portálu Microsoft Azure klasické. Najdete v tématu [instalace aktualizovat 1.2 prostřednictvím portálu Azure klasické](#install-update-1.2-via-the-azure-classic-portal).

Verze softwaru, které lze upgradovat tímto způsobem jsou aktualizace 0,1, aktualizace 0,2 a aktualizace 0,3.


> [AZURE.IMPORTANT]
>
> - Pokud vaše zařízení se systémem vydání (GA), kontaktujte prosím [Podporu společnosti Microsoft](storsimple-contact-microsoft-support.md) na pomůžou při aktualizaci.
> - Tento postup je nutné provést jenom jednou použít 1.2 aktualizace. Portál Azure klasické slouží k použití následné aktualizace.

Pokud vaše zařízení běží před aktualizace 1 a má brána nastavení pro rozhraní sítě než DATA 0, jehož aplikováním 1.2 aktualizace v následujících dvou způsobů:

- **Možnost 1**: Stáhněte si aktualizaci a použití pomocí `Start-HcsHotfix` rutina z rozhraní prostředí Windows PowerShell zařízení. Toto je doporučený postup. **Nepoužívejte tento způsob použít aktualizace 1.2, pokud vaše zařízení se systémem 1.0 aktualizace nebo aktualizace 1.1.**

- **Možnost 2**: odebrání konfiguraci brány a nainstalujte aktualizaci přímo z portálu Microsoft Azure klasické.


Podrobné pokyny pro každou z nich jsou uvedeny v následujících částech.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>Možnost 1: Pomocí Windows Powershellu pro StorSimple použít aktualizace 1.2 oprava

Tento postup byste měli použít jenom v případě, že používáte aktualizace 0,1, 0,2, 0,3 a pokud váš bráně selhalo při pokusu o instalaci aktualizací z portálu Microsoft Azure klasické. Pokud používáte software vydání (GA), zadejte [Podpory společnosti Microsoft](storsimple-contact-microsoft-support.md) a aktualizovat zařízení.

Nainstalujte aktualizaci 1.2 oprava, stáhněte si a nainstalujte následující hotfix:

| Pořadí  | ZNALOSTNÍ BÁZI KNOWLEDGE BASE        | Popis             | Typ aktualizace  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3063418 | Aktualizace softwaru         |  Běžná     |
| 2      | KB3043005 | Aktualizace řadiče přidružení LSI zabezpečení |  Běžná     |
| 3      | KB3063416 | Firmware disku           | Údržby  |

Před použitím tohoto postupu pro tuto aktualizaci, ujistěte se, že:

- Obě řadiče zařízení jsou online.

Proveďte následující postup použít 1.2 aktualizace. **Aktualizace může trvat zhruba 2 hodin dokončete (asi 30 minut softwaru, 30 minut ovladače 45 minut u firmware disku).**

[AZURE.INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]


## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>Možnost 2: Použití pomocí portálu Azure klasické aktualizace 1.2 po odebrání konfiguraci brány

Tento postup platí pouze pro StorSimple zařízení používáte verzi softwaru před aktualizace 1 mít brány nastavení sítě rozhraní než DATA 0. Je třeba zrušte nastavení brány před použitím aktualizace.

Aktualizace může trvat několik hodin. Pokud váš hostitel se nacházejí v různých podsítí, odebrání konfiguraci brány na rozhraní iSCSI může mít za následek výpadek služeb. Doporučujeme konfigurace DATA 0 přenosů iSCSI zmenšit prostoje.

Provedení následujících kroků můžete zakázat rozhraní sítě s brány a pak nainstalovat aktualizaci.

[AZURE.INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]


## <a name="next-steps"></a>Další kroky

Další informace o [verzi 1.2 aktualizace](storsimple-update1-release-notes.md).
