<properties
    pageTitle="Přidání uživatele do kolekce Azure RemoteApp | Microsoft Azure"
    description="Zjistěte, jak přidávat uživatele ke kolekci Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="how-to-add-a-user-to-your-azure-remoteapp-collection"></a>Postup přidání uživatele do kolekce Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Než vaši uživatelé můžete zobrazit a používejte aplikace v Azure RemoteApp, musíte je udělit přístup k vaší kolekci. Sem zadejte tu část snadno: na kartě **Přístupu uživatelů** zadejte informace o účtu pro uživatele a klikněte na značku zaškrtnutí.

Jaké informace o účtu potřebujete? To záleží na typ kolekce vytvořeného (cloudu nebo hybridní) a jestli používáte Office 365 ProPlus v této kolekci.

## <a name="supported-user-identities"></a>Podporované uživatelských identit

Typy jiné kolekce (cloudu porovnání hybridní) podporují použití identit jiného uživatele pro přístup k aplikacím.  

Kolekce hybridní RemoteApp potřebujete nastavení domény infrastruktury služby Active Directory v místní organizaci a tenanta služby Azure Active Directory s integrace s adresářem (a volitelně jednotné přihlášení). Kromě toho je potřeba vytvořit některé objektů služby Active Directory v místním adresáři.  

Kolekce cloudové RemoteApp všichni uživatelé s Azure Active Directory podporují identit můžete udělit uživatelská oprávnění k RemoteApp zahrnout Accounts Microsoft.  Viz tabulka dole.

Uživatelé Office 365 můžou uživatelům Azure Active Directory. Pokud mají hybridní Azure Active Directory, adresář synchronizovány účty, mohou být uděleny přístupu uživatelů v hybridním nasazení RemoteApp.   

V této tabulce můžete použít jako stručný přehled které identity je podporované v kolekci a jaké jsou požadavky na služby Active Directory.

|Uživatelské účty |Cloud   |Hybridní|
|--------------|--------|------|
|Účet Microsoft|     Ano|    Ne|
|Azure Active Directory (Azure AD)| | |
|Azure AD cloudu pouze    |Ano    |Ne |
|ADsync se synchronizací hesel  |Ano    |Ano    |
|ADsync bez synchronizaci hesel|  Ano |Ne |
|ADsync se službou AD FS  |Ano    |Ano    |
|[Azure 3rd výrobců podporované Zprostředkovatelé identit jiní](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (třeba Ping) |Ano    |Ano|
|Vícefaktorové ověřování    |Ano    |Ano    |

Podívejte se na [Další informace](remoteapp-ad.md) o konfiguraci služby Active Directory RemoteApp.


> [AZURE.NOTE] Uživatelé služby Azure Active Directory musí být z klienta, který máte přidružený k předplatnému. (Můžete zobrazit a upravovat vaše předplatné na kartě **Nastavení** na portálu. [Změna klienta služby Azure Active Directory používaný RemoteApp](remoteapp-changetenant.md) Další informace najdete.)

## <a name="office-365-proplus-user-account-information"></a>Informace o Office 365 ProPlus uživatelského účtu
Pokud používáte Office 365 ProPlus šablony ve vaší kolekci *nebo* , pokud jste vytvořili vlastní obrázek, který používá Office 365, můžete je možné přidat Azure Active Directory uživatelů, kteří mají předplatných Office 365 pro výchozí doménu vašeho předplatného. Další informace najdete v tématu [použití Office 365 na počítačích Azure RemoteApp](remoteapp-o365.md) .
