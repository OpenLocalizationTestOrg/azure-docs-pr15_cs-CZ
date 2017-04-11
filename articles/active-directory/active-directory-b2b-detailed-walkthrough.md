<properties
   pageTitle="Podrobné informace o použití náhledu spolupráce Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B spolupráce podporuje vztahy mezi více společnostmi povolením obchodní partnery selektivně přístup k podnikové aplikace"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-detailed-walkthrough"></a>Náhled spolupráce Azure AD B2B: podrobné informace

Tento návod popisuje použití Azure AD B2B spolupráce. Jako správce IT Contoso chceme sdílení aplikací se zaměstnanci tři partnera společností. Žádná z partnera společnosti musí mít Azure AD.

- Alice z organizační jednoduché partnera
- Jan od střední partnera organizační potřebuje přístup k nastavení aplikace
- Věra z složité organizace partnera, potřebuje přístup k sadu aplikací a členství ve skupinách v Contoso

Po odeslání pozvánky uživatelům partnerů jsme jejich konfigurace v Azure AD udělit přístup k aplikacím a členství ve skupinách portálu Azure. Začneme tím, že přidání Alice.

## <a name="adding-alice-to-the-contoso-directory"></a>Přidání Alice directory společnosti Contoso
1. Vytvořte soubor .csv s záhlaví dle, naplňovat pouze Alice **e-mailu**, **DisplayName**a **InviteContactUsUrl**. **DisplayName** je název, který se zobrazí v pozvánce na schůzku a název, který se zobrazí v adresáři společnosti Contoso Azure AD. **InviteContactUsUrl** způsobem Alice kontaktovat Contoso. V následujícím příkladu určuje InviteContactUsUrl LinkedIn profilu společnosti Contoso. Je důležité pravopisu popisky v první řádek souboru .csv přesně podle [odkaz na formát souboru CSV](active-directory-b2b-references-csv-file-format.md).  
![Ukázkový soubor CSV Alice](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. Na portálu Azure přidání uživatele do directory společnosti Contoso (Active Directory > Contoso > Uživatelé > Přidat uživatele). V rozevíracím seznamu "Uživatel typu" Vyberte "Uživatelů v partnera společnosti". Nahrání souboru CSV. Ujistěte se, že je soubor CSV zavřený před nahráním.  
![Odeslání souboru CSV pro Alice](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. Alice je teď vyjádřených jako externího uživatele v adresáři společnosti Contoso Azure AD.  
![Alice je uvedený v Azure AD](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. Alice obdrží následující e-mailu.  
![E-mail s pozvánkou pro Alice](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Alice klikne na odkaz a tím výzva pozvání přijmout a přihlaste se pomocí své přihlašovací údaje práce. Pokud Alice není v adresáři Azure AD, Alice výzva k přihlášení.  
![Registrace po pozvánku Alice](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. Alice se přesměruje do aplikace Access panelu prázdné, dokud Anna je povolit přístup k aplikacím.  
![Panel přístup pro Alice](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

Tento postup umožní nejjednodušší podobě B2B spolupráce. Jako uživatel v adresáři společnosti Contoso Azure AD Alice můžete k ní mít udělený přístup k aplikacím a skupiny pomocí portálu Azure. Teď Pojďme přidat Jan, kdo potřebuje přístup k aplikacím Moodle a služby Salesforce.

## <a name="adding-bob-to-the-contoso-directory-and-granting-access-to-apps"></a>Přidání Jan directory společnosti Contoso a udělení přístupu k aplikacím
1. Pomocí Windows Powershellu s modul Azure AD nainstalovaný aplikace ID Moodle a služby Salesforce najít. ID můžete získat pomocí rutiny: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` tím se vyvolá seznam všech dostupných aplikací v Contoso a jejich AppPrincialIds.  
![Načtení ID pro Jan](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. Vytvořte soubor .csv obsahující Janův e-mailu a DisplayName **InviteAppID**, **InviteAppResources**a InviteContactUsUrl. Vyplnění **InviteAppResources** s AppPrincipalIds Moodle a služby Salesforce nalezen z prostředí PowerShell oddělená mezerou. Vyplnění **InviteAppId** s stejné AppPrincipalId Moodle brand e-mailu a přihlaste se na stránkách s logem Moodle.  
![Ukázkový soubor CSV Jan](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. Nahrání souboru .csv portálu Azure stejně jako tomu bylo pro Alice. Jan je teď externích uživatelů v adresáři společnosti Contoso Azure AD.

4. Jan obdrží následující e-mailu.  
![E-mail s pozvánkou pro Jan](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Jan klikne na odkaz a bude muset přijmout pozvání. Po osoba je přihlášený, přesměrován do panelu přístup a už použít Moodle a služby Salesforce.  
![Panel přístup pro Jan](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

Přidáme Věra další, kdo potřebuje přístup k aplikacím i členství ve skupinách v adresáři společnosti Contoso.

## <a name="adding-carol-to-the-contoso-directory-granting-access-to-apps-and-giving-group-membership"></a>Přidání Věra directory společnosti Contoso, udělit přístup k aplikacím a poskytuje členství ve skupinách

1. Instalace pro vyhledání ID aplikace a ID skupiny v rámci Contoso modulu Azure AD pomocí prostředí Windows PowerShell.
 - Načtení AppPrincipalId pomocí rutiny `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`stejné jako u Jan
 - Obnovení objektu pro skupiny pomocí rutiny `Get-MsolGroup | fl DisplayName, ObjectId`. Tím se vyvolá seznam všech skupin ve společnosti Contoso a jejich OutFile. ID skupiny lze získat také jako ID objektu na kartě Vlastnosti skupiny na portálu Azure.  
![Načtení ID a skupiny pro Věra](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. Vytvořte soubor .csv, naplňovat Věra jeho e-mailu DisplayName, InviteAppID, InviteAppResources, **InviteGroupResources**a InviteContactUsUrl. **InviteGroupResources** vyplněné tak, že OutFile skupiny MyGroup1 a externích typů oddělená mezerou.  
![Ukázkový soubor CSV Věra](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. Nahrání souboru .csv portálu Azure.

4. Věra je uživatele v adresáři společnosti Contoso a také členem skupiny MyGroup1 a externích typů, jak je vidět na portálu Azure.  
![Věra je uvedený ve skupině v Azure AD](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. Věra obdrží e-mail obsahující odkaz pozvání přijmout. Poté, co uživatel přihlásí, bude se přesměruje do panelu aplikace Access mít taky přístup k Moodle a služby Salesforce.  

To je vše, existuje pro přidávání uživatelů z firmy partnera Azure AD B2B obsah. Tento návod ukázal, jak přidávat uživatele Alice, Jan a Věra directory společnosti Contoso používání tři samostatné CSV souborů. Tento postup lze provést jednodušší kondenzačních samostatné soubory do jednoho souboru.  
![Ukázkový soubor CSV Alice, Jan a Věra](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## <a name="related-articles"></a>Související články
Procházejte naše další články o Azure AD B2B spolupráci:

- [Co je Azure AD B2B spolupráce?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Jak to funguje](active-directory-b2b-how-it-works.md)
- [Odkaz na formát souboru CSV](active-directory-b2b-references-csv-file-format.md)
- [Formát tokenu externích uživatelů](active-directory-b2b-references-external-user-token-format.md)
- [Externí uživatel objektu atributu změny](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuální verze preview omezení](active-directory-b2b-current-preview-limitations.md)
- [Index článek Správa aplikací služby Azure Active Directory](active-directory-apps-index.md)
