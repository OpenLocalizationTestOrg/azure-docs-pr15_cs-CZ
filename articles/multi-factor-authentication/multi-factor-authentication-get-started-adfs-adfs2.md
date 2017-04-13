<properties
    pageTitle="Server Azure MFA pomocí služby AD FS 2.0 | Microsoft Azure"
    description="Tohle je stránka Azure Multi-Factor ověřování, který popisuje, jak začít s Azure MFA a AD FS 2.0."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

# <a name="secure-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-20"></a>Zabezpečené cloudové a místních zdrojů prostřednictvím Azure Multi-Factor ověřování serveru se službou AD FS 2.0

Tento článek se v organizacích, které federovaní Azure Active Directory a chcete zajistit prostředky, které jsou v místním i v cloudu. Ochrana zdrojů pomocí serveru Azure Multi-Factor Authentication a konfigurace bude platit se službou AD FS tak, aby dvoustupňového ověření se spustí pro vysokou hodnotu koncové body.

Tato dokumentace popisuje Server Azure Multi-Factor Authentication pomocí služby AD FS 2.0.  Získáte další informace o [zabezpečení cloudu a místní zdrojů prostřednictvím Azure Multi-Factor ověřování serveru v systému Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md).


## <a name="secure-ad-fs-20-with-a-proxy"></a>Zabezpečené s proxy AD FS 2.0
Zajistit AD FS 2.0 s proxy Server Azure Multi-Factor Authentication nainstalovat proxy server služby AD FS a konfigurace serveru.

### <a name="configure-iis-authentication"></a>Konfigurace ověřování služby IIS

1. V rámci Azure Multi-Factor ověřování serveru klikněte na ikonu **Ověřování služby IIS** v nabídce nalevo.
2. Klikněte na kartu **Založená na formulářích** .
3. Klikněte **Přidat** tlačítko.
<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/setup1.png)</center>
4. Automaticky rozpoznávat uživatelské jméno, heslo a domény proměnných, zadejte přihlašovací adresa URL (třeba https://sso.contoso.com/adfs/ls) v dialogovém okně Auto-Configure Form-Based webu a klikněte na OK.
5. Zaškrtněte políčko **vyžadovat Azure Vícefaktorové ověřování uživatele odpovídají** případně všichni uživatelé se budou importovány k serveru a vyměřené poplatky za jeho dvoustupňového ověření. Pokud velký počet uživatelů nebyly dosud importují do serveru a/nebo bude osvobození od daně z dvoustupňového ověření, nechejte pole zrušené zaškrtnutí políčka. Další informace o této funkci najdete v tématu souboru nápovědy.
6. Pokud proměnné stránky nelze automaticky zjištěny, klepněte **Zadat ručně...** tlačítko v dialogovém okně Auto-Configure Form-Based webu.
7. V dialogovém okně Add Form-Based webu zadejte adresu URL na přihlašovací stránku služby AD FS v poli zadejte adresu URL (třeba https://sso.contoso.com/adfs/ls) a zadejte název aplikace (volitelné). Název aplikace se zobrazí v sestavách Azure Vícefaktorové ověřování a v ověřování zprávy SMS nebo mobilní aplikaci se může zobrazovat. Na adrese URL odeslat najdete v článku nápovědy pro další informace.
8. Nastavení žádosti o formát "ZVEŘEJŇUJÍ nebo získejte."
9. Zadejte uživatelské jméno proměnných (ctl00$ ContentPlaceHolder1$ UsernameTextBox) a heslo proměnnou (ctl00$ ContentPlaceHolder1$ PasswordTextBox). Pokud se založená na formulářích přihlašovací stránku domain textové pole, zadejte proměnnou domény. Budete muset přejděte na přihlašovací stránku ve webovém prohlížeči, klikněte pravým tlačítkem myši na stránce a vyberte **Zobrazit zdroj** zobrazíte jména vstupních polí v na přihlašovací stránku.
10. Zaškrtněte políčko **vyžadovat Azure Vícefaktorové ověřování uživatele shodují** případně všichni uživatelé se budou importovány k serveru a vyměřené poplatky za jeho dvoustupňového ověření. Pokud velký počet uživatelů nebyly dosud importované do serveru a/nebo bude osvobození od daně z dvoustupňového ověření, nechejte pole zrušené zaškrtnutí políčka.
<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/manual.png)</center>
11. Klikněte na **Upřesnit...** s cílem zobrazit upřesňující nastavení. Můžete konfigurovat nastavení, včetně možnosti vyberte souboru vlastní odmítnutí stránky do mezipaměti úspěšných ověření na webu pomocí soubory cookie a vyberte, jak ověřit primární přihlašovací údaje.
12. Protože proxy server služby AD FS není může být připojen k doménu, můžete se připojit k řadiče domény pro import uživatele a předběžné ověření LDAP. V dialogovém okně Advanced Form-Based webu klikněte na kartu **Primární ověřování** a vyberte jako typ před ověřování **Vazby LDAP** .
13. Až budete hotovi, klikněte na tlačítko **OK** se vraťte do dialogového okna Add Form-Based Web. Soubor nápovědy pro další informace najdete na stránce Upřesnit nastavení.
14. Klikněte na tlačítko **OK** zavřete dialogová okna.
15. Jakmile se proměnné adresy URL a stránky byly nerozpoznal nebo zadali, data webu zobrazí v panelu založená na formulářích.
16. Klikněte na kartu **Nativní modul** a vyberte server, na web, který proxy server služby AD FS webem v části (třeba "Výchozí") nebo aplikaci proxy server služby AD FS (třeba "ls" v části "služby AD FS") povolit modul plug-in službu IIS na požadovanou úroveň.
17. Zaškrtněte políčko **Povolit IIS ověřování** v horní části obrazovky.
18. Teď je zapnuté ověřování serveru IIS.

### <a name="configure-directory-integration"></a>Nakonfigurovat integraci adresářů

Povolit ověřování služby IIS, ale k provedení předběžné ověření do svého Active Directory (AD) prostřednictvím protokolu LDAP musíte nakonfigurovat připojení LDAP řadiče domény.

1. Klikněte na ikonu **Integrace adresářů** .
2. Na kartě Nastavení vyberte přepínací tlačítko **použít konkrétní LDAP konfiguraci** .
<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap1.png)</center>
3. Klikněte **Upravit** tlačítko.
4. V dialogovém okně Upravit konfiguraci LDAP zadání hodnot do polí informace potřebné pro připojení k řadiče domény AD. Popisy polí najdete v následující tabulce. Tyto informace se taky součástí souboru nápovědy Azure Multi-Factor ověřování serveru.
5. Test připojení LDAP kliknutím na tlačítko **Testovat** .
<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/ldap2.png)</center>
6. Pokud LDAP test připojení byl úspěšný, klikněte na tlačítko **OK** .

### <a name="configure-company-settings"></a>Konfigurace nastavení společnosti

1. Potom klikněte na ikonu **Nastavení společnosti** a vyberte kartu **Rozlišení uživatelské jméno** .
2. Vyberte **atribut jedinečný identifikátor použití LDAP pro odpovídající uživatelská jména** přepínací tlačítko.
3. Pokud uživatele zadejte své uživatelské jméno ve formátu "doména\uživatelské jméno", serveru musí mít možnost pro odstranění domény vypnout uživatelské jméno při vytváření dotazu LDAP. Které lze provést pomocí nastavení registru ignorovat.
4. Otevřete editor registru a přejděte k HKEY_LOCAL_MACHINE/SOFTWARE/Wow6432Node/pozitiv sítích/PhoneFactor na serveru 64bitová verze. Pokud na serveru 32bitová verze trvat "Wow6432Node" mimo cestu. Vytvoření klíče registru DWORD s názvem "UsernameCxz_stripPrefixDomain" a nastavte hodnotu na 1. Azure Vícefaktorové ověřování teď zabezpečení proxy server služby AD FS.

Ujistěte se, že uživatelé importu ze služby Active Directory k serveru. Pokud byste chtěli povolených vnitřní IP adresy tak, aby dvoustupňového ověření nepožaduje při přihlášení k webu z těchto míst, najdete v [části důvěryhodné IP adresy](#trusted-ips) .

<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/reg.png)</center>

## <a name="ad-fs-20-direct-without-a-proxy"></a>Služba AD FS 2.0 přímé bez proxy serveru

Je možné zabezpečit AD FS, když není používá proxy AD FS. Instalace Server Azure Multi-Factor Authentication na server služby AD FS a konfigurace serveru podle následujících kroků:

1. V rámci Azure Multi-Factor ověřování serveru klikněte na ikonu **Ověřování služby IIS** v nabídce nalevo.
2. Klikněte na kartu **HTTP** .
3. Klikněte **Přidat** tlačítko.
4. V dialogové okno Přidat základní adresa URL zadejte adresu URL webu služby AD FS, kde HTTP ověřování (například https://sso.domain.com/adfs/ls/auth/integrated) do pole Adresa URL základu. Zadejte název aplikace (volitelné). Název aplikace se zobrazí v sestavách Azure Vícefaktorové ověřování a v ověřování zprávy SMS nebo mobilní aplikaci se může zobrazovat.
5. V případě potřeby upravte nečinnosti a maximální doby trvání relace.
6. Zaškrtněte políčko **vyžadovat Azure Vícefaktorové ověřování uživatele shodují** případně všichni uživatelé se budou importovány k serveru a vyměřené poplatky za jeho dvoustupňového ověření. Pokud velký počet uživatelů nebyly dosud importované do serveru a/nebo bude osvobození od daně z dvoustupňového ověření, nechejte pole zrušené zaškrtnutí políčka. Další informace o této funkci najdete v tématu souboru nápovědy.
7. Pokud budete chtít, zaškrtněte políčko mezipaměti souborů cookie.
<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/noproxy.png)</center>
8. Klikněte na tlačítko **OK** .
9. Klikněte na kartu **Nativní modul** a vyberte server, na webu (třeba "výchozí web") nebo služby AD FS aplikaci (třeba "ls" v části "služby AD FS") povolit modul plug-in služby IIS na požadovanou úroveň.
10. Zaškrtněte políčko **Povolit IIS ověřování** v horní části obrazovky. Azure Vícefaktorové ověřování teď zabezpečení služby AD FS.

Ujistěte se, že uživatelé importu ze služby Active Directory k serveru. Pokud byste chtěli povolených vnitřní IP adresy tak, aby dvoustupňového ověření nepožaduje při přihlášení k webu z těchto míst, naleznete v části důvěryhodné IP adresy.


## <a name="trusted-ips"></a>Důvěryhodné IP adresy
Důvěryhodné IP adresy umožňují uživatelům obejít Azure Vícefaktorové ověřování pro web požadavky pocházející z konkrétní IP adresy nebo podsítí. Například můžete uživatelům osvobození od daně z dvoustupňového ověření při přihlášení z kanceláře. V takovém případě zadáte podsítě office jako položku Důvěryhodné IP adresy.

### <a name="to-configure-trusted-ips"></a>Konfigurace důvěryhodných IP adresy


1. V části ověřování služby IIS klikněte na kartu **Důvěryhodné IP adresy** .
1. Klikněte **Přidat** tlačítko.
1. Až se zobrazí dialogové okno Přidat důvěryhodné IP adresy, vyberte jeden z přepínačů **Jediný IP**, **rozsah IP**nebo **podsítě** .
1. Zadejte IP adresu, rozsah IP adresy nebo podsítě, která má být povolené. Pokud zadávání podsítě, vyberte příslušnou masky a klikněte na tlačítko **OK** . Důvěryhodné IP teď přidala.


<center>![Nastavení](./media/multi-factor-authentication-get-started-adfs-adfs2/trusted.png)</center>
