<properties 
    pageTitle="LDAP ověřování a Azure Vícefaktorové ověřování serveru"
    description="Tohle je stránka Azure Multi-Factor ověřování, které vám pomohou při nasazení LDAP ověřování a Azure Multi-Factor ověřování serveru."
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
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP ověřování a Azure Vícefaktorové ověřování serveru


Ve výchozím nastavení je Azure Multi-Factor Authentication Server nakonfigurovaný k importovat a synchronizovat uživatele ze služby Active Directory. Však může být nakonfigurované pro přiřadit různé adresářích LDAP, například ADAM adresáře nebo konkrétní řadiče domény Active Directory. Při konfiguraci připojit k adresáři prostřednictvím protokolu LDAP Server Azure Multi-Factor Authentication lze nastavit má fungovat jako LDAP proxy provádět ověřování. Také umožňuje použití vazbu protokolu LDAP jako cíl RADIUS před ověřování uživatelů při použití ověřování služby IIS nebo primární ověřování na portálu Azure Multi-Factor Authentication uživatele.

Pokud používají Azure Vícefaktorové ověřování proxy serveru pro LDAP, Server Azure Multi-Factor Authentication vložen mezi klientem LDAP (například VPN zařízení aplikace) a serveru adresář LDAP-li přidat vícefaktorové ověřování. Pro Azure Vícefaktorové ověřování fungovala musí být nakonfigurované Server Azure Multi-Factor Authentication komunikovat s servery klienta a na adresář LDAP. V této konfiguraci Server Azure Multi-Factor Authentication přijímá žádosti o LDAP z klienta servery a aplikace a předá je cílový server adresář LDAP ověřit primární pověření. Když odpověď adresář LDAP zjistí, že primární přihlašovací údaje jsou platné, Azure Vícefaktorové ověřování provede druhé faktorem ověřování a odešle zpět v klientovi LDAP odpověď. Celý ověřování proběhne úspěšně pouze v případě, že ověřování serveru LDAP a vícefaktorové ověřování úspěšný.





## <a name="ldap-authentication-configuration"></a>Konfigurace ověřování LDAP


Konfigurace ověřování LDAP, nainstalujte Server Azure Multi-Factor Authentication v systému Windows server. Pomocí následujícího postupu:

1. V rámci Server Azure Multi-Factor Authentication klikněte na ikonu LDAP ověřování v nabídce nalevo.
2. Zaškrtněte políčko Povolit LDAP ověření.![Ověření protokolu LDAP](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)
3. Na kartě klienti změňte Pokud by měl služby Azure Multi-Factor Authentication LDAP poslouchat LDAP požadavků klientů, které bude možné konfigurovat svázat nestandardní porty TCP a SSL port.
4. Pokud máte v plánu používat LDAPS z klienta na server Azure Multi-Factor Authentication, musí být certifikát SSL nainstalovaný na serveru, na kterém běží Server na. Klikněte na Procházet... tlačítko vedle pole certifikát SSL a vyberte nainstalovaný certifikát, který bude sloužit k zabezpečené připojení.
5. Klikněte na Přidat. tlačítko.
6. V dialogovém okně Přidat klienta LDAP zadejte IP adresu spotřebiče, server nebo aplikace, která bude ověřování serveru a název aplikace (volitelné). Název aplikace se zobrazí v sestavách Azure Vícefaktorové ověřování a v ověřování zprávy SMS nebo mobilní aplikaci se může zobrazovat.
7. Zaškrtnutí políčka vyžadovat Azure Vícefaktorové ověřování uživatele POZVYHLEDAT případně všichni uživatelé se budou importovány k serveru a vyměřené poplatky za jeho vícenásobného faktory ověřování. Pokud velký počet uživatelů nebyly dosud importují do serveru a/nebo bude osvobození od daně z vícenásobného faktory ověřování, nechejte pole zrušené zaškrtnutí políčka. Zobrazit soubor nápovědy pro další informace o této funkci.
8. Může opakujte kroky 5 až 7 k přidání dalších LDAP klientů.
9. Při Vícefaktorové ověřování Azure nakonfigurovaný pro příjem LDAP ověřování, třeba proxy těchto ověřování adresář LDAP. Proto cílové kartu pouze zobrazení v jednom šedá možnost použijete cíl LDAP. Konfigurace připojení adresář LDAP, klikněte na ikonu integrace adresářů.
10. Na kartě Nastavení vyberte použít konkrétní LDAP konfigurace přepínací tlačítko.
11. Klikněte na Upravit. tlačítko.
12. V dialogovém okně Upravit konfiguraci LDAP zadání hodnot do polí informace potřebné pro připojení k adresář LDAP. Popisy polí najdete v následující tabulce. Poznámka: Tyto informace se taky součástí souboru nápovědy Azure Multi-Factor ověřování serveru.![Integrace služby Directory](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)
13. Test připojení LDAP kliknutím na tlačítko Testovat.
14. Pokud LDAP test připojení byl úspěšný, klikněte na tlačítko OK.
15. Klikněte na kartu filtry. Server je předkonfigurovaná a načíst kontejnerů, skupiny zabezpečení a uživatele ze služby Active Directory. Pokud vazbu různých adresář LDAP, bude pravděpodobně nutné upravit filtry zobrazené. Klikněte na odkaz Nápověda Další informace o filtry.
16. Klikněte na kartu atributy. Server je předkonfigurovaná a mapovat atributy ze služby Active Directory.
17. Pokud vazbu různých adresář LDAP nebo změnit mapování předkonfigurovaná atributů, klikněte na Upravit. tlačítko.
18. V dialogovém okně upravit atributy změňte mapování LDAP atributů adresáře. Názvy atributů můžete zadané v nebo vybrané kliknutím … tlačítko vedle každého pole.
19. Klikněte na odkaz Nápověda Další informace o atributy.
20. Klikněte na tlačítko OK.
21. Klikněte na ikonu nastavení společnosti a vyberte kartu vyřešení uživatelské jméno.
22. pokoušíte připojit ke službě Active Directory ze serveru doméně, je třeba mohli opustit použití Windows zabezpečení identifikátory shody uživatelská jména přepínač Vybraná. Jinak vyberte atribut použití LDAP jedinečný identifikátor pro odpovídající uživatelská jména přepínací tlačítko. -Li vybrán, pokusí Server Azure Multi-Factor Authentication překládal jedinečný identifikátor adresář LDAP každý uživatelské jméno. Vyhledávání LDAP proběhne na uživatelské jméno atributy podle integrace adresářů -> atributy. Když uživatel se ověří, bude jedinečný identifikátor adresář LDAP přeložit uživatelské jméno a jedinečný identifikátor se použijí u odpovídajících uživatele v datovém souboru Azure Vícefaktorové ověřování. Tato funkce umožňuje porovnávání, uživatelské jméno a co dlouhé a krátké formáty to dokončení konfigurace Azure Multi-Factor ověřování serveru. Server na nakonfigurovanou porty teď naslouchají žádostí o přístup LDAP nakonfigurovanou klientů a nastavení proxy server tyto požadavky na adresář LDAP pro ověřování.


## <a name="ldap-client-configuration"></a>Konfigurace klienta LDAP

Abyste mohli nakonfigurovat klienta LDAP, řiďte se pokyny:

- Konfigurace serveru, spotřebiče nebo aplikaci ověřovat pomocí LDAP server Azure Multi-Factor Authentication, jako kdyby to byl adresář LDAP. Byste měli použít stejné nastavení, které běžně používáte připojit přímo do vaší adresář LDAP, s výjimkou název serveru nebo IP adresu, která bude Server Azure Multi-Factor Authentication.
- Časový limit LDAP 30 60 sekund nakonfigurujte tak, že je potřeba ověřit přihlašovací údaje uživatele s adresář LDAP, ověřování druhé faktory, přijímání svou odpověď a potom odpověď na žádosti o přístup LDAP.
- Pokud používáte LDAPS, spotřebiče nebo serveru dotazů LDAP důvěřovat certifikát SSL nainstalovat na Server Azure Multi-Factor Authentication.
