
<properties 
    pageTitle="Azure AD + požadavky služby Active Directory Azure RemoteApp | Microsoft Azure" 
    description="Informace o nastavení služby Active Directory pro práci s Azure RemoteApp." 
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



# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + požadavky služby Active Directory Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.


Kolekci hybridní Azure RemoteApp nebo cloudu kolekce, kterou chcete vytvořit federaci pomocí AD Connect musíte postupujte takto.

### <a name="connect-azure-ad-and-active-directory"></a>Připojení Azure AD a služby Active Directory

Pokud budete chtít vašeho klienta Azure AD a místním prostředím služby Active Directory připojení, použijte AD Connect. Chvíli potrvá můžete jenom [4 kliknutími](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) připojení dva adresáře.

Poznámka: synchronizace adresářů je nutný pro hybridní kolekce.

### <a name="make-sure-your-domaincom-match"></a>Přesvědčte se, zda vaše "@domain.com" podle
Než začnete, ujistěte se, že UPN pro místní domény shoduje přípony domain Azure AD. 

Po nastavení domény přípony UPN v Azure AD všem uživatelům Azure RemoteApp přihlášení se přihlásit jako “user@ <the suffix you set up>". Ujistěte se, že uživatelé mohli připojit také pod stejným účtem user@suffix do místní domény. V některých případech můžete nastavit jeden doménou v Azure AD při určování jinou doménu UPN pro uživatele na prem. V tomto případě vaši uživatelé nebude možné se připojit k doméně počítače nebo zdroje přes Azure RemoteApp.

Řekněme, že nastavení vaší domény přípony UPN v AAD jako contoso.com, ale někteří uživatelé na místní nebo AD nakonfigurované pro přihlášení @contoso.uk, pak tito uživatelé nebudou moct správně se přihlásit k odběru ARA. Uživatelé UPN v AAD a AD musí být stejný pro přihlášení k možné"

### <a name="create-objects-for-azure-remoteapp"></a>Vytvořit objekty pro Azure RemoteApp
Potřebujete vytvořit následující místní služby Active Directory objekty:

- Účet služby poskytnutí přístupu k prostředkům domény pro aplikace RemoteApp spojením RDSH koncové body v místní doméně.
- Organizační jednotce (OU) obsahovat objekty RemoteApp počítače. Použití na organizační jednotku doporučené (ale není povinná) vyčlenění účty a zásady budete používat s RemoteApp.

Je nutné mít tyto objekty když vytvoříte kolekci RemoteApp proto na začátku udělat tyto kroky.