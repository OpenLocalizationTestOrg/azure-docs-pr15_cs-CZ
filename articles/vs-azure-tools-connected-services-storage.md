<properties 
   pageTitle="Přidání úložiště Azure pomocí připojené služby ve Visual Studiu | Microsoft Azure"
   description="Přidání úložiště Azure do aplikace pomocí dialogu Visual Studio přidat připojené služby"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Přidání Azure úložiště pomocí aplikace Visual Studio připojené služby

## <a name="overview"></a>Základní informace

Visual Studio 2015 můžete připojit všechny C# cloudové služby, .NET back-end mobilních služeb, ASP.NET nebo službu, služby ASP.NET 5 nebo webu služby Azure WebJob k základnímu úložišti Azure pomocí dialogového okna **Přidat připojené služby** . Funkce připojené služby přidá všechny potřebné odkazy a kód připojení a upravuje souborů konfigurace řádně podporovat. Dialogové okno taky přejdete si přečtěte následující dokumentaci informující o další kroky, které se můžou ke spuštění úložiště objektů blob, dotazů a tabulek.

## <a name="supported-project-types"></a>Typy podporovaného projektů

Dialogové okno připojené služby slouží k připojení k úložišti Azure v následujících typech projektu.

- Projekty technologie ASP.NET

- ASP.NET 5 projekty

- .NET cloudové služby Web rolí a pracovní Role projektů

- Projekty .NET Mobilní služby

- Azure WebJob projekty


## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Připojení k úložišti Azure pomocí dialogového okna připojené služby

1. Zkontrolujte, jestli že máte účet Azure. Pokud nemáte účet Azure, můžete zaregistrovat [bezplatnou zkušební verzi](http://go.microsoft.com/fwlink/?LinkId=518146). Až budete mít účet Azure, můžete vytvořit účty úložiště, vytvořte mobilních služeb a konfigurace služby Azure Active Directory.

1. Otevřete projekt ve Visual Studiu, otevřete místní nabídku uzel **odkazy** v Průzkumníku řešení a pak zvolte **Přidat službu připojení**.

    ![Přidání připojené služby](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. V dialogovém okně **Přidat službu připojení** zvolte **Azure úložiště**a potom klikněte na tlačítko **Konfigurace** . Můžete být vyzváni k přihlášení do Azure, pokud jste tak již neučinili.

    ![Přidání dialogové okno připojení služby – úložiště](./media/vs-azure-tools-connected-services-storage/IC796703.png)

1. V dialogu **Úložiště Azure** vyberte existující úložiště účet a klikněte na **Přidat**.

    Pokud potřebujete k vytvoření nového účtu úložiště, přejděte k dalšímu kroku. V opačném případě přejděte ke kroku 6.

    ![Dialogové okno Azure úložiště](./media/vs-azure-tools-connected-services-storage/IC796704.png)

1. Vytvoření nového účtu úložiště: 

    1. Klikněte na tlačítko **vytvořit nový účet úložiště** v dolní části dialogu úložiště Azure.

    1. Vyplňte dialogové okno **Vytvořit účet úložiště** a potom klikněte na tlačítko **vytvořit** .
    
        ![Dialogové okno Azure úložiště](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)

        Až budete zase v dialogu **Úložiště Azure** , se zobrazí v seznamu nové úložiště.

    1. V seznamu vyberte nové úložiště a vyberte **Přidat**.

1. Služba úložiště připojení se zobrazí v části odkaz na službu uzel projektu WebJob.

    ![Azure úložiště ve webovém úlohy projektu](./media/vs-azure-tools-connected-services-storage/IC796705.png)

1. Zkontrolujte, která se zobrazí stránka Začínáme a zjistěte, jak byl upraven projektu. Stránka Začínáme se zobrazí ve vašem prohlížeči pokaždé, když přidáte připojený služby. Projděte si doporučené další kroky a příklady kódu nebo přejděte na stránku příčinách zobrazíte jaké odkazy se přidala do vašeho projektu a jak se změnily kódu a konfigurace soubory.

## <a name="how-your-project-is-modified"></a>Jak se mění projektu

Po skončení dialogu Visual Studio přidá odkazy a upravuje určité konfigurace soubory. Konkrétní změny, závisí na typu projektu. 

 - ASP.NET projektů najdete v článku [Co se stalo – ASP.NET projekty](http://go.microsoft.com/fwlink/p/?LinkId=513126). 
 - ASP.NET 5 projektů najdete v článku [Co se stalo – ASP.NET 5 projekty](http://go.microsoft.com/fwlink/p/?LinkId=513124). 
 - Cloudové služby projekty (role web a pracovní role), najdete v článku [Co se stalo – cloudové služby projekty](http://go.microsoft.com/fwlink/p/?LinkId=516965). 
 - WebJob projektů najdete v článku [Co se stalo - WebJob projekty](./storage/vs-storage-webjobs-what-happened.md).

## <a name="next-steps"></a>Další kroky

1. Použití ukázky Začínáme jako vodítko, typ úložiště, který chcete vytvořit a začněte psát kód pro přístup k účtu úložiště!

1. Pokládat dotazy a nápovědu
     - [Fórum MSDN: Azure úložiště](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)

     - [Blog týmu Azure úložiště](http://blogs.msdn.com/b/windowsazurestorage/)

     - [Úložiště na azure.microsoft.com](https://azure.microsoft.com/services/storage/)

     - [Přečtěte následující dokumentaci úložiště azure.microsoft.com](https://azure.microsoft.com/documentation/services/storage/)

