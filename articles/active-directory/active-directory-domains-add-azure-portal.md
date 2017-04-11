<properties
    pageTitle="Přidání vlastní název domény pro náhled Azure Active Directory | Microsoft Azure"
    description="Jak přidat názvy domén společnosti pro službu Azure Active Directory a jak ověřit název domény."
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
    ms.date="10/17/2016"
    ms.author="curtand"/>

# <a name="add-a-custom-domain-name-to-azure-active-directory-preview"></a>Přidání vlastního názvu domény pro náhled Azure Active Directory

> [AZURE.SELECTOR]
- [Azure portálu](active-directory-domains-add-azure-portal.md)
- [Azure klasické portálu](active-directory-add-domain.md)

Máte jednu nebo více názvy domén, které vaše organizace používá Jednejte a vaši uživatelé přihlášení k podnikové síti s názvem domény společnosti. Použití náhledu Azure Active Directory (Azure AD), můžete přidat název domény společnosti i Azure AD. [Novinky v náhledu](active-directory-preview-explainer.md) Díky tomu vám umožňují přiřadit uživatelská jména v adresáři, které se uživatelům, jako například ‘alice@contoso.com.’ procesu je jednoduchá:

1. Přidejte název vlastní domény do adresáře
2. Přidat položky DNS pro název domény u registrátora název domény
3. Ověřte název vlastní domény v Azure AD

## <a name="how-do-i-add-a-domain-name"></a>Jak přidám název domény?

1.  Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2.  Vyberte **Další služby**, zadejte **Azure Active Directory** do textového pole a potom stiskněte **Enter**.

    ![Otevření Správa uživatelů](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Na zásuvné ***název adresáře*** vyberte **názvy domén**.

4. Na zásuvné ** *název adresáře* - názvy domén** vyberte příkaz **Přidat** .

  ![Výběr příkazu Přidat](./media/active-directory-domains-add-azure-portal/add-command.png)

5. Na zásuvné **název domény** zadejte název vaší vlastní domény do pole, například "contoso.com" a pak vyberte **Přidat doménu**. Nezapomeňte .com, .net nebo jiné nejvyšší úrovně rozšíření.

6. V ***název_domény*** zásuvné (to znamená zásuvné, která se otevře, který má v názvu nový název domény) získáte informace služby DNS položka využívající Azure AD pro ověření, že vaše organizace vlastní vlastní název domény.

  ![získat informace o přístupu ke DNS](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Teď, když jste přidali na název domény, třeba Azure AD ověřit, že vaše organizace vlastní název domény. Před Azure AD můžete provést toto ověření, musíte přidat položky DNS v souboru zóny DNS pro název domény. Tento úkol probíhá na webu doménového registrátora pro název domény.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Přidat položky DNS u doménového registrátora název domény

Dalším krokem pomocí služby Azure AD váš vlastní název domény je aktualizovat soubor zóny DNS pro doménu. Díky Azure AD pro ověření, že vaše organizace vlastní vlastní název domény.

1.  Přihlaste se doménového registrátora domény. Pokud nemáte přístup ke aktualizovat položku DNS, požádejte osobu nebo týmu, kdo má přístup k dokončení kroku 2 a o tom po dokončení.

2.  Soubor zóny DNS pro doménu můžete aktualizujte přidáním položky DNS, které vám Dal Azure AD. Tento údaj DNS umožňuje Azure AD web o ověření vlastnictví domény. Položka DNS nezmění všechny chování, jako jsou směrování pošty nebo webového hostingu.

Nápovědu k přidání položku DNS přečtěte si [pokyny pro přidání na záznam DNS u Oblíbené registrátorů DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Ověřit název domény s Azure AD

Po přidání položku DNS, budete chtít ověřit název domény s Azure AD.

Název domény můžete ověřit, až po rozšíření záznamy DNS. Toto šíření často trvá jen několik sekund, ale někdy může trvat zabere nejméně za hodinu. Pokud ověření nefunguje poprvé, zkuste znovu později.

1.  Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2.  Vyberte **Procházet**, zadejte do textového pole Správa uživatelů a potom stiskněte **Enter**.

    ![Otevření Správa uživatelů](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Na zásuvné **Správa uživatelů – názvy domén** vyberte název neověřený domény, kterou chcete ověřit.

4. Na ***domainname*** zásuvné (to znamená zásuvné, která se otevře, který má v názvu nový název domény) vyberte **ověření** dokončit ověření.

Teď můžete [přiřadit jména uživatelů, které obsahují vlastní název domény](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Řešení potíží

Pokud nemůže ověřit název vlastní domény, zkuste toto. Budete začínat nejběžnější jsme práce dolů nejméně běžné.

1.  **Počkejte za hodinu**. Záznamy DNS muset šířit před Azure AD můžete ověřit doménu. To může trvat zabere nejméně za hodinu.

2.  **Zkontrolujte, zda byl zadán záznam DNS, a její správnost**. To uděláte na webu u doménového registrátora název domény. Azure AD nemůže ověřit název domény, pokud není k dispozici v souboru zóny DNS položky DNS nebo pokud se nejsou přesně shoduje s položkou DNS Azure AD předpokladu, že. Pokud nemáte přístup ke aktualizovat záznamy DNS pro doménu u doménového registrátora název, nasdílet osoba nebo týmu ve vaší organizaci, kteří mají přístup k této položce DNS a zeptejte se, pokud chcete přidat položky DNS.

3.  **Odstranění názvu domény z jiného adresáře v Azure AD**. V jednom adresáři lze ověřit název domény. Pokud název domény byla dříve neověřili do jiné složky, musí před lze ověřit v adresáři vašeho nového odstraní tam. Další informace o odstraňování názvy domén, přečtěte si [Spravovat vlastní názvy domén](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>Přidání další vlastní názvy domén

Pokud vaše organizace využívá víc názvů vlastní domény, například "contoso.com" a "contosobank.com", můžete je přidat až do velikosti 900 názvy domén. Použijte stejný postup v tomto článku přidávat jednotlivé názvy domén.

## <a name="next-steps"></a>Další kroky

[Správa vlastní názvy domén](active-directory-domains-manage-azure-portal.md)
