<properties
   pageTitle="Obecný LDAP spojnice | Microsoft Azure"
   description="Tento článek popisuje, jak nakonfigurovat obecné spojnice LDAP společnosti Microsoft."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-ldap-connector-technical-reference"></a>Technické informace o obecných LDAP spojnice
Tento článek popisuje obecný LDAP spojnice. Článek se týká těchto produktů:

- Správce Microsoft identit 2016 (MIM2016)
- Správce identit Forefront 2010 R2 (FIM2010R2)
    -   Musíte použít oprava 4.1.3671.0 nebo novější [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 a FIM2010R2 spojnice je k dispozici ke stažení z [Webu služby Stažení softwaru](http://go.microsoft.com/fwlink/?LinkId=717495).

Při odkazování na IETF RFC, tento dokument ve formátu (RFC [číslo] / [oddílu v dokumentu RFC]), například (RFC 4512/4.3).
Další informace najdete v http://tools.ietf.org/html/rfc4500 (nutné zobrazit posunutím nahraďte bylo správné číslo Konceptu 4500).

## <a name="overview-of-the-generic-ldap-connector"></a>Základní informace o konektoru obecný LDAP
Obecný spojnice LDAP umožňuje integrace služby synchronizace s serveru v3 LDAP.

Některé operace a prvků schématu, třeba můžou být potřeba k importu delta nejsou zadány v RFC IETF. Tyto operace jsou podporované jenom adresářů LDAP výslovně zadaný.

Z uceleném pohledu tyto funkce jsou podporovány ální spojnice:

Funkce | Podpora
--- | --- |
Připojený zdroj dat | Spojnice je podporována kde na všech serverech v3 LDAP (RFC 4510 kompatibilní s). Po testování s takto: <li>Microsoft Active Directory Lightweight Directory Services (AD ADAM)</li><li>Microsoft Active Directory globální katalog AD</li><li>Server 389 adresáře</li><li>Server Apache adresáře</li><li>Můžete IBM Pošta</li><li>Isode adresáře</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Otevřít DJ</li><li>Otevřít Pošta</li><li>Otevřít LDAP (openldap.org)</li><li>Oracle (dříve neděle) adresáře Server Enterprise Edition</li><li>RadiantOne virtuální adresář serveru (Virtuální diskovou službu)</li><li>Ne jednu adresáře serveru</li>**Důležitá adresáře nejsou podporované:** <li>Microsoft Active Directory Domain Services (AD DS) [použijte předdefinované Active Directory Connector]</li><li>Internetový adresář Oracle (ID objektu)</li>
Scénáře   | <li>Správa životního cyklu objektu</li><li>Správa skupiny</li><li>Správa hesel</li>
Operace |Tyto operace jsou podporovány na všechny adresáře LDAP: <li>Úplný Import</li><li>Export</li>Tyto operace jsou podporované jenom na určité adresáře:<li>Delta importu</li><li>Dialogové okno nastavit heslo, změnit heslo</li>
Schéma | <li>Schéma ke zjištění z schématu LDAP (RFC3673 a RFC4512/4.2)</li><li>Podporuje strukturální třídy, aux tříd a extensibleObject objekt třídy (RFC4512/4.3)</li>

### <a name="delta-import-and-password-management-support"></a>Podpora správy import a heslo delta
Podporované adresářů pro Delta import a Správa hesel:

- Microsoft Active Directory Lightweight Directory Services (AD ADAM)
    - Podporuje všechny operace importu delta
    - Podporuje nastavení hesla
- Microsoft Active Directory globální katalog AD
    - Podporuje všechny operace importu delta
    - Podporuje nastavení hesla
- Server 389 adresáře
    - Podporuje všechny operace importu delta
    - Podporuje nastavit heslo a změnit heslo
- Server Apache adresáře
    - Podporuje delta import od tohoto adresáře nemá protokol trvalé změny
    - Podporuje nastavení hesla
- Můžete IBM Pošta
    - Podporuje všechny operace importu delta
    - Podporuje nastavit heslo a změnit heslo
- Isode adresáře
    - Podporuje všechny operace importu delta
    - Podporuje nastavit heslo a změnit heslo
- Novell eDirectory a NetIQ eDirectory
    - Podporuje přidat, aktualizovat a přejmenovat operace importu delta
    - Nepodporuje operace odstranění pro delta import
    - Podporuje nastavit heslo a změnit heslo
- Otevřít DJ
    - Podporuje všechny operace importu delta
    - Podporuje nastavit heslo a změnit heslo
- Otevřít Pošta
    - Podporuje všechny operace importu delta
    - Podporuje nastavit heslo a změnit heslo
- Otevřít LDAP (openldap.org)
    - Podporuje všechny operace importu delta
    - Podporuje nastavení hesla
    - Nepodporuje změnit heslo
- Oracle (dříve neděle) adresáře Server Enterprise Edition
    - Podporuje všechny operace importu delta
    - Podporuje nastavit heslo a změnit heslo
- RadiantOne virtuální adresář serveru (Virtuální diskovou službu)
    - Musí být pomocí verze 7.1.1 nebo vyšší
    - Podporuje všechny operace importu delta
    - Podporuje nastavit heslo a změnit heslo
-  Ne jednu adresáře serveru
    - Podporuje všechny operace importu delta
    - Podporuje nastavit heslo a změnit heslo

### <a name="prerequisites"></a>Zjistit předpoklady pro
Před použitím spojnice, zkontrolujte, jestli že máte následující na serveru synchronizace:

- 4.5.2 rozhraní Microsoft .NET Framework nebo novější

### <a name="detecting-the-ldap-server"></a>Zjištění serveru LDAP
Spojnice vycházejí z předpokladu různých způsobech zjišťování a identifikovat LDAP server. Spojnice používá kořenové DSE, dodavatele název/verze a vyhodnotí schématu zobrazíte jedinečné objekty a atributy známé určitých LDAP serverů. Tato data, pokud najít, se používá k nabízet možností konfigurace v spojnice.

### <a name="connected-data-source-permissions"></a>Připojení zdroje dat oprávnění
K importu a exportu k objektům v adresáři připojené, musí mít účet spojnice dostatečná oprávnění. Spojnice musí psaní oprávnění k exportu a oprávnění, aby mohli importovat jen pro čtení. Konfigurace oprávnění se provádí v prostředí management cílový adresář samotné.

### <a name="ports-and-protocols"></a>Porty a protokoly
Spojnice používá zadané v konfiguraci, ve výchozím nastavení je 389 protokolu LDAP a 636 LDAPS číslo portu.

Pro LDAPS musíte použít protokol SSL 3.0 nebo TLS. Protokol SSL 2.0 nepodporuje a nelze aktivovat.

### <a name="required-controls-and-features"></a>Požadované ovládací prvky a funkce
Tyto ovládací prvky LDAP/funkce musí být dostupné na serveru LDAP spojnice tak, aby fungoval správně:  
`1.3.6.1.4.1.4203.1.5.3`True nebo False filtry

True nebo False filtr není často ohlášení nepodporuje adresářů LDAP a může zobrazují na **Globální stránky** klikněte v části **Povinné funkce nebyla nalezena**. Slouží k vytváření **nebo** filtrů v dotazech LDAP, například při importu více typů objektů. Pokud importujete více než jeden typ objektu, serveru LDAP podporuje tuto funkci.

Pokud používáte v adresáři se ukotvení jedinečný identifikátor následující musí být také k dispozici (viz oddíl [Konfigurace ukotvení](#configure-anchors) dál v tomto článku najdete další informace):  
`1.3.6.1.4.1.4203.1.5.1`Všechny provozní atributy

Pokud v adresáři má více objektů, než vejde v jednom volání do adresáře, je vhodné použít stránkování. Stránkování pracovat, musíte jednu z následujících možností:

**Možnost 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Možnost 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Pokud obou možnostech jsou povoleny v konfiguraci spojnice, použije se pagedResultsControl.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl se používá pouze pro metodu USNChanged delta importu mohli zobrazit odstraněné objekty.

Spojnice pokusí ke zjištění možností prezentovat na serveru. Pokud možnosti nebyl nalezen, upozorněním týkajícím se nachází na stránce globální ve vlastnostech spojnice. Některé LDAP servery prezentovat: všechny ovládací prvky/funkce podporují a i když toto upozornění existuje, spojnice může pracovat bez problémů.

### <a name="delta-import"></a>Delta importu
Delta import je dostupná jenom Pokud byl zjištěn adresář podpory. Slouží aktuálně následujících způsobů:

- LDAP Accesslog. V části [protokolování http://www.openldap.org/doc/admin24/overlays.html#Access](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
- LDAP Changelog. V tématu [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
- Časové razítko. Pro Novell/NetIQ eDirectory spojnice používá k získání datum a čas poslední vytvořit a aktualizovat objekty. Novell/NetIQ eDirectory neposkytuje ekvivalent prostředky k načtení odstraněných objektů. Tuto možnost lze také pokud jiný způsob importu delta je aktivní na serveru LDAP. Tato možnost není možné odstranit import objektů.
- USNChanged. Viz: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Nepodporovaná funkce
Nejsou podporované tyto funkce LDAP:

- LDAP odkazy mezi servery (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Vytvoření nové spojnice
Obecný LDAP spojnici vytvoříte v **Synchronizační službu** vyberte **Agent pro správu** a **vytvořit**. Vyberte spojnici **Obecný LDAP (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Připojení
Na stránce připojení musíte zadat informace Host, portů a vazby. Podle toho, což vazbu je vybraný, další může být informace předávaných v následujících částech.

![Připojení](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

- Nastavení časového limitu se používá pouze první připojení k serveru při zjišťování schématu.
- Pokud vazba je anonymní, pak ani uživatelské jméno a heslo ani certifikát používají.
- Další vazeb zadejte informace o buď v uživatelské jméno a heslo nebo vyberte certifikát.
- Pokud používáte Kerberos k ověření, pak taky poskytují sféry/doméně uživatele.

**Atribut aliasy** textového pole se používá pro atributy definované do schématu s RFC4522 syntaxe. Tyto atributy nebyl nalezen během schématu rozpoznávání a spojnice musí nápovědy k identifikaci tyto atributy. Následující příklad je potřeba k zadané v poli atribut aliasy s navrženými atribut userCertificate jako atribut binární:

`userCertificate;binary`

Následujícím obrázku je příklad pro jak může vypadat toto nastavení:

![Připojení](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Zaškrtněte políčko **Zahrnout provozní atributy ve schématu** tak, aby zahrnoval atributy vytvořené serverem. Jedná se o atributy například kdy byl vytvořen a poslední aktualizace objektu.

Vyberte **Zahrnout extensible atributy ve schématu** Pokud extensible objekty (RFC4512/4.3) se používají a povolení této možnosti umožňuje každé atribut, který chcete použít u všech objektů. Tato možnost umožňuje schématu obrovské tak, aby pokud adresáři připojeného používá tuto funkci doporučení zachovat možnost nevybírejte.

### <a name="global-parameters"></a>Globální parametry
Na stránce globální parametry nakonfigurujete DN delta – protokol změn a další funkce LDAP. Na stránce se předem vyplněné informace poskytované LDAP serverem.

![Připojení](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

V horní části zobrazí informace poskytované vlastní server, jako je název serveru. Spojnice také ověří, že se účastní DSE kořenové povinné ovládací prvky. Pokud tyto možnosti řízení tu nejsou uvedené, jsou uvedeny upozornění. Seznam adresářů některé LDAP úkolů všechny použité funkce DSE kořenové a je možné, že spojnice funguje bez problémů i v případě upozornění existuje.

Zaškrtávací políčka **podporuje ovládací prvky** řízení chování určitých operací:

- S vybranou položkou odstranit strom, hierarchie odstraněna jedním LDAP voláním. S stromu odstranit nevybrané spojnice znamená rekurzivní odstranit v případě potřeby.
- S vybranou stránkovaného výsledky spojnice znamená stránkovaného import s velikostí zadanou v spouštěcím kroky.
- VLVControl a SortControl je alternativy pagedResultsControl k načtení dat z adresář LDAP.
- Pokud jsou všechny tři možnosti (pagedResultsControl, VLVControl a SortControl) nevybrané spojnice importuje objektů v jedné dávce může selhat, pokud je to velké adresář.
- ShowDeletedControl se používá pouze po způsob importu Delta USNChanged.

Protokol změn DN je kontextu názvů používá protokol změn delta, například **cn = changelog**. Tato hodnota je nutné zadat do budete moct udělat delta importovat.

Následující obrázek je seznam výchozí – protokol změn DNs:

Adresář | Delta – protokol změn
--- | ---
Microsoft AD LDS a AD ° c | Automatické zjišťování. USNChanged.
Server Apache adresáře | Není k dispozici.
Adresář 389 | Protokol změn. Výchozí hodnota použít: **cn = changelog**
Můžete IBM Pošta | Protokol změn. Výchozí hodnota použít: **cn = changelog**
Isode adresáře | Protokol změn. Výchozí hodnota použít: **cn = changelog**
Novell/NetIQ eDirectory | Není k dispozici. Časové razítko. Použití spojnice poslední aktualizace datum a čas získat přidali a aktualizovat záznamy.
Otevřít DJ/Pošta | Protokol změn.  Výchozí hodnota použít: **cn = changelog**
Otevřít LDAP | Přístup k protokolu. Výchozí hodnota použít: **cn = accesslog**
Oracle DSEE | Protokol změn. Výchozí hodnota použít: **cn = changelog**
RadiantOne virtuální diskovou službu | Virtuální adresář. Závisí na adresáři připojené k virtuální diskovou službu.
Ne jednu adresáře serveru | Protokol změn. Výchozí hodnota použít: **cn = changelog**

Atribut heslo se jmenuje atribut, který konektoru má použít pro nastavení hesla v tématu Změna hesla a hesla: nastavení operace.
Tato hodnota je ve výchozím nastavení **userPassword** , ale bude možné měnit v případě potřeby pro určitý LDAP systém.

V seznamu další oddíly je možné přidat další obory názvů detekovaný automaticky. Například toto nastavení slouží-li několik serverů logické clusteru, která by měla být importovat všechny najednou. Stejně jako služby Active Directory můžete mít víc domén v jedné doménové ale všech domén sdílet jedno schéma, můžete stejné simulovaného zadáním další obory názvů do tohoto pole. Každý obor názvů můžete importovat z jiných serverů a další nakonfigurovaný na stránce Konfigurovat oddíly a hierarchie. Pomocí kombinace kláves Ctrl + Enter nový řádek.

### <a name="configure-provisioning-hierarchy"></a>Konfigurace vytváření hierarchie
Tato stránka slouží k mapování součást název domény, například OU typu objektu, který by měl být zřízení, například názvu.

![Vytváření hierarchie](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Konfigurace vytváření hierarchie, můžete nakonfigurovat spojnice tak, aby automaticky vytvářet strukturu v případě potřeby. Například, pokud existuje datacentrum názvů = contoso, datacentrum = com a nový objekt cn = Jana, ou = Seattlu, c = US, datacentrum = contoso, datacentrum = zřízení modelu com a potom na spojnici objekt můžete vytvořit typ země USA a názvu pro Seattlu Pokud můžou být ještě nejsou v adresáři.

### <a name="configure-partitions-and-hierarchies"></a>Konfigurace oddíly a hierarchie
Na stránce oddíly a hierarchie vyberte všechny obory názvů s objekty, které plánujete import a export.

![Oddíly](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Pro každý názvů je také možné ji nakonfigurovat nastavení připojení, které chcete přepsat hodnoty uvedené v dialogovém okně připojení. Pokud tyto hodnoty zůstaly na své výchozí prázdné hodnoty, použije se informace na obrazovce připojení.

Je taky možné vybrat, které kontejnery a organizačních jednotek konektoru má import a export do.

### <a name="configure-anchors"></a>Konfigurace ukotvení
Tato stránka vždy obsahovat hodnotu předkonfigurované a nelze změnit. Pokud byl zjištěn serveru dodavatel, může ukotvení vyplněné s neměnný atribut, například identifikátor GUID objektu. Pokud nebyl nalezen nebo je známo nastavit, aby se neměnný atribut, pak spojnice používá jako ukotvení dn (rozlišující název).

![ukotvení](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)

Následující obrázek je seznam na serverech LDAP a ukotvení používá:

Adresář | Ukotvení atribut
--- | ---
Microsoft AD LDS a AD ° c | objectGUID
Server 389 adresáře | rozlišující název
Apache adresáře | rozlišující název
Můžete IBM Pošta | rozlišující název
Isode adresáře | rozlišující název
Novell/NetIQ eDirectory | IDENTIFIKÁTOR GUID
Otevřít DJ/Pošta | rozlišující název
Otevřít LDAP | rozlišující název
Oracle ODSEE | rozlišující název
RadiantOne virtuální diskovou službu | rozlišující název
Ne jednu adresáře serveru | rozlišující název

## <a name="other-notes"></a>Další poznámky
Tato část obsahuje informace, které jsou specifické pro tento spojnice nebo z jiného důvodu důležitá se aspekty.

### <a name="delta-import"></a>Delta importu
Delta vodoznak v otevřené LDAP je UTC datum a čas. Z tohoto důvodu musí být synchronizovány hodiny FIM synchronizace služby od otevřený LDAP. Pokud ne, některé položky v protokolu změn delta může vynechány.

Pro Novell eDirectory není delta import zjišťování libovolný objekt odstraní. Z toho důvodu je třeba spustit úplný import pravidelně zobrazíte všechny odstraněné objekty.

Adresářů s protokol změn delta, který je založený na typu datum a čas doporučujeme spuštění úplného importu ve pravidelných termínech. Tento proces umožňuje sync engine zobrazíte a rozdíly mezi LDAP server a jaká jsou aktuálně do pole spojnice.

## <a name="troubleshooting"></a>Řešení potíží

-   Informace o tom, jak povolit protokolování pro řešení potíží s spojnice zjistěte, [jak povolit trasování událostí pro Windows trasování spojnice](http://go.microsoft.com/fwlink/?LinkId=335731).
