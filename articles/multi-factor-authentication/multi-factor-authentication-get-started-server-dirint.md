<properties 
    pageTitle="Integrace adresářů mezi Azure Vícefaktorové ověřování a služby Active Directory"
    description="Tohle je stránka Azure Multi-Factor ověřování, který popisuje, jak integrovat Server Azure Multi-Factor Authentication službou Active Directory, takže synchronizace adresářů."
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

# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Integrace adresářů mezi Azure MFA serveru a služby Active Directory

V části Integrace adresářů umožňuje konfigurace serveru a integrace se službou Active Directory nebo jiné adresář LDAP.  Umožňuje konfigurace atributy, které odpovídají schéma adresáře a nastavit automatické synchronizaci uživatelů.

## <a name="settings"></a>Nastavení
Ve výchozím nastavení je Azure Multi-Factor Authentication Server nakonfigurovaný k importovat a synchronizovat uživatele ze služby Active Directory.  Na kartě umožňuje změnit výchozí chování a přiřadit různé adresář LDAP, ADAM adresáře nebo konkrétní řadiče domény Active Directory.  Je také používat LDAP ověřování proxy LDAP nebo vazby LDAP jako cíle RADIUS, před ověřování pro ověřování služby IIS nebo primární ověřování pro uživatele portálu.  Následující tabulka popisuje jednotlivé nastavení.

![Nastavení](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Funkce | Popis |
| ------- | ----------- |
| Použití služby Active Directory | Vyberte možnost použít Active Directory pro účely import a synchronizace služby Active Directory.  Toto je ve výchozím nastavení. <br>Poznámka: Musí být počítač připojen k doméně a musí být přihlášení pod svým účtem domény pro integrace služby Active Directory správně fungovat. |
| Zahrnout důvěryhodná | Zaškrtněte políčko zahrnout důvěryhodná mít agent pokus o připojení k doménám důvěryhodný aktuální doménu, jiné doméně ve struktuře nebo domény účastní vztahu důvěryhodnosti.  Při importu ani synchronizace uživatelů z libovolných pracovních důvěryhodná, zrušte zaškrtnutí zaškrtávacího políčka, aby se zvýšil výkon.  Výchozí hodnota je zaškrtnuté políčko. |
| Použití konkrétní konfigurace LDAP | Vyberte možnost použít LDAP používat LDAP nastavené pro import a synchronizace. Poznámka: Při použití LDAP uživatelského rozhraní změní odkazy ze služby Active Directory LDAP. |
| Obrázek tlačítka | Tlačítka pro úpravu umožňuje aktuální nastavení konfigurace LDAP upravit. |
| Použití dotazů rozsahem atribut | Označuje, zda má být použita dotazů rozsahem atribut.  Dotazy rozsahem atribut povolte efektivně adresářích kvalifikování záznamy na základě položek v atributu jiný záznam.  Server Azure Multi-Factor Authentication používá dotazů rozsahem atribut pro efektivní dotazování na uživatele, které jsou členem skupiny zabezpečení.   <br>Poznámka: Existují některé případy, kdy dotazů rozsahem atribut jsou podporované, ale není určená k použití.  Pokud skupiny zabezpečení obsahuje členů z víc než jednu doménu, například Active Directory mohou být problémy s dotazy rozsahem atribut.  V tomto případě by měl být zaškrtnuté políčko. |

Následující tabulka popisuje nastavení konfigurace LDAP.

| Funkce | Popis |
| ------- | ----------- |
| Server | Zadejte název hostitele nebo IP adresu serveru adresář LDAP.  Zálohování serveru může být zadán taky oddělené středníkem. <br>Poznámka: SSL po vazba typu plně kvalifikovaný název hostitele požaduje obecně. |
| Základní název domény | Rozlišující název objektu základní adresáře, ze které se spustí všechny dotazy adresáře.  Například datacentrum = abc, datacentrum = cz. |
| Vazba typu - dotazů | Vyberte typ odpovídající vazbu pro použití při vytváření vazby hledání v adresáři LDAP.  Používá se pro importy, synchronizace a řešení uživatelské jméno. <br><br>  Anonymní - proběhne anonymní vazbu.  Nepoužije vazbu DN a vytvoření vazby heslo.  To funkční pouze v případě adresář LDAP umožňuje anonymní vazby a oprávnění umožňují dotazu na odpovídající záznamy a atributy.  <br><br> Jednoduché – vazbu DN a vytvoření vazby heslo bude předané jako prostý text se vytvořit vazbu s adresář LDAP.  To by měl použít pouze pro účely testování ověřit zastižení na server a účet vazbu obsahuje odpovídající přístup.  Doporučujeme, aby SSL být použita po instalaci odpovídající certifikátu.  <br><br> K šifrování SSL - vazbu DN a vytvoření vazby heslo bude protokolem SSL svázat adresář LDAP.  Při této akci musí místně nainstalované certifikátu, že adresář LDAP považuje za důvěryhodnou.  <br><br> Windows – vazbu uživatelské jméno a heslo svázat se použijí k zabezpečené připojení k ADAM adresáře nebo řadiče domény Active Directory.  Svázat uživatelské jméno je prázdné, použijí se přihlášený účet uživatele se vytvořit vazbu. |
| Vazba typu - ověřování | Vyberte typ odpovídající vazbu pro použití při provádění ověření vazeb LDAP.  V tématu popisy ve skupinovém rámečku Typ vazby - dotazů typu vazbu.  Například to umožňuje anonymní vazbu má být použita pro dotazy během SSL vazbu se používá k zabezpečení LDAP vazbu ověřování. |
| Vazba DN nebo uživatelské jméno vazbu | Zadejte rozlišující název záznam uživatele pro účet, který chcete použít při vytváření vazby adresář LDAP.<br><br>Vazba rozlišující název, který se používá pouze po vytvoření vazby typu jednoduché nebo SSL.  <br><br>Zadejte uživatelské jméno účtu systému Windows používat, když vazba adresář LDAP po svázat typ systému Windows.  Pokud necháte prázdné, přihlášený účet uživatele se použijí k vázat. |
| Svázat heslo | Zadejte heslo vazbu svázat DN nebo použitá k vytvoření vazby adresář LDAP uživatelské jméno.  Abyste mohli nakonfigurovat heslo pro službu Multi-Factor ověřování serveru AdSync, musí být povolený synchronizace a služba musí běžet v místním počítači.  Heslo uloží se v systému Windows ukládají uživatelská jména a hesla v části účet, který běží služba Multi-Factor ověřování serveru AdSync jako.  Hesla se rovněž uloženo v části účet, který uživatelského rozhraní Multi-Factor ověřování serveru běží jako a potom v části účet, který běží služba Multi-Factor ověřování serveru jako.  <br><br> Poznámka: Protože heslo jsou uloženy pouze místního serveru Windows ukládají uživatelská jména a hesla, tento krok bude potřeba udělat na všech serverech Auth Multi-Factor, který potřebuje přístup k heslo. |
| Omezení velikosti dotazu | Určete omezení velikosti platí pro maximální počet uživatelů, které vrátí hledání adresář.  Toto omezení by měly odpovídat konfigurace na adresář LDAP.  Velké hledání, kde není podporováno stránkování import a synchronizace se pokusí načtete uživatele v dávkách.  Omezení velikosti zadáno, tady je větší než limit nakonfigurovaný na adresář LDAP, někteří uživatelé unikly. |
| Tlačítko Test | Klikněte na tlačítko Testovat testování vazby k serveru LDAP.  <br><br> Poznámka: Možnost použití LDAP nemusí být vybrány k otestování vazby.  Díky vazby otestuje před pomocí konfigurace LDAP. |

## <a name="filters"></a>Filtry
Filtry umožňují nastavit kritéria pro záznamy při hledání adresář.  Pomocí nastavení filtru můžete omezit objekty, které chcete synchronizovat.  

![Filtry](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure Vícefaktorové ověřování má následující možnosti 3.

- **Kontejner filtr** – zadat kritéria filtru slouží k získání kontejneru záznamy při hledání adresář.  Služby Active Directory a ADAM (| () objectClass=organizationalUnit)(objectClass=container)) se obecně používá.  Pro ostatních adresářů LDAP kritéria filtru, které nesplňuje každý typ objektu kontejneru se má používat v závislosti na schéma adresáře.  <br>Poznámka: Pokud prázdné, ((objectClass=organizationalUnit)(objectClass=container)) se použije ve výchozím nastavení.

- **Filtr skupiny zabezpečení** – slouží k měli nárok záznamy skupiny zabezpečení při hledání adresáře kritéria filtru.  Služby Active Directory a ADAM (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) se obvykle používá.  Pro ostatních adresářů LDAP kritéria filtru, které nesplňuje každého typu objektu skupiny zabezpečení se má používat v závislosti na schéma adresáře.  <br>Poznámka: Pokud prázdné, (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) se použije ve výchozím nastavení.

- **Filtr uživatelů** – zadat kritéria filtru použít pro získání záznamy uživatelů při hledání adresář.  Služby Active Directory a ADAM, (a (objectClass=user)(objectCategory=person)) se obecně používá.  U ostatních adresářů LDAP (objectClass = inetOrgPerson) nebo něco podobného bude použito v závislosti na schéma adresáře. <br>Poznámka: Pokud prázdné, (a (objectCategory=person)(objectClass=user)) se použije ve výchozím nastavení.

## <a name="attributes"></a>Atributy
Atributy lze přizpůsobit podle potřeby u určitého adresáře.  Umožňuje přidat vlastní atributy a graf doladit synchronizace pouze atributy, které potřebujete.  Hodnota pro každé pole atributu musí být název atributu způsobem definovaným ve schématu adresář.  Použití další informace v následující tabulce.

![Atributy](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Funkce | Popis |
| ------- | ----------- |
| Jedinečný identifikátor | Zadejte název atributu atribut, který slouží jako jedinečný identifikátor kontejneru, skupiny zabezpečení a uživatelských záznamů.  Ve službě Active Directory je to obvykle objectGUID.  V ostatních LDAP implementací může být entryUUID nebo podobným.  Výchozí hodnota je objectGUID. |
| ... Tlačítka (vyberte atribut) | Jednotlivá pole atribut je trojtečkou (...) tlačítko vedle této položky, který se zobrazí dialogové okno Vybrat atribut povolení atribut vybrat ze seznamu. <br><br>Vyberte atribut.<br><br>Poznámka: Atributy lze zadat ručně a nemusí podle atributu v seznamu atributů. |
| Jedinečný identifikátor typ | Vyberte požadovaný typ atributu jedinečný identifikátor.  Ve službě Active Directory je atribut objectGUID typu GUID.  V ostatních LDAP implementací může být typu pole bajtů ASCII nebo řetězec.  Výchozí hodnota je GUID. <br><br>Poznámka: Je třeba nastavit správně od položky synchronizace se odkazuje podle jejich jedinečný identifikátor a jedinečný identifikátor URI typu slouží k přímo najít objekt v adresáři.  Toto nastavení řetězec při skutečně uchovává hodnotu jako pole bajtů znaků ASCII zabrání synchronizace z správně funguje. |
| Rozlišující název | Zadejte název atributu atribut, který obsahuje rozlišující název pro každý záznam.  Ve službě Active Directory je to obvykle distinguishedName.  V ostatních LDAP implementací může být entryDN nebo podobným.  Výchozí hodnota je distinguishedName. <br><br>Poznámka: Pokud atribut obsahující pouze rozlišující název, který neexistuje, atribut adspath lze.  "LDAP: / /<server>/" část cesty bude automaticky odstranit vypnutí ponechte jenom rozlišující název objektu. |
| Název kontejneru | Zadejte název atributu atribut, který obsahuje jméno container záznamu.  Hodnota atributu zobrazí se v hierarchie Container při importu ze služby Active Directory nebo přidání synchronizace položek.  Výchozí hodnota je název. <br><br>Poznámka: Pokud různých kontejnerů používat různé atributy pro jejich jména, více atributů jméno container může být zadán v oddělených středníky.  První název atributu kontejneru na objekt kontejneru použijí zobrazíte její název. |
| Název skupiny zabezpečení | Zadejte název atributu atribut, který obsahuje název v záznamu skupiny zabezpečení.  Hodnota atributu zobrazí se v seznamu skupiny zabezpečení při importu ze služby Active Directory nebo přidání synchronizace položek.  Výchozí hodnota je název. |
| Uživatelé | Následující atributy se používají pro hledání, zobrazení, import a synchronizace informace o uživateli z adresáře. |
| Uživatelské jméno | Zadejte název atributu atribut, který obsahuje uživatelské jméno v záznamu uživatele.  Hodnota atributu se použije jako uživatelské jméno Multi-Factor ověřování serveru.  Druhá atribut může být zadán jako zálohu první.  Atribut druhý bude použít pouze v případě prvního atributu neobsahuje hodnota pro uživatele.  Výchozí nastavení se userPrincipalName sAMAccountName. |
| Křestní jméno | Zadejte název atributu atribut, který obsahuje jméno uživatele záznamu.  Výchozí hodnota je givenName (jméno). |
| Příjmení | Zadejte název atributu atribut, který obsahuje příjmení uživatelský záznam.  Výchozí hodnota je okně. |
| E-mailovou adresu | Zadejte název atributu atribut, který obsahuje e-mailovou adresu v záznamu uživatele.  E-mailovou adresu se použijí k odeslání úvodní a k aktualizaci e-mailů pro uživatele.  Výchozí hodnota je pošty. |
| Skupiny uživatelů | Zadejte název atributu atribut, který obsahuje skupiny uživatelů v záznamu uživatele.  Skupiny uživatelů můžete použít k filtrování uživatelů ve službě agent a sestav v portálu pro správu Multi-Factor ověřování serveru. |
| Popis | Zadejte název atributu atribut, který obsahuje popis v záznamu uživatele.  Popis se používá pouze pro vyhledávání.  Výchozí hodnota je popis. |
| Hlasový hovor jazyka | Zadejte název atributu atribut, který obsahuje krátký název jazyk, který chcete použít pro hlasové volání pro uživatele. |
| Jazyk textu služby SMS | Zadejte název atributu atribut, který obsahuje krátký název jazyk, který chcete použít pro textové zprávy SMS pro uživatele. |
| Jazyk aplikace telefonu | Zadejte název atributu atribut, který obsahuje krátký název jazyk, který chcete používat pro telefon aplikace textové zprávy pro uživatele. |
| MÍSTOPŘÍSEŽNÉM tokenu jazyka | Zadejte název atributu atribut, který obsahuje krátký název jazyk, který chcete použít pro MÍSTOPŘÍSEŽNÉM tokenu textové zprávy pro uživatele. |
| Telefony | Následující atributy slouží k importu nebo synchronizovat telefonních čísel uživatelů.  Pokud název atributu není zadán pro typ telefonu, typ telefonu není možné při importu ze služby Active Directory nebo přidání synchronizace položek. |
| Business | Zadejte název atributu atribut, který obsahuje telefonní číslo v záznamu uživatele.  Výchozí hodnota je telephoneNumber. |
| Domů | Zadejte název atributu atribut, který obsahuje výchozí telefonní číslo v záznamu uživatele.  Výchozí hodnota je telefon domů. |
| Operátor | Zadejte název atributu atribut, který obsahuje číslo operátoru uživatelský záznam.  Výchozí hodnota je operátor. |
| Mobilní telefon | Zadejte název atributu atribut, který obsahuje číslo mobilního telefonu v záznamu uživatele.  Výchozí hodnota je mobilní. |
| Fax | Zadejte název atributu atribut, který obsahuje faxové číslo v záznamu uživatele.  Výchozí hodnota je facsimileTelephoneNumber. |
| IP telefonu | Zadejte název atributu atribut, který obsahuje číslo IP telefonu v záznamu uživatele.  Výchozí hodnota je ipPhone. |
| Vlastní | Zadejte název atributu atribut, který obsahuje vlastní telefonní číslo v |
|  | záznam uživatele.  Výchozí hodnota je prázdná. |
| Rozšíření | Zadejte název atributu atribut, který obsahuje Linka telefonního čísla v záznamu uživatele.  Hodnota pole rozšíření se použije jako koncovku na primární telefonní číslo.  Výchozí hodnota je prázdná. <br><br>Poznámka: Pokud není zadán atribut příponu, rozšíření může být součástí atribut telefonu.  Rozšíření by měl předcházet spolu se symbolem x, můžete analyzovat.  Například 555-123-4567 x890 byste měli mít za následek 555-123-4567 jako telefonní číslo a 890 jako rozšíření. |
| Obnovit výchozí tlačítko | Kliknutím na tlačítko Obnovit výchozí vrátíte všechny atributy zpátky na výchozí hodnota.  Výchozí hodnoty by měl s normálním schématu služby Active Directory a ADAM fungovat správně. |

Chcete-li upravit atributy, stačí klikněte na tlačítko Upravit na kartě atributy.  Tím zobrazíte s windows, které umožňuje upravit vlastnosti.

![Úprava atributy](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>Synchronizace
Synchronizace zachová databázi uživatelů Azure Multi-Factor synchronizovat s uživateli ve službě Active Directory nebo jiný adresář Lightweight Directory Access Protocol (LDAP) Lightweight Directory Access Protocol.  Proces je podobný Ruční import uživatelů ze služby Active Directory, ale pravidelně zjišťuje uživatele služby Active Directory a změny skupiny zabezpečení zpracovat.  Poskytuje také zakázání nebo odebrání uživatelů odebrali kontejner nebo skupiny zabezpečení a odebrání uživatelů ze služby Active Directory Odstraněná.

Služba Multi-Factor Auth ADSync je služba Windows, která provádí pravidelných hlasování služby Active Directory.  Toto je nechcete být zaměňovány s Azure AD Sync nebo Azure AD Connect.  Auth ADSync Multi-Factor, i když založená na podobně jako základ kódu je specifických pro službu Azure Multi-Factor Authentication.  Je nainstalovaná ve stavu Zastaveno a spuštěná služba Multi-Factor Auth Server při konfiguraci spustit.  Pokud máte konfigurace serveru Auth Multi-Factor více serveru, může Auth ADSync Multi-Factor spustit pouze v jednom serveru.

Rozšíření serveru DirSync LDAP poskytované společností Microsoft využívá služba Multi-Factor Auth ADSync pro efektivní hlasování změny.  Tento nástroj DirSync ovládací prvek volající může mít "adresáři získat změny" doprava a DS replikace Get změny rozšířeného přístupu ovládacího prvku doprava.  Ve výchozím nastavení těchto oprávnění přiřazené k účtům Administrators a LocalSystem na řadiče domény.  Služba Multi-Factor Auth AdSync nastaven spustit LocalSystem ve výchozím nastavení.  Proto je nejjednodušší ke spuštění služby u řadiče domény.  Služba fungovat jako účet s oprávněními pro menší pokud ji chcete-li vždy provést úplné synchronizaci nakonfigurovat.  To je méně efektivní, ale vyžaduje menší účet oprávnění.

Pokud nakonfigurovaný na používání LDAP a adresář LDAP podporuje ovládací prvek DirSync, pak dotazování pro uživatele a změny skupiny zabezpečení budou funguje stejně, stejně jako se službou Active Directory.  Pokud adresář LDAP nepodporuje ovládací prvek DirSync, bude při každém cyklu provést úplné synchronizaci.

![Synchronizace](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

Další informace o všech jednotlivá nastavení na kartě synchronizace použijte v následující tabulce.

| Funkce | Popis |
| ------- | ----------- |
| Povolení synchronizace se službou Active Directory | Když zaškrtnete toto políčko, budou pravidelně dotazování služby Active Directory pro změny spuštěná služba Multi-Factor ověřování serveru. <br><br>Poznámka: musí být přidán nejméně jednu položku synchronizace a synchronizovat teď je nutné před serveru Auth Multi-Factor služby začnou zpracování změny. |
| Synchronizace všech | Zadejte časový interval služba Multi-Factor Auth počká mezi dotazování a zpracování změny. <br><br> Poznámka: Zadaný interval je údaj o čase mezi datem začátku každé obrázku.  Pokud čas zpracování změny překročí interval, hlasování se zase spustí službu. |
| Odebrání uživatelů už není ve službě Active Directory | Při kontrole služba Multi-Factor Auth zpracovat neplatné uživatelské služby Active Directory odstranit a odeberte související uživatele Multi-Factor ověřování serveru. |
| Vždy provést úplné synchronizaci | Když zaškrtnete toto políčko, služba Multi-Factor Auth vždy provést úplné synchronizaci.  Při zaškrtnuté, služba Multi-Factor Auth provede Přírůstková synchronizace pomocí dotazu jenom uživatelé, které se změnily.  Výchozí hodnota je zrušené zaškrtnutí políčka. <br><br> Poznámka: Pokud zaškrtnuté, synchronizaci lze provést pouze při adresáři podporuje ovládací prvek DirSync a použitý se vytvořit vazbu s adresářem účet má příslušná oprávnění k provádění DirSync přírůstková dotazů.  Pokud tento účet nemá potřebná oprávnění nebo víc domén se účastní synchronizace, provádět úplné synchronizaci se doporučuje. |
| Vyžadovat schválení správce při víc než X uživatelé budou zakázané nebo odebrat | Zakázání nebo odebrání uživatelů, kteří jsou už členem kontejneru položku nebo skupinu zabezpečení je možné konfigurovat položky synchronizace.  Jako ochranu může být správce schválení požadované při prahovou hodnotu větší než počet uživatelů, zakázání nebo odebrání.  Když zaškrtnete toto políčko, budou schválení potřebné pro dané prahové hodnoty.  Výchozí hodnota je 5 a rozsah hodnot je od 1 do 999. <br><br> Schválení usnadnit odesláním první e-mailové oznámení pro správce. E-mailového oznámení vám bude radit pokyny pro revize a schválení zakázáním a odebrání uživatelů.  Když spustíte uživatelského rozhraní Multi-Factor ověřování serveru vás vyzve ke schválení. |

Tlačítko **Synchronizovat teď** můžete spustit úplné synchronizaci pro vybrané položky synchronizace.  Úplné synchronizaci je potřeba při každém synchronizace položky jsou přidali, upravit, odebrat nebo se změněným pořadím.  Je taky potřeba před službu Multi-Factor Auth AdSync bude provozní, protože nastaví výchozí bod, ze které bude službu hlasování přírůstková změny.  Pokud jste provedli změny u položek synchronizace a úplné synchronizaci nebyla provedena, budete vyzváni k Sesynchronizovat při přechodu na jiný oddíl nebo při zavření uživatelského rozhraní.

Tlačítko **Odebrat** umožňuje správcům ze seznamu Multi-Factor ověřování serveru synchronizace položky odstranit jeden nebo více položek synchronizace.

>[AZURE.WARNING]Po odebrání záznam položky synchronizace jej nebude možné obnovit. Musíte se znova přidejte záznam položky synchronizace omylem odstranili.

Synchronizace vybrané položky synchronizace mohly být odebrány z Multi-Factor ověřování serveru.  Služba Multi-Factor Auth už zpracuje položky synchronizace.

Tlačítka nahoru a dolů povolit správce chcete změnit pořadí položek synchronizace.  Pořadí je důležité, protože stejný uživatel může být členem skupiny víc než jednu položku synchronizace (jako je kontejner a skupiny zabezpečení).  Nastavení pro uživatele při synchronizaci se pocházet z první položky synchronizace v seznamu, ke kterému je uživatel přidružen.  Proto by měl být synchronizace položky vloženy v pořadí priorit.

>[AZURE.TIP]Úplné synchronizaci se provedou po odstranění položky synchronizace.  Úplné synchronizaci se provedou po řazení položek synchronizace.  Klikněte na tlačítko Synchronizovat teď provádět úplné synchronizaci.

## <a name="multi-factor-auth-servers"></a>Servery Multi-Factor Auth
Další servery Auth Multi-Factor může nastavit tak, aby sloužit jako záložní proxy RADIUS LDAP proxy, nebo pro ověřování služby IIS. Konfigurace synchronizace bude sdílet všechny agentů. Pouze jednu z těchto agentů však může být službu Multi-Factor ověřování serveru. Tato karta umožňuje vybrat Auth Server Multi-Factor, kterou chcete používat pro synchronizaci.

![Služba Multi-Factor Auth serverů](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)
