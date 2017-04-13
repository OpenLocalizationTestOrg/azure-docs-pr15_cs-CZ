<properties
   pageTitle="Ověřování a autorizace s Power BI vložený"
   description="Ověřování a autorizace s Power BI vložený"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Ověřování a autorizaci s Power BI vložený

Služba Power BI vložený používá **klíče** a **Tokeny aplikace** pro a tak mohli ověřovat, místo ověřování explicitní koncových uživatelů. V tomto modelu aplikace má na starosti ověřování a povolení pro vaši koncoví uživatelé. V případě potřeby aplikace vytvoří a odešle tokeny aplikace, která sděluje, naší služby k vykreslování požadovanou zprávu. Tento návrh nevyžaduje aplikace pomocí služby Azure Active Directory pro uživatele a tak mohli ověřovat, ale pořád můžete.

## <a name="two-ways-to-authenticate"></a>Dva způsoby, jak ověřit

**Klíč** – pomocí kláves pro všechna Power BI vložený REST API volání. Klávesy najdete **Azure portál** kliknutím na **všechna nastavení** a potom **přístupových kláves z verze**. Vždy zacházejte kód, jako by byly hesla. Klávesy mít oprávnění k provádění všech rozhraní REST API zavolat na kolekci konkrétní pracovního prostoru.

Pomocí klíče Telefonuji REST, přidejte následující záhlaví se tak mohli ověřovat:            

    Authorization: AppKey {your key}

**Aplikace token** - tokeny se používají pro všechny vložení žádosti o aplikaci. Jsou určeny proběhnout klienta, ať jste omezení do jedné sestavy a je nejvhodnější chcete nastavit dobu platnosti.

Tokeny aplikace jsou (JSON Web tokenu) JWT podpisem jednu z vašich klíčů.

Tokenu aplikace mohou obsahovat následující požadavky:

| Deklarace      | Popis        |
|--------------|------------|
| **verze**      | Verze aplikace token. 0.2.0 je aktuální verzi.       |
| **oblast**      | Určeného příjemce tokenu. Pro použití Power BI vložený: "https://analysis.windows.net/powerbi/api".  |
| **iss**      |  Řetězec určující aplikace, který vystavil tokenu.    |
| **Typ**     | Typ token aplikace, který se vytváří. Aktuální jediný podporovaný typ je **Vložit**.   |
| **WCN**      | Název kolekce pracovního prostoru tokenu vydána.  |
| **souboru**      | Pracovní prostor ID tokenu vydána.  |
| **odstranění**      | ID sestavy tokenu vydána.     |
| **uživatelské jméno** (volitelné) |  Použití s RLS, toto je řetězec, který vám pomůže identifikovat uživatele při použití RLS pravidel. |
| **role** (volitelné)   |   Řetězec obsahující role vyberte při použití pravidla řádku úroveň zabezpečení. Pokud předávání více než jednu roli, by měl předané jako palčivost pole.    |
| **Exp** (volitelné)    |   Označuje čas, ve kterém tokenu vyprší. Tyto by měl být předaný jako časová razítka Unix.   |
| **NBF** (volitelné)    |   Označuje při spuštění v jakém tokenu platná. Tyto by měl předaný jako časová razítka Unix.   |

Token aplikace ukázka bude vypadat takhle:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-coded.png)


Když dekódovat, bude vypadat takto:

![](media\power-bi-embedded-app-token-flow\power-bi-embedded-app-token-flow-sample-decoded.png)


## <a name="heres-how-the-flow-works"></a>Funguje takto tok

1. Zkopírujte klávesy rozhraní API aplikace. Klávesy získáte v **Portálu Azure**.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

2. Token uplatňuje deklaraci a má čas vypršení platnosti.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-2.png)

3. Token získá přihlášenými pomocí rozhraní API přístupových kláves.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-3.png)

4. Požadavky na uživatele k zobrazení sestavy.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-4.png)

5.  Token proběhne pomocí rozhraní API přístupových kláves.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-5.png)

6.  Power BI vložený odešle zprávu o uživateli.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-6.png)

Po **Power BI vložený** odešle zprávu o uživateli, uživatel může zobrazit sestavu v svou vlastní aplikaci. Například pokud jste importovali [Analýza PBIX dat prodeje ukázkové](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), vzorku web appu by vypadal takto:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)

## <a name="see-also"></a>Viz taky
- [Začínáme s Microsoft Power BI vložený ukázka](power-bi-embedded-get-started-sample.md)
- [Obvyklé scénáře Microsoft Power BI vložený](power-bi-embedded-scenarios.md)
- [Začínáme s Microsoft Power BI vložený](power-bi-embedded-get-started.md)
