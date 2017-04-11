<properties
    pageTitle="Přeinstalujte Azure zásobníku | Microsoft Azure"
    description="Přeinstalujte Azure vrstvě."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="erikje"/>

# <a name="redeploy-azure-stack"></a>Přeinstalujte Azure zásobníku

Pokud chcete přeinstalujte zásobníku Azure, musíte začít přes zcela podle níže uvedeného popisu.

## <a name="steps-to-redeploy-azure-stack"></a>Postup přeinstalujte Azure zásobníku

1. Restartujte hostiteli do původní operačního systému (nainstalovaný úplné obnovení). Toto není výchozím nastavením v nabídce spuštění, je nutné použít KVM nebo místní konzoly vyberte během restartování počítače (během instalace, můžete s názvem "spuštění z virtuální pevný disk" OS na "AzureStack TP2", tím vám pomůže zjistit, které je OS).

    Nemusíte odebrat stávající spouštěcí (nové podpory skript, který "PrepareBootFromVHD.ps1" má na starosti, které za vás.)

2. Pokud nemáte KVM nebo byste chtěli zvolte OS spouštěcí restartování:
    
    1. Vyhledejte.\BootMenuNoKVM.ps1 skriptu. Tento soubor je k dispozici další podporu skripty obsažených v této Tvůrce dotazů.
    
    2. Spusťte skript se zvýšenými oprávněními. Vyberte název původní hostitelského operačního systému. Hostiteli do původní hostitelského operačního systému tím se spustí bez nutnosti KVM přístup.
    
    3. Po dokončení skript se zobrazí výzva k potvrzení restartování počítače.

    - Pokud jsou jiní uživatelé přihlášeni, tento příkaz se nezdaří.

    - Spusťte tento příkaz jenom: restartování počítače – vynucení 
 
3. Odstranění CloudBuilder.vhdx soubor, který byl použit jako součást předchozí nasazení.

    Není potřeba odstranit existující fondu úložiště z předchozí TP2 nasazení. Skript pro nasazení rozpozná a vyčistí stávající a potom vytvoří nový.

5. Přeinstalujte v kopírování novou kopii CloudBuilder.vhdx, spusťte ho atd.

## <a name="next-steps"></a>Další kroky

[Připojení k Azure zásobníku](azure-stack-connect-azure-stack.md)
