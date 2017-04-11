<properties
   pageTitle="Formát souboru CSV Azure Active Directory B2B (verze Preview) spolupráce | Microsoft Azure"
   description="Azure Active Directory B2B podporuje vztahy mezi více společnostmi povolením obchodní partnery selektivně přístup k podnikové aplikace"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Náhled spolupráce Azure AD B2B: formát souboru CSV

Náhled verzi Azure AD B2B spolupráce vyžaduje zadání informace o partnerovi uživatele k odeslání pomocí portálu Azure AD soubor CSV. Soubor CSV by měl obsahovat požadované popisky pod a volitelná pole podle potřeby. Úprava ukázkový soubor CSV (viz níže) beze změny pravopis popisky v prvním řádku.

>[AZURE.NOTE] První řádek štítků (například e-mailu a DisplayName) je nezbytné k souboru CSV analyzovat úspěšně. Pole podle níže uvedených musí odpovídat pravopis. Tyto popisky identifikovat obsah pod nimi. Volitelná pole, které nepotřebujete můžete jejich popisky odebere ze souboru CSV. Celý sloupec může být prázdné.

## <a name="required-fields-br"></a>Požadovaná pole: <br/>
**E-mailu:** E-mailové adresy pozvaných uživatelů. <br/>
**DisplayName:** Zobrazované jméno pozvaných uživatelů (obvykle křestní jméno a příjmení).<br/>


## <a name="optional-fields-br"></a>Volitelná pole: <br/>

**InvitationText:** Přizpůsobení textu e-mailovou pozvánku po branding aplikace a před zaruč_cena odkaz.<br/>
**InvitedToApplications:** AppIDs na podnikové aplikace přiřazení uživatelů. AppIDs jsou zobrazitelné ve výsledcích vyhledávání v prostředí PowerShell voláním`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:** OutFile pro skupiny přidat uživatele. OutFile jsou zobrazitelné ve výsledcích vyhledávání v prostředí PowerShell voláním`Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectURL:** Adresa URL pro přímý po přijetí pozvání pozvaných uživatelů. Je vhodné firemní adresy URL (třeba [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)). Pokud toto pole není zadán, pozvaní uživatelé dostali na panelu aplikace Access, odkud můžete přejít na zvolený podnikové aplikace. Adresa URL aplikace Access panely je formuláře `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/>
**CcEmailAddress**: e-mailovou adresu zkopírovat mailem pozvánku. Pokud pole CcEmailAddress se používá, této pozvánce nelze použít pro vytvoření klienta nebo ověření e-mailu uživatelů.<br/>
**Jazyk:** Jazyk pro pozvánku e-mailu a zaruč_cena zkušenosti s "en" (v angličtině) jako výchozí, pokud nejsou zadány. Další 10 podporované jazyk, který kódy jsou:<br/>
1. de: němčina<br/>
2. ES: Španělština<br/>
3. FR: francouzština<br/>
4. ho: Italština<br/>
5. ja: japonština<br/>
6. Ko: korejština<br/>
7. pt BR: portugalština (Brazílie)<br/>
8. RU: ruština<br/>
9. zh HANS: zjednodušená čínština<br/>
10. zh HANT: tradiční čínština<br/>

## <a name="sample-csv-file"></a>Ukázkový soubor CSV
Tady je ukázkový soubor CSV můžete změnit.

>[AZURE.NOTE] Zkopírujte a vložte takto do programu Poznámkový blok a uložte jej s příponou "CSV". Potom upravte v aplikaci Excel. Se strukturovanými do tabulky s popisky v prvním řádku.

> Přidejte další volitelná pole v aplikaci Excel určením popisek a vyplnění sloupce pod ní.

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>Související články
Procházet naše další články o Azure AD B2B spolupráci

- [Co je Azure AD B2B spolupráce?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Jak to funguje](active-directory-b2b-how-it-works.md)
- [Podrobné informace](active-directory-b2b-detailed-walkthrough.md)
- [Formát tokenu externích uživatelů](active-directory-b2b-references-external-user-token-format.md)
- [Externí uživatel objektu atributu změny](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuální verze preview omezení](active-directory-b2b-current-preview-limitations.md)
- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
