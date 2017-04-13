<properties 
    pageTitle="Vícefaktorové ověřování serveru IIS ověřování a Azure"
    description="Tohle je stránka Azure Multi-Factor ověřování, které vám pomohou při nasazení serveru IIS ověřování a Azure Multi-Factor ověřování serveru."
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

# <a name="iis-authentication"></a>Ověření Internetové informační služby

V části ověřování služby IIS Server Azure Multi-Factor Authentication umožňuje povolení a konfigurace ověřování služby IIS pro integraci s webovými aplikacemi Microsoft IIS. Server Azure Multi-Factor Authentication nainstaluje modul plug-in, která můžete filtrovat žádostí k webovému serveru IIS přidat Azure Vícefaktorové ověřování. Modul plug-in IIS podporuje ověřování pomocí formulářů a integrované HTTP ověřování systému Windows. Důvěryhodné že IP adresy můžete konfigurovat taky k osvobození od daně interní IP adres z dvojúrovňové ověřování.


![Ověření Internetové informační služby](./media/multi-factor-authentication-get-started-server-iis/iis.png)


## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Pomocí ověřování serveru IIS založená na formulářích s Azure Vícefaktorové ověřování serveru

Zabezpečení služby IIS webové aplikace, která používá ověřování pomocí formulářů, nainstalujte Server Azure Multi-Factor Authentication na webovém serveru IIS a konfigurace serveru podle následujícího postupu.

1. V rámci Server Azure Multi-Factor Authentication klikněte na ikonu ověřování služby IIS v nabídce nalevo.
2. Klikněte na kartu založená na formulářích.
3. Klikněte na Přidat. tlačítko.
4. Zjišťování uživatelské jméno, heslo a doménu proměnné automaticky, zadejte přihlašovací adresa URL (například https://localhost/contoso/auth/login.aspx) v dialogovém okně Auto-Configure Form-Based webu a klikněte na OK.
5. Zaškrtnutí políčka vyžadovat Vícefaktorové ověřování uživatele POZVYHLEDAT případně všichni uživatelé se budou importovány k serveru a vyměřené poplatky za jeho vícefaktorové ověřování. Pokud velký počet uživatelů nebyly dosud importují do serveru a/nebo bude osvobození od daně z vícefaktorové ověřování, nechejte pole zrušené zaškrtnutí políčka.
6. Pokud proměnné stránky nelze automaticky rozpoznat, které můžete klikněte zadat ručně tlačítko v dialogovém okně Auto-Configure Form-Based webu.
7. V dialogovém okně Add Form-Based webu zadejte adresu URL do pole Adresa URL zadejte na přihlašovací stránku a zadejte název aplikace (volitelné). Název aplikace se zobrazí v sestavách Azure Vícefaktorové ověřování a v ověřování zprávy SMS nebo mobilní aplikaci se může zobrazovat. Na adrese URL odeslat najdete v článku nápovědy pro další informace.
8. Vyberte ve správném formátu žádost. Tato možnost "Příspěvku nebo pro získání" u většiny webových aplikací.
9. Zadejte proměnné uživatelské jméno, heslo proměnných a domény proměnnou (Pokud se zobrazí na přihlašovací stránce). Budete muset přejděte na přihlašovací stránku ve webovém prohlížeči, klikněte pravým tlačítkem myši na stránce a vyberte "Zobrazení zdroje" zobrazíte jména vstupních polí v rámci stránky.
10. Zaškrtnutí políčka vyžadovat Azure Vícefaktorové ověřování uživatele POZVYHLEDAT případně všichni uživatelé se budou importovány k serveru a vyměřené poplatky za jeho vícefaktorové ověřování. Pokud velký počet uživatelů nebyly dosud importované do serveru a/nebo bude osvobození od daně z vícefaktorové ověřování, nechejte pole zrušené zaškrtnutí políčka. Najdete v článku soubor nápovědy pro další informace o této funkci.
11.  Klikněte Upřesnit. s cílem zobrazit upřesňující nastavení, včetně možnosti vyberte souboru vlastní odmítnutí stránky do mezipaměti úspěšných ověření na web dobu používání souborů cookie a vyberte, zda chcete ověřit primární pověření proti doménu systému Windows, adresář LDAP nebo serveru RADIUS. Po dokončení klikněte na tlačítko OK se vraťte do dialogového okna Add Form-Based Web. Soubor nápovědy pro další informace najdete na stránce Upřesnit nastavení.
12. Klikněte na tlačítko OK.
13. Po proměnné adresy URL a stránky byly nerozpoznal nebo zadali, data webu se zobrazí na panelu založená na formulářích.
14. V tématu Povolení služby IIS moduly plug-in pro Azure Multi-Factor Authentication Server přímo pod dokončete konfiguraci ověřování serveru IIS.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Pomocí integrovaného ověřování systému Windows Server Azure Vícefaktorové ověřování

Zajistit IIS webové aplikace, která používá ověřování integrované Windows HTTP instalace Server Azure Multi-Factor Authentication na webovém serveru IIS a konfigurace serveru podle následujícího postupu.

1. V rámci Server Azure Multi-Factor Authentication klikněte na ikonu ověřování služby IIS v nabídce nalevo.
2. Klikněte na kartu HTTP. Klikněte na kartu založená na formulářích.
3. Klikněte na Přidat. tlačítko.
4. V dialogové okno Přidat základní adresa URL zadejte adresu URL webu, kde je ověřování HTTP provedených (například http://localhost/owa) do pole Adresa URL Base a zadejte název aplikace (volitelné). Název aplikace se zobrazí v sestavách Azure Vícefaktorové ověřování a v ověřování zprávy SMS nebo mobilní aplikaci se může zobrazovat.
5. Upravte nečinnosti a Maximum doby trvání relace pokud nestačí výchozí.
6. Zaškrtnutí políčka vyžadovat Vícefaktorové ověřování uživatele POZVYHLEDAT případně všichni uživatelé se budou importovány k serveru a vyměřené poplatky za jeho vícefaktorové ověřování. Pokud velký počet uživatelů nebyly dosud importované do serveru a/nebo bude osvobození od daně z vícefaktorové ověřování, nechejte pole zrušené zaškrtnutí políčka. Zobrazit soubor nápovědy pro další informace o této funkci.
7. Pokud budete chtít, zaškrtněte políčko mezipaměti souborů cookie.
8. Klikněte na tlačítko OK.
9. Naleznete v části [Povolit IIS moduly plug-in pro Server Azure Multi-Factor Authentication](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server) přímo pod dokončete konfiguraci ověřování serveru IIS.


## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>Povolení služby IIS moduly plug-in pro Server Azure Vícefaktorové ověřování

Po konfiguraci formuláře založené na nebo adresy URL HTTP ověřování a nastavení, třeba vyberte místo, kde mají načíst a povolené moduly plug-in Azure Multi-Factor Authentication IIS ve službě IIS. Pomocí následujícího postupu:

1. Pokud ve službě IIS 6, klikněte na kartu ISAPI a vyberte web, na kterém běží webové aplikace v části (například výchozí web) povolit Azure Multi-Factor Authentication upgradovanými modul plug-in pro daný web.
2. Pokud ve službě IIS 7 nebo novější, klikněte na kartu nativní modul a vyberte server, webů aplikace povolit modul plug-in služby IIS na požadované úrovně.
3. Zaškrtněte políčko Povolit IIS ověřování v horní části obrazovky. Azure Vícefaktorové ověřování teď zabezpečení vybrané aplikace služby IIS. Ujistěte se, že uživatelé importu k serveru. Pokud byste chtěli povolených, vnitřní IP adresy, že dvojúrovňové ověřování nepožaduje při přihlašování k webu z těchto míst, naleznete v části důvěryhodné IP adresy dole.


## <a name="trusted-ips"></a>Důvěryhodné IP adresy

IP adresy důvěryhodní uživatelé obejít Azure Vícefaktorové ověřování pro web požadavky pocházející z konkrétní IP adresy nebo podsítí. Například můžete uživatelům osvobození od daně z Azure Vícefaktorové ověřování při přihlášení z kanceláře. V takovém případě zadáte podsítě office jako položku Důvěryhodné IP adresy. Konfigurace důvěryhodných IP adresy následujícím způsobem:

1. V části ověřování služby IIS klikněte na kartu Důvěryhodné IP adresy.
2. Klikněte na Přidat. tlačítko.
3. Po zobrazení dialogového okna Přidat důvěryhodné IP adresy vyberte jednoho IP, rozsah IP adres podsítí přepínací tlačítko.
4. středový IP adresu rozsah IP adres nebo podsítě, která má být povolené. Pokud zadávání podsítě, vyberte příslušnou masky a klikněte na tlačítko OK. Povolených teď přidala.
