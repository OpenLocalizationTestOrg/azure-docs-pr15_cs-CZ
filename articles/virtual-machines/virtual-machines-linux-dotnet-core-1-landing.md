<properties
   pageTitle="Základní DotNet Azure virtuálního počítače kurzu 1 | Microsoft Azure"
   description="Kurz DotNet Core Azure virtuálního počítače"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="automating-application-deployments-to-azure-virtual-machines"></a>Automatizace nasazení aplikace na virtuálních počítačích Azure

Čtyři řady podrobné informace o nasazení a konfiguraci Azure zdroje a aplikace pomocí spravovat zdroje Azure šablony. V této řadě Ukázková šablona nasazeném a zkoumat nasazení šablony. Cíl řady je Vzdělávejte vztah mezi Azure zdroje a obsahují rukou prostředí nasazování šablon plně integrovaný správce prostředků Azure. Tento dokument předpokládá základní úroveň znalostí pomocí Správce prostředků Azure, před spuštěním tento kurz seznámení s základní koncepty správce prostředků Azure.

## <a name="music-store-application"></a>Aplikace hudbu úložiště přihlašovacích údajů

Ukázka použít v této řadě je .net základní aplikací simulace úložištěm hudbu nakupování. Tato aplikace může být nasazené na Linux nebo Windows virtuální systému, ukázka nasazení byly vytvořeny pro obě. Aplikace obsahuje webovou aplikaci a databázi SQL. Před čtení články z této série nasazení aplikace pomocí nasazení tlačítko Najít na této stránce. Plně nasazený Architektura aplikace / Azure vypadá na následujícím obrázku. 

Šablona hudbu úložiště přihlašovacích údajů správce najdete tady [Hudbu úložiště Linux šablony]( https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db)

![Aplikace hudbu úložiště přihlašovacích údajů](./media/virtual-machines-linux-dotnet-core/music-store.png)

Každý z těchto složek, včetně přiřazení šablony JSON je zkontrolován v následujících článcích čtyři.

- [**Architektura aplikací**](./virtual-machines-linux-dotnet-core-2-architecture.md) – součásti aplikace například webů a databází musí být hostované ve počítače Azure zdroje, jako jsou virtuálních počítačích a databáze Azure SQL. Tento dokument provede mapování výpočetním potřeby Azure zdroje a nasazení tyto materiály šablonou správce prostředků Azure. 

- [**Přístup a zabezpečení**](./virtual-machines-linux-dotnet-core-3-access-security.md) – hostování aplikací v Azure, je třeba vzít v úvahu způsob přístupu k aplikaci a různými aplikaci součásti přístup k sobě. Tento dokument obsahuje podrobnosti poskytování a zabezpečení internet aplikace a přístup mezi součástí aplikace.

- [**Dostupnost a měřítko**](./virtual-machines-linux-dotnet-core-4-availability-scale.md) – dostupnost a měřítko odkazují na možnost aplikace dodržovat předpisy spuštění během infrastruktury prostoje a možnost zobrazit výpočetním zdroje podle aplikace služba. Tento dokument podrobnosti součásti potřebných pro nasazení Vyrovnávání zatížení a vysoce dostupných aplikací.

- [**Nasazení aplikace**](./virtual-machines-linux-dotnet-core-5-app-deployment.md) – při instalaci aplikace na virtuálních počítačích Azure metodu prodloužením doby binární soubory aplikace nainstalovaných v počítači virtuální musí považovat za. Tento dokument obsahuje podrobnosti automatizace instalace aplikací pomocí Azure virtuálního počítače vlastní skript rozšíření.

Cíl při vytváření šablon správce prostředků Azure je k automatizaci nasazení Azure infrastruktura a instalace a konfigurace aplikace hostovaných na tuto Azure infrastrukturu. Procházení tyto články poskytuje příklad toto prostředí.

## <a name="deploy-the-music-store-application"></a>Nasazení aplikace hudbu úložiště přihlašovacích údajů

Aplikace úložiště přihlašovacích údajů hudbu můžete nasazené pomocí kliknutím na toto tlačítko.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-linux%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Správce prostředků Azure šablona vyžaduje následující hodnoty parametrů.

|Název parametru |Popis   |
|---|---|
|SSHKEYDATA   | SSH klíčové data použitá k zabezpečený přístup k virtuální počítač. Informace o vytváření dvojici klíč SSH najdete v tématu [Vytvoření SSH klávesové zkratky pro Linux VMs v Azure](virtual-machines-linux-mac-create-ssh-keys.md).  |
|ADMINUSERNAME   | Správce uživatelské jméno, které se použije ve počítače virtuální a databáze SQL Azure.  |
|SQLADMINPASSWORD | Heslo, které slouží k databázi SQL Azure.  |
|NUMBEROFINSTANCES | Počet virtuálních počítačích vytvořit. Každý z těchto virtuálních počítačích hostovat webovou aplikaci Store hudby a všechny přenosy je rozloženy do nich. |
|PUBLICIPADDRESSDNSNAME | Globálně jedinečný název DNS přidružené na veřejnou IP adresu. |

Po dokončení nasazení šablony přejděte na veřejnou IP adresu pomocí libovolného prohlížeče internet. .Net zobrazí základní Hudba Web.

## <a name="next-steps"></a>Další kroky

<hr>

[Krok 1: architektura aplikací šablonami správce prostředků Azure](./virtual-machines-linux-dotnet-core-2-architecture.md)

[Krok 2: přístup a zabezpečení v Azure správce prostředků šablony](./virtual-machines-linux-dotnet-core-3-access-security.md)

[Krok 3 – dostupnost a velkém měřítku v Azure správce prostředků šablony](./virtual-machines-linux-dotnet-core-4-availability-scale.md)

[Krok 4 – nasazení aplikace šablonami správce prostředků Azure](./virtual-machines-linux-dotnet-core-5-app-deployment.md)


