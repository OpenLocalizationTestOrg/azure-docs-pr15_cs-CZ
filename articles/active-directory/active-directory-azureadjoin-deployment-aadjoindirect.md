<properties
    pageTitle="Příklady použití a nasazení aspektech pro připojení Azure AD | Microsoft Azure"
    description="Vysvětluje, jak správce může nastavit Azure AD připojení koncových uživatelů (zaměstnanců, studenty, jiní uživatelé). Také popisuje různé reálný scénáře pro používání Azure AD připojení."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

< značky ms.workload="identity ms.service="active directory"" ms.tgt_pltfrm="na" ms.devlang="na" ms.topic="article" ms.date="09/27/2016"

    ms.author="femila"/>

# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Příklady použití a nasazení aspektech pro Azure AD připojení

## <a name="usage-scenarios-for-azure-ad-join"></a>Scénáře použití Azure AD připojení
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Scénář 1: Firmy převážně v cloudu

Azure Active Directory připojení (připojit se ke Azure AD) vám může hodit aktuálně ovládání a správy identit vyhovovaly v cloudu nebo jsou přesunutí do cloudu brzy bude k dispozici. Můžete použít účet, který jste vytvořili v Azure AD pro přihlášení k Windows 10. Prostřednictvím [proces prvního spuštění prostředí (FRX)](active-directory-azureadjoin-user-frx.md)nebo spojením Azure AD z [nabídky nastavení](active-directory-azureadjoin-user-upgrade.md)se můžete připojit svých počítačích Azure AD uživatelů.  Uživatele můžete taky moct užít dobrý pocit jednoho přihlašování (SSO) aplikace access do cloudu zdrojů, jako jsou Office 365 v prohlížeči nebo v aplikacích Office.

### <a name="scenario-2-educational-institutions"></a>Scénář 2: Vzdělávací instituce.

Vzdělávací instituce mají obvykle dva typy uživatele: pedagogický sbor a studenty. Členové pedagogové jsou považovány za dlouhodobé členů organizace. Vytváření místních účtů pro ně je žádoucí. Ale studenti jsou shorter-term členů organizace a dá se ovládat svých účtů v Azure AD. To znamená, že měřítko adresáře můžete posune do cloudu místo jsou uložené místního. Také znamená to, že studentů bude moct přihlášení k Windows pomocí svých účtů Azure AD a získat přístup k Office 365 zdrojů v aplikacích Office.

### <a name="scenario-3-retail-businesses"></a>Scénář 3: Maloobchodní podniky

Maloobchodě mít sezónní pracovníků a dlouhodobé zaměstnanců. Obvykle vytvářejí místních účtů a určenou dlouhodobé úvazek doméně počítačích. Ale sezónní je shorter-term členů organizace a je vhodné ke správě svých účtů kde uživatelské licence snadněji přesouvat kolem. Při vytvoření jejich uživatelských účtů v cloudu s licencí Office 365 potřebujete tyto uživatele výhodách přihlášení k Windows a Office aplikace se účet Azure AD při zachování větší flexibilitu s jejich licence po odejdou.

### <a name="scenario-4-additional-scenarios"></a>: 4 Další scénáře

Spolu s výhody bylo popsáno dříve, můžete využívat mají uživatelé spojení jejich zařízení a Azure AD kvůli zjednodušené spojování prostředí, Správa efektivně zařízení, zápis správy automatické mobilní zařízení a jednotné přihlašování Azure AD a místních zdrojů.  


## <a name="deployment-considerations-for-azure-ad-join"></a>Nasazení aspektech pro Azure AD připojení

### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Povolení uživatelům připojení zařízení vlastnictví společnosti přímo do Azure AD


Podniky můžete poskytnout účty jen cloud partner společnosti a organizace. Tyto partnerů můžete pak snadný přístup k aplikace společnosti a zdroje s jednotného přihlašování. Tento scénář je k dispozici uživatelé s přístupem zdroje primárně v cloudu, například Office 365 nebo SaaS aplikace, které jsou závislé na Azure AD pro ověřování.

### <a name="prerequisites"></a>Zjistit předpoklady pro
**Na úrovni organizace (správce)**

*   Azure předplatné s Azure Active Directory  

**Na úrovni uživatele**

*   Windows 10 (edice Professional a Enterprise)

### <a name="administrator-tasks"></a>Úlohy správy
* [Nastavení registrace zařízení](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Úkoly uživatele
* [Nastavení nové zařízení Windows 10 s Azure AD během instalace](active-directory-azureadjoin-user-frx.md)
* [Nastavení zařízení s Windows 10 s Azure AD v nabídce nastavení](active-directory-azureadjoin-user-upgrade.md)
* [Připojit se ke osobní zařízení Windows 10 pro vaši organizaci](active-directory-azureadjoin-personal-device.md)



## <a name="enable-byod-in-your-organization-for-windows-10"></a>Povolení BYOD ve vaší organizaci pro Windows 10
Můžete nastavit uživatele a zaměstnanců používat své osobní zařízení Windows (BYOD) pro přístup k aplikace společnosti a prostředky. Uživatele můžete přidat jejich Azure AD účty (pracovní nebo školní účty) osobní Windows zařízení při přístupu k prostředkům zabezpečené a požadavkům způsobem.

### <a name="prerequisites"></a>Zjistit předpoklady pro
**Na úrovni organizace (správce)**

*   Azure AD předplatného

**Na úrovni uživatele**

*   Windows 10 (edice Professional a Enterprise)


### <a name="administrator-tasks"></a>Úlohy správy

* [Nastavení registrace zařízení](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Úkoly uživatele
* [Připojit se ke osobní zařízení Windows 10 pro vaši organizaci](active-directory-azureadjoin-personal-device.md)


## <a name="additional-information"></a>Další informace
* [Windows 10 Enterprise: způsoby použití zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu na zařízeních s Windows 10 až Azure Active Directory připojit se ke](active-directory-azureadjoin-user-upgrade.md)
* [Ověření identity bez hesla prostřednictvím Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Další informace o použití scénáře pro Azure AD připojení](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojte zařízení doméně Azure AD pro Windows 10 prostředí](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení připojení Azure AD](active-directory-azureadjoin-setup.md)
