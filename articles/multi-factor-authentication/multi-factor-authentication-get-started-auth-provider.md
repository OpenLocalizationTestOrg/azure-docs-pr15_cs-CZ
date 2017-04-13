<properties
    pageTitle="Získání začít Azure Multi-Factor Auth poskytovatele | Microsoft Azure"
    description="Naučte se vytvářet poskytovatele Azure Multi-Factor Auth."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>



# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Začínáme se zprostředkovatelem Auth Multi-Factor Azure
Ve výchozím nastavení pro globální správce, kteří mají Azure Active Directory a uživatele Office 365 neexistuje dvoustupňového ověření. Ale pokud chcete využívat výhod [rozšířených funkcí](multi-factor-authentication-whats-next.md) pak můžete by měl zakoupit plnou verzi z Azure Multi-Factor Authentication (MFA).

> [AZURE.NOTE]  Přiřazení poskytovatele Azure Multi-Factor Auth slouží umožní využít výhod funkce poskytované plnou verzi Azure MFA. Je pro uživatele, kteří **nemají licence až Azure MFA, Azure AD Premium nebo EMS**.  Azure MFA Azure AD Premium a EMS zahrnuje úplnou verzi Azure MFA ve výchozím nastavení.  Pokud máte licencí, není nutné poskytovatele Azure Multi-Factor Auth.

Přiřazení poskytovatele Azure Multi-Factor Auth je potřeba pro stažení SDK.

> [AZURE.IMPORTANT]  Pro stažení SDK, vytvořte poskytovatele Azure Multi-Factor Auth, i když máte Azure MFA, AAD Premium nebo EMS licence.  Když vytvoříte poskytovatele Azure Multi-Factor Auth k tomuto účelu a už máte licencí, ujistěte se, aby se vytvořil poskytovatel s modelem **Za uživatele** . Spojte zprostředkovatele adresář, který obsahuje Azure MFA, Azure AD Premium nebo EMS licencí.  Zajistíte tím, že je pouze faktura případné další jedinečných uživatelů pomocí SDK než počet licencí, které vlastníte.


## <a name="to-create-a-multi-factor-auth-provider"></a>Aby se vytvořil poskytovatel Auth Multi-Factor

Pomocí následujících kroků k vytvoření poskytovatele Azure Multi-Factor Auth.

1. Přihlaste se k [Azure klasické portál](https://manage.windowsazure.com) jako správce.
2. V levé části vyberte **Služby Active Directory**.
3. Na stránce služby Active Directory nahoře vyberte **Multi-Factor Authentication poskytovatelů**.
![Vytváření zprostředkovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)
4. V dolní části klikněte na **Nový**.
![Vytváření zprostředkovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)
5. V části aplikace služby vyberte **Poskytovatele Auth Multi-Factor**
![vytváření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)
6. Vyberte **vytvořit**.
![Vytváření zprostředkovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)
5. Vyplňte následující pole a vyberte **vytvořit**.
    1. **Název** – název poskytovatele Auth Multi-Factor.
    2. **Použití modelu** , jestli chcete povolit jednotlivé uživatele nebo platíte za ověření. Vyberte jednu ze dvou možností:
        - Za ověření – nákup model, který poplatky za ověření. Obvykle používá pro používajících Azure Vícefaktorové ověřování do spotř webových aplikací.
        - Uživatele povoleno – povolené nákupu model, který poplatky za uživatele. Obvykle používá pro přístupu zaměstnance k aplikací, jako je Office 365. Vyberte tuto možnost, pokud máte někteří uživatelé, které jsou již licenci pro Azure MFA.
    2. **Adresář** klienta Azure Active Directory, kterému je přidružen Multi-Factor Authentication poskytovatele. Mějte na paměti toto:
        - Adresář služby Azure AD aby se vytvořil poskytovatel Auth Multi-Factor nepotřebujete. Nechte pole prázdné, pokud jsou pouze plánujete používat Azure Multi-Factor ověřování serveru nebo SDK.
        - Zprostředkovatel Auth Multi-Factor musí být přidružené adresáře služby Azure AD umožní využít výhod rozšířených funkcí.
        - Azure AD Connect AAD Sync nebo DirSync jsou pouze požadavku, když synchronizujete prostředí služby Active Directory místního adresáře služby Azure AD.  Pokud používáte pouze Azure AD adresář, který se nesynchronizuje, a pak tento krok není povinný.
![Vytváření zprostředkovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)
5. Po kliknutí na vytvořit zprostředkovatele Multi-Factor Authentication a zobrazí zpráva s oznámením: **úspěšné vytvoření Multi-Factor Authentication poskytovatele**. Klikněte na **Ok**.
![Vytváření zprostředkovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)
