<properties
    pageTitle="Generalize Windows virtuální pevný disk | Microsoft Azure"
    description="Naučte se používat Sysprep k generalize Windows OM pomocí služby nasazení modelu správce prostředků."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
    
# <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Generalize Windows virtuálního počítače pomocí Sysprep

Tato část popisuje generalize počítači se systémem Windows virtuální sloužící jako obrázek. Tento nástroj odebírá všechny osobní informace o účtu, mimo jiné a připraví počítač má být použit jako obrázek. Podrobnosti o Sysprep najdete v tématu [způsob použití Sysprep: Úvod](http://technet.microsoft.com/library/bb457073.aspx).

Zkontrolujte, že v počítači spuštěna role serveru podporují tímto programem. Další informace najdete v tématu [Sysprep podpora role serveru](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

>[AZURE.IMPORTANT] Pokud používáte Sysprep před nahráním vaší virtuální pevný disk Azure poprvé, zkontrolujte, jestli že máte [počítat vaší OM](virtual-machines-windows-prepare-for-upload-vhd-image.md) před spuštěním Sysprep. 

1. Přihlaste se k počítači virtuální Windows.

2. Jako správce otevřete okno příkazového řádku. Přejděte v adresáři **%windir%\system32\sysprep**a znovu spusťte `sysprep.exe`.

3. V dialogovém okně **Nástroj pro přípravu systému** zaškrtněte **prostředí mimo pole zadejte systém (počáteční nastavení počítače)**a ujistěte se, že je zaškrtnuté políčko **Generalizace** .

4. **Ve skupinovém rámečku **Možnosti vypnutí**vyberte položku vypnout.**

5. Klikněte na **OK**.

    ![Zahájení Sysprep](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

6. Po dokončení Sysprep vypne virtuální počítač. 

## <a name="next-steps"></a>Další kroky

- Pokud OM je místně, můžete teď [Nahrát virtuální pevný disk na Azure](virtual-machines-windows-upload-image.md).
- Pokud OM už v Azure, můžete teď [Vytvořit obrázek z generalized OM](virtual-machines-windows-capture-image.md).