<properties
    pageTitle="Azure Active Directory B2C: Extensible zásad framework | Microsoft Azure"
    description="Téma v rámci extensible zásad Azure Active Directory B2C a o tom, jak vytvořit různé typy zásad"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-extensible-policy-framework"></a>Azure Active Directory B2C: Framework Extensible zásad

## <a name="the-basics"></a>Základní informace

Extensible zásad framework B2C Azure Active Directory (Azure AD) je základní: Zkontrolujte sílu službu. Zásady plně popisují prostředí identitu příjemce ATP registrace, přihlašovací profil úpravy. Například registrace zásady umožňují určit chování nakonfigurováním následující nastavení:

- Typy účtů (účty sociálních jako je Facebook nebo místních účtů, například e-mailová adresa), které se zobrazují koncovým můžete se přihlásit k aplikaci.
- Atributy (například křestního jména, PSČ a velikosti) mají být vybrány z spotřebitele během registrace.
- Použití Vícefaktorové ověřování.
- Vzhled a chování všech registrace stránek.
- Informace o (manifesty jako nároky v token), že aplikace přijme, kdy zásad spustit dokončení.

Můžete vytvořit více zásad různých typů klienta a jejich použití v aplikacích v případě potřeby. Zásady použití ve více aplikacích. Vývojáři definovat a úprava spotř identity zkušenosti s minimálními nebo žádné změny jejich kódu.

Zásady jsou k dispozici pro použití prostřednictvím rozhraní jednoduché vývojář. Aplikace spustí zásadu pomocí standardní ověřování žádost HTTP (předávání parametru zásad v žádosti o) volání a přijímání přizpůsobený token jako odpověď. Například jediný rozdíl mezi žádosti vyvolání registrace zásady a můžou být vyvolání zásadu přihlašovací je název zásady používaného v parametru řetězce dotazu "p":

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Podrobné informace o rozhraní zásad přečtěte si tento [příspěvek blogu](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-policy"></a>Vytvoření registrace zásad

Aby registrace na aplikaci, musíte vytvořit zásadu registrace. Tuto zásadu popisuje funkce, které uživatelé budou absolvovat během registrace a obsah tokeny, které aplikaci se zobrazí na úspěšné zápisy.

1. [Podle těchto kroků přejděte na zásuvné funkce B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klikněte na **Registrace zásady**.
3. Klikněte na tlačítko **+ Přidat** v horní části zásuvné.
4. Určuje **název** název registrace zásady používané aplikací. Zadejte například "SiUp".
5. Klikněte na **Zprostředkovatelé identit jiní** a vyberte "Registrace používání e-mailu". Pokud už nakonfigurovali v případě potřeby můžete vybrat taky Zprostředkovatelé identit sociální. Klikněte na **OK**.
6. Klikněte na **Registrace atributy**. Tady zvolíte atributy, které chcete shromažďovat od příjemce během registrace. Vyberte například "Země/oblasti", "Zobrazované jméno" a "PSČ". Klikněte na **OK**.
7. Klikněte na tlačítko **deklarace aplikace**. Tady zvolíte deklarace, které chcete vrácených tokeny odeslané zpátky do okna aplikace po úspěšné registrace prostředí. Vyberte například "Zobrazované jméno", "Zprostředkovatele identit", "PSČ", "Je uživatel nový" a "ID objektu uživatele".
8. Klikněte na **vytvořit**. Všimněte si, že zásad vytvořili, zobrazuje se jako "**B2C_1_SiUp**" ( **B2C\_1\_ ** fragment se automaticky přidá) v zásuvné **Registrace zásady** .
9. Otevřete zásadu kliknutím na "**B2C_1_SiUp**".
10. Vyberte "Contoso B2C aplikace" v rozevíracím seznamu **aplikací** a `https://localhost:44321/` v **odpověď URL nebo přesměrovat URI** rozevíracího seznamu.
11. Klikněte na **Spustit**. Otevře na nové záložce prohlížeče a spuštěním prostřednictvím prostředí pro uživatele z registrace aplikace.

    > [AZURE.NOTE]
    Trvá na minutu pro vytváření a zásad aktualizace se projeví.

## <a name="create-a-sign-in-policy"></a>Vytvoření zásad přihlášení

Pokud chcete povolit přihlášení na aplikaci, musíte se k vytvoření zásad přihlášení. Tuto zásadu popisuje funkce, které uživatelé odejde během přihlašování a obsah klíčovými slovy, která se zobrazí aplikace na úspěšné přihlášení.

1. [Podle těchto kroků přejděte na zásuvné funkce B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klikněte na **Sign in Zásady**.
3. Klikněte na tlačítko **+ Přidat** v horní části zásuvné.
4. Určuje **název** název zásady přihlašovací používané aplikací. Zadejte například "SiIn".
5. Klikněte na **Zprostředkovatelé identit jiní** a vyberte "Místního účtu přihlašovacího". Pokud už nakonfigurovali v případě potřeby můžete vybrat taky poskytovatelé sociální identity. Klikněte na **OK**.
6. Klikněte na tlačítko **deklarace aplikace**. Tady zvolíte deklarace, které se má vrátit v tokeny odeslané zpátky do okna aplikace po úspěšném přihlašovací setkat i v případě. Vyberte například "Zobrazované jméno", "Zprostředkovatele identit", "PSČ" a "ID objektu uživatele". Klikněte na **OK**.
7. Klikněte na **vytvořit**. Všimněte si, že zásad vytvořili, zobrazuje se jako "**B2C_1_SiIn**" ( **B2C\_1\_ ** fragment se automaticky přidá) v zásuvné **zásady přihlášení** .
8. Otevřete zásadu kliknutím na "**B2C_1_SiIn**".
9. Vyberte "Contoso B2C aplikace" v rozevíracím seznamu **aplikací** a `https://localhost:44321/` v **odpověď URL nebo přesměrovat URI** rozevírací seznam.
10. Klikněte na **Spustit**. Otevře na nové záložce prohlížeče a spuštěním prostřednictvím prostředí pro uživatele o přihlášení do aplikace.

    > [AZURE.NOTE]
    Trvá na minutu pro vytváření a zásad aktualizace se projeví.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Vytvoření zásad pro zápis nebo přihlášení

Tuto zásadu úchyty pro obě prostředí spotř registrace a přihlašovací pomocí jednoho konfigurace. Uživatelé se řízenému dolů vpravo path (registrace nebo přihlášení) v závislosti na kontextu. Popisuje také obsah tokenů, které aplikaci se zobrazí po úspěšném zápisy nebo přihlášení.  Příklad kódu pro zásady pro zápis nebo přihlašovací je [k dispozici tady](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

1. [Podle těchto kroků přejděte na zásuvné funkce B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klikněte na **registrace nebo přihlašovací zásady**.
3. Klikněte na tlačítko **+ Přidat** v horní části zásuvné.
4. Určuje **název** název registrace zásady používané aplikací. Zadejte například "SiUpIn".
5. Klikněte na **Zprostředkovatelé identit jiní** a vyberte "Registrace používání e-mailu". Pokud už nakonfigurovali v případě potřeby můžete vybrat taky poskytovatelé sociální identity. Klikněte na **OK**.
6. Klikněte na **Registrace atributy**. Tady zvolíte atributy, které chcete shromažďovat od příjemce během registrace. Vyberte například "Země/oblasti", "Zobrazované jméno" a "PSČ". Klikněte na **OK**.
7. Klikněte na tlačítko **deklarace aplikace**. Tady zvolíte deklarace, které se má vrátit v tokeny odeslané zpátky do okna aplikace po úspěšném registrace nebo přihlašovací setkat i v případě. Vyberte například "Zobrazované jméno", "Zprostředkovatele identit", "PSČ", "Je uživatel nový" a "ID objektu uživatele".
8. Klikněte na **vytvořit**. Všimněte si, že zásad vytvořili, zobrazuje se jako "**B2C_1_SiUpIn**" ( **B2C\_1\_ ** fragment se automaticky přidá) v zásuvné **zásady registrace nebo přihlášení** .
9. Otevřete zásadu po kliknutí na "**B2C_1_SiUpIn**".
10. Vyberte "Contoso B2C aplikace" v rozevíracím seznamu **aplikací** a `https://localhost:44321/` v **odpověď URL nebo přesměrovat URI** rozevíracího seznamu.
11. Klikněte na **Spustit**. Otevře na nové záložce prohlížeče a spuštěním prostřednictvím registrace nebo přihlašovací prostředí pro uživatele podle konfigurace.

    > [AZURE.NOTE]
    Trvá na minutu pro vytváření a zásad aktualizace se projeví.

## <a name="create-a-profile-editing-policy"></a>Vytvoření profilu úpravy zásad

Povolit úpravy v aplikaci profilu, bude potřeba vytvořit zásady pro úpravy profilu. Tuto zásadu popisuje funkce, které uživatelé budou absolvovat během úprav profilu a obsah tokeny, které aplikaci se zobrazí na úspěšném dokončení.

1. [Podle těchto kroků přejděte na zásuvné funkce B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klikněte na **profil úprav zásady**.
3. Klikněte na tlačítko **+ Přidat** v horní části zásuvné.
4. Určuje **název** název zásady používané aplikací pro úpravy profilu. Zadejte například "SiPe".
5. Klikněte na **Zprostředkovatelé identit jiní** a vyberte "E-mailová adresa". Pokud už nakonfigurovali v případě potřeby můžete vybrat taky poskytovatelé sociální identity. Klikněte na **OK**.
6. Klikněte na **profil atributy**. Tady můžete zvolit, atributy, které příjemci mohli prohlížet a upravovat. Vyberte například "Země/oblasti", "Zobrazované jméno" a "PSČ". Klikněte na **OK**.
7. Klikněte na tlačítko **deklarace aplikace**. Tady zvolíte deklarace, které se má vrátit v tokeny odeslané zpátky do okna aplikace po úspěšném profilu možnosti úprav. Vyberte například "Zobrazované jméno" a "PSČ".
8. Klikněte na **vytvořit**. Všimněte si, že zásad vytvořili, zobrazuje se jako "**B2C_1_SiPe**" ( **B2C\_1\_ ** fragment se automaticky přidá) v **profilu úpravy zásady** zásuvné.
9. Otevřete zásadu po kliknutí na "**B2C_1_SiPe**".
10. Vyberte "Contoso B2C aplikace" v rozevíracím seznamu **aplikací** a `https://localhost:44321/` v **odpověď URL nebo přesměrovat URI** rozevírací seznam.
11. Klikněte na **Spustit**. Otevře na nové záložce prohlížeče a spuštěním prostřednictvím prostředí pro uživatele v aplikaci pro úpravy profilu.

    > [AZURE.NOTE]
    Trvá na minutu pro vytváření a zásad aktualizace se projeví.
    
## <a name="create-a-password-reset-policy"></a>Vytvoření zásad resetování hesla

Povolit jemně odstupňovaná heslo resetovat na aplikaci, musíte vytvořit zásady resetování hesla. Poznámku, resetování hesel klienta celé nastaveného [tady](active-directory-b2c-reference-sspr.md) pořád platí pro zásady přihlášení. Tuto zásadu popisuje prostředí procházejících spotřebitele bude při obnovení hesla a obsah tokeny, které aplikaci se zobrazí na úspěšném dokončení.

1. [Podle těchto kroků přejděte na zásuvné funkce B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klikněte na tlačítko **zásady resetování hesla**.
3. Klikněte na tlačítko **+ Přidat** v horní části zásuvné.
4. **Název** Určuje, že název zásady používané aplikací resetování hesel. Zadejte například "SSPR".
5. Klikněte na **Zprostředkovatelé identit jiní** a vyberte "Resetování hesla pomocí e-mailovou adresu". Klikněte na **OK**.
6. Klikněte na tlačítko **deklarace aplikace**. Tady zvolíte deklarace, které se má vrátit v tokeny po úspěšné heslo resetovat prostředí odeslán zpátky do okna aplikace. Vyberte například "ID objektu uživatele".
7. Klikněte na **vytvořit**. Všimněte si, že zásad vytvořili, zobrazuje se jako "**B2C_1_SSPR**" ( **B2C\_1\_ ** fragment se automaticky přidá) v zásuvné **zásady resetování hesla** .
8. Otevřete zásadu kliknutím na "**B2C_1_SSPR**".
9. Vyberte "Contoso B2C aplikace" v rozevíracím seznamu **aplikací** a `https://localhost:44321/` v **odpověď URL nebo přesměrovat URI** rozevíracího seznamu.
10. Klikněte na **Spustit**. Otevře na nové záložce prohlížeče a spuštěním prostřednictvím heslo resetovat prostředí pro uživatele v aplikaci.

    > [AZURE.NOTE]
    Trvá na minutu pro vytváření a zásad aktualizace se projeví.

## <a name="additional-resources"></a>Další zdroje informací

- [Token, relace a jednotné přihlašování](active-directory-b2c-token-session-sso.md).
