<properties 
    pageTitle="Použití oprávnění root na virtuálních počítačích Linux | Microsoft Azure" 
    description="Naučte se používat oprávnění root na počítač virtuální Linux v Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>


# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Pomocí oprávnění root na virtuálních počítačích Linux v Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Ve výchozím nastavení `root` uživatele je zapnuta Linux virtuálních počítačích v Azure. Uživatelé, můžete spustit příkazy zvýšenými oprávněními pomocí `sudo` příkaz. Však možnosti se může lišit podle toho, jak systém protože ji někdo zřídil.

1. **SSH spolu s heslo nebo heslo jenom** - virtuální počítač protože ji někdo zřídil buď certifikátem (`.CER` souboru) nebo klíč SSH i heslo, nebo jenom uživatelské jméno a heslo. V tomto případě `sudo` vás vyzve k heslo uživatele před spuštěním příkazu.

2. **Pouze klíč SSH** - virtuální počítač byl zřízení pomocí certifikátu (`.cer`, `.pem`, nebo `.pub` souboru) nebo SSH klíče, ale žádné heslo.  V tomto případě `sudo` **nejsou** nabídnout výběr heslo uživatele před spuštěním příkazu.


## <a name="ssh-key-and-password-or-password-only"></a>SSH klíče a heslo nebo heslo jenom

Přihlaste se k virtuálního počítače Linux ověřování SSH klíč nebo heslo, a potom použijte příkazy pomocí `sudo`, například:

    # sudo <command>
    [sudo] password for azureuser:

V tomto případě se uživateli výzva k zadání hesla. Po zadání hesla `sudo` se spustí příkaz s `root` oprávnění.

Můžete taky povolit passwordless sudo úpravou `/etc/sudoers.d/waagent` souborů, například:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Tato změna umožní passwordless sudo uživatelem "azureuser".

## <a name="ssh-key-only"></a>SSH pouze klíče

Přihlaste se k Linux virtuálního počítače pomocí SSH klíčové ověřování, a potom použijte příkazy pomocí `sudo`, například:

    # sudo <command>

V tomto případě uživatel **není** výzva k zadání hesla. Po stisknutí `<enter>`, `sudo` se spustí příkaz s `root` oprávnění.

 
