<properties
    pageTitle="Poznámky k verzi pro služby Azure BizTalk | BizTalk služby Microsoft Azure"
    description="Seznam známých problémech BizTalk služby Azure" 
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="release-notes-for-azure-biztalk-services"></a>Poznámky k verzi pro služby Azure BizTalk

Poznámky k verzi pro službu Microsoft Azure BizTalk obsahovat známé problémy v této verzi.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>Co je nového v listopadu aktualizace BizTalk služeb
* Šifrování na zbývající může být užitečné v portálu BizTalk služby. V tématu [Povolení šifrování na zbývající BizTalk služby portálu](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Historie aktualizace

### <a name="october-update"></a>Aktualizace dne

* Účty organizace podporuje:  
 * **Scénář**: registrované služba BizTalk nasazení pomocí účtu Microsoft (například user@live.com). V tomto scénáři pouze Account Microsoft uživatelé můžou určit BizTalk služby na portálu BizTalk služby. Účet organizace pro nelze použít.  
 * **Scénář**: registrované služba BizTalk nasazení pod účtem organizace v Azure Active Directory (třeba user@fabrikam.com nebo user@contoso.com). V tomto scénáři pouze uživatelům Azure Active Directory ve stejné organizaci využívající spravovat službu BizTalk pomocí portálu BizTalk služby. Nelze použít s účtem Microsoft.  
* Při vytváření BizTalk služby Azure klasické portálu jsou automaticky registrované na portálu BizTalk služby.
 * **Scénář**: přihlásit na portál Azure klasické vytvořit službu BizTalk a potom vyberte **Spravovat** velmi poprvé. Po otevření portálu BizTalk služby služba BizTalk automaticky zaregistruje a je připraven k vaší nasazení.  
 Podívejte se [registrace a aktualizace nasazení BizTalk služby na portálu BizTalk služby](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>Aktualizace ze srpna 14
* Smlouva a most oddělení – obchodní partnera smlouvami a mosty jsou teď oddělené na portálu BizTalk služby. Teď vytvoříte smlouvami a mosty samostatně a za běhu mosty překládal dohodu na základě hodnot v okně Upravit. Viz [Vytvoření dohod BizTalk služby Azure](https://msdn.microsoft.com/library/azure/hh689908.aspx), [Vytvořit upravit most portálu služby BizTalk](https://msdn.microsoft.com/library/azure/dn793986.aspx), [vytvořit AS2 mostu služby portálu BizTalk](https://msdn.microsoft.com/library/azure/dn793993.aspx), a [jak mosty vyřešit dohod za běhu?](https://msdn.microsoft.com/library/azure/dn794001.aspx)  
* Možnost vytvoření šablon pro smlouvy ukončené.  
* Odeslat straně smlouvy teď můžete určit sady různých oddělovač u všech schémat. Toto nastavení je určené ve skupinovém rámečku nastavení protokolů odeslat straně smlouvy. Další informace najdete v tématu [Create an, chyby X12 smlouva BizTalk služby Azure](https://msdn.microsoft.com/library/azure/hh689847.aspx) a [Vytvoření dohodu EDIFACT BizTalk služby Azure](https://msdn.microsoft.com/library/azure/dn606267.aspx). Dva přidávání nových jednotek také k rozhraní API OM TPM k tomuto účelu. V tématu [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) a [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standardní XSD konstrukce, včetně odvozené typy nyní podporuje. V tématu [použití standardní XSD konstrukce vaší mapy](https://msdn.microsoft.com/library/azure/dn793987.aspx) a [Použití odvozené typy mapování scénáře a příklady](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 podporuje nové algoritmy Mikrofonu pro podepisování zprávy a nové algoritmy šifrování. V tématu [Vytvoření dohodu AS2 služby Azure BizTalk](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
## <a name="know-issues"></a>Vědět problémy

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Problémy s připojením po BizTalk služby aktualizace portálu

  Pokud máte portál služeb BizTalk během upgradu BizTalk služby vrátit změny ve službě otevřít, může obličej problémy s připojením pomocí portálu BizTalk služby.  
  Jako alternativu může restartujte prohlížeče, odstranění mezipaměti prohlížeče nebo spuštění portálu v soukromé režimu.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual STUDIU nemůžete najít artefakt, pokud kliknete na tlačítko chybě nebo upozornění v projektu BizTalk služby
Nainstalujte Visual Studio 2012 aktualizace 3 RC 1 vyřešit problém.  

### <a name="custom-binding-project-reference"></a>Odkaz na projekt vlastní vazba
Zvažte sebou máte projekt BizTalk služby ve Visual Studiu řešení následujících situací:  
* Ve stejném řešení Visual Studio je BizTalk služby projektu a vlastní vazba projektu. Odkaz na daném souboru projektu vlastní vazba má služba BizTalk projekt.
* Odkaz na vlastní vazby/chování DLL má projekt BizTalk služby.

"Vytvoříte" řešení ve Visual Studiu úspěšně. Potom "znovu nebo vymazání řešení. Po, když znovu vytvořit nebo vyčistit znovu následující chyba:  
  Nelze zkopírovat soubor <Path to DLL> na "bin\Debug\FileName.dll". Proces nejde přístup k souboru "bin\Debug\FileName.dll", protože ho používá jiný proces.  

#### <a name="workaround"></a>Řešení:
* Pokud je nainstalovaná [aplikace Visual Studio 2012 aktualizace 3](https://www.microsoft.com/download/details.aspx?id=39305) , máte následujících dvou možností:

  * Restartujte aplikaci Visual Studio nebo

  * Restartujte řešení. Proveďte pouze sestavení na řešení.  

* Bez nainstalovaného [Visual Studio 2012 aktualizace 3](https://www.microsoft.com/download/details.aspx?id=39305) otevřete Správce úloh, klikněte na kartě procesy, MSBuild.exe proces kliknutím na a potom klikněte na tlačítko Ukončit proces.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Směrování koncové body BasicHttpRelay není podporovaný z mosty a portál služeb BizTalk netisknutelné znaky jsou upřednostněné jako záhlaví HTTP

Pokud používáte netisknutelné znaky jako součást propagovaných vlastností pro zprávy, nemůžete relay cílů, které použít vazbu BasicHttpRelay směrovány tyto zprávy. Navíc propagovaných vlastností, které jsou k dispozici jsou součástí sledování kódovaný jako adresa URL pro objekty BLOB a rušení zakódovaný pro cíle.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>MDN odeslaný asynchronní, i když není zaškrtnutá možnost odesílat asynchronní MDN  
Zvažte tento scénář – Pokud zaškrtnete políčko **Odeslat asynchronní MDN** a, zadejte adresu URL pro odeslání asynchronní MDN a potom zrušte zaškrtnutí políčka **Odeslat asynchronní MDN** ještě jednou MDN pořád odeslaný do zadané adrese URL i když není vybraná možnost odeslat asynchronní MDNs.  
Jako alternativu musíte před zrušením zaškrtnutí políčka **Odeslat asynchronní MDN** vymazat zadané adrese URL a nasazení smlouva AS2.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Prázdné znaky za platné výměny vyvolat zobrazení prázdné zprávy zasílané do koncový bod pozastavení  
Pokud existují prázdné znaky za IEA segmentu, disassembler považován za konec aktuální výměny a sleduje následujícího prázdné znaky jako další zprávu. Protože to není platný výměny, může sledovat jednu úspěšného zpráva se pošle směrovat cíle a jeden prázdné zprávy koncový bod pozastavení.  
### <a name="tracking-in-biztalk-services-portal"></a>Sledování BizTalk služby portálu  
Sledování událostí ukládány až upravit zpracování zpráv a žádné korelace. Pokud zpráva nepovede mimo fázi Protocol (protokol), sledování se zobrazí jako úspěšná. V takovém případě naleznete v části LOG ve sloupci **Podrobnosti** v **Sledování** podrobnosti chyby.
X12 přijímání a odesílání nastavení ([Create an, chyby X12 smlouva BizTalk služby Azure](https://msdn.microsoft.com/library/azure/hh689847.aspx)) zadejte informace v oblasti protokol.  

### <a name="update-agreement"></a>Aktualizace smlouva  
Portál služeb BizTalk umožňuje upravit kvalifikátor identitu při dohodu nakonfigurovaný. Může být výsledkem inconsistence vlastnosti. Je třeba dohodu pomocí ZZ:1234567 a ZZ:7654321 kvalifikátor. V nastavení profilu portál služeb BizTalk změníte ZZ:1234567 být 01:ChangedValue. Otevřete smlouvu a zobrazí se místo ZZ:1234567 01:ChangedValue.
Chcete-li změnit kvalifikátor identitu, odstraňte smlouvu, aktualizovat **identit** v profilu partnera a znovu vytvořte smlouvu.  
> AZURE. Upozornění: Toto chování ovlivní X12 a AS2.  

### <a name="as2-attachments"></a>AS2 přílohy  
Přílohy pro AS2 zpráv nejsou podporované v posílat a dostávat. Konkrétně tiše ignorovány přílohy a obsah zprávy zpracován jako běžný AS2 zprávu.  
### <a name="resources-remembering-path"></a>Zdroje: Pamatovat cesta  
Při přidávání **zdrojů**, dialogové okno nemusí mějte na paměti cesty dříve používané k přidání zdroje. Pamatuje cestu dřív používali, zkuste přidat web služby portálu BizTalk mezi **Důvěryhodné servery** v Internet Exploreru.  
### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Pokud přejmenujete název entity most a zavřete projektu bez uložení změn, jeho následném otevření entitu má za následek chybu
Zvažte situace v následujícím pořadí:  
* Přidání most (například XML One-Way konferenčního mostu) služba BizTalk projektu  

* Přejmenování mostu zadáním hodnoty pro vlastnost název Entity. To přejmenuje souboru přidružená .bridgeconfig nahraďte názvem, který jste zadali.  

* Zavřete soubor .bcs (zavřením kartě ve Visual Studiu) bez uložení změn.  

* Znovu otevřete soubor .bcs z Průzkumníka řešení.  
Zjistíte, že během souboru přidružená .bridgeconfig má nový název, který jste zadali, název subjektu na návrhovou plochu je pořád starými názvy. Pokud se pokusíte otevřít konfiguraci mostu poklepáním component mostu, se zobrazí chybová zpráva:  
  "<old name>'Entity je přidružený soubor'<old name>.bridgeconfig" neexistuje.  
Aby se zabránilo spuštění do tento scénář, zkontrolujte, že po přejmenování entity v projektu služba BizTalk uložit změny.  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>Služba BizTalk projektu vytvoří úspěšně i v případě artefaktem byla vyloučené ze projekt aplikace Visual Studio
Představte místo, kam přidat artefaktem (například soubor XSD) do služby BizTalk projektu, zahrnout tuto artefakt konfiguraci mostu (například zadáním psaní zprávy žádost) a potom vyloučit z projekt aplikace Visual Studio. V takovém případě vytváření projektu nedávají všechny chyby, dokud odstraněné artefakt na disku ve stejném umístění na místo, kam jste obdrželi v projektu Visual Studio.
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>Projekt BizTalk služby nekontroluje dostupnost schématu při konfiguraci mostu
V služba BizTalk projektu Pokud schéma, které se přidá do projektu importuje jiné schéma projekt BizTalk služby nekontroluje zda importovaného schématu se přidá do projektu. Pokud se pokusíte vytvořit tohoto projektu, nebudete mít chyby Tvůrce dotazů.
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>Zprávu s odpovědí most požadavek odpověď XML je vždy znaková sada UTF-8
V této verzi produktu znakovou odpovědi z mostu požadavek odpověď XML nastavenou vždy UTF-8.
### <a name="user-defined-datatypes"></a>Uživatelem definované datové typy
Adaptéry BizTalk adaptér Pack v rámci funkce služby adaptér BizTalk můžete využít uživatelem definovaných datové typy pro operace adaptér.
Při použití datové typy definované uživatelem, zkopírujte (DLL) jednotka: \Program Files\Microsoft BizTalk adaptér Service\BAServiceRuntime\bin\ nebo k globální mezipaměti sestavení (GAC) na serveru hostingu služba BizTalk adaptér. Tato chyba jinak, může dojít v klientském počítači:  
```<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <faultcode>s:Client</faultcode>
  <faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
  <detail>
    <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
    </AFConnectRuntimeFault>
  </detail>
</s:Fault> ```  
> [AZURE.IMPORTANT] Doporučujeme používat GACUtil.exe pro instalaci souboru do globální mezipaměti sestavení. GACUtil.exe dokumenty, jak používat tento nástroj a parametry příkazového řádku Visual Studio.  

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>Restartování webu BizTalk adaptér služby
Nainstalujte modul **Runtime služby adaptér BizTalk** * vytvoří * *Služba adaptér BizTalk* * Web ve službě IIS, která obsahuje * *BAService* * aplikace.* *BAService** aplikace interně používá předávací vazby rozšiřte reach koncový bod služby místní do cloudu. Služby hostované místním systémem odpovídající koncový bod relay registrována na Bus služby pouze při spuštění služby místní.  

Pokud ukončení a spuštění aplikace, není dodržet konfigurace pro automatické spuštění aplikace. Takže po zastavení **BAService** vždy po restartování webu **BizTalk adaptér služby** místo. Zahájení nebo ukončení aplikace **BAService** .
### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>U adresy a entity názvy součástí LOB neměly používat speciální znaky
Pro názvy adresa a entity součástí LOB – byste neměli používat speciální znaky. Pokud to bude dojde k chybě při nasazení projektu BizTalk služby. Pro některé znaky like "%" na webu služby adaptér BizTalk může přejděte do přestal stavu a budete muset ručně spustit.
### <a name="test-map-with-get-context-property"></a>Testování mapování vlastnost kontextu Get
Obsahuje-li transformace operace mapy **Získat vlastnost kontextu** , se nepovede **Test mapa** . Jako dočasné řešení nahraďte operaci mapy **Získat vlastnost kontextu** řetězec Concatenate mapy operace obsahující formální data. Tím naplnění cílového schématu a umožňují že testování další transformace funkce.
### <a name="test-map-property-does-not-display"></a>Test mapovat vlastnost nezobrazí.
Vlastnosti **Test mapy** se nezobrazují ve Visual Studiu. K tomu může dojít, pokud nejsou současně ukotvený okno **Vlastnosti** a okna **Průzkumník řešení** . Tento problém vyřešíte ukotvěte **Vlastnosti** a **Řešení Průzkumníka** windows.  
### <a name="datetime-reformat-drop-down-is-grayed-out"></a>Změnit formát data a času rozevíracího seznamu se zobrazuje šedě
Když operace mapování změnit formát data a času se přidá na návrhovou plochu a nakonfigurovali, může zobrazit zašedle rozevírací seznam formátů. To může dojít, pokud je počítač zobrazení nastaven **Střední – 125 %** nebo **větší – 150 %**. Vyřešíte nastavte displej **menší – 100 % (výchozí)** provedením následujících kroků:  
1. Otevřete **Ovládací panely** a klepněte na položku **Vzhled a přizpůsobení**.
2. Klikněte na **Zobrazit**.
3. Klikněte na **menší – 100 % (výchozí)** a klikněte na **použít**.

Rozevírací seznam **formátů** by nyní očekávaným způsobem.
### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Duplicitní dohod na portálu BizTalk služby
Zvažte následující situaci:
1. Vytvoření dohodu pomocí rozhraní API OM správy partnera obchodování.
2. Otevřete smlouvu v portálu BizTalk služby ve dvou různých karet.
3. Nasazení smlouva z obou karet.
4. V důsledku toho obou dohod nenasadí následek duplicitní položky v portálu BizTalk služby

**Alternativní řešení**. Otevřete libovolnou barevnou duplicitní dohod na portálu služby BizTalk a zrušit nasazení.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Mosty nepoužívejte aktualizované certifikát i po certifikát byl aktualizovaný v úložišti artefakt
Zvažte tyto scénáře:  

**Scénář 1: Použití na základě Miniatura certifikátů k zabezpečení přenosu zprávy z most koncový bod služby**  
Zvažte situace kterém používat certifikáty založené na miniaturu v projektu BizTalk služby. Aktualizovat certifikát na portálu služby BizTalk se stejným názvem, ale různých Miniatura, ale není projekt BizTalk služby příslušně aktualizovat. V tomto případě může dál mostu zpracování zpráv, protože starší certifikát data může být v mezipaměti kanálu. Až to zpracování zprávy se nezdaří.  

**Alternativní řešení**: certifikát v aplikaci project služba BizTalk aktualizujte a přeinstalujte projektu.  

**Scénář 2: Použití chování řízené k identifikaci certifikátů k zabezpečení přenosu zprávy z most koncový bod služby**

Zvažte situace kde používáte název založen chování k identifikaci certifikáty v projektu BizTalk služby. Aktualizovat certifikát na portálu služby BizTalk však neaktualizují projekt BizTalk služby příslušným způsobem. V tomto případě může dál mostu zpracování zpráv, protože starší certifikát data může být v mezipaměti kanálu. Až to zpracování zprávy se nezdaří.  

**Alternativní řešení**: certifikát v aplikaci project služba BizTalk aktualizujte a přeinstalujte projektu.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Mosty pokračovat ve zpracování zpráv, i když máte databázi SQL v režimu offline
Mosty BizTalk služby pokračovat ve zpracování zprávy určitou dobu, i když databázi Microsoft Azure SQL (který ukládá pracovního informace, například nasazeném artefakty a potrubí), je offline. Je to proto přihlašovacích údajů BizTalk používá režim cached artefakty a konfiguraci mostu.
Pokud nechcete, aby mosty ke zpracování všech zpráv při offline databáze SQL, můžete zastavit nebo pozastavit službu BizTalk rutin prostředí PowerShell BizTalk služby. V tématu [Azure BizTalk služby správy vzorku](http://go.microsoft.com/fwlink/p/?LinkID=329019) pro rutiny prostředí Windows PowerShell ke správě operace.  
### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Přečtení zprávy XML v mostu vlastního kódu součásti obsahuje další znak Kusovníku
Zvažte scénáři, ve které chcete číst zprávu XML konferenčního mostu vlastního kódu. Pokud používáte System.Text.Encoding.UTF8.GetString(bytes) rozhraní API .NET přebytečné Kusovníku je součástí výstupu na začátku zprávy. Ano, pokud nechcete, aby výstup navíc Kusovníku znak, musíte použít ```System.IO.StreamReader().ReadToEnd()```.
### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Posílání zpráv do most pomocí WCF není adekvátní
Zprávy zasílané do most pomocí WCF měřítko. Měli byste místo toho použít HttpWebRequest požadovaná scalable klienta.
### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>UPGRADE: Token poskytovatele chyby po upgradu z BizTalk služby náhledu na obecný dostupnosti (GA)
Existuje upravit nebo AS2 smlouvu s aktivní listy. Když služba BizTalk z náhledu upgradovaný na verzi GA, může dojít takto:
* Chyba: Poskytovatel tokenu nepodařilo poskytují tokenu zabezpečení. Poskytovatel tokenu vrátila zprávu: název vzdáleného nelze přeložit.

* Dávkové úkoly jsou zrušeny.

**Alternativní řešení**: po službu BizTalk se aktualizuje na obecný dostupnosti (GA), přeinstalujte smlouvu.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>UPGRADE: Nástrojů zobrazí staré ikony mostu po upgradu BizTalk Services SDK
Upgrade dřívější verzi BizTalk služby SDK, která vám to staré ikony představující mosty, panelu nástrojů zůstane po zobrazení staré ikony mosty. Však most přidáte do návrhové ploše služba BizTalk projektu, povrch zobrazuje nové ikony.  

**Alternativní řešení**. Tento problém můžete vyřešit tak, že odstraňování souborů .tbd v části <system drive>: \Users\<uživatele > \AppData\Local\Microsoft\VisualStudio\11.0.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>UPGRADE: Aktualizace BizTalk portál z náhledu GA může zobrazit chyba označující, že možnost Upravit není k dispozici
Pokud jste přihlášení k portálu BizTalk služby a služby BizTalk z náhledu upgradovaný na verzi GA, se může zobrazit tato chyba na portálu:  

Tato možnost není k dispozici v rámci tuto verzi sady Microsoft Azure BizTalk Services. Pokud chcete použít tyto funkce přepněte do příslušné edition.  

**Řešení**: odhlášení z portálu, zavřít a otevřít v prohlížeči a potom Přihlaste se k portálu.  
### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>UPGRADE: Nová data sledování nezobrazuje po upgradu služeb BizTalk na GA
Předpokládejme situaci, kde máte mostu XML používaný BizTalk služby náhled předplatného. Odeslání zprávy do mostu a odpovídající data sledování je dostupná na portálu BizTalk služby. Teď když runtime bitů portál BizTalk služby a služby BizTalk jsou aktualizovali GA a odeslat zprávu o stejný koncový bod mostu nasazený dříve, data sledování nezobrazuje u zpráv odeslaných po upgradu.  

### <a name="pipelines-vs-bridges"></a>Mosty v/s potrubí
V tomto dokumentu termín "potrubí" a "mosty" slouží jako synonyma. Obě nechtěli v podstatě totéž, což je jednotka zpracování zprávy nasazený na BizTalk služby.  

### <a name="concepts"></a>Koncepty  

[BizTalk služby](https://msdn.microsoft.com/library/azure/hh689864.aspx)   
