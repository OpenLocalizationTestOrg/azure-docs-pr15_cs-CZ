<properties
        pageTitle="Obnovení hesla Linux OM a SSH klíč z rozhraní příkazového řádku | Microsoft Azure"
        description="Jak používat rozšíření VMAccess z Azure rozhraní příkazového řádku (rozhraní příkazového řádku) resetovat heslo Linux OM nebo SSH klíč, opravte konfigurace SSH a kontrola konzistence disku"
        services="virtual-machines-linux"
        documentationCenter=""
        authors="cynthn"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="06/14/2016"
        ms.author="cynthn"/>

# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Jak resetovat heslo Linux OM nebo SSH klíč, opravte konfigurace SSH a kontrola konzistence disku s příponou VMAccess


Pokud nemůže připojit k počítači virtuální Linux v Azure kvůli zapomenuté heslo nesprávné klávesy zabezpečené prostředí (SSH), nebo na problém s konfigurací SSH pomocí koncovku VMAccessForLinux rozhraní příkazového řádku Azure resetovat heslo nebo SSH klíč, opravte konfigurace SSH a kontrola konzistence disku. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky pomocí modelu správce prostředků](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

S Azure rozhraní příkazového řádku můžete pomocí příkazu **azure OM rozšíření nastavení** z příkazového řádku rozhraní (flám, terminálu, příkazový) pro přístup k příkazům. Spusťte **rozšíření OM azure nápovědy nastavení** pro použití podrobné rozšíření.

S Azure rozhraní příkazového řádku můžete udělat následující úkoly:

+ [Resetování hesla](#pwresetcli)
+ [Obnovit SSH klíč](#sshkeyresetcli)
+ [Resetování hesla a klávesu SSH](#resetbothcli)
+ [Vytvořit nový uživatelský účet sudo](#createnewsudocli)
+ [Obnovení konfigurace SSH](#sshconfigresetcli)
+ [Odstraněním uživatelského účtu](#deletecli)
+ [Zobrazit stav koncovku VMAccess](#statuscli)
+ [Kontrola konzistence přidané disků](#checkdisk)
+ [Oprava přidané disků na Linux OM](#repairdisk)


## <a name="prerequisites"></a>Zjistit předpoklady pro

Budete potřebovat provést následující akce:

- Musíte si [nainstalovat Azure rozhraní příkazového řádku](../xplat-cli-install.md) a [Připojit se ke svému předplatnému](../xplat-cli-connect.md) Azure články přidruženého k vašemu účtu.
- Nastavte správné režim pro klasické nasazení modelu zadáním následujícího příkazového řádku:
        
        azure config mode asm
        
- Pokud chcete obnovit jedné máte nové heslo nebo sadu SSH kláves. Pokud chcete obnovit konfigurace SSH tyto nepotřebujete.


## <a name="pwresetcli"></a>Resetování hesla

1. Vytvoření souboru s názvem PrivateConf.json tyto řádky. Nahrazení hranatých závorkách a & #60; zástupný text & #62; hodnoty vlastními informacemi.

        {
        "username":"<currentusername>",
        "password":"<newpassword>",
        "expiration":"<2016-01-01>"
        }

2. Spusťte tento příkaz zaokrouhlením název virtuálního počítače pro #60; OM název & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

## <a name="sshkeyresetcli"></a>Obnovit SSH klíč

1. Vytvoření souboru s názvem PrivateConf.json tento obsah. Nahrazení hranatých závorkách a & #60; zástupný text & #62; hodnoty vlastními informacemi.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>"
        }

2. Spusťte tento příkaz zaokrouhlením název virtuálního počítače pro #60; OM název & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Resetování hesla a klávesu SSH

1. Vytvoření souboru s názvem PrivateConf.json tento obsah. Nahrazení hranatých závorkách a & #60; zástupný text & #62; hodnoty vlastními informacemi.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>",
        "password":"<newpassword>"
        }

2. Spusťte tento příkaz zaokrouhlením název virtuálního počítače pro #60; OM název & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="createnewsudocli"></a>Vytvořit nový uživatelský účet sudo

Pokud jste zapomněli svoje uživatelské jméno, můžete vytvořit nový účet s autorita sudo VMAccess. V tomto případě existující uživatelské jméno a heslo nebude mění.

Když Pokud chcete vytvořit nového uživatele sudo s přístupem heslo, používat skript [resetování hesla](#pwresetcli) a zadejte nové uživatelské jméno.

Pokud chcete vytvořit nového uživatele sudo SSH klíčové přístup, používat skript [obnovit klíč SSH](#sshkeyresetcli) a zadejte nové uživatelské jméno.

Taky můžete [Resetovat hesla a klávesu SSH](#resetbothcli) vytvořit nového uživatele s heslem a SSH klíčové přístup.

## <a name="sshconfigresetcli"></a>Obnovení konfigurace SSH

Pokud konfigurace SSH je ve stavu nežádoucí, může taky ztratíte přístup k OM. Rozšíření VMAccess slouží k obnovení konfigurace do výchozího stavu. Postup, stačí nastavit klíč "reset_ssh" na "True". Rozšíření bude restartujte SSH server port SSH na vaše OM otevřít a obnovit konfigurace SSH výchozí hodnoty. Uživatelský účet (jméno, heslo nebo SSH klíče) se nezmění.

> [AZURE.NOTE] Konfigurační soubor SSH, který získá resetování je umístěn na /etc/ssh/sshd_config.

1. Vytvoření souboru s názvem PrivateConf.json tento obsah.

        {
        "reset_ssh":"True"
        }

2. Spusťte tento příkaz zaokrouhlením název virtuálního počítače pro #60; OM název & #62;. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="deletecli"></a>Odstraněním uživatelského účtu

Pokud chcete odstraněním uživatelského účtu bez přihlášení do bude v angličtině přímo, můžete tento skript.

1. Vytvoření souboru s názvem PrivateConf.json tento obsah zaokrouhlením uživatelské jméno pro odebrání #60; usernametoremove & #62;. 

        {
        "remove_user":"<usernametoremove>"
        }

2. Spusťte tento příkaz zaokrouhlením název virtuálního počítače pro #60; OM název & #62;. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="statuscli"></a>Zobrazit stav koncovku VMAccess

Zobrazit stav koncovku VMAccess, spusťte tento příkaz.

        azure vm extension get

## <a name="a-namecheckdiskacheck-consistency-of-added-disks"></a>< name = "checkdisk" <</a>Kontrola konzistence přidané disků

Spuštění fsck na všech discích v počítači virtuální Linux, budete potřebovat provést následující akce:

1. Vytvoření souboru s názvem PublicConf.json tento obsah. Zkontrolujte, že Disk trvá logická hodnota určující, zda chcete zkontrolovat disků připojené k virtuálního počítače nebo ne. 

        {   
        "check_disk": "true"
        }

2. Spusťte tento příkaz spustit, nahraďte název virtuálního počítače pro #60; OM název & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 

## <a name='repairdisk'></a>Oprava disků 

Oprava disků, které nejsou připojení nebo připojení konfigurace chyb, můžete koncovku VMAccess resetovat konfigurace připojení ve vašem počítači virtuální Linux. Nahraďte název disku #60; yourdisk & #62;.

1. Vytvoření souboru s názvem PublicConf.json tento obsah. 

        {
        "repair_disk":"true",
        "disk_name":"<yourdisk>"
        }

2. Spusťte tento příkaz spustit, nahraďte název virtuálního počítače pro #60; OM název & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## <a name="next-steps"></a>Další kroky

* Pokud chcete použít rutiny prostředí PowerShell Azure nebo správce prostředků Azure šablony resetovat heslo nebo SSH klíč, konfiguraci SSH a kontrola konzistence disku, najdete [si přečtěte následující dokumentaci rozšíření VMAccess na GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 

* Můžete také [Azure portál](https://portal.azure.com) k resetování hesla nebo SSH klíč Linux OM nasazenou v modelu klasické nasazení. Nelze použít aktuálně portálu tohoto pro Linux OM nasazenou v modelu nasazení Správce prostředků.

* V tématu [o rozšíření virtuálního počítače a funkce](virtual-machines-linux-extensions-features.md) pro informace o používání OM rozšíření Azure virtuálních počítačích.
