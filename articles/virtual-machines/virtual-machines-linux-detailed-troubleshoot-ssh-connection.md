<properties
    pageTitle="Podrobné SSH Poradce při potížích pro Azure OM | Microsoft Azure"
    description="Podrobnější SSH návody na řešení potíží pro problémů s připojením ke Azure virtuálního počítače"
    keywords="SSH připojení odmítnuto, ssh chyba, azure ssh, SSH připojení se nezdařila."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="detailed-ssh-troubleshooting-steps"></a>Podrobný postup řešení potíží SSH

Existuje mnoho možných příčin, které SSH klienta nemusí mít přístup službě SSH v OM. Pokud jste postupovali Další [Obecné SSH řešení potíží](virtual-machines-linux-troubleshoot-ssh-connection.md), musíte dál Poradce při potížích s problém s připojením. Tento článek vás provede podrobně Poradce při potížích popisuje, jak zjistit, kde se nezdařilo SSH připojení a jak ji řešit.

## <a name="take-preliminary-steps"></a>Podniknout předběžná opatření

Na následujícím obrázku vidíte součásti zapojené.

![Diagram zobrazující součástí SSH služby](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Podle těchto kroků můžete zjistit příčinu chyby a zjistit řešení a užitečné tipy.

Nejdřív zkontrolujte stav OM na portálu.

[Azure portálu](https://portal.azure.com):

1. VMs vytvořený s využitím modelu klasické nasazení, vyberte možnost **Vyhledat** > **virtuálních počítačích (klasické)** > *OM název*.

    – NEBO –

    VMs vytvořený s využitím modelu správce prostředků, vyberte možnost **Vyhledat** > **virtuálních počítačích** > *OM název*.

    Podokno Stav OM byste měli vidět **spuštěný**. Posuňte se dolů na poslední aktivity zobrazit pro výpočetním, ukládání a síťových prostředků.

2. Vyberte **Nastavení** a zkontrolujte koncové body, IP adresy a další nastavení.

    K identifikaci koncové body v VMs, které jsou vytvořené pomocí Správce prostředků, ověřte definováno [skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) . Taky ověřte, že tato pravidla bylo použito skupiny zabezpečení sítě a že se odkazuje ve podsítě.

[Azure klasické portálu](https://manage.windowsazure.com)pro VMs vytvořené pomocí klasické nasazení modelu:

1. Vyberte **virtuálních počítačích** > *OM název*.
2. Vyberte OM **řídicí panel** pro kontrolu stavu.
3. Vyberte **Monitor** můžete zobrazit posledních aktivit pro výpočetním, ukládání a síťových prostředků.
4. Vyberte **koncové body** a ujistěte se, že je koncový bod pro přenos SSH.

Pokud chcete ověřit připojení k síti, zkontrolujte nakonfigurované koncové body a zjistěte, pokud se dostanete OM pomocí jiného protokolu HTTP ATP jiné služby.

Po provedení těchto kroků zkuste SSH připojit znovu.


## <a name="find-the-source-of-the-issue"></a>Vyhledání zdroje problém

Klient SSH ve vašem počítači může selhat kontaktovat službě SSH v Azure OM kvůli problémům nebo kontrolovaný v následujících tématech:

- [SSH klientského počítače](#source-1-ssh-client-computer)
- [Organizace okraj zařízení](#source-2-organization-edge-device)
- [Cloud koncový bod služby a získat přístup k seznamu ovládacího prvku (ACL)](#source-3-cloud-service-endpoint-and-acl)
- [Skupiny zabezpečení sítě](#source-4-network-security-groups)
- [Na základě Linux Azure OM](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>Zdroj 1: SSH klientského počítače

K odstranění počítači jako zdroj chyby, zkontrolujte, že ho můžete připojovat SSH k jiné místním počítači se systémem Linux.

![Diagram, který upozorňuje na součásti SSH klientského počítače](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Pokud připojení se nezdaří, zkontrolujte následující ve vašem počítači:

- Místní nastavení brány firewall blokuje příchozí nebo odchozí přenosy SSH (TCP 22)
- Místně nainstalovaný klientský software proxy, který brání SSH připojení
- Místně nainstalovaný sledování software, který brání SSH připojení sítě
- Jiné druhy softwaru zabezpečení, které monitorovat data nebo povolit nebo zakázat určité typy přenosů

Pokud jedna z těchto podmínek použít, dočasně vypnout software a zkuste SSH připojení k místním počítači, pokud chcete zjistit, důvod, proč připojení je blokován ve vašem počítači. Pracujte s správce sítě problém můžete vyřešit tak nastavení software, aby umožňoval připojení SSH.

Pokud používáte ověřovací certifikát, zkontrolujte, jestli máte oprávnění ke složce .ssh v adresáři domácí:

- Chmod 700 ~/.ssh
- Chmod 644 ~/.ssh/\*.pub
- Chmod 600 ~/.ssh/id_rsa (nebo jiných souborů, které obsahují vaše privátních klíčů uložené v nich)
- Chmod 644 ~/.ssh/known_hosts (obsahuje hosts, která jste připojili ke prostřednictvím SSH)

## <a name="source-2-organization-edge-device"></a>Zdroj 2: Organizace okraj zařízení

K odstranění zařízení okraj organizace jako zdroj selhání, zkontrolujte, že počítač, na kterém má přímé připojení k Internetu můžete provádět SSH připojení OM Azure. Pokud pracujete OM připojení prostřednictvím sítě VPN na webu nebo připojení k Azure ExpressRoute, přejděte ke [Zdroj 4: síťové skupiny zabezpečení](#nsg).

![Diagram, který upozorňuje na zařízení okraj organizace](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Pokud nemáte počítač, na kterém je přímo připojení k Internetu, vytvořte nový OM Azure ve vlastní skupině prostředků nebo cloudových služeb a používá. Další informace najdete v tématu [Vytvoření virtuálního počítače systémem Linux v Azure](virtual-machines-linux-quick-create-cli.md). Odstranění pole Skupina zdroje nebo OM a cloudové služby, až to budete mít s testování.

Pokud vytvoříte připojení k SSH s počítačem, který má přímé připojení k Internetu Zkontrolujte zařízení okraj organizace:

- Vnitřní firewall, která blokuje SSH přenosy Internetu
- Proxy server, který brání SSH připojení
- Průnik zjišťování nebo sledování software systém na zařízeních v síti okraj, který brání SSH připojení k síti

Práce s správce sítě opravte nastavení zařízení okraj organizace umožňuje SSH přenosy k Internetu.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Zdroj 3: Koncový bod služby cloudu a ACL

> [AZURE.NOTE] Tento zdroj platí jenom pro VMs, které jsou vytvořené pomocí klasické nasazení modelu. Pro VMs, které jsou vytvořené pomocí Správce prostředků, přejděte ke [zdroje 4: síťové skupiny zabezpečení](#nsg).

K odstranění koncového bodu služby cloudu a ACL jako zdroj chyby, zkontrolujte, že jiný OM Azure ve stejné síti virtuální můžete provádět SSH připojení vaší OM.

![Diagram, který upozorňuje na koncový bod služby cloudu a ACL](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Pokud nemáte jiné OM ve stejné síti virtuální, mohli snadno vytvářet nové. Další informace najdete v tématu [Vytvoření Linux OM na Azure pomocí rozhraní příkazového řádku](virtual-machines-linux-quick-create-cli.md). Navíc OM odstranění: Když dokončíte testování.

Pokud vytvoříte připojení k SSH s OM ve stejné síti virtuální Zkontrolujte tyto věci:

- **Konfigurace koncový bod pro přenos SSH v cíli OM.** Soukromé port TCP koncového bodu by měly odpovídat port TCP, na kterém je službě SSH v OM přijímá. (Výchozí port je 22). U VMs vytvořený s využitím nasazení modelu správce prostředků, ověřte číslo portu SSH TCP Azure portálu tak, že vyberete **Procházet** > **virtuálních počítačích (v2)** > *OM název* > **Nastavení** > **koncové body**.

- **ACL koncového bodu SSH přenosy na cílovém virtuální počítači.** ACL umožňují zadat povolené nebo odepřen příchozích z Internetu, založené na její zdrojová IP adresa. Jsou chybně nakonfigurované ACL můžete zabránit SSH příchozích koncový bod. Kontrola ACL zajistit, aby příchozí přenosy na veřejnou IP adresy vašeho proxy ani není povolené jiných edge serveru. Další informace najdete v tématu [o přístup k síti seznamy řízení (ACL)](../virtual-network/virtual-networks-acl.md).

Odstranění koncového bodu jako zdroj problém, odebrat aktuální koncový bod, vytvoříte nový koncový bod a zadejte název SSH (TCP 22 pro port číslo portu veřejných a privátních). Další informace najdete v tématu [Nastavení koncové body v počítači virtuální v Azure](virtual-machines-windows-classic-setup-endpoints.md).

<a id="nsg"></a>
## <a name="source-4-network-security-groups"></a>Zdroje 4: Skupiny zabezpečení sítě

Skupiny zabezpečení sítě umožňují mít přesnější kontrolu povolené příchozí a odchozí přenosy. Můžete vytvořit pravidla, které zahrnují podsítí a cloudových služeb v Azure virtuální sítě. Zkontrolujte pravidla skupiny zabezpečení sítě zajistit, že jsou povoleny SSH přenosy do a z Internetu.
Další informace najdete v tématu [informace o skupinách zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).

## <a name="source-5-linux-based-azure-virtual-machine"></a>Zdroj 5: Na základě Linux Azure virtuálního počítače

Poslední zdroj možným potížím je Azure samotného virtuálního počítače.

![Diagram, který upozorňuje na základě Linux Azure virtuálního počítače](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Pokud jste to ještě neudělali, postupujte podle části pokyny [k resetování hesla nebo SSH na základě Linux virtuálních počítačích](virtual-machines-linux-classic-reset-access.md).

Zkuste se znovu připojit ze svého počítače. Pokud se stále nezdaří, jsou uvedené některé možné problémy:

- Na cílovém virtuální počítači není spuštěná služba SSH.
- Služba SSH nesleduje na TCP port 22. Otestovat, nainstalujte klienta telnet ve vašem počítači a spusťte "telnet *cloudServiceName*. cloudapp.net 22". Určuje, pokud virtuální počítač umožňuje příchozí a odchozí komunikaci SSH koncový bod.
- Místní brána firewall v cílovém virtuální počítači obsahuje pravidla, které brání příchozí nebo odchozí přenosy SSH.
- Průnik zjišťování nebo sledování software, který běží v Azure virtuálního počítače sítě brání SSH připojení.


## <a name="additional-resources"></a>Další zdroje informací
Další informace o řešení potíží s aplikací access najdete v tématu [Poradce při potížích s přístupu k spuštění aplikace v Azure virtuálního počítače](virtual-machines-linux-troubleshoot-app-connection.md)