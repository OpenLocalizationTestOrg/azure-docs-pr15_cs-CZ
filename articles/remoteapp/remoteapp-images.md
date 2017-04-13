<properties
    pageTitle="Co je obrázky šablon Azure RemoteApp? | Microsoft Azure"
    description="Informace o obrázky šablon zahrnutých ve službě Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Co je obrázky šablon Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Vaše předplatné Azure RemoteApp zahrnuje tři obrázky šablon:


- Windows Server 2012
- Microsoft Office 365 ProPlus (vyžaduje předplatné Office 365)
- Microsoft Office 2013 Professional Plus (jen zkušební verze)

> [AZURE.IMPORTANT]Vaše předplatné Azure RemoteApp uděluje vám umožňuje přístup k softwaru v obrázcích, s výjimkou Office 365 ProPlus, které vyžadují samostatné předplatné a Office 2013, které nelze použít v výroby. To znamená, že můžete sdílet programy nebo aplikací na obrázky šablon s jinými uživateli. Pokud vytvoříte kolekci, která používá Windows serveru 2012 R2 obrázky, můžete publikovat systém Centrum Endpoint Protection uživatelům přistupovat prostřednictvím RemoteApp.
>
> Podívejte se na [Podrobnosti licencování RemoteApp](remoteapp-licensing.md) Další informace. A informace o licencování [Office pomocí s Azure RemoteApp](remoteapp-o365.md) pro Office.

Přečtěte si informace o obsahuje jednotlivé obrázky.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>Windows Server 2012 R2 ("vanilkové obrázek")
Tento obrázek je založená na operačním systému Microsoft Windows serveru 2012 R2 Datacentra a obsahuje následující rolí a funkce nainstalované požadavkům pro obrázky šablon Azure RemoteApp:


- .NET framework 4.5, 3.5.1, 3.5
- Práce s počítačem
- Služby podpory rukopisu
- Média Foundation
- Host (hostitel) relací vzdálené plochy
- Prostředí Windows PowerShell 4.0
- ISE v prostředí Windows PowerShell
- Podpora WoW64

Tento obrázek obsahuje také v následujících aplikacích nainstalovaný:

- Adobe Flash Playeru
- Program Microsoft Silverlight
- Microsoft System Center 2012 Endpoint Protection
- Microsoft Windows Media Player


## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (předplatné povinné)
Office 365 je často požadovaných aplikace, proto jsme vytvořili "vlastní" obrázek pro práci s.

Tento obrázek je rozšíření vanilkové obrázku a má tyto prvky systému Microsoft Office 365 ProPlus nainstalovaná kromě součásti popsaná v systému Windows Server 2012 R2 obrázek:


- Aplikace Access
- Aplikace Excel
- Lync
- Aplikace OneNote
- OneDrive pro firmy (Všimněte si, že není při použití s Azure RemoteApp podporovaná agenta synchronizace)
- Aplikace Outlook
- Aplikace PowerPoint
- Aplikace Word
- Nástroje kontroly pravopisu Microsoft Office

Obrázek obsahuje taky Visio Pro a Project Pro.

A v následujících aplikacích, například:

- SQL Native client –
- Ovladač ODBC
- Dolování dat SQL serveru klienta
- MasterDataServices klienta
- Microsoft Publisher
- PowerQuery
- Power mapu


Plnou funkčnost Office 365 ProPlus aplikací je dostupný jenom u uživatelů, kteří mají plán Office 365 ProPlus. Podrobné informace o Office 365 plánech předplatného najdete v článku [plán služeb Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx). Máte ještě další otázky? Přečtěte si článek [Office 365 + RemoteApp](remoteapp-o365.md) . Podívejte se na nový v článku [používání předplatného Office 365 s Azure RemoteApp](remoteapp-officesubscription.md)také.

Všimněte si, že budete muset samostatně - licencí Office 365 ProPlus Visio Pro a Project Pro každý mají svoje vlastní licenci.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (jen zkušební verze)
Bezplatnou zkušební období můžete vyzkoušet službu s obrázkem Office 2013.

Tento obrázek je rozšíření vanilkové obrázku a má tyto prvky systému Microsoft Office 2013 Professional Plus nainstalovaný kromě součásti popsaná v systému Windows Server 2012 R2 obrázek:


- Aplikace Access
- Aplikace Excel
- Lync
- Aplikace OneNote
- OneDrive pro firmy (Všimněte si, že není při použití s Azure RemoteApp podporovaná agenta synchronizace)
- Aplikace Outlook
- Aplikace PowerPoint
- Projektu
- Aplikace Visio
- Aplikace Word
- Nástroje kontroly pravopisu Microsoft Office

> [AZURE.IMPORTANT]**Právní informace:** Tento obrázek neobsahuje licenci Microsoft Office a *není možné použít pro výroby*. Obrázek je určen pouze pro zkušební verzi Office 2013 Professional Plus. Pokud chcete používat aplikace Office v Azure RemoteApp pro výrobní, budete muset používat Office 365 ProPlus obrázek. Podrobné informace o licencování Office najdete v článku [použití Office 365 na počítačích Azure RemoteApp](remoteapp-o365.md)
