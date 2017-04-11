<properties
   pageTitle="Kurz: Zpracovat EDIFACT faktury pomocí služby Azure BizTalk | BizTalk služby Microsoft Azure"
   description="Postup vytvoření a konfigurace políčko spojnice nebo rozhraní API aplikace a její použití v aplikaci logiky v aplikaci služby Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="deonhe"/>

# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Kurz: Proces EDIFACT faktury BizTalk služby Azure
Portál služeb BizTalk slouží ke konfiguraci a nasazení X12 a EDIFACT smlouvami. V tomto kurzu podíváme na tom, jak vytvořit dohodu EDIFACT pro výměnu faktur mezi obchodními partnery. Tento kurz zapisuje kolem řešení pro firmy začátku do konce týkající se dvěma obchodními partnery Northwind a Contoso, které vyměňovat EDIFACT zprávy.  

## <a name="sample-based-on-this-tutorial"></a>Ukázka podle tohoto kurzu
Tento kurz zapsán kolem výběru **Odesílání EDIFACT faktur pomocí BizTalk služby**, které je k dispozici ke stažení z [Galerie MSDN kódu](http://go.microsoft.com/fwlink/?LinkId=401005). Můžete použít vzorek a procházet tento kurz pochopit, jak tvořily původní vzorku. Nebo můžete použít tento kurz vytvoření vlastních řešení nakreslenými základu. Tento kurz je určen k druhý přístup tak, že víte, jak byla vytvořena tato řešení. Také co nejvíce kurzu je v souladu s ukázkou a používá stejnou názvy pro artefakty (například schémata, transformace) jako použitých ve výběru.  

>[AZURE.NOTE] Protože tato řešení zahrnuje odeslání zprávy z konferenčního mostu EAI mostu upravit, opakované používání ukázku [konferenčního mostu služby BizTalk zřetězení vzorku](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) .  

## <a name="what-does-the-solution-do"></a>Co dělá řešení?

V tomto řešení Northwind obdrží EDIFACT faktur od společnosti Contoso. Tyto faktury nejsou ve formátu Standardní upravit. Ano před odesláním faktury Northwind, ho musí být transformovat do dokumentu EDIFACT faktury (nazývané také INVOIC). Na potvrzení musí Northwind zpracovat EDIFACT fakturu a odešle zpět řízení zprávu (nazývané také CONTRL) společnosti Contoso.

![][1]  

Dosáhnout tento scénář firmy Contoso používá funkce dostupné s Microsoft Azure BizTalk Services.

*   Contoso používá EAI mosty transformace původní fakturu EDIFACT INVOIC.

*   Most EAI do mostu odeslat upravit nasazený jako součást dohodu nakonfigurovaný na portálu služby BizTalk pošle zprávu.

*   Most odeslat upravit zpracuje EDIFACT INVOIC a přesměrovává na Northwind.

*   Po přijetí faktury, dostanou Northwind se vrátí CONTRL zprávy upravit mostu nasazený jako součást smlouvu.  

> [AZURE.NOTE] V případě potřeby tato řešení také demonstruje použití dávky odeslání faktury v dávkách místo odesílání každé faktury samostatně.  

Dokončete scénáře používáme fronty Bus služby a odešlete faktur od společnosti Contoso Northwind dostanete potvrzení od Northwind. Tyto fronty dá vytvořit klientské aplikaci, která je k dispozici ke stažení a je součástí ukázka balíčku, který je k dispozici jako součást tohoto kurzu.  

## <a name="prerequisites"></a>Zjistit předpoklady pro

*   Obor názvů Bus služby, musíte mít. Pokyny týkající se vytváření obor najdete v tématu [Postupy: vytvoření nebo úprava Namespace služby Bus služby](https://msdn.microsoft.com/library/azure/hh674478.aspx). Předpokládejme, že již máte obor služby Bus zřízení, s názvem **edifactbts**.

*   Musíte mít předplatné BizTalk služby. Pokyny najdete v tématu [Vytvoření služba BizTalk portálu Azure klasické](http://go.microsoft.com/fwlink/?LinkID=302280). Tento kurz Předpokládejme, že máte BizTalk služby předplatného s názvem **contosowabs**.

*   Zaregistrujte předplatného BizTalk služby na portálu BizTalk služby. Pokyny najdete v tématu [Registrace BizTalk nasazení služby na portálu BizTalk služby](https://msdn.microsoft.com/library/hh689837.aspx)

*   Máte nainstalované Visual Studio.

*   Musí mít nainstalovaný BizTalk Services SDK. V SDK si můžete stáhnout z [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-the-service-bus-queues"></a>Krok 1: Vytvoření fronty Bus služby  
Tato řešení používá služby Bus fronty na exchange zpráv mezi obchodními partnery. Contoso a Northwind odesílat zprávy do fronty ze kdy mosty EAI a/nebo upravit používat je. Pro toto řešení musíte tři fronty Bus služby:

*   **northwindreceive** – Northwind obdrží faktury od společnosti Contoso prostřednictvím této fronty.

*   **contosoreceive** – Contoso přijímá potvrzení od Northwind po této fronty.

*   **pozastavené** – všechny pozastavené zprávy jsou směrovány do této fronty. Zprávy se pozastaví, pokud se nepodaří během zpracování.

Vytvoření tyto služby Bus fronty pomocí klientské aplikaci součástí ukázka balíčku.  

1.  Z umístění, kam jste stáhli vzorku otevřete **Kurz odeslání faktury pomocí BizTalk služby upravit Bridges.sln**.

2.  Stisknutím klávesy **F5** k vytvoření a spuštění **Kurz klientské** aplikaci.

3.  Na obrazovce zadejte obor názvů služby Bus ACS, název vydavatele a Vystavitel klíče.

    ![][2]  
4.  Okno se zprávou vyzve, že tři fronty vytvoří v služby Bus názvů. Klikněte na **OK**.

5.  Nechte kurz klienty. Otevřít, klikněte na **Službu Bus** > **_služby Bus názvů_** > **fronty**a ověřte, že jsou vytvořeny tři fronty.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Krok 2: Vytvoření a nasazení obchodní smlouva partnera
Vytvořte obchodní smlouvu partnera mezi Contoso a Northwind. Obchodní smlouva partnera definuje smlouva trade mezi dvěma obchodních partnerů, například schéma které zprávy chcete použít, který zpráv protokol použít, atd. Obchodní smlouva partnera obsahuje dva upravit mosty, jeden k odesílání zpráv obchodním partnerům (nazývané **Upravit odeslat mostu**) a druhý přijímat zprávy od obchodní partnerů (nazývané **Upravit přijímání mostu**).

V rámci tohoto řešení mostu odeslat upravit odpovídá straně odeslat smlouvu a slouží k odeslání faktury EDIFACT z Contoso Northwind. Podobně mostu přijímání upravit odpovídá straně přijmout smlouvu a slouží k potvrzení přijímání z databáze Northwind.  

### <a name="create-the-trading-partners"></a>Vytvořit obchodní partnery

Abyste mohli začít vytvořte obchodní partnery pro Contoso a Northwind.  

1.  Na portálu služby BizTalk na kartě **partnerům** klikněte na **Přidat**.

2.  Na stránce Nový partnera zadejte **Contoso** jako partner název a klikněte na **Uložit**.

3.  Opakujte krok k vytvoření druhého partnera **Northwind**.  

### <a name="create-the-agreement"></a>Vytvoření smlouvu
Obchodní dohod partnera vzniká mezi firmy profily obchodních partnerů. Tato řešení používá výchozí partnera profily, které jsou automaticky vytvořeny v případě jsme vytvořili partnerů.  

1.  Na portálu BizTalk služby klikněte na **smlouvami** > **Přidat**.

2.  Na stránce **Obecné nastavení** novou smlouvu zadejte hodnoty, jak je znázorněno na následujícím obrázku a pak klikněte na **pokračovat**.

    ![][3]  

    Po klepnutí na tlačítko **pokračovat**, dvě karty, **Nastavení pro odesílání** a **Přijímání nastavení** k dispozici.

3.  Vytvořte smlouvu odeslat mezi Contoso a Northwind. Tato smlouva určuje, jak Contoso rozešle faktury EDIFACT Northwind.

    1.  Zaškrtněte políčko **Odeslat nastavení**.

    2.  Zachovat výchozí hodnoty na kartách **Příchozí adresy URL**, **transformace**a **Batching** .

    3.  Na kartě **protokol** v části **schémata** nahrajte schématu **EFACT_D93A_INVOIC.xsd** . Toto schéma je dostupný u ukázka balíčku.

        ![][4]  
    4.  Na kartě **Transport** zadejte podrobnosti o fronty Bus služby. Odeslat straně smlouvy používáme fronty **northwindreceive** k odeslání faktury EDIFACT Northwind a **pozastavené** fronty chcete směrovat všechny zprávy, které nezdaří během zpracování a jsou pozastavené. Jste vytvořili tyto fronty v **Krok 1: vytvoření fronty služby Bus** (v tomto tématu).

        ![][5]  

        V části **Nastavení přenosu > přenosová typ** a **Nastavení pozastavení zprávy > přenosová typ**, vyberte Bus služby Azure a obsahují hodnoty, jak je vidět na obrázku.

4.  Vytvořte přijmout smlouvu mezi Contoso a Northwind. Tato smlouva určuje, jak Contoso obdrží potvrzení z databáze Northwind.

    1.  Klikněte na **Nastavení**.

    2.  Zachovat výchozí hodnoty na kartách **Transport** a **transformace** .

    3.  Na kartě **protokol** v části **schémata** nahrajte schématu **EFACT_4.1_CONTRL.xsd** . Toto schéma je dostupný u ukázka balíčku.

    4.  Na kartě **postup** vytvoření filtru zajistit, že jsou směrovány pouze potvrzení z databáze Northwind Contoso. V části **Směrování nastavení**klikněte na tlačítko **Přidat** směrování filtr.

        ![][6]  
        1.  Zadejte hodnoty **Název pravidla**, **pravidla směrování**a **směrování cíl** znázorněná na obrázku.

        2.  Klikněte na **Uložit**.

    5.  Na kartě **směrování** znovu zadejte, kde jsou směrovány pozastavené potvrzení (potvrzení, kterým se nepodaří během zpracování). Nastavení typu transport Bus služby Azure, typ cílového směrovat **fronty**typ ověřování **Sdílené podpis aplikace Access** (přidružení zabezpečení), zadat připojovací řetězec přidružení zabezpečení pro službu Bus názvů a potom zadejte název fronty jako **pozastavené**.

5.  Nakonec klikněte na **Deploy** nasazení smlouvu. Poznámka: koncové body místo, kam odesílat a přijímat smlouvami nenasadí.

    *   Na kartě **Odeslat nastavení** v části **Příchozí adresy URL**nezapomeňte koncový bod. Odešlete zprávu od společnosti Contoso Northwind pomocí konferenčního mostu odeslat upravit, musíte odeslat zprávu o tomto koncového bodu.

    *   Na kartě **Zobrazit nastavení** v části **přenos**nezapomeňte koncový bod. Odešlete zprávu z databáze Northwind Contoso pomocí upravit přijímání most, musíte odeslat zprávu o tomto koncového bodu.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>Krok 3: Vytvoření a nasazení projektu BizTalk služby

V předchozím kroku nasazeném upravit odesílání a přijímání smlouvami zpracuje EDIFACT faktury a potvrzení. Tyto smlouvami můžete jenom zpracování zpráv, které odpovídají standardní schéma EDIFACT zprávy. Však za scénáře pro toto řešení Contoso pošle fakturu Northwind v podnikových speciální schéma. Ano před odesláním zprávy do konferenčního mostu odeslat upravit ho musí transformovat z podnikových schématu standardní schéma EDIFACT faktury. BizTalk služby EAI projektu znamená.

Projekt BizTalk služby **InvoiceProcessingBridge**, který transformací zprávy je také součástí vzorku, který jste stáhli. Projekt obsahuje následující artefakty:

*   **INHOUSEINVOICE. XSD** – schématu podnikových faktury odesílaný velkému Northwind.

*   **EFACT_D93A_INVOIC. XSD** – schématu standardní EDIFACT faktury.

*   **EFACT_4.1_CONTRL. XSD** – schématu EDIFACT potvrzení, které Northwind odesílá společnosti Contoso.

*   **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – transformaci namapuje schématu podnikových faktury na standardní schéma EDIFACT faktury.  

### <a name="create-the-biztalk-services-project"></a>Vytvoření projektu BizTalk služby
1.  Ve Visual Studiu řešení rozbalte InvoiceProcessingBridge projekt a potom otevřete soubor **MessageFlowItinerary.bcs** .

2.  Klikněte na libovolné místo na plátno a nastavte **Adresy URL služby BizTalk** v poli vlastnosti chcete-li zadat název svého předplatného služeb BizTalk. Například `https://contosowabs.biztalk.windows.net`.

    ![][7]  
3.  Z panelu nástrojů tažením plátna **Most One-Way Xml** . Nastavení vlastnosti **Název entita** a **Relativní adresu** mostu **ProcessInvoiceBridge**. Poklikejte na **ProcessInvoiceBridge** otevřete povrchu mostu konfigurace.

4.  V poli **Typy zpráv** klepněte na znaménko plus (**+**) s cílem určit schématu příchozí zprávy. Vzhledem k tomu příchozích zpráv pro most EAI vždy podnikových faktury, nastavte **INHOUSEINVOICE**.

    ![][8]  
5.  Klikněte na obrazec **Transformace Xml** a do pole vlastnosti pro vlastnost **mapy** klikněte na tlačítko se třemi tečkami (****...). V dialogovém okně **Výběr mapování** vyberte soubor transformace **INHOUSEINVOICE_to_D93AINVOIC** a klikněte na **OK**.

    ![][9]  
6.  Vraťte se do **MessageFlowItinerary.bcs**a z panelu nástrojů, přetáhněte **Koncový bod služby externí Two-Way** napravo od **ProcessInvoiceBridge**. Nastavte jeho vlastnost **Název Entity** **EDIBridge**.

7.  V okně Průzkumník rozbalte **MessageFlowItinerary.bcs** a poklikejte na soubor **EDIBridge.config** . Nahraďte obsah **EDIBridge.config** takto.

    > [AZURE.NOTE] Proč je potřeba upravit tento soubor config? Koncový bod externí služby, který je teď nově přidaná do návrháře plátna most představuje mosty upravit, které jsme nasazeny dříve. Upravit mosty jsou mezi dvěma stranami mosty s odeslat a na straně. Most EAI, který je teď nově přidaná do návrháře mostu je však most jednosměrné. Ano zpracovat vzorků exchange odlišnou zprávu ze dvou mosty, používáme chování vlastního mostu včetně jejich konfigurace v souboru config. Vlastní chování navíc zpracuje také ověřování koncový bod mostu odeslat upravit. Toto chování vlastního je k dispozici jako samostatného vzorku v [konferenčního mostu služby BizTalk zřetězení ukázka – EAI na Upravit](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Toto řešení opakované používání vzorku.  
    
    ```
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <extensions>
          <behaviorExtensions>
            <add name="BridgeAuthentication" 
                  type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
          </behaviorExtensions>
        </extensions>
        <behaviors>
          <endpointBehaviors>
            <behavior name="BridgeAuthenticationConfiguration">
              <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
              <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                    issuername="owner" 
                                    issuersecret="[YOUR ACS SECRET]" />
              <webHttp />
            </behavior>
          </endpointBehaviors>
        </behaviors>
        <bindings>
          <webHttpBinding>
            <binding name="BridgeBindingConfiguration">
              <security mode="Transport" />
            </binding>
          </webHttpBinding>
        </bindings>
        <client>
          <clear />
          <!--
            Go BizTalk Portal > Agreement > Send Settings > Inbound URL
            Copy the Endpoint URL and paste it in the below address field
          -->
          <endpoint name="TwoWayExternalServiceEndpointReference1" 
                    address="[YOUR EDI BRIDGE SEND URI]" 
                    behaviorConfiguration="BridgeAuthenticationConfiguration" 
                    binding="webHttpBinding" 
                    bindingConfiguration="BridgeBindingConfiguration" 
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />
        </client>
      </system.serviceModel>
    </configuration>

    ```
8.  Aktualizace souboru EDIBridge.config zahrnout podrobnosti o konfiguraci

    *   V části _<behaviors>_, poskytnutí ACS názvů a key přidružený k předplatnému BizTalk služby.

    *   V části _<client>_, poskytnutí koncového bodu, kde nasazení smlouva odeslat upravit.

    Uložte změny a zavřete soubor konfigurace.

9.  Z panelu nástrojů klikněte na **spojnici** a připojit se ke **ProcessInvoiceBridge** a **EDIBridge** součásti. Vyberte spojnici a do pole vlastnosti podmínku **Filtr** na **Pouze celá**. Zajistíte tím, že všechny zprávy zpracován mostu EAI jsou směrovány mostu upravit.

    ![][10]  
10.  Uložení změn řešení.  

### <a name="deploy-the-project"></a>Nasazení projektu

1.  Na počítači, které jste vytvořili BizTalk Services project stáhněte a nainstalujte certifikát SSL pro vaše předplatné BizTalk služby. Ve skupinovém rámečku BizTalk služby, klikněte na **řídicí panel**a klikněte na **Stáhnout certifikát SSL**. Poklepejte na certifikát a sledování se zobrazí výzva ke dokončit instalaci. Ujistěte se, že musíte nainstalovat certifikát v části úložiště certifikátů **Důvěryhodných kořenových certifikátů** .

2.  Ve Visual Studiu Průzkumníku klikněte pravým tlačítkem **InvoiceProcessingBridge** projektu a potom klikněte na **nasazení**.

3.  Obsahují hodnoty, jak je vidět na obrázku a potom klikněte na **nasazení**. Přihlašovací údaje ACS služby BizTalk se připojíte klepnutím **Informace o připojení** na řídicím panelu BizTalk služby.

    ![][11]  

    V podokně výstup zkopírujte koncového bodu, kde mostu EAI nasazení, například `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Tato adresa URL koncového bodu budete později potřebovat.  

## <a name="step-4-test-the-solution"></a>Krok 4: Testování řešení


V tomto tématu se podíváme na to, jak testování řešení pomocí **Kurz klientské** aplikace k dispozici v rámci výběru.  

1.  Ve Visual Studiu stisknutím klávesy F5 zahájení **Kurzu klienta**.

2.  Na obrazovce může mít hodnoty, který je předběžně z kroku tam, kde jsme vytvořili fronty Bus služby. Klikněte na tlačítko **Další**.

3.  V dalším okně poskytnout ACS přihlašovacích údajů pro služby BizTalk předplatné a koncové body kde EAI a upravit (přijmout) nasazený mosty.

    Koncový bod mostu EAI měli zkopírovali v předchozím kroku. Upravit přijímat koncový bod konferenčního mostu služby portálu BizTalk, přejděte na Smlouvu > Nastavení přijímání > Transport > koncového bodu.

    ![][12]  
4.  Do dalšího okna pod Contoso, klikněte na tlačítko **Odeslat podnikových faktury** . V souboru otevřete dialogové okno, otevřete soubor INHOUSEINVOICE.txt. Zkontrolujte obsah souboru a klikněte na tlačítko **OK** k odeslání faktury.

    ![][13]  
5.  V několik sekund, než faktury obdrženou k Northwind. Klikněte na odkaz **Zobrazit zprávu** Zobrazit fakturu přijata Northwind. Všimněte si, jak fakturu přijata Northwind ve standardní schématu EDIFACT době, kdy je odeslaný Contoso schématem podnikových.

    ![][14]  
6.  Vyberte faktury a potom klikněte na **Odeslat potvrzení**. V dialogovém okně, které se zobrazí Všimněte si, že ID výměny stejné v přijatých faktury a potvrzení odesílaného. Klikněte na OK v dialogovém okně **Odeslat potvrzení** .

    ![][15]  
7.  Ve chvíli potvrzení úspěšně obdrženou k Contoso.

    ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Krok 5 (nepovinné): odeslání EDIFACT faktury v listech 
Upravit služby BizTalk mosty podporuje také dávky odchozích zpráv. Tato funkce je užitečné pro příjem partnerů, kteří dává přednost sadu zpráv (splňují určité kritérium) místo jednotlivých zpráv.

Nejdůležitější poměr při práci s listy je aktuální verzi dávky taky se mu říká vydání kritéria. Kritéria vydání můžou být založená na způsob přijímání partnera chce přijímat zprávy. Pokud dávky aktivní, mostu upravit neodesílá odesílané zprávy přijímání partnera, dokud splněna kritéria vydání. Například dávkování kritérií na základě odeslání zprávy velikost listu pouze v případě "n" dávce zprávy. Kritérií dávky může být také založená na čase tak, že dávka se odesílá ve stejnou dobu každý den. V tomto řešení pokusíme velikost zprávy na základě kritérií.

1.  Na portálu BizTalk služby klikněte na smlouvu, kterou jste vytvořili. Klikněte na nastavení Odeslat > dávky > Přidat dávku.

2.  Název listu zadejte **InvoiceBatch**, zadat popis a klikněte na tlačítko **Další**.

3.  Určení dávku kritérii, která definuje zprávy, které musí dávce. V tomto řešení jsme dávky všech zpráv Ano vyberte použít definice možnost Upřesnit a zadejte hodnotu **1 = 1**. Toto je nějaké podmínky, které bude vždy true a proto bude dávce všech zpráv. Klikněte na tlačítko **Další**.

    ![][17]  
4.  Určete kritéria vydání dávku. V rozevíracím poli vyberte **MessageCountBased**a **počet**, zadejte **3**. To znamená, že dávka tři zprávy se pošle Northwind. Klikněte na tlačítko **Další**.

    ![][18]  
5.  Zkontrolujte souhrn a potom klikněte na **Uložit**. Klikněte na tlačítko **instalovat** na přeinstalujte smlouvu.

6.  Přejděte zpět do **Kurz klienta**, klikněte na tlačítko **Odeslat podnikových fakturu**a postupujte podle pokynů k odeslání faktury. Zjistíte, že faktura je obdrženou k Northwind, protože není splněná velikost dávky. Opakujte tento krok dvakrát, abyste měli tři faktury zprávy poslané na Northwind. To splňuje kritéria vydání dávku 3 zpráv a teď byste měli vidět faktury na Northwind.


<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

