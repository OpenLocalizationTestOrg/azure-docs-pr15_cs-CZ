<properties 
    pageTitle="Jak používat Twilio pro hlasové zprávy a zprávy SMS (skutečné) | Microsoft Azure" 
    description="Zjistěte, jak chcete někomu zavolat a odeslání zprávy SMS ke službě rozhraní API Twilio na Azure. Ukázky napsané v skutečné." 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Jak používat Twilio pro hlasovou a možnosti služby SMS skutečné
Tato příručka ukazuje, jak provádět běžné úlohy programování pomocí rozhraní API Twilio služby Azure. Scénáře nichž se uplatní zahrnují uskutečnit telefonní hovor a posílání zpráv služby krátké zprávy (SMS). Další informace o Twilio a používání hlasové zprávy a zprávy SMS v aplikacích naleznete v části [Další kroky](#NextSteps) .

## <a id="WhatIs"></a>Co je Twilio?
Twilio je rozhraní API telefonních webové služby, které můžete použít existující web jazyky a dovedností k vytvoření hlasu a aplikací služby SMS. Twilio je služba třetích stran (ne Azure funkce a ne k produktu Microsoft).

**Twilio hlasu** umožňuje aplikace uskutečňovat a přijímat telefonní hovory. **Služby SMS Twilio** umožňuje aplikace uskutečňovat a přijímat zprávy SMS. Umožní **Twilio klient** aplikace povolit hlasové komunikace pomocí existujícího připojení k Internetu, včetně mobilního připojení.

## <a id="Pricing"></a>Twilio ceny a speciální nabídky
Informace o cenách Twilio je k dispozici [Cena za Twilio] [twilio_pricing]. Azure zákazníci obdrží [zvláštní nabídkou][special_offer]: bezplatné platební 1000 texty nebo 1000 příchozí minut. Registrace k této nabídce nebo získání dalších informací, navštivte [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Koncepty
Rozhraní API Twilio je rozhraní API RESTful poskytující hlasové zprávy a funkce služby SMS pro aplikace. Jsou dostupné ve více jazycích; knihoven klienta seznam najdete v tématu [Twilio rozhraní API knihovny] [twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML je sada založeném na jazyce XML pokynů informovat Twilio způsobu zpracování hovoru nebo služby SMS.

Jako příklad následující TwiML by převést **Vítáme** na řeč.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Všechny dokumenty TwiML mají `<Response>` jako jejich kořenovém prvku. V ní pomocí Twilio slovesa definovat chování aplikace.

### <a id="Verbs"></a>TwiML akce
Twilio slovesa jsou značky XML, které určuje, Twilio co **udělat**. Například ** &lt;Přivítejte&gt; ** slovesné pokyn Twilio zvukově prezentování zprávu na volání. 

Následuje seznam Twilio akcí.

* ** &lt;Dial&gt;**: připojí volající na jiný telefon.
* ** &lt;Shromáždění&gt;**: shromažďuje číslicemi zadaná na klávesnici telefonu.
* ** &lt;Zavěšení&gt;**: ukončí hovor.
* ** &lt;Přehrát&gt;**: přehrávání zvukového souboru.
* ** &lt;Pozastavit&gt;**: tiše čeká určitý počet sekund.
* ** &lt;Záznam&gt;**: zaznamenává volajícího hlasu a vrátí URL soubor, který obsahuje záznam.
* ** &lt;Přesměrovat&gt;**: převede ovládací prvek volání nebo služby SMS TwiML na jinou adresu URL.
* ** &lt;Odmítnout&gt;**: odmítne příchozí volání na číslo Twilio bez fakturace můžete
* ** &lt;Přivítejte&gt;**: převede text na řeč, která je vytvořená Telefonuji.
* ** &lt;Služby Sms&gt;**: odešle zprávu služby SMS.

Další informace o Twilio akcí, jejich atributy a TwiML najdete v tématu [TwiML] [twiml]. Další informace o rozhraní API Twilio, najdete v článku [Rozhraní API Twilio] [twilio_api].

## <a id="CreateAccount"></a>Vytvoření účtu Twilio
Až budete chtít zřízení účtu Twilio, přihlásit na [Akci Twilio] [try_twilio]. Můžete začít s bezplatného účtu a později upgradovat, váš účet.

Při vaší registraci Twilio účtu, zobrazí se bezplatné telefonní číslo aplikace. Zobrazí se také účet ID zabezpečení a auth token. Jak je potřeba volat rozhraní API Twilio. Abyste zabránili neoprávněnému přístupu ke svému účtu, aby byly v ověřovací token bezpečí. Váš účet ID zabezpečení a auth token je možné zobrazit na [stránce účtu Twilio][twilio_account]v polích s popisky **účet ID zabezpečení** a **AUTH TOKENU**, respektive.

### <a id="VerifyPhoneNumbers"></a>Ověření telefonních čísel
Kromě počet je dán Twilio, můžete taky ověřit, čísla (tedy vašeho mobilního telefonu nebo z domova telefonní čísla) řízení pro použití v aplikacích. 

Informace o tom, jak ověřit telefonního čísla, najdete v článku [Správa čísel] [verify_phone].

## <a id="create_app"></a>Vytvoření skutečné aplikace
Skutečné aplikace, která používá službu Twilio a běží v Azure neliší od jiných skutečné aplikace, která používá službu Twilio. Během Twilio services jsou RESTful a od skutečné můžete volat několika způsoby, tento článek se zaměřuje na používání služeb Twilio s [knihovnou Twilio Pomocníka pro skutečné][twilio_ruby].

První [Instalace sestavení nové OM Linux Azure] [ azure_vm_setup] sloužit jako hostitel pro nové skutečné webové aplikace. Ignorujte kroky týkající se vytváření profilů aplikace jenom nastavuje OM. Zkontrolujte, že vytvoření koncového bodu s externím portu 80 a port pro interní 5000.

V následujících příkladech použijeme [Sinatra][sinatra], velmi jednoduché webové rozhraní pro skutečné. Ale určitě můžete použít knihovnu Twilio Pomocníka pro skutečné s jakékoli jiné web framework včetně skutečné na profilů.

SSH do nového OM a vytvořit adresář pro novou aplikaci. V adresáři vytvořte soubor s názvem Gemfile a zkopírujte následující kód do něho:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Na příkazovém řádku spusťte `bundle install`. Nainstaluje závislosti výše. Dále vytvořte soubor s názvem `web.rb`. To bude, kde jsou umístěná kód pro web app. Do něj vložte tento kód:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

Teď byste měli být schopní spuštění příkazu `ruby web.rb -p 5000`. To bude číselník nakreslenými malé webový server na portu 5000. Je třeba moct vyhledat pro tuto aplikaci v prohlížeči navštivte adresu URL se nastavuje OM Azure. Až dojdete webovou aplikaci v prohlížeči, jste připraveni začít vytvářet Twilio aplikace.

## <a id="configure_app"></a>Konfigurace aplikace pomocí Twilio
Můžete nakonfigurovat webovou aplikaci používat knihovnu Twilio aktualizací vaší `Gemfile` zahrnout tento řádek:

    gem 'twilio-ruby'

Na příkazovém řádku spusťte `bundle install`. Nyní otevřete `web.rb` včetně tento řádek nahoře:

    require 'twilio-ruby'

Máte teď všechno nastavené pro účely pomocné knihovny Twilio skutečné ve web appu.

## <a id="howto_make_call"></a>Postup: odchozí hovor
Na následujícím obrázku je vytvoření odchozího hovoru. Klíčové pojmy zahrnout používat knihovnu Twilio Pomocníka pro skutečné pro volání rozhraní REST API a vykreslování TwiML. Nahradit hodnoty **od** a **do** telefonní čísla a zrušte zaškrtnutí ověření telefonní číslo **z** účtu Twilio před spuštěním kód.

Přidání této funkce můžete `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
      
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
      
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end
    
Pokud jste otevřít nakreslenými `http://yourdomain.cloudapp.net/make_call` v prohlížeči, který spustí volání k rozhraní API Twilio telefonní hovor. První dvě parametrů v `client.account.calls.create` jsou velmi zřejmé: číslo volání je `from` a číslo hovor `to`. 

Třetí parametr (`url`) je adresa URL, která vyžádá Twilio zobrazíte pokyny, jak postupovat, jakmile se hovor spojí. V tomto případě jsme nastavuje adresu URL (`http://yourdomain.cloudapp.net`), který vrátí simple TwiML dokumentu a používá `<Say>` příkaz pro proveďte některé převod textu na řeč a řekněte "Ahoj opice" osobě, která hovor přijímá.

## <a id="howto_recieve_sms"></a>Postup: Receive zprávy SMS
V předchozím příkladu jsme, které iniciuje **odchozí** telefonní hovor. Tento čas, použijeme telefonní číslo, které Twilio uvedli během registrace zpracuje **příchozí** zprávy SMS.

První, přihlaste se do [řídicího panelu Twilio][twilio_account]. Klikněte na "Čísla" v horním navigačním podokně a potom klepněte na číslo Twilio, které jste uvedli. Zobrazí se dvě adresy URL, které můžete konfigurovat. Adresa URL požadavku na požadavek URL hlasu a služby SMS. Toto jsou adresy URL, která volá Twilio kdykoli zavolat je volání nebo služby SMS odeslaný do vaše číslo. Adresy URL se označuje také jako "web zavěšení".

Rádi zpracovat příchozí zprávy SMS, tak se Pojďme aktualizovat adresu URL `http://yourdomain.cloudapp.net/sms_url`. Pojďte dále a klikněte na Uložit změny v dolní části stránky. Teď se změnami `web.rb` Pojďme program naše aplikace na to:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Po provedení změn, zkontrolujte, že opětovné spuštění aplikace pro web. Teď uzavře telefonu a odešlete zprávu SMS Twilio číslo. Neprodleně by měla získat služby SMS odpověď, která říká "Hey, Děkujeme za příkazem ping! Twilio a Azure rock! ".

## <a id="additional_services"></a>Postup: pomocí dalších Twilio služby
Kromě příklady ukazuje tato část nabízí Twilio rozhraní API webových, která můžete využít další funkce Twilio Azure aplikace. Úplné podrobnosti najdete v [dokumentaci k rozhraní API Twilio] [twilio_api_documentation].

### <a id="NextSteps"></a>Další kroky
Teď, když jste se naučili základní informace o službě Twilio, tyto odkazy vedou na další informace:

* [Pokyny pro Twilio zabezpečení] [twilio_security_guidelines]
* [Twilio HowTos a příklady kódu] [twilio_howtos]
* [Výukové programy pro rychlý úvod Twilio][twilio_quickstarts] 
* [Twilio na GitHub] [twilio_on_github]
* [Obraťte se na podporu Twilio] [twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
