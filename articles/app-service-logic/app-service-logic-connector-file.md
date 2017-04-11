<properties
    pageTitle="Pomocí konektoru souborů v aplikacích pro použití logických operátorů | Aplikace služby Microsoft Azure"
    description="Postup vytvoření a konfigurace soubor spojnice nebo rozhraní API aplikace a její použití v aplikaci logiky v aplikaci služby Azure"
    authors="rajeshramabathiran"
    manager="erikre"
    editor=""
    services="logic-apps"
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="rajram"/>

# <a name="get-started-with-the-file-connector-and-add-it-to-your-logic-app"></a>Začínáme s konektoru souboru a přiřadit ji někomu aplikace pro použití logických operátorů
>[AZURE.NOTE] Tuto verzi článku platí pro logiku aplikace 2014-12-01-schématu verzi.

Připojení k systému souborů chcete nahrát, stáhněte si a dalších souborů na hostitelském počítači. Lze aktivovat aplikace logiku podle různých zdrojů dat a nabídky spojnic zaujmout a zpracování dat. Konektor souboru můžete přiřadit obchodní data pracovního postupu a proces jako součást tento pracovní postup v aplikaci použití logických operátorů. 

Konektor soubor používá Connection Manager hybridní pro hybridní připojení k systému souborů Host (hostitel).

## <a name="creating-a-file-connector-for-your-logic-app"></a>Vytvoření souboru konektor aplikace použití logických operátorů ##
Použití konektoru souboru, je potřeba nejdřív vytvořit instance soubor konektor rozhraní API aplikace. Lze to takto:

1.  Otevření webu Azure Marketplace pomocí + nový možnost na levé straně portálu Azure.
2.  Vyhledejte "souboru spojnice".
3.  Ve výsledcích hledání vyberte **Soubor spojnici (verze preview)** .
4.  Klikněte na tlačítko **vytvořit**
5.  Konfigurace konektoru souborů takto:  
![][1]

    - **Název** - uveďte název souboru spojnice
    - **Nastavení balíčku**
        - **Kořenová složka** - určete cesta ke kořenové složce na hostitelském počítači. Např. D:\FileConnectorTest
        - **Služba Bus připojovací řetězec** - zadat připojovací řetězec Bus služby. Přesvědčte, že oboru bus služby je typu standardní a ne pro použití relé Bus služby základní.  Service Bus Relay se používá pro připojení k Správce připojení hybridní.
    - **Plán služeb aplikací** - vyberte nebo vytvořte plán služeb aplikací
    - **Ceny osy** – zvolte ceny osy konektoru
    - **Pole Skupina zdroje** : Vyberte nebo vytvořte novou skupinu zdroje, kde se má nacházet spojnice
    - **Předplatné** - zvolte předplatné má tato spojnice byly vytvořeny v
    - **Umístění** – zvolte místo, kam se mají spojnice tak, aby být nasazené zeměpisné polohy

4. Klikněte na na vytvořit. Vytvoří nový soubor spojnice

## <a name="configure-hybrid-connection-manager"></a>Konfigurace hybridního Connection Manager ##
Po vytvoření instance rozhraní API aplikace procházením jeho řídicího panelu.  Lze provést kliknutím na Procházet > rozhraní API aplikace > Vybrat soubor konektor rozhraní API aplikace.  Tady je třeba nakonfigurovat hybridní Connection Manager.
Další informace o konfiguraci a odstraňování potíží Connection Manager hybridní v tématu [použití hybridního Connection Manager].

## <a name="using-the-file-connector-in-your-logic-app"></a>Pomocí konektoru soubor v aplikaci použití logických operátorů ##
Po vytvoření aplikace pro rozhraní API teď můžete konektoru soubor jako akce logiky aplikace. K tomuto účelu potřebujete:

1.  Vytvoření nové aplikace logiky a zvolte stejné skupiny prostředků tato role má konektoru soubor. Postupujte podle pokynů k [Vytvoření nové aplikace logiky].

2.  Otevřete "Aktivačních událostí akcím a jejich" prostředí vytvořené aplikace logiky otevřete logiky aplikace Návrhář a konfigurace vaší toku.

3.  Konektor soubor se zobrazí v části "Rozhraní API aplikace v této skupině zdroje" v galerii na pravé straně.

4.  Soubor konektor rozhraní API aplikace můžete přetáhnout do editoru kliknutím na "soubor spojnice". soubor spojnice zpřístupňuje jednu aktivační událost a 4 akce:  
![][5]

6.  Každý z nich tyto zpřístupňuje některé vlastnosti. Na následujícím obrázku jsou uvedeny vlastnosti aktivační události a stažení souboru akce:  
![][6]

7. Po tyto konfigurace aktivační události a akce lze ve vaší toku. Podobně příkazu jiné akce je možné konfigurovat taky.

> [AZURE.NOTE] Aktivační událost soubor odstraní soubor po je úspěšně číst ze složky.

## <a name="file-connector-rest-apis"></a>Soubor je konektor rozhraní REST API ##
Používání konektoru mimo aplikaci použití logických operátorů, můžete využít REST API zveřejněné příslušným spojnice. Se může zobrazit tento rozhraní API definice pomocí procházet -> rozhraní Api aplikace -> soubor spojnice. Teď, klepněte na lens rozhraní API definice zobrazíte rozhraní API zveřejněné příslušným tato spojnice v části Souhrn:  
![][7]

Podrobnosti o rozhraní API najdete v [souboru konektor rozhraní API definice].

## <a name="do-more-with-your-connector"></a>Další možnosti s na spojnici
Teď, když se vytvoří spojnice, můžete ho přidat do pracovního postupu firmy pomocí aplikace logiky. V tématu [Jaké aplikace logiky?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Pokud chcete začít s aplikacemi jiných Azure logiky před registrací účet Azure, přejděte k [Použití logických operátorů zkuste aplikaci](https://tryappservice.azure.com/?appservice=logic), kde můžete okamžitě vytvořit aplikaci logiky krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

Zobrazte odkaz Swagger rozhraní REST API [spojnicemi a rozhraní API aplikace odkaz](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Můžete taky zkontrolovat Statistika a řízení jistoty ke spojnici připojit. V tématu [Správa a sledování předdefinované rozhraní API aplikace a spojnice](app-service-logic-monitor-your-connectors.md).

<!-- Image reference -->
[1]: ./media/app-service-logic-connector-file/img1.PNG
[5]: ./media/app-service-logic-connector-file/img5.PNG
[6]: ./media/app-service-logic-connector-file/img6.PNG
[7]: ./media/app-service-logic-connector-file/img7.PNG

<!-- Links -->
[Vytvoření nové aplikace logiky]: app-service-logic-create-a-logic-app.md
[Soubor konektor rozhraní API definice]: https://msdn.microsoft.com/library/dn936296.aspx
[Použití hybridního Connection Manager]: app-service-logic-hybrid-connection-manager.md
