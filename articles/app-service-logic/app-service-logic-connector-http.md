<properties
   pageTitle="Použití posluchače HTTP a spojnice v aplikacích pro použití logických operátorů | Aplikace služby Microsoft Azure "
   description="Postup vytvoření a konfigurace posluchače HTTP a HTTP akce spojnice nebo rozhraní API aplikace a její použití v aplikaci logiky v aplikaci služby Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/31/2016"
   ms.author="prkumar"/>


# <a name="get-started-with-the-http-listener-and-http-action-and-add-it-to-your-logic-app"></a>Začínáme s posluchače HTTP a HTTP akcí a přiřadit ji někomu aplikace pro použití logických operátorů

> [AZURE.NOTE]Vzhledem k tomu jeho funkce nyní obsažen ve výchozím nastavení jako **Ruční spuštění** při vytváření nové aplikace použití logických operátorů jsme jsou končí podpora tato spojnice.  Doporučujeme upgradovat všech svých aplikací logiku, kteří používají tento spojnice.  
> Tuto verzi článku platí pro použití logických operátorů aplikace 2014 12 01 náhled schématu verze.

Přímé připojení k HTTP zdroje můžete poslouchat požadavků HTTP a nakonfigurovat HTTP webové požadavky. Existují některé scénáře, kdy budete muset pracovat s přímé připojení HTTP, včetně:

1.  Se dají logiky aplikace, která podporuje webu nebo mobilní uživatel interaktivní front-end.
2.  Získání a zpracování dat z webové služby, které nemá nepřítomnosti políčko spojnice.
3.  Chcete-li provést akce, které jsou již vystaveného jako webové služby, ale není k dispozici jako rozhraní API aplikace.

Pro podobnému sledu dvěma způsoby:

1. **Nastavit informace HTTP posluchače**: Tato spojnice slouží jako aktivační událost a přijímá žádosti o HTTP na nakonfigurováno koncového bodu. Po přijetí hovoru na nakonfigurováno koncový bod spustí novou instanci tok a předá data dostali v žádosti o řízení toku zpracování. Je také je možné konfigurovat automaticky odpovědět příchozí žádost, když má tok zahájením nebo umožňují vytvářet odpověď podle spuštění toku a odpověď odesílala volající.
2. **Nastavit informace HTTP akce**: to je možné konfigurovat žádost web všechny koncový bod k dispozici na Internetu načte data zpět odpověď a díky kterému je dostupný pro další akce v toku využívat.

Použití logických operátorů aplikace můžete aktivovat podle různých zdrojů dat a nabídky spojnic zaujmout a zpracování dat v rámci tok. Přidání konektoru HTTP pro obchodní data pracovního postupu a proces jako součást tento pracovní postup v aplikaci použití logických operátorů. 

## <a name="creating-an-http-listener-for-your-logic-app"></a>Vytváření posluchače HTTP aplikace logiky
Spojnice lze vytvořit v aplikaci logiky nebo vytvořit přímo z webu Azure Marketplace. Vytvoření spojnice z Marketplace:  

1. V Azure startboard vyberte **Marketplace**.
2. Vyhledejte "HTTP", vyberte posluchače HTTP a vyberte **vytvořit**.
3.  Konfigurace posluchače HTTP následujícím způsobem:  
![][1]

4.  Při nastavování nastavení balíčku, zobrazí se následující možnosti na tom, jestli má posluchače automaticky odpovědět nebo vyžadují explicitní odpověď. Nastavte **hodnotu False** odešlete svoji vlastní odpověď:  
![][2]

5.  Vytvoříte kliknutím na **OK** .
6.  Po vytvoření instance aplikace rozhraní API otevřete nastavení a konfigurace zabezpečení. Nastavit informace HTTP posluchače v současné době podporuje základní ověřování. Můžete nakonfigurovat to pomocí možnosti zabezpečení při otevření HTTP posluchače:  
![][3]
  
    **Známé problémy** Nastavení *zabezpečení zobrazit "Žádné" Výchozí hodnota však je definován. Nastavení musí změnit Basic a zpět žádná před uložením zajistit, že posluchače HTTP správně nakonfigurované.*  

7. Nakonec nastavení zabezpečení rozhraní API aplikace veřejnosti (anonymní) Pokud chcete povolit externí klientům umožnit přístup k koncový bod. Toto nastavení je k dispozici v části "všechna nastavení > Nastavení aplikace" HTTP posluchače rozhraní API aplikace:![][10]

Po dokončení, teď můžete vytvářet logiky aplikaci používat posluchače HTTP.

## <a name="using-the-http-listener-in-your-logic-app"></a>Použití posluchače HTTP v aplikaci použití logických operátorů
Po vytvoření aplikace pro rozhraní API teď můžete posluchače HTTP jako spouštěč logiky aplikace. K tomuto účelu potřebujete:

4.  Vytvoření nové aplikace logiku.
5.  Otevřete "aktivačních událostí akcím a jejich" otevřete návrháře logiky aplikace a konfigurace vaší toku. Nastavit informace HTTP posluchače je uvedený v galerii. Vyberte ho.
6.  Nyní je možné nastavit metodu HTTP a relativní adresu URL, na které požadujete posluchače aktivace tok:  
![][4]  
![][5]

7.  Získáte dokončení URI dvojitým kliknutím na HTTP posluchače zobrazit její konfiguraci a zkopírujte adresu URL pro "Host" rozhraní API aplikace:  
![][6]
8.  Teď můžete použít tato data obdrželi žádost HTTP v jiné akce v toku následujícím způsobem:  
![][7]  
![][8]
9.  Nakonec Neodesílat odpověď, přidat další posluchače HTTP a vyberte akci, poslat odpověď HTTP. Nastavte ID požadavku na ID žádosti získané posluchače HTTP a naplnění text odpovědi a HTTP stav, který chcete vrátit zpět:  
![][9]

## <a name="using-the-http-action"></a>Použití akce HTTP
Akce HTTP je podporován aplikací logiky a nevyžaduje rozhraní API aplikace první, nebudou moct používat ho vytvořit. Můžete vložit akce HTTP kdykoli v logických aplikace a zvolte URI, záhlaví a textu pro volání.
Akce HTTP podporuje více možností zabezpečení straně klienta. Zobrazí [Možnosti zabezpečení straně klienta](../scheduler/scheduler-outbound-authentication.md).

Výstup akce HTTP je záhlaví a textu, které lze použít další proudu v toku podobný jak spotřebované výstup další akce a spojnic.

## <a name="do-more-with-your-connector"></a>Další možnosti s na spojnici
Teď, když se vytvoří spojnice, můžete ho přidat do pracovního postupu firmy pomocí aplikace logiku. V tématu [Jaké aplikace logiku?](app-service-logic-what-are-logic-apps.md).

Zobrazte odkaz Swagger rozhraní REST API [spojnicemi a rozhraní API aplikace odkaz](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Můžete taky zkontrolovat Statistika a řízení jistoty ke spojnici připojit. V tématu [Správa a sledování předdefinované rozhraní API aplikace a spojnic](app-service-logic-monitor-your-connectors.md).

> [AZURE.NOTE] Pokud chcete začít s aplikacemi jiných logiku Azure před registrací účet Azure, přejděte na [Zkuste aplikaci logiku](https://tryappservice.azure.com/?appservice=logic). Můžete okamžitě vytvořit aplikaci logiky krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

<!--Image references-->
[1]: ./media/app-service-logic-connector-http/1.png
[2]: ./media/app-service-logic-connector-http/2.png
[3]: ./media/app-service-logic-connector-http/3.png
[4]: ./media/app-service-logic-connector-http/4.png
[5]: ./media/app-service-logic-connector-http/5.png
[6]: ./media/app-service-logic-connector-http/6.png
[7]: ./media/app-service-logic-connector-http/7.png
[8]: ./media/app-service-logic-connector-http/8.png
[9]: ./media/app-service-logic-connector-http/9.png
[10]: ./media/app-service-logic-connector-http/10.png
