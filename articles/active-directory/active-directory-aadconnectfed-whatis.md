<properties
    pageTitle="Azure AD Connect a federace | Microsoft Azure"
    description="Tato stránka je centrální umístění pro všechny následující dokumentaci pro týkající se služby AD FS operace pomocí Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="anandy"/>


# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect a federace

Azure AD Connect umožňuje konfigurace federace s místní AD FS a Azure AD. Federace poštu na umožníte uživatelům přihlásit k služby Azure AD na základě jejich místní hesla a v podnikové síti, aniž byste museli znova zadejte své heslo. Možnost federace se službou AD FS umožňuje nasazení nové nebo určit existující AD FS ve Windows serveru 2012 R2 farmy.

Toto téma je na domovské stránce informace o federace související funkce pro odkazy Azure AD Connect a seznamy s další témata s ním souvisejí. Odkazy na Azure AD Connect naleznete v tématu Integrace místních identit s Azure Active Directory.

## <a name="azure-ad-connect---federation-topics"></a>Azure AD Connect - federace témata

| Téma | Co zahrnuje a kdy ke čtení |
|:------|:-----------|
| **Azure AD Connect přihlašovací možností uživatele** ||
| [Principy možností přihlášení uživatele](active-directory-aadconnect-user-signin.md) | Popis různých přihlašovací možností uživatele a jak mají vliv na Azure přihlašovací uživatelské prostředí |
| **AD FS instalace pomocí Azure AD Connect**||
| [Předpoklady](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) | Předpoklady pro úspěšné instalace služby AD FS přes Azure AD Connect|
| [Konfigurace služby AD FS farmy](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) | Jak nainstalovat novou službu AD FS farmu pomocí Azure AD Connect |
| **Změna konfigurace služby AD FS** | |
| [Oprava vztah důvěryhodnosti](active-directory-aadconnect-federation-management.md#reparing-the-trust) | Jak se dají opravit aktuální vztah důvěryhodnosti mezi místní služba AD FS a Office 365 / Azure |
| [Přidání nového server služby AD FS](active-directory-aadconnect-federation-management.md#adding-a-new-ad-fs-server) | Rozbalení farmě služby AD FS další služby AD FS příspěvek počáteční instalace serveru |
| [Přidání nového serveru AD FS WAP](active-directory-aadconnect-federation-management.md#adding-a-new-wap-server) | Rozbalení farmě služby AD FS další WAP příspěvek počáteční instalace serveru |
| [Přidání nového federované domény](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain) | Přidání další domény do federovaní Azure AD |
|**Příspěvek instalace úkoly**||
| [Přidání vlastní firemní logo nebo obrázek](active-directory-aadconnect-federation-management.md#add-custom-company-logo-or-illustration)| Změnit přihlášení zadáním vlastní logo, které se zobrazí na přihlašovací stránce AD FS |
| [Přidání popisu přihlášení](active-directory-aadconnect-federation-management.md#add-sign-in-description) | Změna popisu přihlášení na přihlašovací stránce AD FS | 
| [Změna služby AD FS převzetí pravidel](active-directory-aadconnect-federation-management.md#modifying-ad-fs-claim-rules) | Upravit nebo přidat pravidla deklarace ve službě AD FS odpovídající konfigurace synchronizace Azure AD Connect |


## <a name="additional-resources"></a>Další zdroje informací

* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
* [Nasazení AD FS v Azure](active-directory-aadconnect-azure-adfs.md)
* [Dostupnost křížově zeměpisné AD FS nasazení v Azure pomocí Správce přenosy Azure](active-directory-adfs-in-azure-with-azure-traffic-manager.md)


