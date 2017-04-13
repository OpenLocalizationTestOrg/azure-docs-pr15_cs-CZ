<properties
    pageTitle="Nejčastější dotazy týkající se Linux VMs | Microsoft Azure"
    description="Najdete odpovědi na některé běžné dotazy týkající se Linux virtuálních počítačích vytvořené pomocí modelu správce prostředků."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Časté otázky o Linux virtuálních počítačích

Tento článek se zaměřuje na některé běžné otázky o Linux virtuálních počítačích vytvořené v Azure pomocí nasazení modelu správce prostředků. Verze Windows v tomto tématu najdete v tématu [Časté otázky o Windows virtuálních počítačích](virtual-machines-windows-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Co je možné spouštět na OM Azure?

Všechny účastníky je možné spouštět serverový software na Azure virtuálního počítače. Další informace najdete v tématu [Linux na Azure-Endorsed rozdělení](virtual-machines-linux-endorsed-distros.md)


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Jakou velikost úložiště můžete používat s virtuálního počítače?

Každý disk dat můžete mít až 1 TB. Počet disků dat, které můžete použít závisí na velikosti virtuální počítač. Podrobnosti najdete v tématu [velikosti virtuálních počítačích](virtual-machines-linux-sizes.md).

Účet Azure úložiště poskytuje úložiště pro disk operačního systému a všechny disků data. Každý disk je soubor VHD uložených jako objektů blob stránky. Ceny podrobnosti, najdete v článku [Úložiště ceny podrobnosti](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Jak lze získat přístup k počítači virtuální?

Připojení ke vzdálené se přihlaste k počítači virtuální pomocí zabezpečené prostředí (SSH) navažte. Postupujte podle pokynů na tom, jak připojit [z Windows](virtual-machines-linux-ssh-from-windows.md) nebo [Mac a Linux](virtual-machines-linux-mac-create-ssh-keys.md). Ve výchozím nastavení umožňuje SSH maximálně 10 souběžné připojení. Toto číslo můžete zvýšit úpravou konfiguračního souboru.


Pokud máte problémy, podívejte se na [řešení potíží s zabezpečené prostředí (SSH) připojení](virtual-machines-linux-troubleshoot-ssh-connection.md).


## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Můžete používat dočasné disku (/ vývojáře/sdb1) k ukládání dat?

Nepoužívejte dočasné disku (/ vývojáře/sdb1) pro ukládání data. Není pouze pro dočasné uložení. Můžete rizika ztráty dat, která není možné obnovit.


## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Můžete kopírovat nebo klonovat existující OM Azure?

Ano. Pokyny najdete v tématu [Vytvoření kopie Linux virtuálního počítače v modelu nasazení Správce prostředků](virtual-machines-linux-copy-vm.md).


## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Proč nevidím Kanada centrální a východ Kanada oblastí pomocí Správce prostředků Azure?

Dva nové oblasti Kanada centrální a východ Kanada nejsou automaticky registrované pro vytváření virtuálních počítačů pro existující Azure předplatné. Tato registrace probíhá automaticky při nasazení virtuálního počítače prostřednictvím portálu Azure do jiných oblastí pomocí Správce prostředků Azure. Po nasazení virtuálního počítače na jakékoli jiné oblasti, Azure nové oblasti mělo být k dispozici pro následné virtuálních počítačích.


## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Můžu přidat NIC Moje v angličtině po jeho vytvoření?

Ne. Přidání NIC lze provést jenom na údaje o času vytvoření.


## <a name="are-there-any-computer-name-requirements"></a>Jsou všechny požadavky název počítače?

Ano. Název počítače může obsahovat maximálně 64 znaků. Další informace kolem pojmenování svých prostředcích najdete [pokyny pro pojmenování infrastruktury](virtual-machines-linux-infrastructure-naming-guidelines.md) .


## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Jaké jsou požadavky na uživatelské jméno při vytváření virtuálního počítače?

Uživatelská jména musí být 1-64 znaků.

Následující uživatelská jména nejsou povoleny:

<table>
    <tr>
        <td style="text-align:center">Správce </td><td style="text-align:center"> Správce </td><td style="text-align:center"> uživatel </td><td style="text-align:center"> Uživatel 1</td>
    </tr>
    <tr>
        <td style="text-align:center">Test </td><td style="text-align:center"> uživatel2 </td><td style="text-align:center"> test1 </td><td style="text-align:center"> UŽIVATEL3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> na</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> ADM </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">zálohování </td><td style="text-align:center"> konzoly </td><td style="text-align:center"> David </td><td style="text-align:center"> Host</td>
    </tr>
    <tr>
        <td style="text-align:center">Petr </td><td style="text-align:center"> Vlastník </td><td style="text-align:center"> kořenové </td><td style="text-align:center"> Server</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> Podpora </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> systémových</td>
    </tr>
    <tr>
        <td style="text-align:center">test2 </td><td style="text-align:center"> Test3 </td><td style="text-align:center"> Uživatel4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Jaké jsou požadavky na heslo při vytváření virtuálního počítače?

Hesla musí být 6 72 znaků a musí splňovat 3 mimo tyto požadavky 4 složitost:

- Nižší znaky
- Horní znaky
- Máte číslici
- Neobsahují speciální znaky (Regex shodovat [\W_])

Následující hesla nejsou povoleny:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$ word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Heslo!</td>
        <td style="text-align:center">Hesla1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
