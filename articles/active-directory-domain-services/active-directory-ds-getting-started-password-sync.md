<properties
    pageTitle="Azure AD Domain Services: Povolení synchronizace hesel | Microsoft Azure"
    description="Začínáme s Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Povolení synchronizace hesel Azure AD Domain Services
V předchozím úkoly povolené služby Azure AD domén pro vašeho klienta Azure AD. Dalším úkolem je to povolíte pověření hash potřebných pro ověřování NTLM a Kerberos synchronizovat s Azure AD Domain Services. Po nastavení synchronizace pověření si mohli uživatelé přihlašovat spravovaných doménu pomocí své přihlašovací údaje společnosti.

Kroky se různých podle toho, jestli vaše organizace má jenom cloudu Azure AD klient nebo je nastavený na synchronizovat s svého místního adresáře pomocí Azure AD Connect.

<br>

> [AZURE.SELECTOR]
- [Jen cloudu Azure AD klienta](active-directory-ds-getting-started-password-sync.md)
- [Synchronizovat Azure AD klienta](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Krok 5: Povolení synchronizace hesel AAD Domain Services pro jen cloudu Azure AD klienta
Azure AD Domain Services musí pověření hash ve formátu vhodné pro NTLM a Kerberos ověřování pro ověřování uživatelů ve spravovaných doméně. Pokud povolíte AAD Domain Services pro vašeho klienta, Azure AD generovat ani ukládání přihlašovacích údajů hash ve formátu potřebných pro NTLM nebo Kerberos ověřování. Zjevných bezpečnostních důvodů Azure AD taky neukládá žádné přihlašovací údaje ve formátu prostého textu. Proto Azure AD nemá umožňuje generovat tyto NTLM nebo Kerberos pověření hash hodnoty na základě existující přihlašovacích údajů uživatelů.

> [AZURE.NOTE] Pokud má vaše organizace jen cloudu Azure AD klienta, uživatele, kteří potřebují použití služby Azure AD domény musí změnit své heslo.

Tento proces změnit heslo způsobí, že pověření hash požadované službou Azure AD Domain pro ověřování protokolem Kerberos a NTLM vygenerovalo v Azure AD. Můžete buď vypršení platnosti hesla pro všechny uživatele v klientovi, které je nutné použít Azure AD Domain Services nebo sdělte tyto uživatele změnit své heslo.


### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Povolit NTLM a Kerberos generování hash přihlašovací údaje pro jen cloudu Azure AD klienta
Tady jsou pokyny, abyste mohli poskytnout koncovým uživatelům, aby on může změnit svoje hesla:

1. Přejděte na stránku Panel Azure AD přístup pro vaši organizaci na [http://myapps.microsoft.com](http://myapps.microsoft.com).

2. Vyberte kartu **profilu** na této stránce.

3. Klikněte na dlaždici **změnit heslo** na této stránce.

    ![Vytvořte virtuální sítě pro službu Azure AD domény.](./media/active-directory-domain-services-getting-started/user-change-password.png)

    > [AZURE.NOTE] Pokud nevidíte možnost **změnit heslo** na stránce Panel přístup, nakonfigurujte vaše organizace má [Správa hesel v Azure AD](../active-directory/active-directory-passwords-getting-started.md).

4. Na stránce **změnit heslo** zadejte stávající heslo (starý) a zadejte nové heslo a potvrďte ho. Klikněte na **Odeslat**.

    ![Vytvořte virtuální sítě pro službu Azure AD domény.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Po změně hesla nové heslo bude možné v Azure AD Domain Services brzy. Po několika minutách (obvykle se o 20 minut), můžete se k počítači připojený k spravované domény pomocí nově změnit heslo.

<br>

## <a name="related-content"></a>Související obsah

- [Jak aktualizovat svůj vlastní heslo](../active-directory/active-directory-passwords-update-your-own-password.md)

- [Začínáme používat správu heslo v Azure AD](../active-directory/active-directory-passwords-getting-started.md).

- [Povolení synchronizace hesel AAD Domain Services pro synchronizované Azure AD klienta](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Spravovat spravovaná domain Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)

- [Spojení Windows virtuálního počítače a spravovaných domain Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)

- [Spojení červené klobouk Enterprise Linux virtuálního počítače a spravovaných domain Azure AD Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
