<properties
    pageTitle="Vytvořit jednotku D: OM disk dat | Microsoft Azure"
    description="Popisuje, jak změnit písmena pro Windows OM tak, že používáte jednotku D: jako jednotka data."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Použití jednotku D: jako jednotka dat na Windows OM 

Pokud potřebujete-li jednotku D pro uložení dat aplikace, postupujte podle těchto pokynů pro účely jiné písmeno dočasné disku. Nikdy umožňuje dočasné disku obsahují data, která je nutné zachovat.

Pokud změně velikosti nebo **Zastavit (Deallocate)** virtuálního počítače, tato aktivace umístění virtuálního počítače do nového hypervisor. Plánované nebo neplánované údržbě mohou také spustit toto umístění. V tomto scénáři bude dočasné disku přiřazen k první písmeno dostupné jednotky. Pokud máte aplikaci, která konkrétně vyžaduje jednotku D:, budete muset tímto postupem dočasně přesunutí pagefile.sys, připojit nová data disku a přiřadit mu písmeno D a potom pagefile.sys zpět na dočasné jednotku. Po dokončení Azure nevezme zpět D: Pokud OM přesune do různých hypervisor.

Další informace o použití dočasné disku Azure najdete v článku [Principy dočasné jednotka na virtuálních počítačích Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="attach-the-data-disk"></a>Připojte disk dat

Nejdřív musíte připojte disk dat do virtuálního počítače. 

- Používat portál, přečtěte si, [jak připojit disk dat na portálu Azure](virtual-machines-windows-attach-disk-portal.md)
- Používat portál klasického naleznete v článku [připojení disk dat do virtuálního počítače Windows](virtual-machines-windows-classic-attach-disk.md). 


## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Dočasně přesuňte pagefile.sys disk C

1. Připojení k virtuální počítač. 

2. Klikněte pravým tlačítkem myši na nabídku **Start** a vyberte **systém**.

3. V levé nabídce vyberte **Upřesnit nastavení systému**.

4. Ve skupinovém rámečku **výkon** vyberte **Nastavení**.

5. Vyberte kartu **Upřesnit** .

5. V části **virtuální paměť** klikněte na **změnit**.

6. Vyberte jednotku **C** a potom klikněte na **Velikost určí systém** a klikněte na **Nastavení**.

7. Vyberte jednotku **D** a potom klikněte na **bez stránkovací soubor** a klikněte na **Nastavení**.

8. Klikněte na použít. Zobrazí se upozornění, že počítači se musí restartovat změny se projeví.

9. Restartujte virtuální počítač.




## <a name="change-the-drive-letters"></a>Změna písmenům 

1. Po restartování OM se znovu přihlaste OM.

2. Klepněte na tlačítko **Start** a zadejte **diskmgmt.msc** a stiskněte klávesu Enter. Správa disků začnou.

3. Klikněte pravým tlačítkem myši na **D**jednotku dočasné úložiště a vyberte **Změnit písmeno jednotky a cest**.

4. V části písmeno vyberte jednotku **G** a potom klikněte na **OK**. 

5. Klikněte pravým tlačítkem myši na disku data a vyberte **Změnit písmeno jednotky a cest**.

6. V části písmeno vyberte jednotku **D** a potom klikněte na **OK**. 

7. Klikněte pravým tlačítkem myši na **G**, jednotku dočasné úložiště a vyberte **Změnit písmeno jednotky a cest**.

8. V části písmeno vyberte jednotku **E** a pak klikněte na **OK**. 

> [AZURE.NOTE] Pokud vaše OM jiných disků nebo jednotky, použijte metodu stejné Chcete-li přiřadit písmena disku a jednotky. Chcete, aby konfigurace disku je:  
>- C: OS disku  
>- D: datový Disk  
>- E: dočasné disku



## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Přesunutí pagefile.sys jednotku dočasné úložiště 

1. Klikněte pravým tlačítkem myši na nabídku **Start** a vyberte **systém**

2. V levé nabídce vyberte **Upřesnit nastavení systému**.

3. Ve skupinovém rámečku **výkon** vyberte **Nastavení**.

4. Vyberte kartu **Upřesnit** .

5. V části **virtuální paměť** klikněte na **změnit**.

6. Vyberte jednotku s operačním systémem **C** a klikněte na **bez stránkovací soubor** a potom klikněte na **Nastavení**.

7. Vyberte jednotku dočasné úložiště **E** a potom klikněte na **Velikost určí systém** a klikněte na **Nastavení**.

8. Klikněte na tlačítko **použít**. Zobrazí se upozornění, že počítači se musí restartovat změny se projeví.

9. Restartujte virtuální počítač.




## <a name="next-steps"></a>Další kroky
- Dostupné úložiště do počítače virtuální zvyšují připojením [disk další data](virtual-machines-windows-attach-disk-portal.md).



