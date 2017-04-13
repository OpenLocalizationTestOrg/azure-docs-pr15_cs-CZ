<properties
    pageTitle="Poradce při potížích s odstranění účtů Azure úložiště, kontejnerů nebo VHD v klasické nasazení | Microsoft Azure"
    description="Poradce při potížích s odstranění účtů Azure úložiště, kontejnerů nebo VHD v klasické nasazení"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="tysonn"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="genli"/>

# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Poradce při potížích s odstranění účtů Azure úložiště, kontejnerů nebo VHD v klasické nasazení

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Chyby můžou zobrazit, když se pokusíte odstranit účet Azure úložiště, kontejner nebo virtuální pevný disk v [Azure portál](https://portal.azure.com/) nebo [Azure klasické portálu](https://manage.windowsazure.com/). V následujících případech může způsobovat problémy:

-   Když odstraníte virtuálního počítače, disk a virtuální pevný disk se automaticky neodstraní. Který může být důvod selhání při odstranění účtu úložiště. Neodstraňujte jsme disku tak, aby se můžete připojit jiné OM pomocí disku.

-   Je stále zapůjčenou na disku nebo objektů blob, který máte přidružený k disku.

Pokud Azure problém není určená v tomto článku, navštivte fóra Azure na [webu MSDN a přetečení zásobníku](https://azure.microsoft.com/support/forums/). Můžete publikovat problém na tyto fóra nebo na @AzureSupport na Twitter. Žádost o Azure podporu můžete poslat taky, tak, že vyberete, **využijte možnost získání podpory** na webu [Azure podpory](https://azure.microsoft.com/support/options/) .

## <a name="symptoms"></a>Příznaky

V následující části jsou uvedeny běžné chyby, které můžou zobrazit, když se pokusíte odstranit účty Azure úložiště, kontejnerů nebo VHD.

### <a name="scenario-1-unable-to-delete-a-storage-account"></a>Scénář 1: Nelze odstranit účet úložiště

Pokud přejdete na úložiště účet [Azure portál](https://portal.azure.com/) nebo [Azure klasické portálem](https://manage.windowsazure.com/) a vyberte **Odstranit**, může se zobrazit tato chybová zpráva:

*Úložiště účtu StorageAccountName obsahuje OM obrázky. Zkontrolujte, zda že jsou odebrány tyto obrázky OM před odstraněním tohoto účtu úložiště.*

Tato chyba se může zobrazit:

**Na Azure portálu**:

*Odstranění účtu úložiště < OM-– název účtu úložiště-> se nezdařila. Nemůžete odstranit účtu úložiště < OM-– název účtu úložiště->: "úložiště účet < OM-– název účtu úložiště-> má některé aktivní obrázky a/nebo discích. Zajistit, aby se před odstraněním tohoto účtu úložiště odeberou tyto obrázky a/nebo discích. ".*

**Na Azure klasické portálu**:

*Účet úložiště < OM-– název účtu úložiště-> má některé aktivní obrázky a/nebo discích, například xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Zajistit, aby tyto obrázky a/nebo discích odeberou se před odstraněním tohoto účtu úložiště.*

Nebo

**Na Azure portálu**:

Účet úložiště *< OM-– název účtu úložiště-> má 1 nádoba, které mají aktivní obrázek nebo artefakty disku. Zajistit, aby tyto artefakty budou odebrány z úložiště obrázek před odstraněním tohoto účtu úložiště*.

**Na Azure klasické portálu**:

Účet odeslat se nepodařilo úložiště *< OM-– název účtu úložiště-> má 1 nádoba, které mají aktivní obrázek nebo artefakty disku. Zajistěte, aby že tyto artefakty budou odebrány z úložiště obrázek před odstraněním tohoto účtu úložiště. Při pokusu o odstranění účtu úložiště a je stále aktivní disků přidružená, zobrazí se zpráva s upozorněním, jsou aktivní disků, které je potřeba odstranit*.

### <a name="scenario-2-unable-to-delete-a-container"></a>Scénář 2: Nelze odstranit kontejneru

Při pokusu o odstranění kontejneru úložiště, může se zobrazit tato chyba:

*Se nepovedlo odstranit kontejner úložiště <container name>. Chyba: "v kontejneru je aktuálně zapůjčenou a žádné zapůjčení ID zadaná v žádosti o*.

### <a name="scenario-3-unable-to-delete-a-vhd"></a>Scénář 3: Nelze odstranit virtuální pevný disk

Po odstranění virtuálního počítače a pak se pokusíte odstranit objekty BLOB pro přidružené VHD, může se zobrazí následující zpráva:

*Odstranění objektů blob "cesta/XXXXXX-XXXXXX-s operačním systémem-1447379084699.vhd'. Chyba: "v objektů blob je aktuálně zapůjčenou a žádné zapůjčení ID zadaná v žádosti o.*

## <a name="solution"></a>Řešení
Chcete-li vyřešit nejčastější problémy, zkuste následujícím způsobem:

### <a name="step-1-delete-any-os-disks-that-are-preventing-deletion-of-the-storage-account-container-or-vhd"></a>Krok 1: Odstranění každého OS disku, který brání odstranění účtu úložiště, kontejneru nebo virtuální pevný disk

1. Přejděte na [portál Azure klasické](https://manage.windowsazure.com/).
2. Vyberte **virtuální počítač** > **DISCÍCH**.

    ![Obrázek disků ve virtuálních počítačích na Azure klasické portálu.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)

3. Vyhledejte disků, které jsou přidružené k účtu úložiště, kontejner nebo virtuálního pevného disku, který chcete odstranit. Pokud zaškrtnete políčko místo disku, bude hledat přidružené úložiště klienta, kontejneru nebo virtuální pevný disk.

    ![Obrázek zobrazující informace o disků umístění na Azure klasické portálu](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)

4. Potvrďte, že žádný OM je uvedená na **připojené k** oblasti disků a potom odstraňte discích.

    > [AZURE.NOTE] Pokud disk je připojen k virtuálního počítače, nebudete moct odstraňte ji. Disků se odpojí od odstraněné OM asynchronní. Může trvat několik minut po odstranění OM pro toto pole zrušit.

### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-the-storage-account-or-container"></a>Krok 2: Odstranění žádné OM obrázky, které brání odstranění účtu úložiště nebo kontejneru

1. Přejděte na [portál Azure klasické](https://manage.windowsazure.com/).
2. Vyberte **virtuální počítač** > **obrázky**a potom odstraňte obrázky, které jsou přidružené k účtu úložiště, kontejner nebo virtuální pevný disk.

    Poté pokusu o odstranění účtu úložiště, kontejner nebo virtuálního pevného disku znovu.

> [AZURE.WARNING] Ujistěte se, že obecnějším údajům všechno, co, který chcete uložit před odstraněním účtu. Není možné obnovit odstraněné úložiště účet nebo načíst těch typů obsahu, který obsahuje před odstraněním. Toto také platí pro všechny zdroje v okně účet: Po odstranění virtuálního pevného disku, objektů blob, tabulky, fronty nebo soubor, se trvale odstraní. Zajistěte, aby byl zdroj není používá.

## <a name="about-the-stopped-deallocated-status"></a>Informace o stavu Zastaveno (odebrána)

VMs vytvořené v modelu klasické nasazení a který zachovají bude mít stav **Zastaveno (uvolnit)** na [Azure portál](https://portal.azure.com/) nebo [Azure klasické portálu](https://manage.windowsazure.com/).

**Azure klasické portálu**:

![Zastaveno (Deallocated) stavu pro VMs na Azure portálu.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)


**Azure portálu**:

![Zastaveno (zrušeny, takže) stav VMs na Azure klasické portálu.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

"Zastaveno (odebrána)" stavu aktualizací počítač zdroje, jako je třeba procesoru, paměti a sítě. Disků, ale pořád uchovávají tak, že můžete rychle znovu vytvořit OM v případě potřeby. Discích vzniká nad VHD, které jsou podporované službou Azure úložiště. Účet úložiště má tyto VHD a disků mít zapůjčení na tyto VHD.

## <a name="next-steps"></a>Další kroky

- [Odstranění účtu úložiště](storage-create-storage-account.md#delete-a-storage-account)
- [Jak se zalamují uzamčené zapůjčení úložiště objektů blob Microsoft Azure (Powershellu)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
