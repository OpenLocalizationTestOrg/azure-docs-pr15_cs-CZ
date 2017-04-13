<properties
    pageTitle="Začínáme s Azure úložiště v pět minut | Microsoft Azure"
    description="Na objektů BLOB Microsoft Azure, tabulky a fronty pomocí spustí Azure úložiště Visual Studiu a emulátoru Azure úložiště ním rychle. Spusťte aplikaci úložišti Azure první pět minut."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="get-started-with-azure-storage-in-five-minutes"></a>Začínáme s Azure úložiště v pět minut

## <a name="overview"></a>Základní informace

Není těžké si Začínáme s vývojem s úložištěm Azure. Tento kurz se dozvíte, jak aplikace úložišti Azure tak rychle a spustit. Rychlý Start šablon zahrnutých v Azure SDK pro .NET použijete. Tyto Snadné spuštění obsahovat připravené k použití kódu, který ukazuje některé základní programovací scénáře s úložištěm Azure.

Další informace o úložišti Azure před začte kód, podívejte se na [Další kroky](#next-steps).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete, musíte následující požadavky:

1. Kompilace a sestavíte aplikaci, musíte mít na verzi [Aplikace Visual Studio](https://www.visualstudio.com/) v počítači nainstalovaný.

2. Nainstalujte nejnovější verzi [Azure SDK pro .NET](https://azure.microsoft.com/downloads/). V SDK obsahuje rychlý úvod Azure ukázkové projekty, emulátoru Azure úložiště a [Azure úložiště klienta knihovny pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

3. Zkontrolujte, jestli [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) nainstalované v počítači, jako je potřeba rychlý úvod Azure ukázkové projekty, které budeme používat v tomto kurzu.

    Pokud si nejste jisti, jakou verzi systému .NET Framework nainstalovaných v počítači, přečtěte si článek [jak: určení .NET Framework verze jsou nainstalované](https://msdn.microsoft.com/vstudio/hh925568.aspx). Nebo stiskněte tlačítko **Start** nebo klávesu Windows, napište **Ovládací panely**. Potom klikněte na **programy** > **programy a funkce**a zjistěte, jestli .NET Framework 4.5 je mezi nainstalovaných aplikací.

4. Musíte mít účet Azure úložiště a Azure předplatného.

    - Předplatné Azure, získáte [Bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/) [Možnosti nákupu](https://azure.microsoft.com/pricing/purchase-options/)a [Nabízí člena](https://azure.microsoft.com/pricing/member-offers/) (u členů MSDN, Microsoft Partner Network a BizSpark a dalších aplikacích Microsoftu).
    - Vytvoření účtu úložiště v Azure, zjistěte, [jak vytvořit účet úložiště](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Spusťte aplikaci první úložišti Azure proti Azure úložiště v cloudu

Až budete mít účet, můžete vytvořit jednoduchý aplikace úložišti Azure pomocí jedné z ukázkové projekty Azure Snadné spuštění ve Visual Studiu. Tento kurz se zaměřuje na ukázkové projekty Azure úložištěm: **úložišti Azure: objektů BLOB**, **úložišti Azure: soubory**, **úložišti Azure: fronty**, a **úložišti Azure: tabulky**:

1. Spusťte aplikaci Visual Studio.
2. V nabídce **soubor** klikněte na **Nový projekt**.
3. V dialogovém okně **Nový projekt** klikněte na **nainstalovat** > **šablony** > **Visual Basic** > **cloudu** > **Rychlé prohlídky** > **Datové služby**.
    na. Vyberte si jednu z následujících šablon: **úložišti Azure: objektů BLOB**, **úložišti Azure: soubory**, **úložišti Azure: fronty**, nebo **úložišti Azure: tabulky**.
    b. Ujistěte se, že **.NET Framework 4.5** je zúžený na cílové rozhraní.
    - 3.c. Zadejte název projektu a vytvořte nové aplikace Visual Studio řešení, jak je znázorněno:

    ![Rychlé zahájení práce Azure][Image1]

Můžete chtít zkontrolovat zdrojového kódu před spuštěním aplikace. Chcete-li zobrazit kód, vyberte na **kartě zobrazení ve Visual Studiu** **Průzkumník řešení** . Potom poklikejte na soubor Program.cs.

Potom spusťte ukázková aplikace:

1.  Ve Visual Studiu vyberte **Průzkumník řešení** v nabídce **Zobrazit** . Otevřete konfiguračního souboru a komentáře se připojovací řetězec pro emulátoru Azure úložiště:

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  Zrušte komentář připojovací řetězec pro službu Azure úložiště a zadejte úložiště název a přístup klíč účtu v konfiguračního souboru:`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    K načtení svůj klíč účtu přístup úložiště, najdete v článku [Správa přístupových kláves úložiště](storage-create-storage-account.md#manage-your-storage-access-keys).

3.  Poté, co zadáte název účtu úložiště a přístupové klávesy v app.config, v nabídce **soubor** klikněte na **Uložit vše** uložit všechny soubory projektu.
4.  V nabídce **vytvořit** klikněte na tlačítko **Sestavit řešení**.
5.  V nabídce **ladění** stisknutím klávesy **F11** spusťte řešení krok za krokem nebo stisknutím klávesy **F5** spusťte řešení.


## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Spusťte aplikaci první úložišti Azure místně proti emulátoru úložiště Azure

[Azure úložiště emulátoru](storage-use-emulator.md) poskytuje místním prostředí, která napodobí objektů Blob Azure, fronty a tabulky služby pro účely vývoj. Emulátoru úložiště můžete použít k testování aplikace úložiště místně, bez vytvoření Azure předplatného nebo účtu úložiště a bez by náklady.

To vyzkoušet, vytvoření aplikace jednoduché úložišti Azure pomocí jedné z ukázkové projekty Azure Snadné spuštění ve Visual Studiu. Tento kurz se zaměřuje na ukázkové projekty **Úložišti objektů Blob Azure**, **Úložiště tabulek Azure**a **Úložiště fronty Azure** :

1. Spusťte aplikaci Visual Studio.
2. V nabídce **soubor** klikněte na **Nový projekt**.
3. V dialogovém okně **Nový projekt** klikněte na **nainstalovat** > **šablony** > **Visual Basic** > **cloudu** > **Rychlé prohlídky** > **Datové služby**.
    na. Vyberte si jednu z následujících šablon: **úložišti Azure: objektů BLOB**, **úložišti Azure: soubory**, **úložišti Azure: fronty**, nebo **úložišti Azure: tabulky**.
    b. Ujistěte se, že **.NET Framework 4.5** je zúžený na cílové rozhraní.
    c. Zadejte název projektu a vytvořte nové aplikace Visual Studio řešení, jak je znázorněno:

    ![Rychlé zahájení práce Azure][Image1]

4.  Ve Visual Studiu vyberte **Průzkumník řešení** v nabídce **Zobrazit** . Pokud jste už přidali jeden otevřete konfiguračního souboru a komentáře se připojovací řetězec pro váš účet Azure úložiště. Potom zrušte komentář připojovací řetězec pro emulátoru Azure úložiště:

    `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Můžete chtít zkontrolovat zdrojového kódu před spuštěním aplikace. Chcete-li zobrazit kód, vyberte na **kartě zobrazení ve Visual Studiu** **Průzkumník řešení** . Potom poklikejte na soubor Program.cs.

Pak spustíte aplikaci vzorku v emulátoru úložiště Azure:

1.  Stiskněte klávesu Windows, vyhledejte *emulátoru úložišti tabulek Microsoft Azure*nebo na tlačítko **Start** a spuštění aplikace. Po spuštění emulátor uvidíte ikonu a v oblasti pro zobrazení úkolů Windows oznámení.
2.  Ve Visual Studiu **Sestavit řešení** v nabídce klikněte na **vytvořit** .
3.  V nabídce **ladění** stisknutím klávesy **F11** spusťte řešení krok za krokem nebo stisknutím klávesy **F5** spusťte řešení od začátku do konce.

## <a name="next-steps"></a>Další kroky

Naleznete v následujících zdrojích zobrazíte další informace o úložišti Azure:

* [Úvod k základnímu úložišti Microsoft Azure](storage-introduction.md)
* [Začínáme s Průzkumníka úložišť Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Začínáme s úložiště objektů Blob Azure pomocí .NET](storage-dotnet-how-to-use-blobs.md)
* [Začínáme s úložiště tabulek Azure pomocí .NET](storage-dotnet-how-to-use-tables.md)
* [Začínáme s úložištěm fronty Azure pomocí .NET](storage-dotnet-how-to-use-queues.md)
* [Začínáme s úložiště souborů Azure v systému Windows](storage-dotnet-how-to-use-files.md)
* [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
* [Si přečtěte následující dokumentaci Azure úložiště](https://azure.microsoft.com/documentation/services/storage/)
* [Microsoft Azure úložiště klienta knihovny pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Služby Azure úložiště rozhraní REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png
