<properties
    pageTitle="Azure Vícefaktorové ověřování – další krok"
    description="Tohle je stránka ověřování Multi-Factor Azure, který popisuje, co dělat další s MFA.  Platí to i sestav, podvodům upozornění, jednorázové přemostění, vlastní hlasových zpráv, ukládání do mezipaměti, důvěryhodných IP adresy a aplikace hesla."
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
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="kgremban"/>

# <a name="configuring-azure-multi-factor-authentication"></a>Konfigurace Azure Vícefaktorové ověřování

Tento článek pomáhá spravovat Azure Vícefaktorové ověřování teď, když jste do začátků.  Popisuje různé témata, která vám pomohou při využívejte Azure Vícefaktorové ověřování.  Všechny tyto funkce jsou dostupné ve všech verzích Azure Vícefaktorové ověřování.

Konfigurace pro některé z následujících funkcí naleznete v portálu pro správu Azure Multi-Factor Authentication. Existují dva různé způsoby, které máte přístup k portálu Správa MFA, které se dělají prostřednictvím portálu Azure. První je správa zprostředkovatele Auth Multi-Factor používáte-li na základě spotřeba MFA. Druhý spočívá ve využití nastavení služby MFA. Druhou možnost vyžaduje poskytovatele Auth Multi-Factor nebo licenci Azure MFA, Azure AD Premium nebo Suite mobilita organizace.

Přístup k portálu Správa MFA prostřednictvím poskytovatele Azure Multi-Factor Auth, přihlaste se k portálu Azure jako správce a vyberte možnost služby Active Directory. Klikněte na kartu **Multi-Factor Auth poskytovatelů** a pak vyberte adresáře a klikněte na tlačítko **Spravovat** v dolní.

Přístup k portálu Správa MFA pomocí stránky nastavení služby MFA, přihlaste se k portálu Azure jako správce a vyberte možnost služby Active Directory. Klikněte na adresář a potom klikněte na kartu **Konfigurovat** . V části vícefaktorové ověřování vyberte **Spravovat nastavení služby**. V dolní části na stránce nastavení služby MFA klikněte na odkaz **přejděte na portál** .


Funkce| Popis| Co je součástí
:------------- | :------------- | :------------- |
[Podvod upozornění](#fraud-alert)|Podvod upozornění můžete nakonfigurované a nastavit tak, aby uživatelé mohli vykazovat podvodné pokusy o přístup k jejich zdroje.|Jak nastavit, konfigurace a vykazování podvodu
[Jednorázové přemostění](#one-time-bypass) |Jednorázové přemostění umožňuje uživateli ověření jednou vynecháním"" vícefaktorové ověřování.|Jak se dají vytvořit a nakonfigurovat jednorázové přemostění
[Vlastní hlasové zprávy](#custom-voice-messages) |Vlastní hlasových zpráv umožňují použít vlastní záznamy ve formátu nebo pozdravy s vícefaktorové ověřování. |Jak se dají vytvořit a nakonfigurovat vlastní pozdravy a zprávy
[Ukládání do mezipaměti](#caching-in-azure-multi-factor-authentication)|Ukládání do mezipaměti umožňuje nastavit určitého časového období tak, že pokusy následné ověření úspěšné automaticky. |Jak se dají vytvořit a nakonfigurovat mezipaměť ověřování.
[Důvěryhodné IP adresy](#trusted-ips)|Důvěryhodné že IP adresy je funkce vícefaktorové ověřování, který umožňuje správcům spravované nebo federované klienta možnost obejít vícefaktorové ověřování pro uživatele, kteří jsou přihlášení z místní intranet společnosti.|Konfigurace a nastavení IP adresy, které jsou osvobození od daně pro vícefaktorové ověřování
[Aplikace hesla](#app-passwords)|Zadání hesla aplikace umožňuje aplikaci, která není MFA podporující obejít vícefaktorové ověřování a pokračovat v práci.|Informace o heslech aplikace.
[Mějte na paměti Vícefaktorové ověřování pro nalezenou zařízení a prohlížeče](#remember-multi-factor-authentication-for-devices-users-trust)|Umožňuje nezapomeňte zařízení pro sadu počet dní, když uživatel úspěšně proběhlo pomocí MFA.|Informace o povolení této funkce a nastavte počet dní.
[Lze vybrat ověření metody](#selectable-verification-methods)|Můžete zvolit, které jsou dostupné pro uživatele, jak pomocí metody ověřování.|Informace o povolení nebo zakázání specifických metod ověřování například hovor nebo textovou zprávu.



## <a name="fraud-alert"></a>Podvod upozornění
Podvod upozornění můžete nakonfigurované a nastavit tak, aby uživatelé mohli vykazovat podvodné pokusy o přístup k jejich zdroje.  Uživatele můžete vykázat jejich podvod pomocí mobilní aplikace nebo přes telefon.

### <a name="to-set-up-and-configure-fraud-alert"></a>Nastavení a konfigurace podvod upozornění

1.  Přihlaste se k http://azure.microsoft.com
2.  Přejděte na portál správy MFA podle pokynů v horní části této stránky.
3.  V Azure Multi-Factor Authentication portálu pro správu klikněte na nastavení v části konfigurovat.
4.  V části podvod upozornění na stránce Nastavení zaškrtněte umožnit uživatelům odesílat upozornění podvod zaškrtávací políčko.
5.  Pokud chcete uživatelům budou blokovány při hlášení podvodu, zaškrtněte v bloku uživatele při hlášení podvodu.
6.  **Kód pro sestavy podvodům se dá během počáteční s pozdravem** do textového pole zadejte číslo kód, který lze použít při volání ověření. Pokud uživatel zadá tento kód plus # místo jenom znaménko #, bude vykázaného podvod upozornění.
7.  V dolní části klikněte na Uložit.

>[AZURE.NOTE]
>Pozdravů hlasové výchozí společnosti Microsoft sdělte uživatelům stiskněte 0# odesílat upozornění na podvodu. Pokud chcete použít kód než 0, by měl zaznamenat a uložit vlastní pozdravů hlasové vlastní příslušné pokyny.


![Cloud](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="to-report-fraud-alert"></a>Do sestavy podvod upozornění
Podvod upozornění můžete vykázat dvěma způsoby.  Buď přes mobilní aplikaci nebo pomocí telefonu.  

### <a name="to-report-fraud-alert-with-the-mobile-app"></a>Do sestavy podvod upozornění pomocí mobilní aplikace



1. Odeslání ověření s telefonem, vyberte ho a spusťte aplikaci Microsoft Authenticator.
2. Vykazování podvodu, klikněte na Storno a podvod sestavy. Tím se vyvolá pole oznamující, že budete upozorněni zaměstnanců IT podpory vaší organizace.
3. Klikněte na sestavu podvodu.
4. V aplikaci klikněte na Zavřít.

![Cloud](./media/multi-factor-authentication-whats-next/report1.png)


![Cloud](./media/multi-factor-authentication-whats-next/fraud2.png)

### <a name="to-report-fraud-alert-with-the-phone"></a>Do sestavy podvod upozornění s telefonem

1. Po ověření příchodu volání na váš telefon, odpovězte na ni.  
2. Sestava podvodu, zadejte kód, který má nakonfigurovaný tak, aby odpovídala s sestav podvod přes telefon a pak znak #. Zobrazí se oznámení odeslání podvod upozornění.
3. Ukončete hovor.

### <a name="to-view-the-fraud-report"></a>Chcete-li zobrazit sestavy podvodu

1. Přihlaste se k [http://azure.microsoft.com](https://azure.microsoft.com/)
2. V levé části vyberte služby Active Directory.
3. Nahoře vyberte Multi-Factor Auth poskytovatelů. Tím se vyvolá seznam poskytovatelů Auth Multi-Factor.
4. Pokud máte víc než jeden Multi-Factor Auth zprostředkovatele, vyberte tu, kterou chcete zobrazit zprávu upozornění podvodům a klikněte na spravovat v dolní části stránky. Pokud máte jenom jednu, klikněte na spravovat. Tím se otevře Azure Multi-Factor Authentication portálu pro správu.
5. Na Azure Multi-Factor Authentication portálu pro správu, na levé straně klikněte v části zobrazit sestavu klikněte na podvod upozornění.
6. Určete rozsah kalendářních dat, kterou chcete zobrazit v sestavě. Můžete taky určit žádné konkrétní uživatelská jména, telefonní čísla a jeho stav.
7. Klikněte na spustit. Tím se vyvolá zprávu podobnou té dole. Pokud chcete export sestavy můžete taky klikněte na Exportovat do souboru CSV.

## <a name="one-time-bypass"></a>Jednorázové přemostění

Jednorázové přemostění umožňuje uživateli ověření jednou vynecháním"" vícefaktorové ověřování. Přemostění je dočasný a vyprší po určitý počet sekund.  Abyste v situacích, kdy mobilní aplikace nebo telefon nechodí oznámení nebo telefonní hovor, můžete povolit jednorázové obejít tak, aby uživatel přístup požadované zdroje.

### <a name="to-create-a-one-time-bypass"></a>Vytvoření jednorázové přemostění

1.  Přihlaste se k http://azure.microsoft.com
2.  Přejděte na portál správy MFA podle pokynů v horní části této stránky.
3.  V Azure Multi-Factor Authentication portálu pro správu, pokud se zobrazí název svého poskytovatele MFA Azure nebo klientovi na levé straně s + vedle ní, klikněte + najdete v článku různých skupin replikace MFA serveru a ve skupině Azure výchozí. Klikněte na příslušné skupiny.
4.  V části Správa uživatelů klikněte na **Nepoužívat jednorázová**.
![Cloud](./media/multi-factor-authentication-whats-next/create1.png)
5.  Na stránce jednorázová přemostění klikněte na **Nový jednorázová přemostění**.
6.  Zadejte uživatelské jméno uživatele, počet sekund, nacházející se přemostění bude, důvod přemostění a klikněte na **obejít**.
![Cloud](./media/multi-factor-authentication-whats-next/create2.png)
7.  V tomto okamžiku uživatel musíte se přihlásit před vypršením platnosti jednorázové přemostění.



### <a name="to-view-the-one-time-bypass-report"></a>Chcete-li zobrazit sestavy jednorázové přemostění

1. Přihlaste se k [http://azure.microsoft.com](https://azure.microsoft.com/)
2. V levé části vyberte služby Active Directory.
3. Nahoře vyberte Multi-Factor Auth poskytovatelů. Tím se vyvolá seznam poskytovatelů Auth Multi-Factor.
4. Pokud máte víc než jeden Multi-Factor Auth zprostředkovatele, vyberte tu, kterou chcete zobrazit zprávu upozornění podvodům a klikněte na spravovat v dolní části stránky. Pokud máte jenom jednu, klikněte na spravovat. Tím se otevře Azure Multi-Factor Authentication portálu pro správu.
5. Na Azure Multi-Factor Authentication portálu pro správu, na levé straně klikněte v části zobrazit sestavu klikněte na nepoužívat jednorázová.
6. Určete rozsah kalendářních dat, kterou chcete zobrazit v sestavě. Můžete taky určit žádné konkrétní uživatelská jména, telefonní čísla a jeho stav.
7. Klikněte na spustit. Tím se vyvolá zprávu podobnou té dole. Pokud chcete export sestavy můžete taky klikněte na Exportovat do souboru CSV.

<center>![Cloud](./media/multi-factor-authentication-whats-next/report.png)</center>

## <a name="custom-voice-messages"></a>Vlastní hlasové zprávy

Vlastní hlasových zpráv umožňují použít vlastní záznamy ve formátu nebo pozdravy s vícefaktorové ověřování.  Tyto mohou sloužit kromě nebo nahrazení Microsoft záznamy.

Než začnete mějte na paměti toto:

- Aktuální podporované formáty souborů se .wav MP3.
- Maximální velikost souboru je 5 MB.
- Doporučujeme, aby pro ověřování zprávy, které být delší než 20 sekund. Všechno, co větší než to může způsobit ověření se nezdaří, protože uživatel přestat reagovat před dokončí zprávu a ověření vyprší její časový limit.



### <a name="to-set-up-custom-voice-messages-in-azure-multi-factor-authentication"></a>Nastavit vlastní hlasových zpráv v Azure Vícefaktorové ověřování
1.  Vytvoření vlastní hlasovou zprávu pomocí jednoho z formátů souborů podporovaných.
2.  Přihlaste se k http://azure.microsoft.com
3.  Přejděte na portál správy MFA podle pokynů v horní části této stránky.
4.  V Azure Multi-Factor Authentication portálu pro správu klikněte v části konfigurovat hlasových zpráv.
5.  V části hlasové zprávy klikněte na **Nová zpráva hlasové**.
![Cloud](./media/multi-factor-authentication-whats-next/custom1.png)
6.  V konfigurovat: nové hlasové zprávy klepněte na tlačítko **Spravovat soubory zvuk**.
![Cloud](./media/multi-factor-authentication-whats-next/custom2.png)
7.  V konfigurovat: zvukové soubory stránky, klikněte na **Soubor nahrát zvuk**.
![Cloud](./media/multi-factor-authentication-whats-next/custom3.png)
8.  V konfigurovat: nahrajte zvukový soubor, klikněte na tlačítko **Procházet** a přejděte na hlasovou zprávu, klikněte na **Otevřít**.
![Cloud](./media/multi-factor-authentication-whats-next/custom4.png)
9.  Přidejte popis a klikněte na nahrát.
10. Jakmile se to dokončí, zobrazí se zpráva potvrzující mít úspěšně nahrát soubor.
11. Na levé straně klikněte na hlasové zprávy.
12. V části hlasové zprávy klikněte na nová zpráva hlasové.
13. V rozevíracím seznamu jazyk vyberte jazyk.
14. Pokud tato zpráva je v určité aplikaci, zadejte ho do pole aplikace.
15. V typ zprávy vyberte typ zprávy přepsat naše novou vlastní zprávou.
16. V rozevíracím seznamu zvukový soubor vyberte zvukový soubor.
17. Klikněte na **vytvořit**. Zobrazí se zpráva potvrzující, že jste vytvořili úspěšně hlasovou zprávu.
![Cloud](./media/multi-factor-authentication-whats-next/custom5.png)</center>



## <a name="caching-in-azure-multi-factor-authentication"></a>Ukládání do mezipaměti v Azure Vícefaktorové ověřování

Ukládání do mezipaměti umožňuje nastavit určitého časového období tak, že pokusy následné ověření úspěšné automaticky. To primárně používají při místní systémy třeba VPN odešlou více ověření probíhá první žádost dál. Díky tomu další žádosti o úspěšné automaticky po úspěšném ověření průběhu uživatele. Všimněte si, že ukládání do mezipaměti není určená k pro přihlášení k Azure AD.


### <a name="to-set-up-caching-in-azure-multi-factor-authentication"></a>Nastavit ukládání do mezipaměti v Azure Vícefaktorové ověřování

1.  Přihlaste se k http://azure.microsoft.com
2.  Přejděte na portál správy MFA podle pokynů v horní části této stránky.
3.  V Azure Multi-Factor Authentication portálu pro správu klikněte v části konfigurovat ukládání do mezipaměti.
4.  Na konfigurovat ukládání do mezipaměti stránky klikněte na novou mezipaměť
5.  Vyberte typ mezipaměti a sekund mezipaměti. Klikněte na vytvořit.

<center>![Cloud](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Důvěryhodné IP adresy

Důvěryhodné že IP adresy je funkce vícefaktorové ověřování, který umožňuje správcům spravované nebo federované klienta možnost obejít vícefaktorové ověřování pro uživatele, kteří jsou přihlášení z místní intranet společnosti. Funkce jsou dostupné pro Azure AD klienti, které mají Azure AD Premium, Enterprise mobilita Suite nebo Azure Vícefaktorové ověřování licencí.


Typ Azure AD klienta| Dostupné možnosti důvěryhodných adres IP
:------------- | :------------- |
Spravovat|Konkrétní rozsahy IP adres – správce můžete určit rozsah IP adres, které se může vyhnout vícefaktorové ověřování pro uživatele, kteří jsou přihlášení z firemní síti intranet.
Federované|<li>Všichni uživatelé federovaný – všechny federované uživatelům, kteří jsou přihlášení z uvnitř organizace se vyhne vícefaktorové ověřování pomocí deklaraci webová služba AD FS.</li><li>Konkrétní rozsahy IP adres – správce můžete určit rozsah IP adres, které se může vyhnout vícefaktorové ověřování pro uživatele, kteří jsou přihlášení z firemní síti intranet.

To obejít funguje jenom z uvnitř podnikové síti intranet. Tak například pokud vyberete všechny federovaní uživatelé a uživatel přihlásí z mimo firemní síti intranet, které má uživatel ověření pomocí vícefaktorové ověřování i v případě, že uživatel představuje deklaraci AD FS. Následující tabulka popisuje, kdy Multi-Factor authentication a aplikace hesla podporují ve vaší síti corpnet nebo mimo ni vaší síti corpnet při zapnuté funkci důvěryhodné IP adresy.


|Důvěryhodné povolené IP adresy| Důvěryhodné IP adresy zakázaný
:------------- | :------------- | :------------- |
Vnitřní síti corpnet|Pro prohlížeč toky vícefaktorové ověřování nejsou nutné.|Pro prohlížeč toky povinné vícefaktorové ověřování
|K aplikacím klienta RTF běžná hesla fungují, pokud uživatel nemá vytvořeného všechna aplikace hesla. Po vytvoření heslo k aplikaci jsou potřeba aplikaci hesla.|K aplikacím klienta RTF, požadování hesel aplikace
Externí síti corpnet|Pro prohlížeč toky vícefaktorové ověřování povinné.|Pro prohlížeč toky vícefaktorové ověřování povinné.
|Aplikace klienta RTF, požadování hesel aplikace.|Aplikace klienta RTF, požadování hesel aplikace.

### <a name="to-enable-trusted-ips"></a>Chcete-li povolit důvěryhodná IP adresy

1. Přihlaste se k portálu Azure classic.
2. Na levé straně klikněte na služby Active Directory.
3. V části adresář klikněte na adresář, který chcete nastavit důvěryhodné IPsing na.
4. Na adresář, který jste vybrali klikněte na konfigurovat.
5. V části vícefaktorové ověřování klikněte na nastavení služby Správa.
6. Na stránce nastavení služby pod důvěryhodné IP adresy na výběr tyhle možnosti:

    - Pro žádosti o federovaní uživatelé pocházející z mé intranet – všechny federované uživatelů, kteří jsou přihlášení z podnikové síti se vyhne vícefaktorové ověřování pomocí deklaraci webová služba AD FS.
    - Požadavky z určitého rozsahu veřejnou IP adresy – zadejte IP adresy v polích pomocí zápisu CIDR k dispozici. Příklad: xxx.xxx.xxx.0/24 IP adres v oblasti xxx.xxx.xxx.1 – xxx.xxx.xxx.254 nebo xxx.xxx.xxx.xxx/32 jedné IP adresy. Můžete zadat až 50 rozsahy IP adres.

7. Klikněte na Uložit.
8. Po použití aktualizací klikněte na Zavřít.



![Důvěryhodné IP adresy](./media/multi-factor-authentication-whats-next/trustedips3.png)




## <a name="app-passwords"></a>Aplikace hesla

V některých aplikacích, jako je třeba Office 2010 nebo starší a Apple Mail nelze použít vícefaktorové ověřování.  Použít tyto aplikace, musíte používat "aplikace hesla" místo tradiční heslo.  Heslo aplikace umožňuje aplikaci vyhnout vícefaktorové ověřování a pokračovat v práci.

>[AZURE.NOTE] Moderní ověřování pro klienty Office 2013
>
> Klienty Office 2013 (včetně aplikace Outlook) nyní podporuje nové ověřování protokoly a lze povolit pro podporu Vícefaktorové ověřování.  To znamená, že po povolení aplikace hesla nejsou potřebné pro práci s klienty Office 2013.  Další informace najdete Dívejte [Office 2013 moderní ověřování veřejné ohlásí](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).



### <a name="important-things-to-know-about-app-passwords"></a>Důležité vědět o aplikaci hesla

Následující obrázek je důležitý seznam věcí, které byste měli vědět o aplikaci hesla.

- Uživatelé mohou mít víc aplikace hesla, jejichž zvyšuje povrchu pro krádežím identity. Protože aplikace hesla těžké si zapamatovat, může podporovat lidí si to zapisovat. Tím se nedoporučuje a by se nedoporučuje, protože pouze jeden faktor se členství požaduje pro přihlášení pomocí aplikace hesla.
- Aplikace, které mezipaměti hesla a použít v místním scénáře začít když nefunguje, protože aplikace heslo Neznámý mimo id organizace. Příklad Exchange e-maily, které jsou místní ale archivované e-mailu v cloudu. Stejné heslo nefunguje.
- Skutečné heslo je automaticky generované a není zadán uživatelem. Jedná se vzhledem k tomu, že je automaticky generované heslo pro mohl uhádnout složitější a bezpečnější.
- Momentálně je maximálně 40 hesla uživatele. Zobrazí se výzva k odstranit jednoho z existující aplikace hesla k vytvoření nové.
- Po povolení vícefaktorové ověřování uživatelského účtu aplikace hesla se dá používat se většina prohlížení klientů, jako je Outlook i Lync, ale akci správy nelze provést heslem aplikace prostřednictvím prohlížení aplikací, jako je Windows PowerShell i v případě, že se má účet správce.  Zajištění vytvořte účet služby s silné heslo spuštění skriptů Powershellu a neumožňují tohoto účtu pro vícefaktorové ověřování.

>[AZURE.WARNING]  Aplikace hesla nefungují v hybridním prostředí místo, kam klienti komunikovat s obou místních i cloudových koncové body služby Automatická konfigurace. Je to proto hesel domény jsou potřebné k ověření místní a hesla aplikace jsou potřebné k ověření se v cloudu.


### <a name="naming-guidance-for-app-passwords"></a>Pojmenování pokyny pro aplikaci hesla
Doporučujeme, aby aplikace heslo názvy stromová struktura zařízení, na které se používají. Například pokud používáte přenosný počítač s podporou prohlížeče aplikace jako je Outlook, Word a Excel, stačí jedno heslo aplikace s názvem přenosných Kam zmizely toto heslo aplikace ve všech těchto aplikacích. Se dá sice vytvořit samostatné hesel pro všechny tyto aplikace, se nedoporučuje. Doporučená způsobem je pomocí jednoho heslo aplikací na zařízení.


<center>![Cloud](./media/multi-factor-authentication-whats-next/naming.png)</center>


### <a name="federated-sso-app-passwords"></a>Hesla aplikace federované (SSO)
Azure AD podporuje federaci s místním systému Windows Server Active Directory Domain Services (AD DS). Pokud vaše organizace používá federated(SSO) s Azure AD a budete používat Azure Vícefaktorové ověřování, bude následující důležité informace, že byste měli znát při používání aplikace hesel. To platí jenom pro zákazníky federated(SSO).

- Heslo aplikace ověřený Azure AD a tedy obchází federace. Federace pouze aktivně používá při nastavování aplikace heslo.
- Uživatelé federated(SSO) jsme nikdy najdete zprostředkovatele identit (IdP) na rozdíl od pasivní tok. Hesla jsou uložena do pole id organizace. Pokud uživatel opustí společnosti, má tyto informace na plovoucí dlaždice na organizační id pomocí DirSync v reálném čase. Zakázání nebo odstranění účtu může trvat až tři hodin synchronizovat, zpoždění zakázání nebo odstranění aplikace hesla v Azure AD.
- Místní nastavení řízení přístupu klienta nejsou dodržet aplikace heslem
- Místní ověřování protokolování / auditování možnost je k dispozici pro aplikaci heslo
- Další education koncových uživatelů je nutný pro klienta Microsoft Lync 2013. Kroky najdete v článku jak změnit heslo v e-mailu aplikace heslo.
- Některé rozšířené architektonických návrhů může vyžadovat kombinací organizační uživatelského jména a hesla aplikace při použití vícefaktorové ověřování s klienty, podle toho, kde jsou ověření. Pro klienty, které ověřování infrastrukturu na pracovišti použijete organizační uživatelské jméno a heslo. Pro klienty, které ověřování Azure AD byste použili heslo aplikace.

Předpokládejme například, že máte architektura, která se skládá z následujících akcí:

- Jsou federování instanci aplikace místní služby Active Directory s Azure AD
- Používáte Exchange online
- Používáte Lync, pomocí kterého se konkrétně na pracovišti
- Používáte Azure Vícefaktorové ověřování


![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

 V těchto případech musí postupujte takto:

- Při přihlášení k Lyncu, použijte uživatelské jméno a heslo vaší organizace.
- Při pokusu o otevření adresáře přes klienta aplikace Outlook, který se připojuje k Exchange online, použijte aplikaci heslo.

### <a name="allowing-app-password-creation"></a>Povolení aplikace heslo vytváření
Ve výchozím nastavení uživatelé nemohou vytvářet aplikace hesla.  Tato funkce je zapnutá.  Pokud chcete uživatelům povolit možnost vytvořit aplikaci hesla, použijte následující postup.

#### <a name="to-enable-users-to-create-app-passwords"></a>Chcete-li povolit uživatelům vytvářet aplikace hesla



1. Přihlaste se k portálu Azure klasické.
2. Na levé straně klikněte na služby Active Directory.
3. V části adresář klikněte v adresáři pro uživatele, kterého chcete povolit.
4. Nahoře klikněte na uživatele.
5. V dolní části stránky klikněte na spravovat ověřeného Multi-Factor  
6. V horní části stránky vícefaktorové ověřování klikněte na nastavení služby.
7. Ujistěte se, že je vybrán přepínač vedle umožnit uživatelům vytvářet aplikace hesla se přihlásit do aplikace jiné prohlížeče.


![Vytvoření aplikace hesla](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="creating-app-passwords"></a>Vytvoření aplikace hesel
Během počáteční registrace mohou uživatelé vytvářet aplikace hesla.  Jsou uvedeny možnost na konci procesu registrace, který umožňuje, aby byla vytvořená.

Kromě uživatelů můžete taky vytvořit aplikaci hesla později změnou jejich nastavení Azure portálu Office 365 je portál nebo tak, že

### <a name="to-create-app-passwords-in-the-office-365-portal"></a>Vytvoření aplikace hesla v portálu Office 365
--------------------------------------------------------------------------------


1. Přihlaste se k portálu Office 365
2. V pravém horním rohu vyberte nastavení widgety
3. V levé části vyberte další ověření zabezpečení
4. Na pravé straně klepněte na možnost **Aktualizovat telefonní čísla zabezpečení účtu**
5. Na stránce proofup nahoře vyberte aplikaci hesla
6. Klikněte na tlačítko **vytvořit**
7. Zadejte název aplikace heslo a klikněte na tlačítko **Další**
8. Zkopírujte heslo aplikace do schránky a vložte je do aplikace.

<center>![Cloud](./media/multi-factor-authentication-whats-next/security.png)</center>


### <a name="to-create-app-passwords-in-the-azure-portal"></a>Vytvoření aplikace hesla v portálu Azure
--------------------------------------------------------------------------------
1. Přihlaste se k portálu Azure klasické.
3. Nahoře klikněte pravým tlačítkem myši na své uživatelské jméno a vyberte další ověření zabezpečení.
5. Na stránce proofup nahoře vyberte aplikaci hesla
6. Klikněte na tlačítko **vytvořit**
7. Zadejte název aplikace heslo a klikněte na tlačítko **Další**
8. Zkopírujte heslo aplikace do schránky a vložte je do aplikace.


![Aplikace hesla](./media/multi-factor-authentication-whats-next/app2.png)

### <a name="to-create-app-passwords-if-you-do-not-have-an-office-365-or-azure-subscription"></a>Vytvoření aplikace hesla, pokud nemáte předplatné Office 365 nebo Azure
--------------------------------------------------------------------------------
1. Přihlaste se k [https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Nahoře vyberte profil.
3. Klikněte na své uživatelské jméno a vyberte další ověření zabezpečení.
5. Na stránce proofup nahoře vyberte aplikaci hesla
6. Klikněte na tlačítko **vytvořit**
7. Zadejte název aplikace heslo a klikněte na tlačítko **Další**
8. Zkopírujte heslo aplikace do schránky a vložte je do aplikace.

![Aplikace hesla](./media/multi-factor-authentication-whats-next/myapp.png)

## <a name="remember-multi-factor-authentication-for-devices-users-trust"></a>Mějte na paměti Vícefaktorové ověřování pro uživatele zabezpečení zařízení

Pamatovat Vícefaktorové ověřování pro zařízení a prohlížeče, aby uživatelé zabezpečení je bezplatné funkce pro všechny uživatele MFA.  Umožňuje uživatelům poskytnout možnost obtokem MFA pro sadu počet dní po provedení úspěšné přihlášení pomocí MFA. To můžete být díky dokonalejšímu použitelnosti pro uživatele.

Však od mohou uživatelé zapamatovatelné MFA důvěryhodných zařízení, může tato funkce snížit zabezpečení účtu. Zajistit zabezpečení účtu obnovíte Vícefaktorové ověřování pro své zařízení pro jednu z následujících situacích:

- Pokud dojít ke své firemní účet
- Pokud dojde ke ztrátě nebo krádeže nalezenou zařízení

> [AZURE.NOTE] Tato funkce implementovaná jako mezipaměti prohlížeče souborů cookie. Pokud nejsou povoleny soubory cookie prohlížeče nefunguje.

### <a name="how-to-enabledisable-remember-multi-factor-authentication"></a>Povolení nebo zakázání zapamatovat vícefaktorové ověřování

1. Přihlaste se k portálu Azure klasické.
2. Na levé straně klikněte na služby Active Directory.
3. V části služby Active Directory klikněte na adresář, který chcete nastavit nezapomeňte Vícefaktorové ověřování pro zařízení.
4. Na adresář, který jste vybrali klikněte na konfigurovat.
5. V části vícefaktorové ověřování klikněte na nastavení služby Správa.
6. Na stránce nastavení služby v části spravovat nastavení zařízení uživatele, vybrat nebo zrušit výběr **Povolit uživatelům zapamatovatelné vícefaktorové ověřování na zařízeních, která budou důvěřovat**.
![Mějte na paměti zařízení](./media/multi-factor-authentication-whats-next/remember.png)
8. Nastavte počet dní, které chcete povolit pozastavení. Výchozí hodnota je 14 dní.
9. Klikněte na Uložit.
10. Klikněte na Zavřít.


## <a name="selectable-verification-methods"></a>Lze vybrat ověření metody
Na cloudu a místní verze můžete zvolit, které metody ověřování jsou dostupné pro uživatele. Následující tabulka obsahuje stručný přehled obou metod.

Pokud vaši uživatelé pro MFA zapsat svých účtů, budou zvolte způsob jejich upřednostňované ověření z možností, které jste povolili. Pokyny pro jejich procesu registrace jsou obsaženy v [Nastavení účtu pro dvoustupňového ověření](multi-factor-authentication-end-user-first-time.md)

Metoda|Popis
:------------- | :------------- |
Volání na telefon |  Umístí automatizovaných hlasových volání na telefon ověřování. Uživatel odpoví hovor a stiskne # na klávesnici telefonu k ověření. Telefonní číslo se nesynchronizuje místní služby Active Directory.
Textovou zprávu do telefonu | Odešle textovou zprávu obsahující ověřovací kód uživateli. Uživatel bude muset buď odpovědět na textovou zprávu s ověřovací kód nebo zadat ověřovací kód do rozhraní přihlašovací.
Oznámení pomocí mobilní aplikace | V tomto režimu aplikace Microsoft Authenticator zabrání neoprávněnému přístupu na účty a přestane podvodné transakce. Důvodem je používat nabízená oznámení na telefonu nebo registrovaných zařízení. Jednoduše zobrazit oznámení a pokud je legitimní klepnete ověření. V opačném případě se může zvolit odepřít nebo odepřít a vykazování podvodné oznámení. Informace o vytváření sestav podvodné oznámení najdete v článku jak používat funkci podvod sestavy a odepřít pro Vícefaktorové ověřování.</br></br>Aplikace Microsoft Authenticator je k dispozici pro [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).|
Ověřovací kód v mobilní aplikaci | V tomto režimu aplikace Microsoft Authenticator mohou sloužit jako software token generovat kód ověření MÍSTOPŘÍSEŽNÉM. Ověřovací kód může být zadán potom spolu s uživatelské jméno a heslo k poskytování druhý formulář ověřování.</li><br><p> Aplikace Microsoft Authenticator je k dispozici pro [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

### <a name="how-to-enabledisable-authentication-methods"></a>Povolení nebo zakázání metody ověřování

1. Přihlaste se k portálu Azure klasické.
2. Na levé straně klikněte na služby Active Directory.
3. V části služby Active Directory klikněte na adresář, který chcete povolit nebo zakázat metody ověřování.
4. Na adresář, který jste vybrali klikněte na konfigurovat.
5. V části vícefaktorové ověřování klikněte na nastavení služby Správa.
6. Na stránce nastavení služeb ve skupinovém rámečku Možnosti ověření vybrat nebo zrušit výběr možnosti, které chcete použít.</br></br>
![Ověření možnosti](./media/multi-factor-authentication-whats-next/authmethods.png)
9. Klikněte na Uložit.
10. Klikněte na Zavřít.
