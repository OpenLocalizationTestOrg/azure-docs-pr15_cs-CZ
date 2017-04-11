<properties
pageTitle="Aplikace služby Azure Active Directory a objektů služby jistinu | Microsoft Azure"
description="Diskuse relace mezi aplikací a služeb objekty v Azure Active Directory"
documentationCenter="dev-center-name"
authors="bryanla"
manager="mbaldwin"
services="active-directory"
editor=""/>

<tags
ms.service="active-directory"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="identity"
ms.date="08/10/2016"
ms.author="bryanla;mbaldwin"/>

# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Aplikace a služby základní objektů v Azure Active Directory
Při čtení o Azure Active Directory (AD) "aplikace" není vždy vymazat přesně co se odkazuje podle autora. Hledání v tomto článku je můžete zlepšit čitelnost, že definujete koncepční a konkrétní aspekty integraci aplikace Azure AD s příkladem registraci a souhlas [více klienta aplikace](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Základní informace
Aplikace Azure AD je širší než jenom list softwaru. Je koncepční termínů týkající se nejen aplikace software, ale taky jeho registrace (označovaná taky jako: Konfigurace identity) s Azure AD, což umožňuje účastnit a tak mohli ověřovat "konverzací" za běhu. Definicí aplikace fungovat v roli [klienta](active-directory-dev-glossary.md#client-application) (jinými zdroj), role [prostředků serveru](active-directory-dev-glossary.md#resource-server) (vystavující rozhraní API pro klienty) a dokonce i. Protokol konverzace je definován [OAuth 2.0 se tak mohli ověřovat udělit toku](active-directory-dev-glossary.md#authorization-grant), s cílem povolení klienta/zdroje přístup/ochraně zdroje dat v tomto pořadí. Teď Pojďme úroveň níž a najdete v článku jak model aplikace Azure AD interně představuje aplikace. 

## <a name="application-registration"></a>Registrace aplikace
Při registraci aplikace v [Azure klasické portál][AZURE-Classic-Portal], dva objekty vytvořené ve vašem klientovi Azure AD: objekt aplikace a služby objekt.

#### <a name="application-object"></a>Objekt aplikace
Aplikace Azure AD je jeho jeden *definované* a pouze aplikace objekt, který je umístěn v Azure AD klienta, kde je aplikace zaregistrovaná, označovány jako "doma" klienta aplikace. Objekt aplikace obsahuje informace týkající se identity pro aplikaci a je šablona, ze kterého odpovídající hlavní objekty služby jsou *odvozeny* pro použití při spuštění. 

Můžete si myslíte o aplikaci jako *globální* znázornění aplikace (pro použití přes všechny klienty) a hlavní služby jako *místní* zastoupení (pro použití v určité klienta). Azure AD grafu [aplikace entity] [ AAD-Graph-App-Entity] definuje schéma objekt aplikace. Objekt aplikace proto má relace 1:1 aplikace software a hodnotu 1:*n* relace s jeho odpovídající objekty hlavní služby *n* .

#### <a name="service-principal-object"></a>Objekt služby
Objekt služby definuje zásady a oprávnění pro aplikaci, poskytnutí základem zabezpečení jistinu představující při přístupu k prostředkům při spuštění aplikace. Azure AD grafu [ServicePrincipal entity] [ AAD-Graph-Sp-Entity] definuje schéma pro službu objekt. 

Služba objekt je potřeba v každého klienta, pro kterou instanci aplikace použití musí zastupování povolení zabezpečeného přístupu k prostředkům vlastněná uživatelských účtů z tohoto klienta. Aplikace jednoho klienta bude mít jenom jednu jistinu služby (v domácí klienta). Více klienta [webové aplikace](active-directory-dev-glossary.md#web-client) bude mít taky zajištěný hlavní název služby do každého klienta, kde správce nebo uživatele ze tomuto klientovi udělíte souhlas, povolí přístup k jejich zdroje. Po souhlas bude k objektu služby projednání pro budoucí se tak mohli ověřovat požadavky. 

> [AZURE.NOTE] Všechny změny provedené v aplikaci objektu, se projeví v jeho služby objekt aplikace domácí klientovi pouze (klienta, kdy byla registrované). Pro více klienta aplikace k objektu aplikace neprojeví v libovolné spotř klientů služby základní objekty, dokud klienta spotř odebere přístup a znovu povolí přístup.

## <a name="example"></a>Příklad
Následující obrázek znázorňuje vztah mezi aplikací objektu a odpovídající aplikace služby základní objektů v kontextu více klienta aplikace vzorku s názvem **HR aplikace**. V tomto scénáři existují tři Azure AD klienti: 

- **Adatum** - klienta používá společnost, která vyvinuté **HR aplikace**
- **Contoso** - organizace Contoso, což je příjemce **HR aplikace** používá klienta
- **Fabrikam** - Fabrikam organizace, který taky používá **aplikaci HR** používá klienta

![Vztah mezi objekt aplikace a služby objekt](./media/active-directory-application-objects/application-objects-relationship.png)

Krok 1 v předchozím obrázku je proces vytváření aplikací a služeb objekty v klientovi výchozí aplikace.

V části Krok 2 až Contoso a Fabrikam správci dokončit souhlas, objekt služby vytvořený ve své společnosti Azure AD klienta a přiřazené oprávnění, která správce. Navíc nezapomeňte, že aplikace HR může být nakonfigurováno/umožňují souhlas uživatelé pro jednotlivé použití.

V kroku 3 mít klienti spotř HR aplikace (Contoso a Fabrikam) každý objekt vlastní služby. Každý představuje jejich použití instanci aplikace za běhu, řídí oprávnění souhlas odpovídajících správcem.

## <a name="next-steps"></a>Další kroky
Objekt aplikace aplikace můžete k nim získat přístup prostřednictvím rozhraní API Azure AD graf jako reprezentované jeho OData [entity aplikace][AAD-Graph-App-Entity]

Aplikace služby objekt můžete k nim získat přístup prostřednictvím rozhraní API Azure AD graf jako představované jeho OData [ServicePrincipal entity][AAD-Graph-Sp-Entity]



<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Classic-Portal]: https://manage.windowsazure.com