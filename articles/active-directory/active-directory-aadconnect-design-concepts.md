<properties
   pageTitle="Azure AD Connect: Návrh koncepty | Microsoft Azure"
   description="Toto téma podrobně popisuje některé oblasti návrh implementaci"
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.custom = "azure-ad-connect"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Identity"
   ms.date="09/13/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-design-concepts"></a>Azure AD Connect: Návrh koncepty
Části tohoto tématu slouží k popisu oblastí, které musí představit během provádění návrhu Azure AD Connect. Toto téma je hloubkové postupy v některých oblastech a koncepce jsou popsány v dalších tématech stejně.

## <a name="sourceanchor"></a>sourceAnchor
Atribut sourceAnchor je definována jako *neměnný po dobu platnosti objektu atributu*. Jednoznačně identifikuje objekt jako stejné objekt místní a v Azure AD. Atribut se nazývá také **immutableId** a jsou použity dva názvy zaměnit.

Neměnný, slovo, které je "nelze změnit", je důležité ji v tomto tématu. Vzhledem k tomu, že Tenhle atribut hodnotu nelze změnit, když byl nastaven, je důležité vyberte návrh, který podporuje nefunguje.

Atribut je použita v následujících situacích:

- Nový server engine synchronizace se vytvořené nebo znovu po obnovení situace havárie, odkazy na Tenhle atribut existující objekty v Azure AD pomocí objekty místní.
- Přesunutí z jen cloudu identity do modelu synchronizované identity Tenhle atribut umožňuje objekty "pevné POZVYHLEDAT" existujících objektů v Azure AD pomocí místní objekty.
- Pokud používáte federace, pak Tenhle atribut společně s **userPrincipalName** je používán deklaraci identifikují uživatele.

Toto téma se jenom pojednává o sourceAnchor se týká uživatelům. Stejná pravidla pro všechny typy objektů vhodná, ale je jenom pro uživatele, že tento problém obvykle je důležité.

### <a name="selecting-a-good-sourceanchor-attribute"></a>Výběr správné sourceAnchor atribut
Hodnota atributu musí dodržovat následující pravidla:

- Být menší než 60 znaků
    - Znaky nejsou z, A-Z nebo 0 – 9 zakódovaný se počítá jako 3 znaky
- Nesmí obsahovat speciální znak: & #92;! # $ % & * + / = ? ^ & #96; { } | ~ (< >) '; : , [ ] " @ _
- Musí být jedinečné
- Musí být řetězec, celé číslo nebo binární.
- Nesmí být založená na jméno uživatele, tyto změny
- Nesmí být malá a velká písmena a vyhněte se hodnoty, které může být trochu jiný, případu
- By měly být přiřazeny při vytvoření objektu

Pokud není vybrané sourceAnchor typu String, Azure AD připojení Base64Encode hodnota atributu zajistit žádné speciální znaky zobrazí. Pokud používáte jiný federačního serveru než ADFS, zkontrolujte serveru můžete taky Base64Encode atribut.

Atribut sourceAnchor rozlišuje malá a velká písmena. Hodnota "JohnDoe" není stejný jako "johndoe". Ale neměli mít dva různé objekty s pouze rozdílem písmeny na začátku slov.

Pokud máte strukturu jednoho je místní, pak atribut, které byste měli použít **objectGUID**. Toto je také při použití Expresní nastavení v Azure AD Connect a také atributem používaný DirSync.

Pokud máte víc struktury a nepřesouvat uživatelů mezi struktury a domény **objectGUID** je dobré atribut můžete i v tomto případě.

Přesunutí uživatelů mezi struktury a domény poté musíte najít atribut, který se nemění nebo přesouvat s uživateli při přesunutí. Doporučený postup je zavést syntetické atribut. Atribut, která může obsahovat něco, který bude vypadat identifikátor GUID bude vhodné. Při vytváření objektu nové GUID vytvořené a razítko na uživatele. Pravidlo vlastní synchronizace lze vytvořit na serveru engine synchronizace vytvořit tuto hodnotu podle **objectGUID** a aktualizovat vybraný atribut přidá. Obsah přesouvaných objekt, nezapomeňte také kopírování obsahu tuto hodnotu.

Jiné řešení je vybrat existující atribut, které znáte se nezmění. Běžně používané atributy zahrnout **číslo zaměstnance**. Pokud byste zvážit atribut, který obsahuje písmena, ujistěte se, je, že možnost případ (velká a malá písmena) můžete změnit hodnotu atributu. Chybné atributy, které by neměly být použity zahrnout tyto atributy s jméno uživatele. V svatbu nebo rozvodu název očekává se, pokud chcete změnit, což není povoleno pro tento atribut. Toto je jedním z důvodů, proč nejsou i umožňuje výběr v Průvodci instalací Azure AD Connect atributy například **userPrincipalName**, **Pošta**a **targetAddress** . Tyto atributy obsahovat také @-character, což není povoleno v sourceAnchor.

### <a name="changing-the-sourceanchor-attribute"></a>Změna atribut sourceAnchor
Hodnota atributu sourceAnchor nelze změnit po objekt byl vytvořen v Azure AD a synchronizaci identitě.

Z tohoto důvodu platí následující omezení Azure AD Connect:

- Atribut sourceAnchor můžete nastavit jenom během počáteční instalace. Pokud je znovu spustit Průvodce instalací tato možnost je jen pro čtení. Pokud potřebujete toto nastavení změnit, musíte odinstalovat a znova nainstalovat.
- Pokud nainstalujete jiný server Azure AD Connect, je nutné vybrat atribut stejné sourceAnchor jako použili. Pokud jste dříve používali DirSync a přesunutí Azure AD Connect, je nutné vzhledem k tomu, který je atribut používaný DirSync použít **objectGUID** .
- Pokud je hodnota sourceAnchor dojde ke změně po objekt vyexportování Azure AD, pak Azure AD Connect synchronizace vyvolá chybu a neumožňuje žádné změny na objekt před byla podařilo odstranit problém a sourceAnchor je změnu zpět v adresáři zdroje.

## <a name="azure-ad-sign-in"></a>Azure AD přihlášení
Při integraci místního adresáře s Azure AD, je důležité pochopit, jak nastavení synchronizace může ovlivnit způsob, jak uživatel ověří. Azure AD pomocí userPrincipalName (UPN) k ověření uživatele. Ale když synchronizujete uživatele, musíte si vybrat atribut určené přínosu userPrincipalName pečlivě.

### <a name="choosing-the-attribute-for-userprincipalname"></a>Výběr atribut userPrincipalName
Při výběru atribut umožňující přínosu Přípony se nemusí používat v Azure jeden zajistila

- Hodnoty atributu odpovídat UPN syntaxi (RFC 822), která je, že by měl být formátuusername@domain
- Přípona do pole hodnoty, odpovídat na jeden z ověření vlastní domény v Azure AD

V dialogovém okně Expresní nastavení je předpokládá, že volby atributu userPrincipalName. Pokud atribut userPrincipalName neobsahuje žádná hodnota je, aby měli uživatelé se přihlásit k Azure a pak je nutné vybrat **Vlastní instalace**.

### <a name="custom-domain-state-and-upn"></a>Stav vlastní domény a UPN
Je důležité, aby bylo ověřenou doménou přípony UPN.

Jan je uživatel na contoso.com. Chcete, aby Jan používat místních hlavních názvů uživatelů john@contoso.com pro přihlášení k aplikaci Azure poté, co jste synchronizovali uživatele do vašeho adresáře contoso.onmicrosoft.com Azure AD. Postup, musíte přidáním a ověřením contoso.com jako vlastní domény v Azure AD před spuštěním synchronizaci uživatelů. Pokud přípony UPN Lukáš, například contoso.com, neodpovídá ověřenou doménou v Azure AD, pak Azure AD nahradí přípony UPN contoso.onmicrosoft.com.

### <a name="non-routable-on-premises-domains-and-upn-for-azure-ad"></a>Domény které nelze směrovat místního a UPN pro Azure AD
V některých organizacích mít-směrovatelné domény, třeba contoso.local nebo jednoduché jediný štítek domén, jako třeba contoso. Nejste ověřit směrovat domain Azure AD. Azure AD Connect můžete synchronizovat s ověřenou doménou v Azure AD. Při vytváření adresáře služby Azure AD vytvoří směrovatelné domény, která bude výchozí doménu pro Azure AD například contoso.onmicrosoft.com. Proto bude ověření dalších směrovatelné domény v tomto případě v případě, že nechcete synchronizovat výchozí domény onmicrosoft.com.

Číst další informace o přidání a ověření domény [přidejte název vlastní domény pro službu Azure Active Directory](active-directory-add-domain.md) .

Azure AD Connect zjistí, pokud používáte v prostředí směrovat domény a chcete řádně podporovat odpovíte z pokračovat Expresní nastavení. Pokud pracujete v doméně směrovat, je pravděpodobné, že UPN uživatelé mít směrovat přípony příliš. Například pokud jsou spuštěné v části contoso.local, Azure AD Connect navrhne použít vlastní nastavení spíše než expresní nastavení. Použití vlastního nastavení, budete moci určit atribut, který má být použit jako UPN pro přihlášení k aplikaci Azure po uživatelů se synchronizují Azure AD.

## <a name="next-steps"></a>Další kroky
Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
