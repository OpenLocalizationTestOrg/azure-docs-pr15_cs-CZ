<properties
    pageTitle="Azure Active Directory B2C: Nástroj Pomocníka pro přizpůsobení uživatelského rozhraní stránky | Microsoft Azure"
    description="Nástroj pomocníka sloužící k demonstrují funkce vlastního nastavení uživatelského rozhraní stránky v Azure Active Directory B2C"
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
    ms.date="07/22/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: Helper nástroj sloužící k demonstrují funkce vlastního nastavení stránky uživatelské rozhraní

Tento článek se Průvodce vyhledáváním na [Hlavní článek přizpůsobení uživatelského rozhraní](active-directory-b2c-reference-ui-customization.md) v B2C Azure Active Directory (Azure AD). Následující kroky popisují, jak neuplatnění funkci přizpůsobení uživatelského rozhraní stránky pomocí vzorku HTML a CSS obsah, který jsme zadali.

## <a name="get-an-azure-ad-b2c-tenant"></a>Získání Azure AD B2C klienta

Před úpravou cokoli, musíte získat [tenanta Azure AD B2C](active-directory-b2c-get-started.md) Pokud ještě nemáte jeden.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Vytvoření zásad pro zápis nebo přihlášení

Ukázkový obsah jsme jste uvedli mohou sloužit k customze dvě stránky [registrace nebo přihlašovací zásad](active-directory-b2c-reference-policies.md): [jednotné přihlašování v stránky](active-directory-b2c-reference-ui-customization.md) a [vlastním uplatněna atributy stránky](active-directory-b2c-reference-ui-customization.md). Po [Vytvoření zásad pro zápis nebo přihlášení](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), přidejte místního účtu (e-mailovou adresu), Facebook, Google a Microsoft jako **Zprostředkovatelé identit jiní**. Toto jsou pouze IDPs, které naše obsahu HTML ukázkové přijímá.  Pokud chcete, můžete také přidat podmnožinu tyto IDPs.

## <a name="register-an-application"></a>Registrace aplikace

Musíte se [Registrace aplikace](active-directory-b2c-app-registration.md) ve vašem klientovi B2C, které lze použít k provedení svoje zásady. Po registraci aplikace, máte několik možností, které můžete použít k skutečně spustit registrace zásad:

- Vytvořit jednu B2C Azure AD rychlého aplikace uvedené v části "Začít" [podepsat nahoru a přihlaste se uživatelé v aplikacích](active-directory-b2c-overview.md#getting-started).
- Používání předdefinovaných [Azure AD B2C hřišť](https://aadb2cplayground.azurewebsites.net) aplikace. Pokud se rozhodnete sdělit nám hřišť, musíte zaregistrovat aplikace ve vašem klientovi B2C pomocí **přesměrovat URI** `https://aadb2cplayground.azurewebsites.net/`.
- Pomocí tlačítka **Spustit** na svoje zásady [Azure portálu](https://portal.azure.com/).

## <a name="customize-your-policy"></a>Přizpůsobit svoje zásady

Chcete-li přizpůsobit vzhled a chování zásad, musíte nejdřív vytvořit soubory ve formátu HTML a šablon stylů CSS pomocí konkrétní konvence Azure AD B2C. Potom můžete nahrát statický obsah veřejně dostupné místo tak, aby Azure AD B2C k němu přístup. Bude vyhrazené webový server, úložiště objektů Blob Azure, Azure obsahu síť pro doručování nebo jakéhokoli jiného statické hostingu zdroje poskytovatele. Požadavky na pouze se, že obsah je k dispozici prostřednictvím HTTPS můžete přistupovat pomocí CORS. Jakmile jste zveřejněným statický obsah na webu, můžete upravit zásady přejděte do tohoto umístění a prezentace obsahu zákazníkům. [Hlavní článek přizpůsobení uživatelského rozhraní](active-directory-b2c-reference-ui-customization.md) podrobně popisuje, jak funkce vlastního nastavení Azure AD B2C funguje.

Pro účely tohoto kurzu jste již vytvořili některý ukázkové obsah jsme hostitelem úložišti objektů Blob Azure. Ukázkový obsah je velmi základním Přizpůsobení motivu fiktivní společnosti "Vzorkový". Vyzkoušejte si to v vlastní zásady, postupujte takto:

1. Přihlaste se k vašemu tenantovi na [Azure portál](https://portal.azure.com/) a přejděte na zásuvné funkce B2C.
2. Klikněte na **registrace nebo přihlašovací zásady** a potom klikněte na svoje zásady (například "b2c\_1\_znaménko\_nahoru\_znaménko\_v").
3. Klikněte na **přizpůsobení uživatelského rozhraní stránky** a potom **jednotné registrace nebo na přihlašovací stránce**.
4. Zapíná a vypíná přepínačem **vlastní stránku používat** na hodnotu **Ano**. V poli **vlastní stránku URI** zadejte `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Klikněte na **OK**.
5. Klikněte na **stránku pro přihlášení místního účtu**. Zapíná a vypíná přepínačem **použít vlastní šablonu** na hodnotu **Ano**. V poli **vlastní stránku URI** zadejte `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
5. Stejný krok opakujte pro **stránku pro přihlášení sociální účtu**.
 Kliknutím na **OK** dvakrát zavřete listy přizpůsobení uživatelského rozhraní.
6. Klikněte na **Uložit**.

Teď můžete vyzkoušet svoje vlastní zásady. Pokud chcete, ale taky jednoduše klikněte na příkaz **Spustit** ve zásuvné zásad můžete použít vlastní aplikaci nebo hřišť Azure AD B2C. V rozevíracím seznamu vyberte aplikace a zvolte vhodné přesměrování URI. Klikněte na tlačítko **Spustit** . Na nové záložce prohlížeče se otevře a můžete spustit prostřednictvím zkušenostech registrace aplikace s nový obsah na místě.

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>Uložení obsahu ukázkové k úložišti objektů Blob Azure

Pokud chcete použít úložišti objektů Blob Azure hostovat svůj obsah stránky, můžete vytvořit účtu úložiště a nástrojem naše B2C pomocníka nahrajte svoje soubory.

### <a name="create-a-storage-account"></a>Vytvoření účtu úložiště

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Klikněte na **+ Nový** > **dat + úložiště** > **účtu úložiště**. Budete potřebovat Azure předplatné Vytvořte účet úložišti objektů Blob Azure. Můžete zaregistrovat bezplatnou zkušební verzi na [Azure webu](https://azure.microsoft.com/pricing/free-trial/).
3. Zadejte **název** účtu úložiště (třeba "contoso") a vyberte příslušné možnosti pro **osy ceny**, **Skupina zdroje** a **předplatné**. Ujistěte se, jestli máte **kód Pin pro Startboard** zaškrtnuto. Klikněte na **vytvořit**.
4. Přejděte zpátky Startboard a klikněte na úložiště klienta, který jste právě vytvořili.
5. V části **Souhrn** klikněte **kontejnery**a potom klikněte na tlačítko **+ Přidat**.
6. Zadejte **název** pro kontejner (například "b2c") a vyberte **kulatý** **Typ přístupu**. Klikněte na **OK**.
7. Kontejner, který jste vytvořili se zobrazí v seznamu na zásuvné **objektů BLOB** . Poznamenejte si adresu URL kontejneru; Příklad by měla vypadat podobně jako `https://contoso.blob.core.windows.net/b2c`. Zavřete zásuvné **objektů BLOB** .
8. Na zásuvné účtu úložiště klikněte na **klíče** a zapište si hodnoty v polích **Název účtu úložiště** a **Primární klíč přístup** .

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Klikněte na **+ Nový** > **dat + úložiště** > **účtu úložiště**. Budete potřebovat Azure předplatné Vytvořte účet úložišti objektů Blob Azure. Můžete zaregistrovat bezplatnou zkušební verzi na [Azure webu](https://azure.microsoft.com/pricing/free-trial/).
3. Vyberte **Úložiště objektů Blob** ve skupinovém rámečku **Typ účtu**a nechat ostatní hodnoty jako výchozí.  Pokud chcete, můžete si můžete upravit pole Skupina zdroje a umístění.  Klikněte na **vytvořit**.
4. Přejděte zpátky Startboard a klikněte na úložiště klienta, který jste právě vytvořili.
5. V části **Souhrn** klikněte na **+ kontejner**.
6. Zadejte **název** pro kontejner (například "b2c") a vyberte **Blob** **Typ přístupu**. Klikněte na **OK**.
7. Otevřete kontejneru **Vlastnosti**a poznamenejte si adresu URL kontejneru; Příklad by měla vypadat podobně jako `https://contoso.blob.core.windows.net/b2c`. Zavřete zásuvné kontejner.
8. Na zásuvné účtu úložiště klepněte na **Ikonu klíče** a poznamenejte si hodnoty v polích **Název účtu úložiště** a **Primární klíč přístup** .

> [AZURE.NOTE]
    **Primární klíč přístup** je důležité zabezpečení pověření.

### <a name="download-the-helper-tool-and-sample-files"></a>Stahování souborů nástroje a ukázkové Pomocník

Můžete stahovat [soubory nástroj a ukázkové pomocníka úložišti objektů Blob Azure jako soubor ZIP](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) nebo klonovat z GitHub:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Toto úložiště obsahuje `sample_templates\wingtip` adresář, který obsahuje příklad HTML, CSS a obrázky. U těchto šablon neodkazuje účtu úložiště objektů Blob Azure musíte se úpravy souborů ve formátu HTML. Otevřít `unified.html` a `selfasserted.html` a nahradit všechny výskyty `https://localhost` s adresou URL vlastního kontejneru, který jste si zapsali v předchozích krocích. Musíte použít absolutní cesta soubory ve formátu HTML, protože v tomto případě se mohly HTML hodit Azure AD, klikněte v části domény `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>Nahrání souborů ukázka

Ve stejném úložišti unzip `B2CAzureStorageClient.zip` a spusťte `B2CAzureStorageClient.exe` souborů v rámci. Tento program jednoduše uložit všechny soubory v adresáři, který zadáte ke svému účtu úložiště a povolit CORS přístup k těmto souborům. Pokud jste postupovali podle výše popsaných kroků, soubory ve formátu HTML a CSS teď ukazující ke svému účtu úložiště. Všimněte si, že název účtu úložiště je součástí, který předchází `blob.core.windows.net`; například `contoso`. Můžete ověřit, že obsah uložil správně pokusu o přístup k `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` v prohlížeči. Abyste měli jistotu, že obsah je teď CORS povoleno také použijte [http://test-cors.org/](http://test-cors.org/) . (Vyhledejte "XHR stav: 200" ve výsledku.)

### <a name="customize-your-policy-again"></a>Přizpůsobit svoje zásady znovu

Teď, když nahrajete ukázkový obsah k účtu úložiště, je třeba upravit registrace zásady se odkazovat. Opakujte kroky výše, ["Vlastní zásady"](#customize-your-policy) oddílem tentokrát pomocí adresy URL vašeho účtu úložiště. Například umístění svého `unified.html` soubor bude `<url-of-your-container>/wingtip/unified.html`.

Teď můžete na tlačítko **Spustit** nebo vlastní aplikaci znovu provést zásadami. Výsledek by měla vypadat téměř přesně stejné něhož je použit stejný v obou případech přehrajte HTML a šablon stylů CSS. Však vaše zásady teď odkazují vlastní instance úložišti objektů Blob Azure a máte volno provádět úpravy a nahrávání souborů znovu při prosím. Další informace o úpravách HTML a CSS odkazují na [Hlavní článek přizpůsobení uživatelského rozhraní](active-directory-b2c-reference-ui-customization.md).
