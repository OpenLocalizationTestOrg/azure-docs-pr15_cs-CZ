<properties 
    pageTitle="Azure Vícefaktorové ověřování – jak to funguje"
    description="Azure Vícefaktorové ověřování pomáhá chránit přístup k datům a aplikace během schůzky uživatele služba jednoduchý proces přihlášení. Poskytuje další zabezpečení vyžadováním druhý formulář ověřování a poskytuje silných ověřovat pomocí celou řadu možností snadno ověření."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

#<a name="how-azure-multi-factor-authentication-works"></a>Jak funguje Azure Vícefaktorové ověřování

Zabezpečení vícefaktorové ověřování spočívá v jeho ve vrstvách přístup. Narušení několika faktorech ověřování představuje významné ověřovací kód pro útočníků. I když se zlými úmysly spravuje další heslo uživatele, je nelze bez nutnosti získáním důvěryhodné zařízení. By měl uživatel dojít ke ztrátě zařízení, ten, kdo ho nalezne nebude moct používat, pokud se u něho také zná heslo uživatele.

![Proofup](./media/multi-factor-authentication-how-it-works/howitworks.png)



Azure Vícefaktorové ověřování pomáhá chránit přístup k datům a aplikace během schůzky požádání uživatele pro jednoduchý proces přihlášení.  Poskytuje další zabezpečení vyžadováním druhý formulář ověřování a poskytuje silných ověřovat pomocí celou řadu možností snadno ověření:

- telefonního hovoru
- textové zprávy
- mobilní aplikace oznámení – umožňuje uživatelům zvolte způsob jejich raději
- mobilní aplikace ověřovací kód
- 3 tokeny MÍSTOPŘÍSEŽNÉM stran

Další informace páni jak to funguje naleznete v následujícím videu.

>[AZURE.VIDEO multi-factor-authentication-deep-dive-securing-access-on-premises]

##<a name="methods-available-for-multi-factor-authentication"></a>Metody pro vícefaktorové ověřování
Pokud se uživatel přihlásí, další ověření se pošle uživateli.  Následuje seznam metody, které se dá použít pro tento druhý ověření.

Metodu ověření  | Popis
------------- | ------------- |
Telefonního hovoru | Volání je umístěn na smartphonu uživatele s žádostí o ověření, že se přihlásíte stisknutím kombinace kláves znak #.  To bude dokončení procesu ověření.  Tato možnost je možné nakonfigurovat a lze změnit na kód, který určíte.
Textové zprávy | Textové zprávy může být odeslané uživatele Smartphone s kódem 6 číslici.  Zadejte tento kód v dokončete proces ověření.
Mobilní aplikace oznámení | Žádost o ověření odešle na smartphonu uživatele požádáte dokončení ověření tak, že vyberete ověření v mobilní aplikaci. Tomu dojde, pokud jste vybrali oznámení aplikace jako primární ověření prostředek.  Pokud budou upozorněni to nejste přihlášení, můžete zvolit vykazování jako podvodu.
Ověřovací kód pomocí mobilní aplikace | Ověřovací kód odešle v mobilní aplikaci, na kterém běží na smartphonu uživatele.  Tomu dojde, pokud jste vybrali ověřovací kód jako primární ověření prostředek.


##<a name="available-versions-of-azure-multi-factor-authentication"></a>Dostupných verzí Azure Vícefaktorové ověřování
Azure Vícefaktorové ověřování je k dispozici ve tři různé verze.  Následující tabulka popisuje tyto podrobněji.

Verze  | Popis
------------- | ------------- |
Vícefaktorové ověřování pro Office 365 | Tato verze funguje výhradně s aplikacemi Office 365 a spravuje se z portálu Office 365. Takže správci teď zabezpečit jejich zdroje Office 365 pomocí vícefaktorové ověřování.  Tato verze je součástí předplatného Office 365.
Vícefaktorové ověřování pro Azure správce | Stejné podmnožinu funkce Vícefaktorové ověřování pro Office 365 bude k dispozici zdarma všem správcům Azure. Každý účet správce Azure předplatného nyní lze získat dodatečnou ochranu povolením tuto funkci vícefaktorové ověřování core. Aby správce, který chce přístup na portál Azure k vytvoření OM, webového serveru, spravovat úložiště, mobilních služeb nebo jiné služby Azure můžete přidat vícefaktorové ověřování svého účtu správce.
Azure Vícefaktorové ověřování | Azure Vícefaktorové ověřování nabízí nejkomplexnější sadu funkcí. <br><br>Poskytuje další možnosti konfigurace prostřednictvím portálu Správa Azure, Upřesnit podávání zpráv a podpora pro oblast místních i cloudových aplikací. Azure Vícefaktorové ověřování se dá zakoupit jako samostatnou licenci a kombinovaný v Azure Active Directory Premium a Enterprise mobilita sadu. <br><br>Jej lze také zakoupit na základě spotřebu vytvořením poskytovatele Azure Multi-Factor Authentication v Azure předplatného.
##<a name="feature-comparison-of-versions"></a>Porovnání funkcí verzí
V následující tabulce níže obsahuje seznam funkcí, které jsou dostupné v různých verzích Azure Vícefaktorové ověřování.


Funkce  | Vícefaktorové ověřování pro Office 365 (součástí Office 365 SKU)|Vícefaktorové ověřování pro správce Azure (zahrnutý v předplatném Azure) | Azure Vícefaktorové ověřování (součástí Azure AD Premium a sadu mobilita Enterprise)
------------- | :-------------: |:-------------: |:-------------: |
Správci můžete chránit účty s MFA| * | * (Dostupné jenom u účtů správce Azure)|*
Mobilní aplikace jako druhý faktorem|* | * | *
Zavolat jako druhý faktor|* | * | *
Služby SMS jako druhý faktor|* | * | *
Aplikace hesla pro klienty, které nepodporují MFA|* | * | *
Správce publikum nemůže ovládat metody ověřování| *| *| *
Režim PIN kódu| | | *
Podvod upozornění| | | *
Sestavy MFA| | | *
Jednorázové přemostění| | | *
Vlastní pozdrav pro telefonní hovory| | | *
Přizpůsobení ID volajícího pro telefonní hovory| | | *
Potvrzení události| | | *
Důvěryhodné IP adresy| | | *
Pozastavení MFA nalezenou zařízení (veřejné verze Preview)| | | *
MFA SDK| | | *
MFA pro místní aplikace pomocí serveru MFA| | | *


##<a name="how-to-get-azure-multi-factor-authentication"></a>Jak získat Azure Vícefaktorové ověřování

Pokud byste chtěli plnou funkčnost nabízená Azure Vícefaktorové ověřování místo pouze ty, které jsou k dispozici pro uživatele Office 365 a Azure správci, spusťte ho několika způsoby:

1.  Nákup licencí Azure Vícefaktorové ověřování a přiřadit uživatelům.
2.  Nákup licencí, které mají Azure Vícefaktorové ověřování kombinovaný obsahují například Azure Active Directory Premium nebo Enterprise mobilita sady a přiřadit uživatelům.
3.  Vytvoření poskytovatele Azure Multi-Factor Authentication Azure předplatného. Pokud ještě nemáte předplatné Azure, můžete registraci Azure zkušební předplatné. Zkušební předplatné muset převedou na běžná předplatná před vypršení platnosti zkušební verze.

Při použití poskytovatele Azure Multi-Factor Authentication existují dva použití modely k dispozici, které je faktura prostřednictvím předplatného Azure:


- **Jednotlivé uživatele**. Obecně pro podniky, které chcete povolit vícefaktorové ověřování pro pevného počtu uživatelů, kteří pravidelně potřebují ověřování.
- **Za ověření**. Obecně pro podniky, které chcete povolit vícefaktorové ověřování pro velkou skupinu externích uživatelů, kteří často nutné ověřování.

Ceny podrobnosti najdete v článku [Azure MFA ceny.](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)

Vyberte počet licencí nebo na základě spotřebu model, který je nejvhodnější pro vaši organizaci.   Klikněte na získat Začínáme najdete v článku [Začínáme](multi-factor-authentication-get-started.md)
