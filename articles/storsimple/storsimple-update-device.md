<properties
   pageTitle="Aktualizovat StorSimple zařízení | Microsoft Azure"
   description="Vysvětluje, jak používat funkci StorSimple aktualizace pro instalaci aktualizace režimu běžné a údržba a hotfix."
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
   ms.date="06/28/2016"
   ms.author="v-sharos" />

# <a name="update-your-storsimple-8000-series-device"></a>Aktualizovat řady 8000 StorSimple zařízení

## <a name="overview"></a>Základní informace

Funkce StorSimple aktualizace umožňují snadno pravidelná aktualizace StorSimple zařízení. V závislosti na typu aktualizace jehož aplikováním aktualizace na zařízení prostřednictvím portálu Azure klasické nebo pomocí rozhraní prostředí Windows PowerShell. Tento kurz popisuje typy aktualizace a jak nainstalovat každý z nich.

Můžete použít dva typy aktualizace zařízení: 

- Běžná (neboli normální režim) aktualizace
- Aktualizace režimu údržby

Nainstalování běžná aktualizace prostřednictvím stránek Azure klasické portál nebo prostředí Windows PowerShell; musíte se však pomocí prostředí Windows PowerShell instalace aktualizací režimu údržbu. 

Každý typ aktualizace je popsán samostatně, pod.

### <a name="regular-updates"></a>Pravidelné aktualizace

Pravidelné aktualizace jsou bez přerušení aktualizace, které lze nainstalovat, pokud je zařízení do normálního režimu. Tyto aktualizace jsou použity prostřednictvím webu Microsoft Update každého řadiče zařízení. 

> [AZURE.IMPORTANT] Přepojení zařízení může dojít během procesu aktualizace. Tento postup však neovlivní dostupnost systému nebo operace.

- Podrobnosti o tom, jak nainstalovat běžná aktualizace prostřednictvím stránek portálu Azure klasické najdete v tématu [instalovat běžná aktualizace prostřednictvím Azure klasické portal(#install-regular-updates-via-the-azure-classic-portal).

- Můžete taky nainstalujete běžná aktualizace prostřednictvím stránek prostředí Windows PowerShell pro StorSimple. Další informace najdete v tématu [instalace běžná aktualizace prostřednictvím stránek prostředí Windows PowerShell pro StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Aktualizace režimu údržby

Údržby režimu aktualizace jsou vyloučit aktualizace například upgrady firmwaru disku. Tyto aktualizace vyžadují zařízení přepněte do režimu údržby. Podrobnosti najdete v tématu [Krok 2: režim zadejte údržby](#step2). Instalace aktualizací režimu údržby nelze používat portál Azure klasické. Místo toho třeba použijete prostředí Windows PowerShell pro StorSimple. 

Podrobnosti o tom, jak instalovat aktualizace režimu údržbu najdete v tématu [instalace údržbu režimu aktualizace prostřednictvím stránek prostředí Windows PowerShell pro StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [AZURE.IMPORTANT] Aktualizace režimu údržby je třeba opatřit samostatně každý řadiče domény. 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Běžná aktualizace prostřednictvím portálu Azure klasické

Portál Azure klasické slouží k použití aktualizací StorSimple zařízení.

[AZURE.INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Instalace běžná aktualizace prostřednictvím stránek prostředí Windows PowerShell pro StorSimple

Můžete taky můžete prostředí Windows PowerShell pro StorSimple použití aktualizací běžné (normálního režimu).

> [AZURE.IMPORTANT] Ačkoli je možné nainstalovat pravidelné aktualizace pomocí Windows Powershellu pro StorSimple, důrazně doporučujeme nainstalovat pravidelné aktualizace prostřednictvím portálu Azure klasické. Začínající Update 1, před kontroly se provedou před instalací aktualizace z portálu. Tyto předběžné kontroly mají prioritu před selháním a zrušte zaškrtnutí zjednodušují prostředí. 

[AZURE.INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Instalace aktualizací režimu údržbu pomocí Windows Powershellu pro StorSimple

Použití režimu aktualizací údržba zařízení StorSimple pomocí Windows Powershellu pro StorSimple. Pozastaví se všechny požadavky v/v v tomto režimu. Služeb, jako je paměti RAM stálé (NVRAM) nebo službu clusterů taky ukončit. Obě řadiče jsou restartování když zadáte nebo ukončit tento režim. Po ukončení tohoto režimu všechny služby bude pokračovat a by měl být správný. (To může trvat několik minut.)

Pokud budete potřebovat použít údržbu režimu aktualizace, zobrazí se upozornění prostřednictvím portálu Azure klasické, jestli máte aktualizace, které je třeba nainstalovat. Toto oznámení bude obsahovat pokyny k používání Windows Powershellu pro StorSimple instalovat aktualizace. Po aktualizaci zařízení umožňuje stejný postup, který se změní zařízení normální režim. Podrobné pokyny najdete v tématu [Krok 4: režim konec údržby](#step4).

> [AZURE.IMPORTANT] 
> 
> - Před zadáním režimu údržby, ověřte, jestli jsou oba řadiče zařízení správný kontrolou **Stavu hardwaru** na stránce **Údržba** Azure klasické portálu. Pokud správce není v pořádku, kontaktujte Microsoft Support k dalším krokům. Další informace přejděte na kontaktovat podporu Microsoftu. 
> - Když jste v režimu údržby, musíte nejdřív nainstalovat aktualizaci na jeden řadiče domény a potom na jiné zařízení.

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Krok 1: Připojení ke konzole sériové<a name="step1">

Nejdřív pomocí aplikace například nátěrové ke konzole sériové. Následující postup vysvětluje, jak používat nátěrové pro připojení ke konzole sériové.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Krok 2: Zadejte režimu údržby<a name="step2">

Po připojení konzoly zjistit, jestli jsou aktualizace nainstalovat a údržba do režimu a nainstalujte je.

[AZURE.INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Krok 3: Aktualizace<a name="step3">

Pak instalovat aktualizace.

[AZURE.INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]
 
### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Krok 4: Údržbu ukončení režimu<a name="step4">

Nakonec ukončete režim na údržbu.

[AZURE.INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Instalace hotfix prostřednictvím prostředí Windows PowerShell pro StorSimple

Na rozdíl od aktualizace pro Microsoft Azure StorSimple hotfix nainstalovaných ve sdílené složce. Stejně jako u aktualizací, existují dva typy hotfix: 

- Běžná hotfix 
- Režim hotfix údržby  

Následující postupy popisují, jak používat Windows PowerShell pro StorSimple pro instalaci běžné a oprav údržbu režim pro.

[AZURE.INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[AZURE.INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Co se stane s aktualizacemi provést factory resetovat zařízení?

Pokud je zařízení obnovit tovární nastavení, všechny aktualizace budou ztraceny. Zařízení factory obnovit registrované a nakonfigurovali, musíte ruční instalace aktualizací pomocí Azure klasické portál a/nebo prostředí Windows PowerShell pro StorSimple. Další informace o factory obnovení najdete v tématu [resetujte tovární výchozí nastavení](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Další kroky

- Další informace o [používání Windows Powershellu pro StorSimple ke správě StorSimple zařízení](storsimple-windows-powershell-administration.md).
- Další informace o [použití služby StorSimple správce ke správě StorSimple zařízení](storsimple-manager-service-administration.md).
