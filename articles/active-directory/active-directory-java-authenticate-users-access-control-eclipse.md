<properties
    pageTitle="Jak pomocí řízení přístupu (Java) | Microsoft Azure"
    description="Informace o vytvoření a řízení přístupu pomocí jazyka Java v Azure."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-authenticate-web-users-with-azure-access-control-service-using-eclipse"></a>Jak ověřit uživatelů webu se službou Azure přístup ovládací prvek prostřednictvím zatmění

Tento průvodce vám ukáže, jak můžete Azure přístup ovládací prvek služby (ACS) v rámci sady nástrojů Azure u zatmění. Další informace o ACS naleznete v části [Další kroky](#next_steps) .

> [AZURE.NOTE]
> Filtr ovládací prvek služby Azure přístup je náhled technologii komunity. Jako předběžnou verzi softwaru není podporována dříve Microsoft.

## <a name="what-is-acs"></a>Co je ACS?

Většina vývojářů nejsou identity odborníky a obecně nechcete, aby se vám čas vývoj a tak mohli ověřovat mechanismy pro svých aplikací a služeb. ACS je Azure služba, která poskytují snadný způsob, jak ověřování uživatelů, kteří potřebují dostat k webových aplikací a služeb, aniž byste museli faktor složité ověřování logiky do kódu.

Tyto funkce jsou dostupné v ACS:

-   Integrace se službou Windows Identity Foundation (WIF).
-   Podpora oblíbených webových Zprostředkovatelé identit jiní (IP adresy) včetně Windows Live ID, Google, Yahoo! a Facebooku.
-   Podpora služby Active Directory Federation Services (AD FS) 2.0.
-   Open Data Protocol (OData)-na základě správy služba, která poskytuje programům ACS nastavení.
-   Portálu pro správu dovolující přístup pro správu k nastavení ACS.

Další informace o ACS najdete v článku [Access 2.0 služby ovládacího prvku][].

## <a name="concepts"></a>Koncepty

Azure ACS jsou založeny na objekty na základě deklarací identit - konzistentní přístup k vytváření metody ověřování pro aplikace spuštěné v místním i v cloudu. Na základě deklarací identit umožňuje běžných aplikací a služeb získat informace, které potřebují o identitě uživateli z jejich organizace, v jiných organizacích a na Internetu.

K provedení těchto úkolů v této příručce, je třeba si uvědomit následující pojmy:

**Klient** : V rámci této příručce s postupy jde prohlížeč, který se pokouší získat přístup k vaší webové aplikace.

**Aplikace Relying výrobce (RP)** – RP aplikace je web nebo službu, outsources ověřování jeden externí autorita. V identity žargonu říkáme RP důvěryhodnosti této autorita. Tato příručka vysvětluje, jak nakonfigurovat aplikace zabezpečení ACS.

**Tokenu** - token najdete zabezpečení dat, obvykle vydaný po úspěšném ověření uživatele. Obsahuje sadu *deklarací*, atributy ověřeného uživatele. Deklaraci může výkres znázorňovat uživatelské jméno, patří identifikátor role uživatele, stáří uživatele a tak dál. Token obvykle digitálně podepsán, což znamená, můžete ji vždy zpět do své Vystavitel a její obsah nelze zfalšován. Uživatel získá přístup k aplikaci RP prezentuje platný token vystaven autoritou, kterou aplikaci RP považuje za důvěryhodnou.

**Zprostředkovatele identit (IP)** - IP je úřadu záležitostí tokenů zabezpečení a ověří identitami uživatele. Skutečná práce vydání tokeny prováděna když zvláštní službu s názvem zabezpečení tokenu služby STS (). Typické IP adresy příklady Windows Live ID, Facebook, úložištích uživatele business (například Active Directory) a tak dále.
Když protože je nakonfigurována tak s informacemi o důvěryhodnosti IP, systém přijmout a ověřte tokeny vydaný tuto IP. ACS můžete důvěřovat více IP adresy najednou, což znamená, že když aplikace důvěryhodný ACS, můžete okamžitě nabídnout aplikace na všechny ověřené uživatele ze všech IP adresy že ACS považuje za důvěryhodné vaším jménem.

**Federace poskytovatele (dokončených produktů)** - IP adresy přímé znají uživatelů a jejich pomocí svých přihlašovacích údajů ověření a problémů tvrzení o co budou vědět o nich znáte. Federace poskytovatele (dokončených produktů) je různé druhy autorita: místo ověřování uživatelů přímo, funguje jako zprostředkovatel a zprostředkovatelů ověřování mezi jeden RP a jednu nebo více IP adresy. IP adresy a snímků / vydání tokenů zabezpečení, tedy používají obě zabezpečení tokenu služby STS (). ACS je jeden dokončených produktů.

**Modul pravidlo ACS** - logickou slouží k transformaci příchozí tokenů z důvěryhodného IP adresy tokeny má být využívané RP kodifikováno v podobě jednoduché deklarací transformace pravidel. ACS funkcemi modul pravidla, který má na starosti použití jakéhokoliv transformace logiky určeným pro vaše RP.

**ACS Namespace** – obor je oddíl nejvyšší úrovně ACS, který použijete pro uspořádání nastavení. Obor názvů obsahuje seznam IP adresy důvěřujete, RP aplikace, které chcete vytisknout, pravidla očekávat pravidlo modulu zpracuje příchozí tokeny s atd. Obor názvů zpřístupňuje různých koncové body, které se použije aplikace a vývojář získat ACS fungoval.

Následující obrázek znázorňuje, jak funguje ověřování ACS s webovou aplikací:

![ACS vývojový diagram][acs_flow]

1.  Klient (v tomto případě prohlížeči) požaduje stránku z RP.
2.  Protože žádost dosud nebyly ověřeny, RP přesměruje uživatele oprávnění, kterou se považuje za důvěryhodnou, což je ACS. ACS představuje uživatel s výběrem IP adresy, kterou jste zadali pro tento RP. Uživatel vybere příslušnou IP.
3.  Klient přejde na stránku ověření adresy IP a zobrazí výzvu k přihlášení.
4.  Po ověření klienta (například identita zadaná přihlašovací údaje) IP problémy tokenu zabezpečení.
5.  Po vystavení tokenu zabezpečení, IP přesměruje klienta na ACS a klient odešle tokenu zabezpečení vydané IP ACS.
6.  ACS ověří tokenu zabezpečení IP vstupů identitu nároky v tomto tokenu do modul pravidla ACS vypočítá deklarací identity výstup a nové tokenu zabezpečení, který obsahuje tyto požadavky výstup při potížích vydaný.
7.  ACS přesměruje klienta RP. Klient odešle nové tokenu zabezpečení vydané ACS RP. RP ověří podpis na tokenu zabezpečení vydaný ACS ověří nároky v tomto tokenu a chybovou stránku, která byla původně požadovaná.

## <a name="prerequisites"></a>Zjistit předpoklady pro

K provedení těchto úkolů v této příručce, budete potřebovat:

- Java vývojář Kit (JDK), v 1,6 nebo novější.
- -Zatmění integrovaném vývojovém prostředí pro vývojáře í Java džínovinu nebo novější. Můžete stáhnout z <http://www.eclipse.org/downloads/>. 
- Rozdělení na základě Java webový server nebo aplikační server, například Apache Tomcat, GlassFish, JBoss aplikační Server nebo molo.
- Azure předplatné, které lze získat z <http://www.microsoft.com/windowsazure/offers/>.
- Uvolněte dubna 2014 Azure sada nástrojů pro Eclipse, nebo později. Další informace najdete v tématu [instalace Azure sada nástrojů pro Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690946.aspx).
- Certifikát X.509 pomocí aplikace. Budete potřebovat tento certifikát v veřejné certifikátu (.cer) a osobní informace Exchange (. Formát PFX). (Možnosti pro vytváření certifikát bude popsána dál v tomto kurzu).
- Znalost Azure výpočet emulátoru a nasazení techniky popisované v [Vytvoření Ahoj prezentace aplikace pro Azure v zatmění](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx).

## <a name="create-an-acs-namespace"></a>Vytvoření ACS Namespace

Pokud chcete v práci začít používat služby Řízení přístupu (ACS) Azure, musíte vytvořit ACS názvů. Obor názvů obsahuje jedinečné obor adresování ACS zdroje z aplikace.

1. Přihlaste se k [portálu Správa Azure][].
2. Klikněte na **služby Active Directory**. 
3. Pokud chcete vytvořit nový obor názvů řízení přístupu, klikněte na **Nový**, klikněte na **Aplikaci služby**, klikněte na **Řízení přístupu**a klikněte na **Vytvořit**. 
4. Zadejte název pro obor. Azure ověřuje, zda je jedinečný název.
5. Vyberte oblast, ve kterém se používá obor názvů. Dosažení nejlepších výsledků dosáhnete oblast, ve kterém jsou nasazení aplikace.
6. Pokud máte víc předplatných vyberte předplatné, které chcete použít pro obor ACS.
7. Klikněte na **vytvořit**.

Azure vytvoří a aktivuje obor názvů. Počkejte, dokud je před pokračováním **aktivní** stav Nový obor názvů. 

## <a name="add-identity-providers"></a>Přidání Zprostředkovatelé identit jiní

V tomto úkolu přidejte IP adresy pro účely ověření s aplikací RP. Pro účely ukázky tento úkol ukazuje, jak přidat Windows Live jako IP, ale můžete použít některou z uvedených v portálu pro správu ACS IP adresy.


1.  Na [Portálu Správa Azure][]klikněte **Služby Active Directory**, vyberte obor názvů řízení přístupu a potom klikněte na **Spravovat**. Na portálu Správa ACS otevře.
2.  V levém navigačním podokně na portálu Správa ACS klikněte na **Zprostředkovatelé identit jiní**.
3.  Windows Live ID je standardně zapnutá a nelze odstranit. Pro účely tohoto kurzu jenom Windows Live ID použít. Tato obrazovka je ale, kde můžete přidat další IP adresy, kliknutím na tlačítko **Přidat** .

Windows Live ID je teď povolené IP pro ACS názvů. Pak zadáte Java webové aplikace (nevytvoří později) jako RP.

## <a name="add-a-relying-party-application"></a>Přidání aplikace předávající stran

V tomto úkolu nakonfigurujete ACS rozpoznatelný webové aplikace Java jako platná RP aplikace.

1.  Na portálu Správa ACS klikněte na **Relying stran aplikace**.
2.  Na stránce **Může aplikace** klikněte na **Přidat**.
3.  Na stránce **Přidat aplikaci stran může** postupujte takto:
    1.  Do pole **název**zadejte název RP. Pro účely tohoto kurzu zadejte **Azure v prohlížeči**.
    2.  V **režimu**, vyberte **Zadejte nastavení ručně**.
    3.  Do **sféry**zadejte identifikátor URI, ke kterému tokenu zabezpečení vydaný ACS slouží k použití. Tento úkol zadejte **http://localhost:8080 /**.
        ![Může stran sféry pro použití ve výpočetním emulátoru][relying_party_realm_emulator]
    4.  **Vrácená** adresa URL zadejte adresu URL ACS vrací tokenu zabezpečení. Tento úkol zadejte **http://localhost:8080/MyACSHelloWorld/index.jsp**
        ![Relying stran návratová adresa URL pro použití ve výpočetním emulátoru][relying_party_return_url_emulator]
    5.  Nechejte výchozí hodnoty v ostatních polích.

4.  Klikněte na **Uložit**.

Teď úspěšně jste nakonfigurovali webové aplikace Java při spuštění v Azure výpočetním emulátoru (na http://localhost:8080 /) je RP v ACS názvů. Dále vytvořte pravidla, která ACS používá ke zpracování žádostí o RP.

## <a name="create-rules"></a>Vytvoření pravidla

V tomto úkolu definujte pravidla, která řídit, jak deklarací jsou předán z IP adresy vaší RP. Pro účely tohoto průvodce můžeme jednoduše nakonfiguruje ACS ke kopírování typy vstupní deklarací a hodnoty přímo v tokenu výstup bez filtrování a jejich úprav.

1.  Na portálu Správa ACS hlavní stránky klikněte na **skupiny pravidel**.
2.  Na stránce **Skupiny pravidel** klikněte na **Výchozí skupiny pravidel pro Azure Web App**.
3.  Na stránce **Upravit skupinu pravidla** klikněte na **Generovat**.
4.  Na **Generovat pravidla: výchozí skupiny pravidel pro Azure Web App** stránky, zajistit, aby je zaškrtnuté políčko Windows Live ID a potom klikněte na **Generovat**.    
5.  Na stránce **Upravit skupinu pravidla** klikněte na **Uložit**.

## <a name="upload-a-certificate-to-your-acs-namespace"></a>Nahrání certifikát ACS názvů

V tomto úkolu nahrajete. PFX certifikát, který se bude používat pro podepisování tokenu žádostí o vytvořil ACS názvů.

1.  Na hlavní stránce portálu Správa ACS klikněte na tlačítko **certifikáty a klíče**.
2.  Na stránce **certifikáty a klávesy** klikněte na **Přidat** nad **Tokenu podepisování**.
3.  Na stránce **Přidat tokenu podpisový certifikát nebo klíč** :
    1. V části **použité pro** klikněte na **Může aplikace** a vyberte **Azure Web App** (který jste předtím nastavili jako název aplikace předávající stran).
    2. V části **Typ** vyberte **Certifikát X.509**.
    3. V části **certifikát** klikněte na tlačítko Procházet a přejděte na soubor certifikátu X.509, který chcete použít. To bude. Soubor PFX. Vyberte soubor, klikněte na **Otevřít**a potom do textového pole **heslo** zadejte heslo certifikát. Všimněte si, že pro účely testování, mohli byste použít ztlumí-signed-certifikát. Vytvoření certifikátu podepsaného svým držitelem, použijte tlačítko **Nový** v dialogovém okně **ACS filtr knihovny** (viz dále) nebo pomocí nástroje **encutil.exe** z [project web][] sady Starter Azure jazyka Java.
    4. Ujistěte se, že je zaškrtnuté políčko **Změnit na primární** . Stránka **Přidat tokenu podpisový certifikát nebo klíč** by měla vypadat podobně jako tento.
        ![Přidání podpisový certifikát tokenů][add_token_signing_cert]
    5. Klepnutím na tlačítko **Uložit** uložte nastavení a zavřete stránku **Přidat tokenu podpisový certifikát nebo klávesa** .

Potom zkontrolujte informace na stránce integraci aplikace a zkopírujte identifikátory URI, budete muset konfigurace webové aplikace Java používat ACS.

## <a name="review-the-application-integration-page"></a>Zkontrolujte stránku integraci aplikace

Můžete najít všechny informace a kód potřebných ke konfiguraci webové aplikace Java (RP aplikace) pro práci s ACS na stránce integraci aplikace portálu pro správu ACS. Tyto informace budete potřebovat při konfiguraci webové aplikace Java federované ověřování.

1.  Na portálu Správa ACS klikněte na **integraci aplikace**.  
2.  Na stránce **Integraci aplikace** klikněte na **Přihlašovací stránky**.
3.  Na stránce **Integrace přihlašovací stránky** klikněte na **Azure Web App**.

V **přihlašovací stránky integrace: Azure v prohlížeči** adresa URL uvedené v stránky **možnost 1: odkaz na stránku hostované ACS přihlášení** se použije ve webové aplikaci Java. Při přidávání knihovnu filtru služby Azure přístup ovládací prvek jazyka Java aplikaci, musíte tuto hodnotu.

## <a name="create-a-java-web-application"></a>Vytvoření webové aplikace Java
1. V rámci zatmění v nabídce klikněte na **soubor**, klikněte na **Nový**a klikněte na **Dynamické Project Web**. (Pokud nevidíte **Dynamické Project Web** uvedené jako projektového plánu dostupné po kliknutí na **soubor**, **Nový**, proveďte následující kroky: klikněte na **soubor**, klikněte na **Nový**, klikněte na **projekt**, rozbalte **Web**, **Dynamické Web projektu**a na tlačítko **Další**.) Pro účely tohoto kurzu zadejte název projektu **MyACSHelloWorld**. (Zajistit použijete tento název, následující kroky v tomto kurzu očekávat souboru WAR se jménem MyACSHelloWorld). Obrazovce bude vypadat takto:

    ![Vytvoření projektu Vítáme pro ACS exampple][create_acs_hello_world]

    Klikněte na **Dokončit**.
2. V zobrazení na zatmění Průzkumník projektu rozbalte **MyACSHelloWorld**. Klikněte pravým tlačítkem na **obsah webu**, klikněte na **Nový**a potom klikněte na **Soubor JSP**.
3. V dialogovém okně **Nový soubor JSP** zadejte název souboru **index.jsp**. Zachovat nadřazenou složku jako MyACSHelloWorld/obsah webu, jak je vidět v následujících tématech:

    ![Přidání souboru JSP například ACS][add_jsp_file_acs]

    Klikněte na tlačítko **Další**.

4. V dialogovém okně **Vybrat šablonu JSP** vyberte **Nový soubor JSP (html)** a klikněte na tlačítko **Dokončit**.
5. Při otevření souboru index.jsp v zatmění přidejte v text k zobrazení **ACS Vítáme!** ve stávající `<body>` prvek. Vaše aktualizované `<body>` obsah objevit jako následující:

        <body>
          <b><% out.println("Hello ACS World!"); %></b>
        </body>
    
    Uložení index.jsp.
  
## <a name="add-the-acs-filter-library-to-your-application"></a>Přidání ACS filtr knihovny do aplikace

1. V prvku zatmění Průzkumník projektu klikněte pravým tlačítkem **MyACSHelloWorld**, klikněte na **Vytvořit cestu**a potom klikněte na **Konfigurovat cesta k vytvoření**.
2. V dialogovém okně **Java sestavit cesty** klikněte na kartu **knihovny** .
3. Klikněte na **Přidat knihovny**.
4. Klikněte na **Azure přístup ovládací prvek služby filtr (podle MS otevřít Odborný)** a klikněte na tlačítko **Další**. Zobrazí se dialogové okno **Filtru služby Azure přístup ovládacího prvku** .  (Pole **umístění** může mít jiné cesty, podle toho, kam jste nainstalovali zatmění, a číslo verze může lišit v závislosti na aktualizace softwaru.)

    ![Přidat knihovnu ACS filtru][add_acs_filter_lib]

5. V prohlížeči otevřít stránku **Přihlašovací stránky integrace** portálu správy zkopírujte adresu URL uvedené v **možnost 1: odkaz na stránku hostované ACS přihlášení** polí a vložte je do pole **Koncový bod ověřování ACS** obrazovky s dialogem zatmění.
6. V prohlížeči otevřít stránku **Upravit může stran aplikaci** portálu pro správu, zkopírujte adresu URL uvedené v poli **sféry** a vložit ho do pole **Může sféry strany** obrazovky s dialogem zatmění.
7. V části **zabezpečení** zatmění dialogového okna Pokud chcete použít existující certifikát, klikněte na tlačítko **Procházet**, vyhledejte certifikát, který chcete použít, vyberte ho a klikněte na tlačítko **Otevřít**. Nebo pokud chcete vytvořit nový certifikát, klikněte na **Nový** zobrazíte dialogové okno **Nový certifikát** , zadejte heslo, název souboru .cer a název souboru .pfx nový certifikát.
8. Zaškrtněte políčko **Vložit certifikátu do souboru WAR**. Vložení certifikát tímto způsobem zahrne ji ve vašem nasazení aniž by bylo potřeba ho ručně přidat jako součást. (Pokud místo toho je třeba uložit certifikát externě ze souboru WAR, můžete přidat certifikát jako součást rolí a zrušte zaškrtnutí políčka **Embed certifikátu do souboru WAR**.)
9. [Volitelné] Zachovat **připojení vyžadují HTTPS** zaškrtnuté. Pokud tuto možnost nastavíte, musíte získat přístup k aplikaci pomocí protokol HTTPS. Pokud nechcete, aby vyžadují připojení HTTPS, zrušte zaškrtnutí políčka tuto možnost.
10. Pro nasazení na emulátoru výpočetním bude vypadat podobně jako tento nastavení **Filtru ACS Azure** .

    ![Azure nastavení filtru ACS pro nasazení na emulátoru výpočetním][add_acs_filter_lib_emulator]

11. Klikněte na **Dokončit**.
12. Klikněte na **Ano** při předání s dialogového okna pole informacemi o tom, že se vytvoří soubor web.xml.
13. Klikněte na **OK** zavřete dialogové okno **Vytvořit cesty Java** .

## <a name="deploy-to-the-compute-emulator"></a>Nasazení emulátoru výpočetních

1. V prvku zatmění Průzkumník projektu klikněte pravým tlačítkem **MyACSHelloWorld**, klikněte na **Azure**a potom klikněte na **balíček pro Azure**.
2. **Název projektu**zadejte **MyAzureACSProject** a klikněte na tlačítko **Další**.
3. Vyberte JDK a aplikační server. (Tyto kroky podrobně kurz [Vytvoření Ahoj prezentace aplikace pro Azure v zatmění](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) ).
4. Klikněte na **Dokončit**.
5. Klikněte na tlačítko **Spustit v Azure emulátoru** .
6. Po spuštění webové aplikace Java ve výpočetním emulátoru zavřete všechny instance prohlížeče (tak, že aktuální relace prohlížeče není ovlivňovat test ACS přihlášení).
7. Spusťte aplikaci otevřením <> http://localhost:8080/MyACSHelloWorld/v prohlížeči ( <nebo/https://localhost:8080/MyACSHelloWorld nebo> Pokud jste zaškrtli **připojení vyžadují HTTPS**). By měl být zobrazuje výzva k přihlášení Windows Live ID a poté je třeba se zpáteční adresa URL zadaná předávající aplikace stran.
99.  Po zobrazení aplikace klikněte na tlačítko **Obnovit emulátoru Azure** .

## <a name="deploy-to-azure"></a>Nasazení Azure

Abyste mohli nasadit Azure, musíte změnit předávající sféry stran a vraťte se adresa URL pro ACS názvů.

1. V portálu pro správu Azure, na stránce **Upravit aplikaci stran může** upravte **sféry** , třeba adresu URL webu nasazený. **Příklad** nahraďte název DNS, který jste definovali pro nasazení.

    ![Může stran sféry pro použití v výrobní][relying_party_realm_production]

2. Změňte **Vrátit adresa URL** je adresa URL aplikace. **Příklad** nahraďte název DNS, který jste definovali pro nasazení.

    ![Může stran zpáteční adresy URL pro použití v výrobní][relying_party_return_url_production]

3. Klepnutím na tlačítko **Uložit** uložte svůj aktualizovaný odpovídání sféry stran a vraťte se změny URL.
4. Nechte stránku **Integrace přihlašovací stránky** v prohlížeči otevřete, budete muset zkopírovat z něho brzy.
5. V prvku zatmění Průzkumník projektu klikněte pravým tlačítkem **MyACSHelloWorld**, klikněte na **Vytvořit cestu**a potom klikněte na **Konfigurovat cesta k vytvoření**.
6. Klikněte na kartu **knihovny** , klikněte na **Filtr služby Řízení přístupu Azure**a potom klikněte na **Upravit**.
7. V prohlížeči otevřít stránku **Přihlašovací stránky integrace** portálu správy zkopírujte adresu URL uvedené v **možnost 1: odkaz na stránku hostované ACS přihlášení** polí a vložte je do pole **Koncový bod ověřování ACS** obrazovky s dialogem zatmění.
8. V prohlížeči otevřít stránku **Upravit může stran aplikaci** portálu pro správu, zkopírujte adresu URL uvedené v poli **sféry** a vložit ho do pole **Může sféry strany** obrazovky s dialogem zatmění.
9. V části **zabezpečení** zatmění dialogového okna Pokud chcete použít existující certifikát, klikněte na tlačítko **Procházet**, vyhledejte certifikát, který chcete použít, vyberte ho a klikněte na tlačítko **Otevřít**. Nebo pokud chcete vytvořit nový certifikát, klikněte na **Nový** zobrazíte dialogové okno **Nový certifikát** , zadejte heslo, název souboru .cer a název souboru .pfx nový certifikát.
10. Zachovat **Embed certifikátu do souboru WAR** zaškrtnuté, za předpokladu, že chcete-li vložit certifikátu do souboru WAR.
11. [Volitelné] Zachovat **připojení vyžadují HTTPS** zaškrtnuté. Pokud tuto možnost nastavíte, musíte pro přístup k aplikaci využívající protokol HTTPS. Pokud nechcete, aby vyžadují připojení HTTPS, zrušte zaškrtnutí políčka tuto možnost.
12. Pro nasazení na Azure bude vypadat podobně jako tento nastavení filtru ACS Azure.

    ![Azure nastavení filtru ACS výrobní nasazení][add_acs_filter_lib_production]

13. Klikněte na tlačítko **Dokončit** zavřete dialogové okno **Upravit knihovnu** .
14. Klikněte na **OK** zavřete dialogové okno **Vlastnosti MyACSHelloWorld** .
15. V zatmění klikněte na tlačítko **publikovat do cloudu Azure** . Odpovězte na dotazy, podobné jako hotový v části **nasadit aplikaci Azure** v tématu [Vytvoření Ahoj prezentace aplikace pro Azure v zatmění](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) . 

Po nasazení webové aplikace zavřete všechny otevřené prohlížeče relace, spuštění webové aplikace a měli výzva k přihlášení přihlašovací údaje Windows Live ID, následovaný na zpáteční adresa URL aplikace předávající stran odesílaného.

Po dokončení pomocí aplikace ACS Vítáme nezapomeňte odstranit nasazení (se dozvíte, jak odstranit nasazení v tématu [Vytvoření Ahoj prezentace aplikace pro Azure v zatmění](http://msdn.microsoft.com/library/windowsazure/hh690944.aspx) ).


## <a name="next_steps"></a>Další kroky

Prozkoumání z zabezpečení vyhodnocení výrazu SAML (Markup Language) vrácené ACS aplikaci najdete v článku [jak zobrazit SAML vrácená službou Azure přístup ovládacího prvku][]. Další prozkoumejte ACS na funkce a experimentovat s složitější scénáře, najdete v článku [Access 2.0 služby ovládacího prvku][].

V tomto příkladu je také použít možnost **Vložení certifikátu do souboru WAR** . Tuto možnost, můžete snadno nasazení certifikát. Pokud místo toho chcete oddělit podpisový certifikát od WAR souboru, můžete použít následující postup:

1. V části **zabezpečení** dialog **Filtr služby Řízení přístupu Azure** zadejte $ **{Obálka. JAVA_HOME}/mycert.cer** a zrušte zaškrtnutí políčka **Embed certifikátu do souboru WAR**. (Pokud se váš soubor takto jmenuje certifikát liší, upravte Můj_certifikát.cer.) Klikněte na tlačítko **Dokončit** zavřete dialogové okno.
2. Zkopírujte certifikát jako součást aplikace nasazení: Průzkumník projektu v zatmění na rozbalte **MyAzureACSProject**, klikněte pravým tlačítkem **WorkerRole1**, klikněte na příkaz **Vlastnosti**, rozbalte **Azure Role**a klepněte na tlačítko **součásti**.
3. Klikněte na **Přidat**.
4. V dialogovém okně **Přidat Component** :
    1. V části **importovat** :
        1. Pomocí tlačítka **soubor** přejděte na certifikát, který chcete použít. 
        2. **Metoda**zvolte **Kopírovat**.
    2. **Jako název**klikněte na textové pole a nechejte výchozí název.
    3. V části **nasazení** :
        1. **Metoda**zvolte **Kopírovat**.
        2. **Do adresáře**zadejte **JAVA_HOME %**.
    4. Dialogové okno **Přidat součást** by měla vypadat podobně jako tento.

        ![Přidání certifikátu součásti][add_cert_component]

    5. Klikněte na **OK**.

V tomto okamžiku certifikátu by být součástí nasazení. Všimněte si, že bez ohledu na to, zda vložení certifikátu do souboru WAR nebo přidání jako součást k nasazení, budete muset nahrát certifikátu do svého názvů podle popisu v části [Odeslat certifikát ACS názvů][] .

[What is ACS?]: #what-is
[Concepts]: #concepts
[Prerequisites]: #pre
[Create a Java web application]: #create-java-app
[Create an ACS Namespace]: #create-namespace
[Add Identity Providers]: #add-IP
[Add a Relying Party Application]: #add-RP
[Create Rules]: #create-rules
[Nahrání certifikát ACS názvů]: #upload-certificate
[Review the Application Integration Page]: #review-app-int
[Configure Trust between ACS and Your ASP.NET Web Application]: #config-trust
[Add the ACS Filter library to your application]: #add_acs_filter_library
[Deploy to the compute emulator]: #deploy_compute_emulator
[Deploy to Azure]: #deploy_azure
[Next steps]: #next_steps
[Web projektu]: http://wastarterkit4java.codeplex.com/releases/view/61026
[Jak zobrazit SAML vrácená službou Azure přístup ovládacího prvku]: /en-us/develop/java/how-to-guides/view-saml-returned-by-acs/
[Služba řízení přístupu 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Windows Identity Foundation]: http://www.microsoft.com/download/en/details.aspx?id=17331
[Windows Identity Foundation SDK]: http://www.microsoft.com/download/en/details.aspx?id=4451
[Portál Správa Azure]: https://manage.windowsazure.com
[acs_flow]: ./media/active-directory-java-authenticate-users-access-control-eclipse/ACSFlow.png

<!-- Eclipse-specific -->
[add_acs_filter_lib]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibrary.png
[add_acs_filter_lib_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryEmulator.png
[add_acs_filter_lib_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddACSFilterLibraryProduction.png

[relying_party_realm_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmEmulator.png
[relying_party_return_url_emulator]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLEmulator.png
[relying_party_realm_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyRealmProduction.png
[relying_party_return_url_production]: ./media/active-directory-java-authenticate-users-access-control-eclipse/RelyingPartyReturnURLProduction.png
[add_cert_component]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddCertificateComponent.png
[add_jsp_file_acs]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddJSPFileACS.png
[create_acs_hello_world]: ./media/active-directory-java-authenticate-users-access-control-eclipse/CreateACSHelloWorld.png
[add_token_signing_cert]: ./media/active-directory-java-authenticate-users-access-control-eclipse/AddTokenSigningCertificate.png
 
