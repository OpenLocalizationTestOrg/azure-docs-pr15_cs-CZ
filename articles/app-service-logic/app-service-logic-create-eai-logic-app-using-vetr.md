<properties
   pageTitle="Vytvoření EAI logiky aplikaci VETR v aplikacích logiky pro aplikaci služby Azure | Microsoft Azure"
   description="Ověřte, kódování a transformace funkce BizTalk XML Services"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="create-eai-logic-app-using-vetr"></a>Vytvoření aplikace logiky EAI pomocí VETR

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Většinu scénářů integrace aplikace Enterprise (EAI) roli prostředníka dat mezi zdrojovým a cíl. Scénářů mají často společnou sadu požadavky:

- Zkontrolujte, jestli jsou dat z různých systémů správně naformátovaný.
- Proveďte "vyhledávací" příchozí data rozhodovat.
- Převedení dat z jednoho formátu do druhého. Například data převeďte z formát dat ve sloupcích systému CRM systému ERP formát dat ve sloupcích.
- Směrovat dat na požadovanou aplikaci nebo systému.

Tento článek popisuje běžné vzorek integrace: "mediací jednosměrné zprávu" nebo VETR (ověřit, Enrich, transformace, směrování). Vzorek VETR zprostředkovává dat mezi entit zdroj a cíl entity. Zdroje dat jsou obvykle zdrojová i cílová.

Zvažte web, který přijme objednávky. Uživatelé zveřejňují příspěvky objednávky, kde systému pomocí protokolu HTTP. Na pozadí systému ověřuje příchozí data správnost normalizuje a potrvají ve frontě Bus služby pro další zpracování. Systém trvá objednávky vypnout fronty očekává v určitém formátu. Tok začátku do konce tedy:

**Nastavit informace HTTP** → **Ověřit** → **transformovat** → **Bus služby**

![Základní VETR toku][1]

Následující rozhraní API aplikace BizTalk nápovědy vytvoření tohoto vzorku:

* **Aktivační událost protokolu HTTP** - zdroje na aktivační událost zprávy
* **Ověřit** – ověření správnosti příchozí dat
* **Transformace** – transformace dat z příchozí formátu na formát požadované podřízené systémem
* **Konektor služby Bus** - Cílová entita místo, kam se odesílá dat


## <a name="constructing-the-basic-vetr-pattern"></a>Vytvoření základní vzorek VETR
### <a name="the-basics"></a>Základní informace

Na portálu Azure vyberte **+ Nový**, vyberte **Web + mobilní telefon**a vyberte **Aplikaci logiky**. Vyberte název, umístění, předplatné, skupina zdroje a umístění, které funguje. Skupiny zdrojů fungují jako kontejnery pro vaše aplikace; všechny zdroje pro aplikaci přejděte na stejné skupiny prostředků.

Pak přidáte aktivačními událostmi a akce.


## <a name="add-http-trigger"></a>Přidání HTTP aktivační událost
1. V dialogovém okně **Logiky šablony aplikací**vyberte **vytvořit od začátku**.
1. Vyberte **HTTP posluchače** z Galerie vytvořit nové posluchače. Zavolejte ho **HTTP1**.
2. Nastavit **automaticky odesílat odpovědi?** hodnotu false. Konfigurace akci aktivace tak, že nastavení _Metoda HTTP_ na _příspěvek_ a _Relativní adresu URL_ do _/OneWayPipeline_:  
    ![Nastavit informace HTTP aktivační událost][2]
3. Vyberte zelené zaškrtnutí dokončete aktivační událost.

## <a name="add-validate-action"></a>Přidání ověřit akce

Teď Pojďme zadejte akce, které každé aktivaci se aktivuje – to znamená vždy, když přijetí hovoru na koncový bod HTTP.

1. Přidání **Hodnocení BizTalk XML** z galerie a nazvěte ji _(Validate1)_ pro vytvoření instance.
2. Konfigurace schéma XSD pro ověřování příchozích zpráv XML. Vyberte akci _ověření_ a potom _triggers('httplistener').outputs. Obsah_ jako hodnota parametru _inputXml_ .

Nyní ověřit akce je první akci po HTTP posluchače: 

![Ověřování BizTalk XML][3]

Podobně přidejte do něj zbytek akce. 

## <a name="add-transform-action"></a>Přidání transformace akce
Pojďme konfigurace transformace chcete normalizovat příchozí data.

1. Přidání **Služby transformace BizTalk** z galerie.
2. Abyste mohli nakonfigurovat transformace transformace příchozích zpráv XML, vyberte akci **transformace** jako akce provést při volání toto rozhraní API. Vyberte ```triggers(‘httplistener’).outputs.Content``` jako hodnota pro _inputXml_. *Mapa* je volitelný parametr vzhledem k tomu příchozí dat je porovnána s všechny nakonfigurované transformace a jenom ty, které odpovídají schématu použijí.
3. Nakonec transformace se spustí pouze v případě, že se mu ověřit. Konfigurace tato podmínka, vyberte ikonu ozubeného kola v pravém horním rohu a vyberte _Přidat splnění podmínky_. Nastavit stav ```equals(actions('xmlvalidator').status,'Succeeded')```:  

![Transformace BizTalk][4]


## <a name="add-service-bus-connector"></a>Přidání spojnice Bus služby
Pak přidáte cíle – fronty Bus služby – k zápisu dat do.

1. Přidání **Spojnice Bus služby** z galerie. Zadejte **název** _Servicebus1_nastavte **Připojovací řetězec** připojovací řetězec bus instanci aplikace služby, nastavte **Název subjektu** do _fronty_a přeskočit **Název předplatného**.
2. Vyberte akci **Poslat zprávu** a pole **obsahu** akce nastavit _actions('transformservice').outputs. OutputXml_.
3. Nastavte pole **Typ obsahu** , aby *aplikace/xml*:  

![Služba Bus][5]


## <a name="send-http-response"></a>Odesílat odpovědi HTTP
Po dokončení kanálem k odesílání zpráv zpracování odešlete zpět na HTTP odpověď pro úspěšné a neúspěšné pomocí následujícího postupu:

1. Přidání **HTTP posluchače** z galerie a vyberte akci, **Poslat odpověď HTTP** .
2. Nastavte hodnotu **ID odpovědi** můžete poslat *zprávu*.
2. Nastavte **Odpověď obsahu** na *zpracování kanálem k odesílání zpráv dokončeno*.
3. **Odpověď stavový kód** s *prioritou 200* označit HTTP 200 OK.
4. Vyberte rozevírací nabídku v pravém horním rohu a vyberte **Přidat splnění podmínky**.  Nastavte podmínky následující výraz:  
    ```@equals(actions('azureservicebusconnector').status,'Succeeded')```  <br/>
5. Opakujte tento postup zaslat odpověď HTTP na selhání stejně. Změna **podmínky** na následující výraz:  
```@not(equals(actions('azureservicebusconnector').status,'Succeeded'))``` <br/>
6. Vyberte **OK** a pak **vytvořit**.



## <a name="completion"></a>Dokončení
Pokaždé, když někdo odešle zprávu koncový bod HTTP, spustí aplikace a provede akce, které jste právě vytvořili. Pokud chcete spravovat tyto aplikace použití logických operátorů vytvořte, vyberte **Procházet** na portálu Azure a vyberte **Logiky aplikace**. Vyberte aplikaci zobrazíte další informace.

Některá užitečná témata:

[Správa a sledování rozhraní API aplikace a spojnic](app-service-logic-monitor-your-connectors.md)  <br/>
[Sledování aplikací logiky](app-service-logic-monitor-your-logic-apps.md)

<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG
