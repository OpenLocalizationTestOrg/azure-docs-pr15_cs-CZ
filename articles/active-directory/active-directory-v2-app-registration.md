<properties
    pageTitle="registrace aplikace verze 2.0 | Microsoft Azure"
    description="Jak si zaregistrovat aplikace s Microsoftem pro povolení přihlašovací a přístup ke službám Microsoft pomocí koncový bod verze 2.0"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Jak si zaregistrovat aplikace s koncovým verze 2.0

Vytvářet aplikace, která přijímá MSA & Azure AD přihlásit, Nejdřív musíte zaregistrovat aplikace u společnosti Microsoft.  V současné době nebudete moct používat všechny existující aplikace může mít s Azure AD nebo MSA – bude nutné vytvořit úplně novou.

> [AZURE.NOTE]
    Některé scénáře Azure Active Directory a funkce jsou podporovány koncový bod verze 2.0.  Pokud chcete zjistit, zda by měly používat koncový bod verze 2.0, přečtěte si informace o [omezení verze 2.0](active-directory-v2-limitations.md).

## <a name="visit-the-microsoft-app-registration-portal"></a>Přejděte na portál registrace aplikace Microsoft
Co je první nejdřív – přejděte [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Toto je novém portálu registrace aplikace místo, kam můžete spravovat své aplikace Microsoft.

Přihlaste se pomocí buď osobní nebo pracovní nebo školní účet Microsoft.  Pokud nemáte něco zaregistrovat si nový osobní účet. Pokračujte, nebude trvat dlouho – jsme budete čekat tady.

Dělat? Můžete by nyní být prohlédnete seznam aplikací Microsoft tedy pravděpodobně prázdné.  Chci změnit.

Klikněte na **Přidat aplikaci**a pojmenujte ho.  Na portálu přiřadí aplikace globálně jedinečné Id aplikace, které budete používat později v kódu.  Pokud aplikace zahrnuje komponentu straně serveru, který potřebuje tokeny přístupu pro rozhraní API, volající (popřemýšlet: Office, Azure nebo vlastní webového rozhraní API), je vhodné vytvořit porušení **Aplikace tajná** tady taky.
<!-- TODO: Link for app secrets -->

Dále přidejte platformy, která budou používat aplikaci.

- Webové aplikace poskytovat **Přesměrování URI** kde přihlašovací nelze odesílat zprávy.
- Mobilní aplikace zkopírujte dolů uri přesměrování výchozí automaticky vytvoří.

Pokud chcete můžete přizpůsobit vzhled a chování vaše přihlašovací stránku v oddílu profilu.  Zkontrolujte, že teprve pak přejděte na klikněte na **Uložit** .

> [AZURE.NOTE] Při vytváření aplikace přes [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)aplikace registraci v domácí klienta účtu, který používáte pro přihlášení k portálu.  To znamená, že nemůže registrovat aplikace ve vašem klientovi Azure AD pomocí osobního účtu Microsoft.  Pokud chcete explicitně registrace aplikace na určité klientovi, přihlaste se pomocí účtu původně vytvořil na tomto klientovi.

## <a name="build-a-quick-start-app"></a>Vytvářet aplikace rychlým startem
Teď, když máte v aplikaci Microsoft, musíte udělat jednu z našich výukové programy pro verze 2.0 – úvodní.  Tady je pár doporučení:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]
