<properties 
    pageTitle="Použití ReportViewer webu | Microsoft Azure"
    description="Toto téma popisuje, jak vytvářet webu Microsoft Azure s ovládacím prvkem Visual Studio ReportViewer zobrazující sestavou uloženou na aplikace Microsoft Azure virtuálního počítače."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management" />
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Použití ReportViewer webu hostované v Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Je možné vytvářet webu Microsoft Azure s ovládacím prvkem Visual Studio ReportViewer zobrazující sestavou uloženou na aplikace Microsoft Azure virtuálního počítače. Ovládací prvek ReportViewer je ve webové aplikaci vytvořit pomocí šablony webové aplikace.

>[AZURE.IMPORTANT] Předlohy ASP.NET MVC webové aplikace nepodporují ovládací prvek ReportViewer.

Do vašeho webu Microsoft Azure zahrnout ReportViewer, budete muset udělali úkoly podle těchto pokynů.

- **Přidání** Sestavení balíček pro nasazení

- **Konfigurace** A mohli ověřovat

- **Publikování** webové aplikace k Azure

## <a name="prerequisites"></a>Zjistit předpoklady pro

Přečtěte si část "obecné doporučení a osvědčené postupy" v [SQL Server Business Intelligence v Azure virtuálních počítačích](virtual-machines-windows-classic-ps-sql-bi.md).

>[AZURE.NOTE] Ovládací prvky ReportViewer jsou odeslané se Visual Studiu standardní vydání nebo vyšší verzí. Pokud používáte edici Web Developer Express, musíte nainstalovat [MICROSOFT sestavy PROHLÍŽEČ 2012 RUNTIME](https://www.microsoft.com/download/details.aspx?id=35747) využívat funkce runtime ReportViewer.
>
>ReportViewer nakonfigurováno v režimu místní zpracování nepodporuje Microsoft Azure.

Prohlédněte si dokument white paper [ovládacího prvku prohlížeče sestav služby Reporting Services a Microsoft Azure virtuálního počítače na základě serverech sestav](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx).

## <a name="adding-assemblies-to-the-deployment-package"></a>Přidání sestavení balíček pro nasazení

Když hostitelem vaší ASP.NET aplikace na místní sestavení ReportViewer obvykle instalují přímo do globální mezipaměti sestavení (GAC) serveru IIS během instalace aplikace Visual Studio a můžete přistupovat přímo pomocí aplikace. Ale pokud hostujete ASP.NET aplikace v cloudu, Microsoft Azure neumožňuje všechno, co je třeba nainstalovat do GAC, tak je nutné zajistit, aby že sestavení ReportViewer jsou k dispozici místně aplikace. Můžete to udělat přidáním odkazů na ně v projektu a nakonfigurovat je zkopírovala místně.

V režimu vzdáleného zpracování ovládací prvek ReportViewer používá následující sestavení:

- **Microsoft.ReportViewer.WebForms.dll**: obsahuje ReportViewer kód, který je potřeba použít ReportViewer na stránce. Odkaz pro toto sestavení se přidá do projektu při přetažení ovládacího prvku ReportViewer stránku ASP.NET v projektu.

- **Microsoft.ReportViewer.Common.dll**: obsahuje třídy používané ovládacím prvkem ReportViewer za běhu. Nebude automaticky přidána do projektu.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Chcete-li přidat odkaz na Microsoft.ReportViewer.Common

- Klikněte pravým tlačítkem myši do projektu **odkazy** a zvolte **Přidat odkaz**, vyberte sestavení .NET karty a klikněte na **OK**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>Zpřístupnění sestavení místně aplikací ASP.NET

1. Ve složce **odkazy** klikněte na sestavení Microsoft.ReportViewer.Common tak, aby jeho vlastností zobrazí v podokně Vlastnosti.

1. V podokně vlastnosti nastavena na hodnotu True **Místní kopie** .

1. Opakujte kroky 1 a 2 pro Microsoft.ReportViewer.WebForms.

### <a name="to-get-reportviewer-language-pack"></a>Chcete-li získat jazykovou sadu ReportViewer

1. Nainstalujte odpovídající balíček redistributable Microsoft sestavy prohlížeč 2012 Runtime ze [Služby Stažení softwaru](http://go.microsoft.com/fwlink/?LinkId=317386).

1. V rozevíracím seznamu vyberte jazyk a na stránce přesměrována odpovídající stránku Centrum pro stažení.

1. Klikněte na **Stáhnout** spusťte stahování ReportViewerLP.exe.

1. Po stažení ReportViewerLP.exe klikněte na **Spustit** ihned nainstalovat, a klikněte na tlačítko **Uložit** uložte do svého počítače. Pokud kliknete na tlačítko **Uložit**, mějte na paměti název složky, kde je uložit.

1. Najděte složku, kam jste soubor uložili. Klikněte pravým tlačítkem ReportViewerLP.exe, klikněte na **Spustit jako správce**a potom klikněte na **Ano**.

1. Po spuštění ReportViewerLP.exe, zobrazí se c:\windows\assembly má zdroj soubory **Microsoft.ReportViewer.Webforms.Resources** a **Microsoft.ReportViewer.Common.Resources**.

### <a name="to-configure-for-localized-reportviewer-control"></a>Konfigurace ovládacího prvku ReportViewer lokalizované

1. Stáhněte a nainstalujte Microsoft sestavy prohlížeč 2012 Runtime redistributable balíček podle pokynů uvedených výše zadané.

1. Vytvoření <language> soubory složky v projektu a kopírovat sestavení přidružené zdroje tam. Kopírování souborů sestavení zdroje jsou: **Microsoft.ReportViewer.Webforms.Resources.dll** a **Microsoft.ReportViewer.Common.Resources.dll**. Vyberte soubory sestavení zdroj a v podokně Vlastnosti nastavte **Kopírovat do adresáře výstup** "**Vždy kopírovat**".

1. Nastavení jazykové verze & UICulture web projektu. Další informace o tom, jak nastavit jazykovou verzi a jazykovou pro webová stránka ASP.NET najdete v tématu [jak: nastavení jazykové verze a jazykovou globalizačních webová stránka ASP.NET](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Konfigurace a tak mohli ověřovat

ReportViewer je potřeba použít správné přihlašovací údaje k ověření se serverem sestavy a její přihlašovací údaje, musíte mít oprávnění tak, že server sestav k otvírat sestavy, které chcete. Další informace o ověřování najdete v článku dokument white paper [ovládacího prvku prohlížeče sestav služby Reporting Services a Microsoft Azure virtuálního počítače na základě serverech sestav](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-the-aspnet-web-application-to-azure"></a>Publikování webové aplikace na Azure

O publikování webové aplikace ASP.NET na Azure najdete v článku [jak: migrace a publikovat webové aplikace Azure z aplikace Visual Studio](../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) a [Začínáme s aplikací Web Apps a ASP.NET](../app-service-web/web-sites-dotnet-get-started.md).

>[AZURE.IMPORTANT] Příkaz Přidat Azure nasazení nebo projekt přidat Azure cloudové služby není zobrazena v místní nabídce v okně Průzkumník řešení, budete muset změnit framework cílového projektu na .NET Framework 4.
>
>Dva příkazy poskytnutí v podstatě stejných funkcí. Jedna nebo na příkaz se zobrazí v místní nabídce podle toho, jakou verzi systému Microsoft Azure SDK jste nainstalovali.

## <a name="resources"></a>Zdroje informací

[Zprávy zasílané společnosti Microsoft](http://go.microsoft.com/fwlink/?LinkId=205399)

[SQL Server Business Intelligence v Azure virtuálních počítačích](virtual-machines-windows-classic-ps-sql-bi.md)

[Použití Powershellu ke vytvoření Azure OM pomocí serveru sestav nativním režimu](virtual-machines-windows-classic-ps-sql-report.md)

[Vytváření sestav ovládacího prvku prohlížeče sestav služeb a Microsoft Azure virtuálního počítače na základě serverech sestav](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)
