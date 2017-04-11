<properties
   pageTitle="Branding pokyny pro aplikace | Microsoft Azure"
   description="Komplexní příručku k prostředkům vývojář určený pro službu Azure Active Directory"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/23/2016"
   ms.author="mbaldwin"/>


# <a name="branding-guidelines-for-applications"></a>Branding pokyny pro aplikace


Toto téma popisuje zásadami značky, kterou byste měli použít při vytváření aplikací s Azure Active Directory (Azure AD). Tyto pokyny vám pomůže přímé zákazníky při chtějí používat svůj pracovní nebo školní účet spravovaný v Azure AD, nebo jejich osobní účet pro přihlášení a přihlaste se k aplikaci.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Osobní účty a pracovní nebo školní účty od společnosti Microsoft

Microsoft spravuje dva typy uživatelských účtů:

- **Osobní účty** (dříve Windows Live ID). Tyto účty představují vztah mezi *jednotlivé* uživatele a Microsoft a používané pro přístup zařízení příjemce a služeb společnosti Microsoft. Tyto účty mají pro vlastní potřebu.

- **Pracovní nebo školní účty.** Tyto účty se spravuje Microsoft jménem organizacím používajícím Azure Active Directory. Tyto účty slouží k přihlášení k Office 365 a jiných podnikové od Microsoftu.

Microsoft přiřadí pracovní nebo školní účty obvykle koncovým uživatelům (zaměstnanců, studenty, federální zaměstnanců) podle jejich organizace (společnosti, školy, government subjekt). Tyto účty standardní přímo v cloudu v platformu Azure AD nebo synchronizované s Azure AD z místního adresáře, například služby Active Directory pro Windows Server. Společnost Microsoft se *uschovatel* pracovní nebo školní účty, ale vlastní a řízené pomocí doplňku organizační účty.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Odkazování na Azure AD účtů v aplikaci

Microsoft nemá vystavit koncových uživatelů Azure nebo názvy značku služby Active Directory a ani by se měly.

- Po přihlášení uživatele byste měli použít názvu a loga co nejvíc organizace. To je lepší než pomocí obecný termíny, třeba "organizaci".

- Pokud nejste přihlášení uživatele, se seznamte s svých účtů jako "pracovní nebo školní účty" a umožňuje vyjádřit, jsou tyto účty spravuje Microsoft loga společnosti Microsoft. Nepoužívejte termíny, třeba "účtu organizace", "firmy účtu" nebo "firemní účet", které vytvářejí zmatky uživatele.

## <a name="user-account-pictogram"></a>Piktogramů uživatelského účtu
V dřívější verzi tyto pokyny doporučujeme používat piktogramů "modré Odznáček". Na základě reakcí uživatele a vývojář, teď doporučujeme použít loga společnosti Microsoft místo. To uživatelům pomůže porozumět, aby se znovu použít účet, že jejich použití s Office 365 nebo jiné Microsoft služby podnikové se přihlásit k aplikaci.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Přihlašování a přihlášení pomocí Azure AD

Aplikace předvádějí samostatné cesty pro zápis a přihlašovací a v následujících částech obsahují vizuální pokyny pro obě scénáře.

**Pokud aplikace podporuje koncový uživatel znaménko nahoru (například bezplatnou zkušební verzi nebo freemium modelu)**: je možné zobrazit tlačítko **přihlášení** , které uživatelům umožňuje přístup k aplikaci s účtem práce nebo svůj osobní účet. Azure AD řádku se zobrazí výzva souhlas prvním přístupu k aplikaci.

**Pokud aplikace vyžaduje oprávnění, která mohou pouze správci souhlas, nebo pokud aplikace licencování organizace**: je vhodné oddělit správce pořízení z přihlašovací uživatele. **Kliknutím na tlačítko "získat tato aplikace"** vás přesměruje správci Přihlaste se a požádáte ho, aby udělit souhlas jménem uživatele ve své organizaci. Tato možnost má tu výhodu potlačení pokynů souhlas koncoví uživatelé mohli aplikaci.

## <a name="visual-guidance-for-app-acquisition"></a>Vizuální pokyny pro získání aplikace

Odkaz "získání aplikace" musíte nastavit přesměrování uživatele na Azure AD udělení přístupu (Povolit) stránka umožňuje správcům organizace povolit aplikaci mít taky přístup k datům jejich organizace, která je hostovaný aplikací Microsoft. Podrobnosti o tom, jak požádat o přístup jsou popsány v článku [Aplikace integrace se službou Azure Active Directory](active-directory-integrating-applications.md) .

Po správci souhlas s aplikací, můžete zvolit přidáte do své uživatele Office 365 aplikace Spouštěč možností (přístupných osobám s postižením od na vafli a od [https://portal.office.com/myapps](https://portal.office.com/myapps)). Pokud budete chtít inzerce tuto možnost, můžete použít termíny jako "Přidat tuto aplikaci pro vaši organizaci" a zobrazit tlačítko takto:

![Typy aplikací a scénáře](./media/active-directory-branding-guidelines/add-to-my-org.png)

Doporučujeme ale napište vysvětlující text namísto tlačítka. Příklad:
> *Pokud už používáte Office 365 nebo jiné služby firmy od společnosti Microsoft, můžete jednoduše udělit < your_app_name > přístup k datům vaší organizace. To vám umožní uživatelům přístup < your_app_name > s jejich stávajících účtů práce.*


## <a name="visual-guidance-for-sign-in"></a>Vizuální pokyny pro přihlášení
Aplikace by měl zobrazit přihlášení na tlačítko, které přesměruje uživatele na přihlašovací koncového bodu, který odpovídá protokol, který používáte pro integraci s Azure AD. V následující části obsahuje podrobné informace na toto tlačítko by měl vypadat takto.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Piktogramů a ", přihlaste se pomocí Microsoft"
Je přidružení loga společnosti Microsoft a "Sign in s Microsoft" termíny, které jednoznačně představuje Azure AD mezi ostatní zprostředkovatelé identit jiní aplikace mohou podporovat. Pokud nemáte dost místa pro "Sign in s Microsoft", bude ok zkrácení "Přihlašovací".

![Typy aplikací a scénáře](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Typy aplikací a scénáře](./media/active-directory-branding-guidelines/sign-in-light.png)

Můžete taky tmavě barevné schéma tlačítek.

![Typy aplikací a scénáře](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Typy aplikací a scénáře](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Branding pokyny, jak a co nedělat

**Nepoužívejte "pracovního nebo školního účtu" v kombinaci s tlačítkem "Sign in s Microsoft" Další vysvětlení umožňuje koncovým uživatelům rozpoznat, ať ji může používat.** **Nepoužívejte jinými termíny, třeba "účet enterprise", "firmy účtu" nebo "firemní účet."**

**Nepoužívejte "Office 365 číslo" nebo "Azure".** Office 365 je také název příjemce nabízející od Microsoftu, která nepoužívá Azure AD pro ověřování.

**Logo společnosti Microsoft se nezmění.**

**Není** vystavit koncovým uživatelům Azure nebo Active Directory značky. Je ale ok pro tyto termíny používat s vývojáři, v oboru IT a správce.

## <a name="navigation-dos-and-donts"></a>Pokyny, jak navigace a co nedělat

**Proveďte** umožňují uživatelům odhlaste se a přepnutí na jiný účet. Zatímco většina lidí mít osobní účet z Microsoft/Facebook nebo Google/Twitter, lidé, jsou často přidružené k více než jedné organizace. Podpora více uživatelů přihlášený je brzy k dispozici.
