<properties
    pageTitle="Zachyťte libovolný obraz z Linux OM | Microsoft Azure"
    description="Naučte se zachyťte libovolný obraz z na základě Linux Azure virtuálního počítače (OM) vytvořená pomocí klasické nasazení modelu."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="iainfou"/>


# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Jak zachytit klasické Linux virtuálního počítače jako obrázek

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zjistěte, jak provádět [tyto kroky pomocí modelu správce prostředků](virtual-machines-linux-capture-image.md).

Tento článek ukazuje, jak zachytit do klasického Azure virtuální počítače se systémem Linux jako obrázek, který chcete vytvořit další virtuálních počítačích. Tento obrázek obsahuje OS disku a disků dat připojených k virtuální počítač. Konfigurace v síti nezahrnuje, musíte nakonfigurovat, při vytváření virtuálních počítačích z obrázku.

Azure ukládá obrázek v části **obrázky**, spolu s obrázky, které jste nahráli. Další informace o obrázky najdete v článku [O obrázky virtuálního počítače v Azure] [].

## <a name="before-you-begin"></a>Než začnete

Tento postup předpokládá, že jste vytvořili Azure virtuálního počítače pomocí klasického nasazení modelu a nakonfigurovali operačního systému, včetně připojení všech dat discích. Pokud je potřeba vytvořit virtuálního počítače, přečtěte si, [jak vytvořit virtuální počítač Linux] [].


## <a name="capture-the-virtual-machine"></a>Zachycení virtuálního počítače

1. [Připojte se k počítači virtuální](virtual-machines-linux-mac-create-ssh-keys.md) pomocí klienta SSH podle svého výběru.

2. V okně SSH zadejte následující příkaz. Výstup `waagent` může trochu lišit v závislosti na verzi tohoto nástroje:

    `sudo waagent -deprovision+user`

    Tento příkaz pokusí vyčištění systému a jeho vhodné pro reprovisioning. Tento operací, jako například tyto věci:

    - Odebere SSH hostitele klíče (Pokud Provisioning.RegenerateSshHostKeyPair je "y" v souboru konfigurace).
    - Vymaže nameserver konfigurace ve /etc/resolv.conf
    - Odebere `root` heslo uživatele z/atd/stín (Pokud Provisioning.DeleteRootPassword "y" v souboru konfigurace)
    - Odebere cached DHCP zapůjčení
    - Název hostitele obnovíte do localhost.localdomain
    - Odstraní poslední zřizování uživatelů účtu (získané /var/lib/waagent) **a Spojenými daty**.

    >[AZURE.NOTE] Zrušení zajišťování odstraní soubory a data, která chcete "generalize" obrázku. Pouze spusťte tento příkaz na počítač virtuální, kterou chcete zachytit jako novou šablonu obrázek. Nezaručuje se, že obrázek zaškrtnuté všechny citlivých informací nebo je vhodné pro redistribuci třetím stranám.


3. Zadejte **y** pokračovat. Můžete přidat `-force` parametr nechcete-li tento potvrzení krok.

4. Zadejte **Konec** ukončíte SSH klienta.

    >[AZURE.NOTE] Zbývající kroky předpokládají, že už máte [nainstalovaný rozhraní příkazového řádku Azure](../xplat-cli-install.md) ve vašem klientském počítači. Všechny následující kroky lze provést také [Azure klasické portálu] [].

5. V klientském počítači otevřete Azure rozhraní příkazového řádku a přihlášení k předplatnému Azure. Podrobné informace najdete [připojit k Azure předplatné z Azure rozhraní příkazového řádku](../xplat-cli-connect.md).

6. Zkontrolujte, jestli že jste v režimu Správa služeb:

    `azure config mode asm`

7. Vypnutí virtuální počítač, který je již poskytování zrušeno v předchozích krocích se:

    `azure vm shutdown <your-virtual-machine-name>`

    >[AZURE.NOTE] Můžete zjistit všechny virtuálních počítačích vytvořené ve vašem předplatném pomocí`azure vm list`

8. Po zastavení virtuální počítač napřed zachyťte pomocí příkazu:

    `azure vm capture -t <your-virtual-machine-name> <new-image-name>`

    Zadejte název obrázek, který chcete místo _Nový název obrázku_. Příkaz vytvoří generalized obrázek s operačním systémem. `-t` Dílčí odstraní původní virtuálního počítače.

9.  Obrázek novinky je teď dostupná v seznamu obrázky, které lze použít ke konfiguraci všechny nové virtuálních počítačích. Můžete ji zobrazit pomocí příkazu:

    `azure vm image list`

    Na [Azure klasické portál] []objeví se v seznamu **obrázky** .

    ![Obrázek snímku povedla](./media/virtual-machines-linux-classic-capture-image/VMCapturedImageAvailable.png)


## <a name="next-steps"></a>Další kroky
Obrázek hotovou použít k vytvoření virtuálních počítačích. Můžete použít příkaz rozhraní příkazového řádku Azure `azure vm create` a zadáním názvu obrázek jste vytvořili. Další informace o příkazu v tématu [použití rozhraní příkazového řádku Azure pomocí klasického nasazení modelu](../virtual-machines-command-line-tools.md) . Můžete taky vytvořit vlastní virtuálního počítače pomocí metody **Z Galerie** a výběrem obrázek, který jste vytvořili pomocí [Azure klasické portál] [] . Podrobné informace najdete v článku [jak vytvářet vlastní virtuálního počítače] [] .

**Viz taky:** [Azure Linux Agent User Guide](virtual-machines-linux-agent-user-guide.md)

[Azure klasické portálu]: http://manage.windowsazure.com
[Virtuální počítač obrázky v Azure]: virtual-machines-linux-classic-about-images.md
[Jak vytvořit vlastní virtuálního počítače]: virtual-machines-linux-classic-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]: virtual-machines-windows-classic-attach-disk.md
[Jak vytvořit virtuální počítač Linux]: virtual-machines-linux-classic-create-custom.md
