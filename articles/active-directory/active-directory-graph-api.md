<properties
   pageTitle="Graf Azure Active Directory rozhraní API | Microsoft Azure"
   description="Podívejte se na rychlý úvod a základní informace Průvodce pro rozhraní API grafu, který umožňuje programový přístup k Azure AD pomocí rozhraní REST API koncové body."
   services="active-directory"
   documentationCenter=""
   authors="PatAltimore"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-graph-api"></a>Graf Azure Active Directory rozhraní API

> [AZURE.IMPORTANT] Azure AD grafu rozhraní API funkce je také k dispozici prostřednictvím [Aplikace Microsoft Graph](https://graph.microsoft.io/)jednotné rozhraní API, který obsahuje rozhraní API z jiných služeb společnosti Microsoft, jako je Outlook, Onedrivu, OneNote, plánování a Office Graphu, získat přístup prostřednictvím jeden koncový bod a pomocí jednoho přístupový token.

Azure Active Directory grafu rozhraní API poskytuje programový přístup k Azure AD pomocí rozhraní REST API koncové body. Aplikace můžete provádět rozhraní API graf vytvořit, číst, aktualizovat a odstranění (CRUD) operací na adresář data a objekty. Například rozhraní API graf podporuje následující společná operace pro objekt uživatele:

- Vytvoření nového uživatele v adresáři

- Získání podrobných vlastností uživatele, například jejich skupin

- Aktualizace vlastností uživatele, jako je jejich umístění a telefonního čísla, nebo změnit svoje heslo

- Zkontrolovat členství ve skupinách uživatele k přístupu na základě rolí

- Zakázání uživatelský účet nebo odstranit úplně

Kromě objekty uživatelů můžete provádět podobné operace na jiné objekty, jako jsou skupiny a aplikací. Aplikace pro volání rozhraní API grafu na adresář, musí být registrován u Azure AD a nakonfigurovat tak, aby k adresáři přístup. Dosahuje se normálně až uživatel nebo správce toku souhlas.

Začít používat Azure Active Directory grafu rozhraní API, najdete v článku [Průvodce rychlým startem rozhraní API grafu](active-directory-graph-api-quickstart.md)nebo zobrazit [interaktivní referenční dokumentaci rozhraní API grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).


## <a name="features"></a>Funkce

Rozhraní API graf obsahuje následující funkce:

- **Koncové body REST API**: API graf je služba RESTful tvořen koncové body, které jsou k nim získat přístup pomocí standardní požadavků HTTP. Rozhraní API grafu podporuje XML a Javascript Object Notation (JSON) typy obsahu pro žádosti o schůzku a odpovědi. Další informace najdete v článku Principy [Azure AD grafu REST API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- **Ověřování s Azure AD**: všechny žádosti o k rozhraní API grafu musí být ověřeny přidáním JSON Web tokenu (JWT) v záhlaví se tak mohli ověřovat žádosti o. Tento token vzniká tím, že žádost o tokenu koncový bod Azure AD a poskytování platných přihlašovacích údajů. Můžete použít toku OAuth 2.0 klienta přihlašovací údaje nebo kód se tak mohli ověřovat udělit toku získat token volání grafu. Další informace najdete [v Azure AD 2.0 OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx).

- **Na základě rolí se tak mohli ověřovat (RBAC)**: skupiny zabezpečení slouží k provádění RBAC v rozhraní API graf. Pokud chcete zjistit, jestli má uživatel přístup k určitému zdroji, například aplikace upoutat operace [Kontrola členství ve skupinách (přenosné)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) , která vrátí hodnotu true nebo false.

- **Rozdíl dotaz**: Pokud chcete vyhledat změny v adresáři mezi dvěma obdobími čas bez nutnosti provádět časté dotazy k rozhraní API grafu, můžete provést žádost o rozdíl dotazu. Tento typ požadavku vrátí jenom změny mezi předchozí rozdílné dotaz a aktuální žádosti. Další informace najdete v tématu [Azure AD grafu rozhraní API rozdílné dotazu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).

- **Rozšíření Directory**: Pokud vyvíjíte aplikace, kterou je potřeba načtení nebo zápisu dat jedinečné vlastnosti pro objekty adresáře, můžete zaregistrovat a používat rozšíření hodnoty pomocí rozhraní API grafu. Pokud vaše aplikace vyžaduje vlastnost ID Skypu pro každého uživatele, zaregistrujete nové vlastnosti v adresáři a bude k dispozici pro každý uživatel objekt. Další informace najdete v článku [rozšíření schématu adresáře rozhraní API Azure AD grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).

- **Zabezpečené obory oprávnění**: AAD grafu rozhraní API aplikace zpřístupňuje obory oprávnění, které umožňují zabezpečené/souhlas přístup k datům AAD a podporoval co nejvíc klientské aplikace typů, včetně:
 - můžou být s uživatelským rozhraním, které jsou uvedeny delegované přístup k datům prostřednictvím se tak mohli ověřovat z přihlášení uživatele (delegovat)
  - používajících aplikace – definování řízení přístupu na základě rolí třeba klienti service/démon (role aplikace)

    Delegované i aplikace role oprávnění obory představují oprávnění vystaveným rozhraní API grafu a mohou být požadována v klientských aplikacích prostřednictvím aplikace registrace oprávnění [funkce na portálu Azure klasické](https://manage.windowsazure.com). Klienti můžete ověřit obory oprávnění udělena k nim kontrolou deklaraci obor (spojovací bod "služby") obdrželi přístupový token delegovaného oprávnění a role ("role") převzetí role oprávnění aplikace. Další informace o [rozsahů Azure AD graf rozhraní API oprávnění](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).


## <a name="scenarios"></a>Scénáře

Rozhraní API grafu umožňuje mnoho scénářů aplikace. V následujících scénářích jsou nejběžnější:

- **Aplikace obchodními (jednoho klienta)**: V tomto scénáři enterprise vývojář vyhovovat organizací, která má předplatné Office 365. Vývojář vytváří webové aplikace, který spolupracuje s Azure AD můžou provádět tyto přiřazení licence uživateli. Tento úkol vyžaduje přístup k rozhraní API grafu tak, aby registry vývojář jednoho tenanta aplikace v Azure AD a nakonfiguruje oprávnění čtení a zápis pro rozhraní API grafu. Potom aplikaci nakonfigurovaný použít vlastní přihlašovací údaje nebo aktuálně přihlášení uživatele k získání tokenu volání rozhraní API grafu.

- **Software jako aplikace služby (více klienta)**: V tomto scénáři nezávisle prodejci vyvíjí hostovanou více klienta webovou aplikaci, která poskytuje funkce správy uživatelů pro jinými organizacemi, které používají Azure AD. Tato funkce vyžaduje přístup k objektů adresáře a tak potřebuje pro volání rozhraní API grafu. Vývojář zaregistruje aplikaci v Azure AD, nakonfiguruje jej vyžadují pro čtení a zápis oprávnění pro rozhraní API grafu a potom umožňuje externí přístup tak, aby jinými organizacemi můžete souhlas s pomocí aplikace v jejich adresář. Pokud uživatel v jiné organizace ověřuje aplikaci poprvé, se zobrazí dialogové okno souhlas s oprávnění, která vyžaduje aplikaci.  Udělení souhlas vám umožní pak aplikace ty požadované oprávnění k rozhraní API grafu v adresáři uživatele. Další informace o rozhraní souhlas najdete v článku [základní informace o rozhraní souhlas](active-directory-integrating-applications.md).

## <a name="see-also"></a>Viz taky

[Průvodce rychlým startem Azure AD grafu rozhraní API](active-directory-graph-api-quickstart.md)

[AD si přečtěte následující dokumentaci ZBÝVAJÍCÍ grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Příručka Azure Active Directory vývojář](active-directory-developers-guide.md)
