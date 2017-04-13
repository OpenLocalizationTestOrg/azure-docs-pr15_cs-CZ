<properties
   pageTitle="Povolení nebo zakázání sledování Azure OM"
   description="Popisuje, jak povolit nebo zakázat sledování Azure OM"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="enable-or-disable-azure-vm-monitoring"></a>Zapnutí nebo vypnutí sledování Azure OM

Tato část popisuje, jak povolit nebo zakázat sledování na virtuálních počítačích se systémem Azure. Ve výchozím nastavení sledování je povolený na Azure virtuálních počítačích nasazení z [Azure portál](https://portal.azure.com) a sledování grafy jsou k dispozici ve výchozím nastavení tečkou 1 minuty. Můžete povolit nebo zakázat sledování pomocí portálu nebo rozhraní Azure příkazového řádku pro Windows (rozhraní příkazového řádku Azure), Mac a Linux. 

## <a name="enable--disable-monitoring-through-the-azure-portal"></a>Povolení nebo zakázání sledování portálu Azure
 
Můžete povolit sledování Azure OM, který obsahuje údaje o vaší instance v období 1 minuty. (použít změny úložiště). Podrobné diagnostiky dat bude k dispozici pro OM v portálu grafy nebo prostřednictvím rozhraní API. Ve výchozím nastavení Azure portále sledování, ale můžete vypnout ho podle níže uvedeného popisu. Můžete povolit sledování při OM běží nebo ve stavu Zastaveno.

- Otevřete Azure portálu na ** [https://portal.azure.com](https://portal.azure.com)**

- V levém navigačním panelu klikněte na virtuálních počítačích.

- V seznamu virtuálních počítačích vyberte instanci pracovního nebo přestal. Otevře se blad virtuálního počítače.

- Klikněte na "Všechna nastavení".

- Klikněte na "Diagnostika".

- Změna stavu zapnuto nebo vypnuto. Můžete také vybrat v tomto zásuvné úroveň sledování podrobnosti, které chcete povolit virtuálního počítače.

[Azure.Note] Diagnostika na přepínač je výchozí nastavení při vytváření nové virtuálního počítače

![Povolení nebo zakázání sledování portálu Azure.][1]


## <a name="enable--disable-monitoring-with-azure-cli"></a>Povolení nebo zakázání sledování s Azure rozhraní příkazového řádku
 
Chcete-li povolit sledování OM Azure.

- Vytvoření souboru s názvem například PrivateConfig.json následující obsah.
        {"storageAccountName": "úložiště účet, který chcete zobrazit data", "storageAccountKey": "klíč účtu"}
- Spusťte tento příkaz Azure rozhraní příkazového řádku.

        azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.0 --private-config-path PrivateConfig.json

[Azure.Note] Můžete změnit z verze 2.0 na pozdější verzi-li k dispozici. 

Další informace o konfiguraci sledování metriky a výběry, najdete dokument – **[Pomocí Linux diagnostiky rozšíření výkon a dat diagnostiky OM Linux Monitor](virtual-machines-linux-classic-diagnostic-extension.md).

<!--Image references-->
[1]: ./media/virtual-machines-linux-vm-monitoring/portal-enable-disable.png
 

