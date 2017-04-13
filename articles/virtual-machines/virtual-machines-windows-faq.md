<properties
    pageTitle="Nejčastější dotazy týkající se Windows VMs | Microsoft Azure"
    description="Najdete odpovědi na některé běžné dotazy týkající se virtuálních počítačích Windows vytvořené pomocí modelu správce prostředků."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Časté otázky o Windows virtuálních počítačích 


Tento článek se zaměřuje na některé běžné dotazy týkající se virtuálních počítačích Windows vytvořené v Azure pomocí nasazení modelu správce prostředků. Verze Linux v tomto tématu najdete v tématu [Časté otázky o Linux virtuálních počítačích](virtual-machines-linux-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Co je možné spouštět na OM Azure?

Všechny účastníky je možné spouštět serverový software na Azure virtuálního počítače. Informace o zásadách podpory pro softwarem serveru Microsoft v Azure najdete v tématu [serveru softwaru podporu pro virtuálních počítačích Azure](https://support.microsoft.com/kb/2721672)

Některých verzích Windows 7 a Windows 8.1 a jsou dostupní MSDN Azure výhoda předplatitele web MSDN pro vývojáře a otestujte přislíbený předplatitele vývoj a testování úkolů. Další informace, včetně pokyny a omezení zobrazovat [obrázky klienta systému Windows pro předplatitele](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Jakou velikost úložiště můžete používat s virtuálního počítače?

Každý disk dat můžete mít až 1 TB. Počet disků dat, které můžete použít závisí na velikosti virtuální počítač. Podrobnosti najdete v tématu [velikosti virtuálních počítačích](virtual-machines-windows-sizes.md).

Účet Azure úložiště poskytuje úložiště pro disk operačního systému a všechny disků data. Každý disk je soubor VHD uložených jako objektů blob stránky. Ceny podrobnosti, najdete v článku [Úložiště ceny podrobnosti](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Jak lze získat přístup k počítači virtuální?

Připojení ke vzdálené pomocí vzdáleného připojení RDP (Desktop) pro Windows OM navažte. Pokyny najdete v tématu [jak připojit a přihlaste se k Azure virtuální počítač s Windows](virtual-machines-windows-connect-logon.md). Než dvě souběžné připojení jsou podporovaná, pokud je server nakonfigurovaný jako hostitel relace Vzdálená plocha.  


Pokud máte problémy s vzdálené plochy, přečtěte si článek [Poradce při potížích s vzdálené plochy připojení k serveru s Windows Azure virtuálního počítače](virtual-machines-windows-troubleshoot-rdp-connection.md). 

Pokud znáte Hyper-V, může být hledáte nástroji podobný VMConnect. Azure nenabízí podobného nástroje, protože konzoly přístup k virtuální počítač není podporován.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Můžete používat dočasné disku (jednotka D: ve výchozím nastavení) k ukládání dat?

Nepoužívejte dočasné disku pro ukládání data. Je dočasný úložiště, tak by mohlo ztráty dat, která není možné obnovit. Při přesouvání virtuálního počítače do jiného hostitele, může dojít ke ztrátě dat. Změna velikosti virtuálního počítače, aktualizace hostitele nebo selhání hardwaru na hostiteli je několik důvodů, které může přesunout virtuálního počítače.

Pokud máte aplikaci, kterou je potřeba použít D: písmeno, můžete přiřadit písmena tak, aby dočasné disk používá než D:. Pokyny najdete v tématu [Změna písmeno dočasném disku Windows](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Změna písmeno dočasné disku

Písmeno můžete změnit tak, že přesunutí souboru stránky a přidělit písmena, ale budete muset Ujistěte se, že udělejte kroky v určitém pořadí. Pokyny najdete v tématu [Změna písmeno dočasném disku Windows](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Můžete přidat existující OM do sady dostupnost?

Ne. Pokud budete potřebovat OM jako součást sady dostupnost, je potřeba vytvořit OM v rámci sady. Momentálně není k dispozici způsob, jak přidat virtuálního počítače dostupné po byl vytvořen.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Můžete nahrávat virtuálního počítače do Azure?

Ano. Pokyny najdete v tématu [nahrání bitové OM Windows Azure](virtual-machines-windows-upload-image.md)

## <a name="can-i-resize-the-os-disk"></a>Můžete změnit velikost disku operační systém?

Ano. Pokyny najdete v tématu [jak rozbalte jednotku OS virtuálního počítače ve skupině Azure zdroje](virtual-machines-windows-expand-os-disk.md).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Můžete kopírovat nebo klonovat existující OM Azure?

Ano. Pokyny najdete v tématu [Vytvoření kopie Windows virtuálního počítače v modelu nasazení Správce prostředků](virtual-machines-windows-vhd-copy.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Proč nevidím Kanada centrální a východ Kanada oblastí pomocí Správce prostředků Azure?

Dva nové oblasti Kanada centrální a východ Kanada nejsou automaticky registrované pro vytváření virtuálních počítačů pro existující Azure předplatné. Tato registrace probíhá automaticky při nasazení virtuálního počítače prostřednictvím portálu Azure do jiných oblastí pomocí Správce prostředků Azure. Po nasazení virtuálního počítače na jakékoli jiné oblasti, Azure nové oblasti mělo být k dispozici pro následné virtuálních počítačích.

## <a name="does-azure-support-linux-vms"></a>Podporuje Azure Linux VMs?

Ano. Pokud chcete rychle vytvořit Linux OM objevovat, v tématu [Vytvoření Linux OM na Azure na portálu](virtual-machines-linux-quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Můžu přidat NIC Moje v angličtině po jeho vytvoření?

Ne. Přidání NIC lze provést jenom na údaje o času vytvoření.

## <a name="are-there-any-computer-name-requirements"></a>Jsou všechny požadavky název počítače?

Ano. Název počítače může obsahovat maximálně 15 znaků. Další informace kolem pojmenování svých prostředcích najdete [pokyny pro pojmenování infrastruktury](virtual-machines-windows-infrastructure-naming-guidelines.md) .

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Jaké jsou požadavky na uživatelské jméno při vytváření virtuálního počítače?

Uživatelská jména může obsahovat maximálně 20 znaků a nelze končit tečkou ("."). 

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
        <td style="text-align:center">zálohování </td><td style="text-align:center"> konzoly </td><td style="text-align:center"> David </td><td style="text-align:center"> hosty</td>
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

Hesla musí být 8 123 znaků a musí splňovat 3 mimo tyto požadavky 4 složitost:

- Nižší znaky
- Horní znaky
- Máte číslici
- Neobsahují speciální znaky (Regex shodovat [\W_])

Následující hesla nejsou povoleny:

<table>
    <tr>
        <td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">Pa$ word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td><td style="text-align:center">Heslo!</td><td style="text-align:center">Hesla1</td><td style="text-align:center">Password22</td><td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
