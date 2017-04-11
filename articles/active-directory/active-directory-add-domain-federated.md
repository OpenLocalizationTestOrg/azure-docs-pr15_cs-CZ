<properties
    pageTitle="Přidání vlastní název domény a nastavení federované přihlašování pro službu Azure Active Directory | Microsoft Azure"
    description="Jak přidat názvy domén společnosti pro službu Azure Active Directory a jak nastavit federovanými přihlašování mezi místní federaci řešení a Azure Active Directory."
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
    ms.topic="get-started-article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="add-your-custom-domain-name-to-azure-active-directory"></a>Přidejte název vlastní domény pro službu Azure Active Directory

Vlastní název domény, například contoso.com, můžete nakonfigurovat tak, aby uživatelé na contoso.com, může mít federované jednoho přihlašování setkat i v případě z vaší podnikové sítě. Pokud už máte Active Directory Federation Services (AD FS) nebo jiné federace serveru ve vaší podnikové síti, je možné nakonfigurovat Azure AD použít vlastní název domény pomocí nástroje Azure AD Connect. Můžete taky použít Azure AD Connect nasazení nové prostředí služby AD FS a nakonfigurovat, federované jednotného přihlašování Azure AD.

Pokud nemáte k dispozici a nemáte v plánu zavedení služby AD FS nebo jiné federačním serveru, postupujte podle těchto pokynů: [Přidání vlastního názvu domény pro službu Azure Active Directory](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-to-your-directory"></a>Přidání vlastního názvu domény do adresáře

1. Přihlaste se k [Azure klasické portál](https://manage.windowsazure.com/) k uživatelskému účtu, který je globálního správce služby Azure AD adresáře.

2. Ve **Službě Active Directory**otevřete svůj adresář a vyberte kartu **domény** .

3. Na panelu s příkazy klikněte na **Přidat**a potom zadejte název vaší vlastní domény, například "contoso.com". Nezapomeňte .com, .net nebo jiné nejvyšší úrovně rozšíření.

4. Zaškrtněte políčko **chtít nastavení této domény pro jednotné přihlašování se můj místní službou Active Directory** .

5. Vyberte možnost **Přidat**.

Spusťte nástroj Azure AD Connect získat položky DNS využívající Azure AD můžete ověřit doménu. Zobrazí se položka DNS v **Azure AD Domain** kroku v průvodci. Co uvidíte tento krok v Průvodci vypadá [v těchto pokynech](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Pokud nástroj Azure AD Connect nemáte, můžete [Stáhnout tady](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Přidat položky DNS u doménového registrátora název domény

Dalším krokem pomocí služby Azure AD váš vlastní název domény je aktualizovat soubor zóny DNS pro doménu. Díky Azure AD pro ověření, že vaše organizace vlastní vlastní název domény.

1. Přihlaste se k webu pro doménovému registrátorovi za svůj název domény. Pokud nebudete mít přístup k tomuto, požádejte osobu nebo týmu ve vaší organizaci, kteří mají přístup k dokončení kroku 2 a o tom po dokončení.

2. Soubor zóny DNS pro doménu můžete aktualizujte přidáním položky DNS, které vám Dal Azure AD. Tento údaj DNS umožňuje Azure AD web o ověření vlastnictví domény. Položka DNS nezmění všechny chování, jako jsou směrování pošty nebo webového hostingu.

Pokud potřebujete pomoc s tímto krokem přečtěte si [pokyny pro přidání na záznam DNS u Oblíbené registrátorů DNS](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-the-domain-name-with-azure-ad"></a>Ověřit název domény s Azure AD

Po přidání položku DNS, budete chtít ověřit název domény s Azure AD.

K ověření domény, vyberte na **Azure AD Domain** kroku průvodce Azure AD Connect **Další** . Azure AD bude hledat položky DNS do souboru zóny DNS pro doménu. Azure AD pouze ověřit název domény, po rozšíření záznamy DNS. Šíření často trvá jen několik sekund, ale někdy může trvat zabere nejméně za hodinu. Pokud ověření nefunguje poprvé, zkuste znovu později.

Přejděte pomocí pokynů v Průvodci Azure AD Connect. To synchronizuje ze systému Windows Server AD uživatelům Azure AD. Synchronizovaní uživatelé v doméně, které jste nakonfigurovali pro federaci budou moct získat federované jednoho přihlašování setkat i v případě z vaší podnikové síti Azure AD.

## <a name="troubleshooting"></a>Řešení potíží

Pokud nemůže ověřit název vlastní domény, zkuste toto. Budete začínat nejběžnější jsme práce dolů nejméně běžné.

1.  **Počkejte za hodinu**. Záznamy DNS muset šířit před Azure AD můžete ověřit doménu. To může trvat zabere nejméně za hodinu.

2.  **Zkontrolujte, zda byl zadán záznam DNS, a její správnost**. To uděláte na webu u doménového registrátora název domény. Azure AD nemůže ověřit název domény, pokud není k dispozici v souboru zóny DNS položky DNS nebo pokud se nejsou přesně shoduje s položkou DNS Azure AD předpokladu, že. Pokud nemáte přístup ke aktualizovat záznamy DNS pro doménu u doménového registrátora název, nasdílet osoba nebo týmu ve vaší organizaci, kteří mají přístup k této položce DNS a zeptejte se, pokud chcete přidat položky DNS.

3.  **Odstranění názvu domény z jiného adresáře v Azure AD**. V jednom adresáři lze ověřit název domény. Pokud název domény byla dříve neověřili do jiné složky, musí před lze ověřit v adresáři vašeho nového odstraní tam. Další informace o odstraňování názvy domén, přečtěte si [Spravovat vlastní názvy domén](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>Přidání další vlastní názvy domén

Pokud vaše organizace využívá víc názvů vlastní domény, například "contoso.com" a "contosobank.com", můžete je přidat až do velikosti 900 názvy domén. Použijte stejný postup v tomto článku přidávat jednotlivé názvy domén.

## <a name="next-steps"></a>Další kroky

-   [Správa vlastní názvy domén](active-directory-add-manage-domain-names.md)
-   [Informace o doménách správy v Azure AD](active-directory-add-domain-concepts.md)
-   [Zobrazit že značky vaší společnosti při přihlášení uživatele](active-directory-add-company-branding.md)
-   [Použití Powershellu ke správě názvy domén v Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
