<properties
    pageTitle="Přehled vlastní názvy domén v Azure Active Directory | Microsoft Azure"
    description="Tento článek vysvětluje koncepční rámec pro použití vlastní názvy domén v Azure Active directory, včetně federace pro jednotné přihlašování"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Přehled vlastní názvy domén v Azure Active Directory

Název domény je důležitou součástí identifikátor pro mnoho prostředků adresáře: je součástí uživatelské jméno nebo e-mailovou adresu pro uživatele, část adresy pro skupinu a může být součástí aplikace ID URI pro aplikaci. Zdroj v Azure Active Directory (Azure AD) mohou obsahovat název domény, která se už ověří, vlastněná adresář, který obsahuje daný zdroj. Jenom globální správce může v Azure AD provádět úlohy správy domény.

Názvy domén v Azure AD jsou jedinečné. Název domény může používat jediné Azure AD. Pokud jeden adresář Azure AD ověřil název domény, můžete žádný Azure AD adresář ověření nebo použijte tento stejný název domény.

## <a name="initial-and-custom-domain-names"></a>Názvy počáteční a vlastních domén

Každý název domény v Azure AD je buď původní název domény, nebo vaší vlastní doménou.

Každý Azure AD je součástí původní název domény v contoso.onmicrosoft.com formuláře. Tento třetí úrovně doménou, v tomto příkladu "contoso.onmicrosoft.com," byl vytvořen, když v adresáři vytvořil, obvykle správce, která vytvořila adresář. Původní název domény pro adresář nelze změnit nebo odstranit. Původní název domény, během plně funkční je určená hlavně má být použit jako mechanismus samozaváděcí až po ověření vaší vlastní doménou.

Ve většině provozním prostředí v adresáři obsahuje alespoň jeden ověření vlastní domény, například "contoso.com," a je tuto vlastní doménu, které jsou viditelné pro koncové uživatele. Vlastní název domény je název domény které vlastní a používá, které organizace, například "contoso.com," pro použití například hostingu jeho webu. Tento název domény je známé zaměstnancům, protože je součástí uživatelské jméno, které jim anonymní se přihlásit k podnikové síti nebo odeslat a načíst e-mailu.

Předtím, než ji může používat Azure AD, musí být vlastní název domény přidané do vašeho adresáře a ověření.

## <a name="verified-and-unverified-domain-names"></a>Názvy domén ověřenou a neověřený

Původní název domény pro adresář je implicitně vyhodnocena jako ověřený Azure AD. Když správce přidá Azure AD vlastní název domény, je zpočátku ve stavu neověřené. Azure AD nepovolí zdroje adresáře používat neověřený doménou. Zajistíte, že pouze jeden adresář můžete použít název určité domény a organizace pomocí názvu domény skutečně vlastní tento název domény.

Azure AD ověří vlastnictví názvu domény tak, že hledáte konkrétní položky v souboru zóny služby (DNS) název domény pro název domény. Pokud chcete ověřit vlastnictví názvu domény, správce získá položku DNS z Azure AD, že Azure AD zkontroluje a přidá tuto položku do souboru zóny DNS pro název domény. Soubor zóny DNS spravují doménového registrátora domény u této domény. Kroky k ověření domény najdete v článku Přidání [vlastní domény do adresáře Azure AD](active-directory-add-domain.md).

Přidání položky DNS do souboru zóny pro název domény nemá vliv na dalších doménových služeb, jako jsou e-mailu nebo webového hostingu.

## <a name="federated-and-managed-domain-names"></a>Názvy domén federované a spravovaných

Abyste uživatelům poskytli federované přihlášení v prostředí mezi místní služby Active Directory a Azure AD je možné konfigurovat vlastní název domény v Azure AD. Konfigurace domén pro federaci vyžaduje aktualizace privilegovaných zdrojů v Azure AD a také do vašeho systému Windows Server služby Active Directory. Konfigurace prováděny z federované domény musí být z Azure AD Connect nebo pomocí Powershellu. Federování vlastní doménu, nelze zahájit z portálu Microsoft Azure klasické. [V tomto videu se dozvíte o konfiguraci služby AD FS pro uživatel, přihlaste se pomocí Azure AD Connect](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Domény, které nejsou federované jsou někdy se jí říká spravované domény. Na počáteční doménu adresářů Azure AD je implicitně vyhodnocena jako spravovanou domény.

## <a name="primary-domain-names"></a>Primární doménami

Název primární domény adresář je název domény, který je předem zúžený na výchozí hodnotu "" doména uživatelské jméno, pokud správce vytvoří nový uživatel v [Azure klasické portál](https://manage.windowsazure.com/) nebo jiného portálu jako je portál pro správu Office 365. Adresář může mít jenom jeden název primární domény. Správce může změnit název primární domény být některé ověření vlastní domény, který není federovaný, nebo na počáteční doménu.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Názvy domén v Azure AD a další služby Microsoft Online Services

Název domény musí být ověřený v Azure AD předtím, než ji může používat jiný Online služby společnosti Microsoft, třeba Exchange Online, SharePoint Online a Intune. Tyto další služby obvykle vyžadují správci přidat jeden nebo více položky DNS, které jsou specifické pro službu.

Azure webovou aplikaci používá vlastní k ověření vlastnictví domény. Domény musí být ověřený pro použití s Azure AD, i když ho byly dříve ověřeny pro použití Azure webovou aplikaci v předplatné, které závisí na Azure AD. Azure webovou aplikaci můžete použít název domény, který ověřil v jiném adresáři v adresáři zabezpečuje web appu.

## <a name="managing-domain-names"></a>Správa názvů domén

Úlohy správy domény můžete udělat z portálu Microsoft Azure klasické a od Powershellu. Mnoho úkoly lze provést pomocí rozhraní API Azure AD grafu (v veřejné verze preview).

-   [Přidání a ověření vaší vlastní doménou](active-directory-add-domain.md)

-   [Správa domén v portálu Azure klasické](active-directory-add-manage-domain-names.md)

-   [Použití Powershellu ke správě názvy domén v Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Spravovat názvy domén v Azure AD pomocí rozhraní API Azure AD grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)
