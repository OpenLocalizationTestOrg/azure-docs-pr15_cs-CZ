<properties 
    pageTitle="Začínáme s Server Azure Multi-Factor Authentication"
    description="Tohle je stránka Azure Multi-Factor ověřování, který popisuje, jak začít s Azure MFA serveru."
    services="multi-factor-authentication"
    keywords="ověřování serveru, azure faktor ověřování aplikace aktivace vícestránkové, ověřování serveru stáhnout"
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

# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Začínáme s Server Azure Multi-Factor Authentication




<center>![Cloud](./media/multi-factor-authentication-get-started-server/server2.png)</center>

Teď můžeme usoudili, zda použít místní vícefaktorové ověřování, nastavíme přejít. Tato stránka popisuje novou instalaci serveru a získání instalace se místní službou Active Directory. Pokud už máte PhoneFactor serveru nainstalovány a pokud chcete upgradovat, najdete v článku [Přechod na Server Azure Multi-Factor](multi-factor-authentication-get-started-server-upgrade.md) nebo pokud hledáte informace o instalaci jenom webová služba najdete v článku [nasazení webová služba Azure Multi-Factor ověřování serveru mobilní aplikace](multi-factor-authentication-get-started-server-webservice.md).


## <a name="download-the-azure-multi-factor-authentication-server"></a>Stažení Server Azure Vícefaktorové ověřování



Server Azure Multi-Factor Authentication můžete si stáhnout dvěma různými způsoby. Jak se dá prostřednictvím portálu Azure. První je Správa poskytovatele Auth Multi-Factor přímo. Druhý spočívá ve využití nastavení služby. Druhou možnost vyžaduje poskytovatele Auth Multi-Factor nebo licenci Azure MFA, Azure AD Premium nebo Suite mobilita organizace.


### <a name="to-download-the-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Pokud si chcete stáhnout server Azure Vícefaktorové ověřování z portálu Microsoft Azure
--------------------------------------------------------------------------------

1. Přihlaste se k portálu Azure jako správce.
2. V levé části vyberte služby Active Directory.
3. Na stránce služby Active Directory klikněte v horní **Multi-Factor Auth poskytovatelů**
4. V dolní části klikněte na **Spravovat**
5. Tím se otevře novou stránku.  Klikněte na **stáhne.** 
 ![Stáhnout](./media/multi-factor-authentication-sdk/download.png)
6. Nad **Generovat aktivace přihlašovací údaje**, klikněte na **Stáhnout.** 
 ![Stáhnout](./media/multi-factor-authentication-get-started-server/download4.png)
7. Uložte soubor.



### <a name="to-download-the-azure-multi-factor-authentication-server-via-the-service-settings"></a>Pokud si chcete stáhnout Azure Multi-Factor ověřování serveru pomocí nastavení služeb


1. Přihlaste se k portálu Azure jako správce.
2. V levé části vyberte služby Active Directory.
3. Dvojitým kliknutím na instanci aplikace Azure AD.
4. Nahoře klikněte na **Konfigurovat**
![stáhnout](./media/multi-factor-authentication-sdk/download2.png)
5. V části vícefaktorové ověřování vyberte **Spravovat nastavení služeb**
6. Na stránce nastavení služby v dolní části obrazovky klikněte na **Přejít na portál**.
![Ke stažení](./media/multi-factor-authentication-get-started-server/servicesettings.png)
7. Tím se otevře novou stránku. Klikněte na **stáhne.**
8. Nad **Generovat aktivace přihlašovací údaje**, klikněte na **Stáhnout.**
9. Uložte soubor.




## <a name="install-and-configure-the-azure-multi-factor-authentication-server"></a>Nainstalujte a nakonfigurujte Server Azure Vícefaktorové ověřování
Teď jste stáhli serveru můžete nainstalovat a ji nakonfigurovat.  Ujistěte se, že server, ke kterému instalujete na splňuje následující požadavky:



Požadavky na Server Azure Vícefaktorové ověřování|Popis|
:------------- | :------------- |
Hardwarová|<li>200 MB místa na disku</li><li>procesor může x32 nebo x64</li><li>1 GB nebo větší RAM</li>
Software|<li>Windows Server 2008 nebo vyšší, je-li hostiteli server OS</li><li>Windows 7 nebo vyšší, pokud hostitele je klient OS</li><li>4.0 rozhraní Microsoft .NET Framework</li><li>IIS 7.0 nebo novější, instalaci portál nebo webové službě uživatele SDK</li>

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Požadavky na Azure brány firewall Multi-Factor ověřování serveru
--------------------------------------------------------------------------------
Každý MFA serveru musí být komunikovat na port 443 odchozí následujícím způsobem:

- https://PFD.phonefactor.NET
- https://pfd2.phonefactor.NET
- https://CSS.phonefactor.NET

Pokud jsou odchozí bránách firewall omezení na port 443, následující rozsahy IP adres potřebovat pro otevření:

IP adres podsítí|Maska|Rozsah IP
:------------- | :------------- | :------------- |
134.170.116.0/25|255.255.255.128|134.170.116.1 – 134.170.116.126
134.170.165.0/25|255.255.255.128|134.170.165.1 – 134.170.165.126
70.37.154.128/25|255.255.255.128|70.37.154.129 – 70.37.154.254

Pokud nepoužíváte Azure Multi-Factor Authentication události potvrzení funkcí a uživatelé nejsou ověřování mobilních aplikacích Multi-Factor Auth ze zařízení v podnikové síti IP oblasti lze snížit následujícím způsobem:


IP adres podsítí|Maska|Rozsah IP
:------------- | :------------- | :------------- |
134.170.116.72/29|255.255.255.248|134.170.116.72 – 134.170.116.79
134.170.165.72/29|255.255.255.248|134.170.165.72 – 134.170.165.79
70.37.154.200/29|255.255.255.248|70.37.154.201 – 70.37.154.206


### <a name="to-install-and-configure-the-azure-multi-factor-authentication-server"></a>Instalace a konfigurace server Azure Vícefaktorové ověřování
--------------------------------------------------------------------------------


1. Poklikejte na spustitelný soubor. Tím se spustí instalace.
2. V dialogovém okně Vybrat složku instalace zkontrolujte správnost složku a klikněte na tlačítko Další.
3. Po dokončení instalace klikněte na Dokončit.  To, spustí se Průvodce konfigurací.
4. Úvodní obrazovka na Průvodce konfigurací, zaškrtněte **Přeskočit pomocí Průvodce konfigurací ověřování** a klikněte na tlačítko **Další**.  Tím zavřete průvodce a spusťte server.
    ![Cloud](./media/multi-factor-authentication-get-started-server/skip2.png)
5. Zpět na stránce, kterou jsme stáhnout serveru klikněte na tlačítko **Generovat pověření aktivace** .  Zkopírujte tyto informace do MFA Server Azure do příslušných polí a klikněte na **Aktivovat**.


Výše uvedené kroky zobrazit Expresní nastavení pomocí Průvodce konfigurací.  Spusťte Průvodce ověřování znovu tak, že ho vyberete v nabídce Nástroje na serveru.



##<a name="import-users-from-active-directory"></a>Import uživatelů ze služby Active Directory

Teď, když server nainstalovali a nakonfigurovali můžete rychle importujete uživatele do Azure MFA Server.

### <a name="to-import-users-from-active-directory"></a>Import uživatelů ze služby Active Directory
--------------------------------------------------------------------------------


1. MFA na Azure serveru nalevo vyberte **Uživatelé**.
2. Dole vyberte možnost **Importovat ze služby Active Directory**.
3. Teď můžete vyhledávat jednotlivé uživatele nebo Hledat v adresáři AD organizačních jednotek s uživateli v nich.  V tomto případě jsme bude určit uživatele, OU.
4. Všichni uživatelé na pravé straně zvýrazněte a klikněte na **importovat**.  Zobrazí automaticky otevírané okno oznámením, že byl úspěšný.  Zavřete okno Importovat.

![Cloud](./media/multi-factor-authentication-get-started-server/import2.png)

## <a name="send-users-an-email"></a>Uživatelům poslat e-mailu
Teď, když uživatelé jste importovali do server Azure Vícefaktorové ověřování, doporučuje poslat uživatelům e-mailu s upozorněním, že máte byla zaregistrované v vícefaktorové ověřování.

S Server Azure Multi-Factor Authentication existují různé způsoby pro nastavení uživatelů pro použití vícefaktorové ověřování.  Například pokud znáte telefonní čísla uživatelů nebo bylo možné telefonní čísla naimportujte Server Azure Multi-Factor Authentication z jejich podnikový adresář, e-mailu informování uživatelů, aby byly nakonfigurovali pomocí Azure Vícefaktorové ověřování, některé pokyny k používání Azure Vícefaktorové ověřování a informujte uživatele o telefonní číslo, které tato osoba obdrží jejich ověřování.  

Obsah e-mailu se budou lišit v závislosti na metodě ověřování, který byl nastaven pro uživatele (například telefonní hovor, služby SMS, mobilní aplikaci).  Například pokud uživatel je potřeba k použití kódu PIN při ověřování, e-mailu se dozvíte, je co byl nastaven počáteční PŘIPNOUT do.  Uživatelé, kteří jsou obvykle musí změnit jejich PIN během jejich první ověření.

Pokud telefonní čísla uživatelů dosud nakonfigurovali nebo importované do Server Azure Multi-Factor Authentication nebo uživatelé můžou předkonfigurovaná a používání mobilní aplikace pro ověřování, který jim pošlete e-mailu, který umožňuje ho informovali, že byly nakonfigurovány používat Azure Vícefaktorové ověřování a jejich dokončení zápisu jejich účtu uživatele portálu Azure Multi-Factor Authentication přesměruje.  Hypertextový odkaz se budou, že uživatel klikne na pro přístup k portálu uživatele. Po kliknutí na hypertextový odkaz, webovém prohlížeči otevřete a zakázku jejich společnosti Azure Multi-Factor Authentication uživatele portál.   


### <a name="configuring-email-and-email-templates"></a>Konfigurace e-mailu a e-mailové šablony

Kliknutím na ikonu e-mail na levé straně můžete nastavit nastavení pro odeslání tyto e-mailů.  Toto je místo, kam můžete zadat informace o SMTP e-mailového serveru a umožňuje vám posílají poštu hromadné celou e-mailu přidáním kontrola odeslat uživatelé zaškrtněte políčko.

![Nastavení e-mailu](./media/multi-factor-authentication-get-started-server/email1.png)

Na kartě obsahu e-mailu zobrazí se všechny různé e-mailové šablony, které jsou dostupné na výběr.  Takže podle toho, jak jste nakonfigurovali uživatelé můžou používat vícefaktorové ověřování, můžete zvolit šablonu, které nejlépe odpovídá můžete.

![E-mailové šablony](./media/multi-factor-authentication-get-started-server/email2.png)

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Jak Server Azure Multi-Factor Authentication zpracovává uživatelská data

Při použití serveru Multi-Factor Authentication (MFA) místní uživatele data se ukládají v místní servery. Žádná trvalý uživatelská data uložený v cloudu. Když uživatel provede dvojúrovňové ověřování, serveru MFA odešle data do cloudové služby Azure MFA provádět ověřování. Pokud tyto požadavky ověřování jsou odeslány cloudové služby, tato pole jsou odesílány žádosti a protokoly zájmu zajištění jsou dostupné v sestavách ověřování/použití zákazníka. Některá pole jsou volitelné, dá se povolit nebo zakázat v rámci Multi-Factor ověřování serveru. Komunikace ze serveru MFA cloudové služby MFA používá SSL/TLS přes port 443 odchozí. Tato pole jsou:

- Jedinečné ID – uživatelské jméno nebo interním serveru ID MFA
- První a poslední jméno – volitelné
- E-mailovou adresu – volitelné
- Telefonní číslo - při provádění hlasový hovor nebo ověřování služby SMS
- Zařízení token - při ověřování mobilní aplikaci
- Režim ověřování
- Výsledek ověřování
- Název serveru MFA
- MFA Server IP
- Klient IP – Pokud je k dispozici



Kromě předchozích polí ověřování výsledek (úspěch/odmítnutí) a důvod, proč se všechny zamítnutí je uložená s daty ověřování a dostupné prostřednictvím ověřování a použití sestavy.


## <a name="advanced-azure-multi-factor-authentication-server-configurations"></a>Rozšířené možnosti konfigurace serveru Azure Vícefaktorové ověřování
Další informace o rozšířené nastavení a informace o konfiguraci použijte tuto tabulku.

Metoda|Popis
:------------- | :------------- |
[Portál uživatele](multi-factor-authentication-get-started-portal.md)|  Informace o nastavení a konfiguraci portálu uživatele včetně nasazení a uživatel samoobslužných funkcí.
[Služba Active Directory Federation](multi-factor-authentication-get-started-adfs.md)|Informace o nastavení Azure Vícefaktorové ověřování se službou AD FS.
[Ověřování](multi-factor-authentication-get-started-server-radius.md)|  Informace o nastavení a konfiguraci Azure MFA Server s POLOMĚREM.
[Ověření Internetové informační služby](multi-factor-authentication-get-started-server-iis.md)|Informace o nastavení a konfiguraci Azure MFA Server pomocí služby IIS.
[Ověřování systému Windows](multi-factor-authentication-get-started-server-windows.md)|  Informace o nastavení a konfigurace Azure MFA Server pomocí ověřování Windows.
[Ověření protokolu LDAP](multi-factor-authentication-get-started-server-ldap.md)|Informace o nastavení a konfiguraci Azure MFA Server s ověřováním LDAP.
[Vzdálené plochy brány a Azure Multi-Factor ověřování serveru pomocí RADIUS](multi-factor-authentication-get-started-server-rdg.md)|  Informace o nastavení a konfigurace Azure MFA Server pomocí vzdálené plochy brány pomocí RADIUS.
[Synchronizace se službou Active Directory pro Windows Server](multi-factor-authentication-get-started-server-dirint.md)|Informace o nastavení a konfigurace synchronizace služby Active Directory a Azure MFA Server.
[Nasazení služby Azure Vícefaktorové ověřování serveru mobilní webové aplikace](multi-factor-authentication-get-started-server-webservice.md)|Informace o nastavení a konfiguraci webová služba Azure MFA serveru.
