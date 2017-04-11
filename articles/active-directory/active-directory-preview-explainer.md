<properties
    pageTitle="Azure Active Directory náhled explainer | Microsoft Azure"
    description="Téma, které vysvětluje rozdíly mezi Azure Active Directory v portálu klasické a náhled Azure Active Directory v portálu Azure."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="preview-of-the-azure-active-directory-management-experience-in-the-azure-portal"></a>Náhled možnostem správy Azure Active Directory v portálu Azure

Možnosti správy Azure Active Directory (Azure AD) je v náhledu na portálu Azure. Můžete ho zkusit zmenšit tak, že přihlásíte k [portálu Azure](https://portal.azure.com) jako globální správce adresáře. Potom vyberte v seznamu služby Azure Active Directory, pokud je viditelné, nebo vyberte **Další služby** , které chcete zobrazit seznam všech služeb. Nepotřebujete Azure předplatné pro použití Azure AD management dojít v portálu Azure.


## <a name="capabilities-of-the-preview-experience"></a>Funkce možnosti náhledu

Možnosti náhledu umožňuje spravovat mnoho adresáře zdroje, jako jsou uživatelé, skupiny a aplikace, i adresáře nastavení, na portálu Azure. Se snažíme vylepšovat této možnosti Zahrnout všechny funkce, které jsou Azure AD management dojít [Azure klasické portálu](https://manage.windowsazure.com). Do té doby existují některé úkoly, které se musí stále dokončit na klasické portálu Správa adresářů.

## <a name="manage-the-same-azure-ad-tenants"></a>Správa stejné Azure AD klientů

Možnosti náhledu čte a zapisuje na stejném klientovi Azure Active Directory jako klasická portálem a centra pro správu Office 365. Změny provedené v některém z těchto portálech se projeví ve všech ostatních.

## <a name="use-the-same-authorization-logic"></a>Použití stejné se tak mohli ověřovat logiky

Možnosti náhledu používá stejnou logiku se tak mohli ověřovat jako existující klienti služby Active Directory. Uživatelé smějí provádět změny adresáře zdroje založené na základě rolí adresáře, například globálního správce, Správce uživatelů, správce hesel. S rolí v Azure zdrojů nebo Azure předplatné nepovolí uživatele ke správě adresáře prostředků. Další informace Azure AD Správa rolí v tématu [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles.md). 

Funkce náhledu je optimalizována pro globální správci. Pokud používáte možnosti náhledu při přihlášení jako uživatel, který není globální správce, bude pravděpodobně setkat i v případě jejich funkčnost bude omezena. Například je možné vybrat ovládací prvek, který umožňuje začít úkol, který nelze dokončit v adresáři. Se snažíme vylepšovat toto prostředí brzy bude k dispozici.
 
## <a name="tell-us-what-you-think"></a>Řekněte nám, co si myslíte

Poskytnutí zpětné vazby k náhledu prostředí v části správce portálu [fórum pro názory Azure AD](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&filter=alltypes&sort=lastpostdesc).
