<properties
   pageTitle="Jak změnit velikost Linux OM | Microsoft Azure"
   description="Jak škálování nebo změnou velikosti OM Neomezovat virtuálního počítače Linux."
   services="virtual-machines-linux"
   documentationCenter="na"
   authors="mikewasson"
   manager="timlt"
   editor=""
   tags=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="mikewasson"/>


# <a name="how-to-resize-a-linux-vm"></a>Jak změnit velikost Linux OM

## <a name="overview"></a>Základní informace 

Když zřizujete virtuálního počítače (OM), můžete změnit měřítko OM nahoru nebo dolů změnou [velikosti OM][vm-sizes]. V některých případech můžete nejdřív zrušit OM. Tomu může dojít, pokud není k dispozici na hardware obrázku, který je hostitelem OM novou velikost.

Tento článek ukazuje, jak změnit velikost Linux OM pomocí [Rozhraní příkazového řádku Azure][azure-cli].

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu.


## <a name="resize-a-linux-vm"></a>Změna velikosti Linux OM 

Pokud chcete změnit velikost virtuálního počítače, proveďte následující kroky.

1. Spusťte tento příkaz rozhraní příkazového řádku. Tento příkaz uvádí velikosti OM, které jsou dostupné na obrázku hardwaru, ve které je hostovaný OM.

    ```
    azure vm sizes -g <resource-group> --vm-name <vm-name>
    ```

2. Pokud je uvedená požadovanou velikost, spusťte tento příkaz změnit velikost OM.

    ```
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    ```

    OM restartuje během tohoto procesu. Po restartování bude přemapovat existující OS a disků data. Všechno, co na dočasné disku budou ztraceny.

    Použití `--enable-boot-diagnostics` možnost umožňuje [spuštění diagnostiky][boot-diagnostics], k přihlášení všechny chyby týkající se po spuštění.

3. Jinak Pokud požadovanou velikost nevidíte, spusťte následující příkazy pro zrušit OM, změnit jeho velikost a restartujte OM.

    ```
    azure vm deallocate -g <resource-group> <vm-name>
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    azure vm start -g <resource-group> <vm-name>
    ```

   > [AZURE.WARNING] Navrácení OM vydání také všechny dynamické IP adresa přiražená bude v angličtině. OS a dat disků nemá vliv.
   
## <a name="next-steps"></a>Další kroky

Další rozšiřitelnost spusťte několika instancích spuštěných OM a rozšiřování. Další informace najdete v tématu [automaticky měřítko Linux počítačích v sadě měřítko virtuálního počítače][scale-set]. 

<!-- links -->
   
[azure-cli]: ../xplat-cli-install.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]: virtual-machines-linux-sizes.md