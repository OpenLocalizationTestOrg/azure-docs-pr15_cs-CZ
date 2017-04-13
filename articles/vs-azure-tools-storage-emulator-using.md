<properties 
   pageTitle="Konfigurace a používání aplikace Visual Studio emulátoru úložiště | Microsoft Azure"
   description="Konfigurace a používání emulátoru úložiště pomocí aplikace Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Konfigurace a používání emulátoru úložiště pomocí aplikace Visual Studio

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Základní informace
Vývojové prostředí Azure SDK obsahuje emulátoru úložiště, nástroj, který napodobuje objektů Blob fronty a tabulky úložiště služby dostupné v Azure v počítači místní rozvoj. Pokud jste vytváření do cloudové služby, které využívá služby Azure úložiště nebo psaní externí aplikace, která volá služby úložiště, můžete otestovat kódu místně proti emulátoru úložiště. Azure nástroje pro Microsoft Visual Studio rozšiřte Visual Studio správy emulátoru úložiště. Nástroje Azure inicializace databázi úložiště emulátoru při prvním použití, spustí služba úložiště emulátoru při spuštění nebo ladění kódu z aplikace Visual Studio a poskytuje jen pro čtení přístup k datům emulátoru úložiště pomocí Průzkumníka úložiště Azure.

Podrobné informace o emulátoru úložiště, včetně systémové požadavky a pokyny ke konfiguraci vlastní najdete v článku [použití emulátor Azure úložiště pro vývoj a testování](./storage/storage-use-emulator.md).

>[AZURE.NOTE] Existují některé rozdíly ve funkci simulace emulátoru úložiště a služby Azure úložiště. Najdete v článku [rozdíly mezi emulace úložiště a služby Azure úložiště](./storage/storage-use-emulator.md) v dokumentaci Azure SDK informace o určité rozdíly.

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Konfigurace připojovací řetězec pro emulátoru úložiště

Přístup k emulátoru úložiště z kódu v roli, budete chtít Konfigurace připojovací řetězec, který odkazuje na emulátoru úložiště a později, můžete změnit tak, aby ukazovaly na účet Azure úložiště. Připojovací řetězec je nastavení konfigurace čitelného pro vaše role pro připojení k účtu úložiště za běhu. Další informace o tom, jak vytvořit připojovací řetězec najdete v článku [Konfigurace aplikace Azure](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage).

>[AZURE.NOTE] Vrátit odkaz na účtu emulátoru úložiště v kódu pomocí vlastnosti **DevelopmentStorageAccount** . Tento postup funguje správně, pokud chcete získat přístup z vašeho kódu emulátoru úložiště, ale pokud chcete publikovat aplikaci Azure, je potřeba vytvořit připojovací řetězec pro přístup ke svému účtu Azure úložiště a upravit kód k použití tohoto připojovací řetězec před publikováním. Pokud přecházíte mezi účtem emulátoru úložiště a účet Azure úložiště často, bude tento proces zjednodušit připojovací řetězec.

## <a name="initializing-and-running-the-storage-emulator"></a>Inicializace a spuštěna skladování emulátoru

Můžete určit, že při spuštění nebo ladění služby ve Visual Studiu Visual Studio se automaticky spustí emulátoru úložiště. V Průzkumníku otevření místní nabídky pro váš projekt **Azure** a zvolte **Vlastnosti**. Na kartě **vývoj** v seznamu **Spustit emulátoru Azure úložiště** zvolte **True** (pokud už není nastavena na hodnotu).

Při prvním spuštění nebo ladění služby z aplikace Visual Studio emulátoru úložiště spustí proces inicializace. Tento proces rezervy místní porty pro emulátoru úložiště a vytvoří databázi emulátoru úložiště. Po dokončení tohoto procesu nemusí znovu spustit, pokud databázi emulátoru úložiště odstraněna.

>[AZURE.NOTE] Počínaje. června 2012 nástroje Azure, emulátoru úložiště získáte ve výchozím nastavení v SQL Express LocalDB. Ve starších verzích nástroje Azure emulátoru úložiště spustí výchozí instanci systému SQL Express 2005 nebo 2008, které je třeba nainstalovat před instalací Azure SDK. Můžete taky spustit emulátoru úložiště proti pojmenované instance serveru SQL Express nebo pojmenované nebo výchozí instanci aplikace Microsoft SQL Server. Pokud potřebujete ke konfiguraci emulátoru úložiště spustit instanci než výchozí instanci, najdete v článku [Emulátor Azure úložiště pro vývoj a testování](./storage/storage-use-emulator.md).

Úložiště emulátoru poskytuje uživatelské rozhraní a zobrazení stavu používaných služeb místní úložiště chcete spustit, zastavit a obnovit je. Po spuštění emulátoru služba úložiště můžete zobrazit v uživatelském rozhraní nebo spustit nebo zastavit službu kliknutím pravým tlačítkem na ikonu v oznamovací oblasti pro emulátoru Microsoft Azure na hlavním panelu Windows.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Zobrazení dat emulátoru úložiště v Průzkumníku serveru

Uzel Azure úložiště v Průzkumníku serveru umožňuje zobrazit data a změnit nastavení objektů blob a tabulky data z účtů úložiště, včetně emulátoru úložiště. Další informace naleznete v tématu [procházení a Správa zdrojů úložiště v Průzkumníkovi serveru](https://msdn.microsoft.com/library/azure/ff683677.aspx) .
