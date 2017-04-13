<properties
    pageTitle="Azure MFA přihlašovacího zkušenosti s Azure Vícefaktorové ověřování"
    description="Tato stránka bude na kde lze zobrazit různé způsoby přihlášení dostupné služby Azure MFA obsahují pokyny."
    keywords="ověřování uživatelů, přihlášení, přihlaste se pomocí mobilní telefon, přihlaste se pomocí Telefon do kanceláře"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="the-sign-in-experience-with-azure-multi-factor-authentication"></a>Přihlášení s Azure Vícefaktorové ověřování
> [AZURE.NOTE]  Následující dokumentaci na této stránce se zobrazí typické přihlášení.  Pokud potřebujete pomoc s přihlášením najdete v článku [máte potíže s Azure Vícefaktorové ověřování](multi-factor-authentication-end-user-manage-settings.md)



## <a name="what-will-your-sign-in-experience-be"></a>Co bude svoje přihlašovací údaje prostředí?
Podle toho, jak se přihlásit a vícefaktorové ověřování se budou lišit uživatelského prostředí.  V této části nabízíme informace na co můžete očekávat při přihlášení.  Zvolte ten, který nejlépe vystihuje co dělají:


Co děláš?|Popis
:------------- | :------------- |
[Přihlášení pomocí office nebo mobilní telefon](#signing-in-with-mobile-or-office-phone) | Toto je, co můžete očekávat při přihlašování do telefonu mobilní telefon nebo office.
[Přihlášení pomocí aplikace Microsoft Authenticator pomocí oznámení](#signing-in-with-the-microsoft-authenticator-app-using-notification) | Toto je co můžete očekávat pomocí aplikace Microsoft Authenticator pomocí upozornění.
[Přihlášení pomocí aplikace Microsoft Authenticator pomocí ověřovací kód](#signing-in-with-the-microsoft-authenticator-app-using-verification-code)|Toto je co můžete očekávat thapp Microsoft Authenticator pomocí ověřovacího kódu.
[Přihlášení pomocí alternativní metody](#signing-in-with-an-alternate-method)|Tím zobrazíte co můžete očekávat, pokud chcete použít jiný způsob.

## <a name="signing-in-with-mobile-or-office-phone"></a>Přihlášení pomocí office nebo mobilní telefon

Tyto informace budou popisují zkušenosti vícefaktorové ověřování pomocí telefonu mobilní telefon nebo office.

### <a name="to-sign-in-with-a-call-to-your-office-or-mobile-phone"></a>Chcete-li Přihlaste se pomocí hovoru na office nebo mobilní telefon

- Přihlaste se k aplikaci nebo služby, třeba Office 365 pomocí uživatelského jména a hesla.
- Microsoft budou volání.

![Microsoft volání](./media/multi-factor-authentication-end-user-signin-phone/call.png)

- Odpovězte telefonu a klepněte na klávesu #.

![Answer (odpověď)](./media/multi-factor-authentication-end-user-signin-phone/phone.png)

- Abyste měli teď se přihlásit.</li>

## <a name="signing-in-with-the-microsoft-authenticator-app-using-notification"></a>Přihlášení pomocí aplikace Microsoft Authenticator pomocí oznámení

Tyto informace budou popisují zkušenosti vícefaktorové ověřování pomocí aplikace Microsoft Authenticator při odesílání oznámení.

### <a name="to-sign-in-with-a-notification-sent-the-microsoft-authenticator-app"></a>Přihlaste se pomocí oznámení posílají aplikace Microsoft Authenticator

- Přihlaste se k aplikaci nebo služby, třeba Office 365 pomocí uživatelského jména a hesla.
- Microsoft odešle oznámení.

![Microsoft odešle oznámení](./media/multi-factor-authentication-end-user-signin-app-notify/notify.png)


- Přijetí telefonního a stiskněte klávesu ověření.  Pokud vaše společnost vyžaduje PIN kódu můžete se zobrazí výzva pro něj tady.

![Ověření](./media/multi-factor-authentication-end-user-signin-app-notify/phone.png)

![Nastavení](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

- Abyste měli teď se přihlásit.


## <a name="signing-in-with-the-microsoft-authenticator-app-using-verification-code"></a>Přihlášení pomocí aplikace Microsoft Authenticator pomocí ověřovací kód

Tyto informace budou popisují zkušenosti vícefaktorové ověřování pomocí aplikace Microsoft Authenticator při použití s ověřovacího kódu.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Se přihlásit pomocí aplikace Microsoft Authenticator ověřovací kód

- Přihlaste se k aplikaci nebo služby, třeba Office 365 pomocí uživatelského jména a hesla.
- Microsoft vás vyzve k ověřovacího kódu.

![Zadejte ověřovací kód](./media/multi-factor-authentication-end-user-signin-app-verify/verify.png)

- Otevřete aplikaci Microsoft Authenticator na telefonu a zadejte kód do pole místo, kam se přihlašujete.

![Získat kód](./media/multi-factor-authentication-end-user-signin-app-verify/phone.png)



- Abyste měli teď se přihlásit.


## <a name="signing-in-with-an-alternate-method"></a>Přihlášení pomocí alternativní metody


V následující části se vidíte, jak se přihlásili alternativní metody při způsobu primární nemusí být k dispozici.

### <a name="to-sign-in-with-an-alternate-method"></a>O přihlášení alternativní metody

- Přihlaste se k aplikaci nebo služby, třeba Office 365 pomocí uživatelského jména a hesla.
- Vyberte použít jiný ověřovací možnost.  Budete prezentovat s výběrem různé možnosti. Číslo zobrazeny založené na jejich množství nastavení.

![Použití alternativní metody](./media/multi-factor-authentication-end-user-signin-alt/alt.png)

- Zvolte metodu alternativní a přihlaste se.
