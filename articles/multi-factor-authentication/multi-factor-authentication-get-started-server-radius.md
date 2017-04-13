<properties 
    pageTitle="Ověřování a Azure Vícefaktorové ověřování serveru"
    description="Tohle je stránka Azure Multi-Factor ověřování, které vám pomohou při nasazení ověřování a Azure Multi-Factor ověřování serveru."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>



# <a name="radius-authentication-and-azure-multi-factor-authentication-server"></a>Ověřování a Azure Vícefaktorové ověřování serveru

V části ověřování RADIUS umožňuje povolení a konfigurace ověřování pro službu Azure Multi-Factor Authentication. RADIUS je standardní protokol k přijímaní žádostí ověřování a zpracuje tyto požadavky. Server Azure Multi-Factor Authentication slouží jako RADIUS server a je vložen mezi klientem RADIUS (například zařízení VPN) a cíl ověřování, které by mohly být Active Directory (AD), adresář LDAP nebo jiném RADIUS serveru, abyste mohli přidat Azure Vícefaktorové ověřování. Pro Azure Vícefaktorové ověřování fungovala musíte nakonfigurovat Server Azure Multi-Factor Authentication tak, aby mohli komunikovat s servery klienta a cíl ověřování. Server Azure Multi-Factor Authentication přijímá žádosti z klienta RADIUS ověří přihlašovací údaje s cílem ověřování, přidá Azure Vícefaktorové ověřování a odešle zpět klienta RADIUS odpověď. Celý ověřování proběhne úspěšně jenom v případě primární ověřování a Vícefaktorové ověřování Azure úspěšné.

>[AZURE.NOTE]
>MFA Server podporuje pouze jedná (heslo ověřování protocol) s MSCHAPv2 (společnosti Microsoft ověřovací kód programu protokol) RADIUS protokoly, když budou sloužit jako serveru RADIUS.  Když MFA server funguje jako proxy RADIUS jiné RADIUS server, který podporuje protokolu například Microsoft NPS můžete použít jiné protokoly například EAP (extensible ověřování protocol).
></br>
>Pokud chcete použít jiné protokoly v této konfiguraci, nebude fungovat jednosměrné služby SMS a MÍSTOPŘÍSEŽNÉM tokeny od serveru MFA nedokáže zahajte úspěšné odpověď ověřovací kód programu RADIUS pomocí protokolu.


![Ověřování](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="radius-authentication-configuration"></a>Konfigurace ověřování RADIUS

Konfigurace ověřování, nainstalujte Server Azure Multi-Factor Authentication v systému Windows server. Pokud ještě prostředí služby Active Directory, má server připojen k domény uvnitř sítě. Konfigurace ověřování serveru Multi-Factor Azure pomocí následujícího postupu:

1. V rámci Server Azure Multi-Factor Authentication klikněte na ikonu ověřování RADIUS v nabídce nalevo.
2. Zaškrtněte políčko Povolit RADIUS ověřování.
3. Na kartě klienti ověřování porty a změňte účetní porty Pokud služby Azure Multi-Factor Authentication RADIUS by měl svázat nestandardní porty poslouchat RADIUS požadavků klientů, které bude možné konfigurovat.
4. Klikněte na Přidat. tlačítko.
5. V dialogovém okně Přidat klienta RADIUS zadejte IP adresu zařízení/serveru, který bude ověřovat Server Azure Multi-Factor Authentication, název aplikace (nepovinné) a sdílené tajná. Sdílené tajná bude potřeba stejné na Azure Multi-Factor Authentication Server a zařízení/server. Název aplikace se zobrazí v sestavách Azure Vícefaktorové ověřování a v ověřování zprávy SMS nebo mobilní aplikaci se může zobrazovat.
6. Zaškrtnutí políčka vyžadovat Vícefaktorové ověřování uživatele POZVYHLEDAT případně všichni uživatelé se budou importovány k serveru a vyměřené poplatky za jeho vícefaktorové ověřování. Pokud velký počet uživatelů nebyly dosud importované do serveru a/nebo bude osvobození od daně z vícefaktorové ověřování, nechejte pole zrušené zaškrtnutí políčka. Zobrazit soubor nápovědy pro další informace o této funkci.
7. Políčko Povolit náhradní MÍSTOPŘÍSEŽNÉM tokenu Pokud budou uživatelé používat mobilní aplikaci Multi-Factor ověřování Azure a chcete použít jako základní ověřování mimo pásma telefonní hovor, služby SMS nebo nabízená oznámení MÍSTOPŘÍSEŽNÉM hesla.
8. Klikněte na tlačítko OK.
9. Může opakujte kroky 4 až 8 přidejte další klientů RADIUS.
10. Klikněte na kartu cílové.
11. Pokud Server Azure Multi-Factor Authentication je nainstalovaná na serveru doméně v prostředí služby Active Directory, vyberte doménu systému Windows.
12. Pokud před adresář LDAP ověřování uživatelů, vyberte vazbu protokolu LDAP. Při použití vazbu protokolu LDAP, musí klikněte na ikonu integrace adresářů a upravte konfigurace LDAP na kartě nastavení tak, aby Server můžete vytvořit vazbu adresáře. Pokyny ke konfiguraci LDAP najdete v Průvodci konfigurací LDAP Proxy.
13. Pokud před jiný server RADIUS ověřování uživatelů, vyberte RADIUS servery.
14. Konfigurace serveru, serveru se proxy RADIUS požadavky na po kliknutí na stránce Přidat... tlačítko.
15. V dialogovém okně Přidat Server RADIUS zadejte IP adresu serveru RADIUS a sdílené tajná. Sdílené tajná bude potřeba stejné na server Azure Multi-Factor ověřování serveru a RADIUS. Použijete-li různé porty serverem RADIUS změňte port ověřování a účetní port.
16. Klikněte na tlačítko OK.
17. Server Azure Multi-Factor Authentication musíte přidat jako klienta RADIUS v jiných RADIUS server tak, aby jej zpracuje odeslaných z Server Azure Multi-Factor Authentication žádostí o přístup. Je nutné použít stejné sdílené tajná konfigurovat Server Azure Multi-Factor Authentication.
18. Může opakujte tento krok můžete přidat další RADIUS servery a nakonfigurovat pořadí, ve kterém serveru by měl jim zavolá pomocí tlačítek nahoru a dolů. To dokončení konfigurace Azure Multi-Factor ověřování serveru. Server je teď naslouchají na nakonfigurováno porty žádostí o přístup RADIUS nakonfigurované klientů.   


## <a name="radius-client-configuration"></a>Konfigurace klienta RADIUS

Abyste mohli nakonfigurovat klienta RADIUS, řiďte se pokyny:

- Konfigurace zařízení/serveru ověřovat pomocí RADIUS k IP adrese na Azure Multi-Factor ověřování serveru, který bude sloužit jako serveru RADIUS.
- Použijte stejný sdílené heslo, které jste nakonfigurovali nad.
- Časový limit RADIUS 30 60 sekund nakonfigurujte tak, že je potřeba ověřit přihlašovací údaje uživatele, provádět vícefaktorové ověřování, přijímání svou odpověď a potom odpověď na žádosti o přístup RADIUS.
