<properties 
    pageTitle="Azure předávací protokol připojení hybridní | Microsoft Azure"
    description="zure Relay hybridní připojení protokol průvodce."
    services="service-bus"
    documentationCenter="na"
    authors="clemensv"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="sethm" />

# <a name="azure-relay-hybrid-connections-protocol"></a>Azure Relay hybridní připojení Protocol (protokol)

Azure Relay patří mezi klíčové funkce pilíře platformu Bus služby Azure. Nové funkce "Hybridní připojení" předávací není zabezpečené, otevřít protokol vývoj na základě HTTP a WebSockets.

Nahrazuje bývalá rovnoměrně z vnější s názvem "BizTalk služby" funkce, které tvořily původní speciální protokol Foundation. Integrace hybridní připojení do služby Azure aplikace, zůstanou fungovat jako-je.

"Hybridní připojení" umožňuje vytvoření obousměrné, binární toku komunikace mezi dvěma síťových aplikací, kterým jednu či obě strany mohou být umístěny za dále a brány firewall.

Tento dokument popisuje interakce klientských s relay hybridní připojení k připojení klientů role posluchače a odesílatele a jak posluchače přijetí nových připojení.

## <a name="interaction-model"></a>Interakce modelu

Předávací hybridní připojení připojí obě strany zadáním rendezvous bodu v Azure cloudu, která obě strany můžete zjišťování a připojování k z perspektivu svou vlastní síť. Tomuto bodu rendezvous jmenuje "Hybridní připojení" v těchto i jiných si přečtěte následující dokumentaci, v rozhraní API a taky na portálu Azure. Koncový bod služby připojení hybridní označenou jako "služba" ve zbytku tohoto dokumentu.

Model interakce leans na klasifikace stanovené mnoho dalších síťová rozhraní API:

Existuje posluchače, který nejdřív označuje připravenost ke zpracování příchozí připojení a následně přijímá je dorazí. Na druhé straně je spojovací klienta, který propojí posluchače očekává toto připojení přijaté k vytvoření cesty obousměrná komunikace. "Připojit", "Poslech", "Přijmout" se stejnými podmínkami, které najdete na většině socket rozhraní API.

Jakýkoli přenášené komunikace model obsahuje obou stran odchozí připojení na koncový bod služby, který vytvoří "posluchače" i "klienta" Obecné používá a také může způsobit další terminologie přetížení; přesné termíny, se kterými proto používáme pro hybridní připojení vypadá takto:

Aplikace na obou stranách připojení se označují jako "klientů", protože jsou klienty služby. Klienta, který čeká a přijímá připojení je "posluchače" nebo označeny jako "posluchače role". Klienta, který zahájí nové připojení k posluchače pomocí služby je označená jako "odesílatele" nebo "odesílatele role".

## <a name="listener-interactions"></a>Interakce posluchače

Posluchače má čtyři komunikace se službou; všechny podrobnosti drátěný popsané dál v tomto dokumentu v části odkaz.

### <a name="listen"></a>Poslech

Vyznačení připravenosti na službu, která je posluchače přijímá připojení, vytvoří připojení socket odchozí web. Signalizace připojení stejný název připojení hybridní konfigurovat oboru Relay a tokenu zabezpečení, která uděluje "Poslech" ve a který název. Když web socket přijme službou, dokončení zápisu a socket založení web bude k dispozici aktivní jako "řídicí kanál" pro povolení všechny následující interakce. Služba umožňuje až 25 souběžné posluchače hybridní připojení. Pokud jsou 2 nebo více aktivní posluchače, příchozí připojení se mezi nimi rozložit v náhodném pořadí; není možné zaručit veletrhu rozdělení.

### <a name="accept"></a>Přijmout

Pokaždé, když se objeví nové připojení pro službu odesílatele, službu vyberte a upozornit na jednu z active posluchačů na hybridní připojení. Oznámení odeslaný do posluchače kanálem otevřete ovládací jako JSON zprávu obsahující adresy URL koncového bodu socket Web, který posluchače musí připojit k pro přijetí připojení.

Adresa URL můžete a musí používat přímo posluchače bez navíc práce; zakódovaný informace platí pouze pro krátký časový úsek v podstatě po dobu, je odesílatel nevadí čekat pro připojení k být založení začátku do konce, ale maximální hodnota je 30 sekund. Adresa URL lze použít pouze pro jeden úspěšné připojení. Jakmile Web socket s adresou URL rendezvous připojení, je všechny další aktivity na tento Web socket přenášené od a do odesílatel bez zásahu a interpretaci službou.

### <a name="renew"></a>Prodloužení předplatného 

Když je aktivní posluchače může platnost tokenu zabezpečení použitý k registraci posluchače a udržovat řídicí kanál Tokenu vypršení platnosti neovlivní probíhající připojení, ale způsobí řídicí kanál přerušování službou při nebo brzy po rychlých vypršení platnosti. Gesto "obnovení" je JSON zprávy, které můžou odesílat posluchače nahrazení tokenu přidružených kanálu ovládací prvek tak, aby řídicí kanál může být zachován po delší dobu.

### <a name="ping"></a>Příkaz ping 

Pokud řídicí kanál zůstane nedělá poměrně dlouho, zprostředkovatelů na cestě, například zatížení vyrovnávání nebo dále může dojít k přerušení připojení pomocí protokolu TCP. Gesto "ping", který předchází tak, že malou část dat na kanál, který připomene, k všechny členy sítě směrování připojení má být aktivní, který slouží také jako liveness test pro posluchače. Pokud příkaz ping selže, řídicí kanál považovat za nelze použít a by měl připojit posluchače.

## <a name="sender-interaction"></a>Interakce odesílatele

Odesílatel obsahuje jenom jeden interakci se službou, připojí.

### <a name="connect"></a>Připojení

Gesto "připojit" otevře socket webové službě zadáním názvu hybridní připojení a (volitelný, ale potřeba ve výchozím nastavení) tokenu zabezpečení udělení oprávnění "Odeslat" v řetězci dotazu. Služba potom komunikovat s posluchače v tak, jak jsme je popsali výše a mít posluchače vytvořit připojení rendezvous spojí se tento Web socket. Po přijetí Web socket všechny další interakce na Web socket proto bude s připojeného posluchače.

## <a name="interaction-summary"></a>Interakce souhrnné

Tento model interakce vyhodnotí, že klienta odesílatele pochází z signalizace s "vyčistit" socket Web které je připojen k posluchače a je třeba žádné další preambles nebo Příprava. Díky prakticky všechny stávající Web socket klienta implementace snadno využít službu hybridní připojení jenom zadáním správně vyrobeno adresu URL do jejich Web socket klienta vrstvy.

Připojení rendezvous Socket webu, které získá posluchače prostřednictvím interakce přijmout také čistou a může být písemný všechny existující Web socket serveru implementace s některé minimálního navíc odběru, která rozlišuje mezi operacemi "přijmout" v místní síti posluchače jejich framework a hybridní připojení vzdálené "přijmout" operace.

## <a name="protocol-reference"></a>Odkaz Protocol (protokol)

Tato část popisuje podrobnosti o protokolu interakcí jsme je popsali výše.

Všechna připojení socket Web jsou volání na port 443 jako upgrade z 1.1 HTTPS, které jsou vyjádřeny běžně tak, že některé webové socket rozhraní nebo rozhraní API. Tady popis bude k dispozici implementaci neutrální, bez znázorňující konkrétní rámec.

## <a name="listener-protocol"></a>Protokol posluchače

Protokol posluchače se skládá ze dvou připojení gest a tři operace zprávy.

### <a name="listener-control-channel-connection"></a>Připojení kanálu ovládací prvek posluchače

Vytvořit Web socket připojení k otevření kanálu ovládacího prvku:

*wss: / / {názvů adresa} /* *$servicebus* */* *hybridconnection /**{Gacutil}? sb hc akce =... & sb hc id = & sb hc token =... "*

*Obor názvů adresu* je plně kvalifikovaný název domény Azure Relay názvů, který je hostitelem hybridní připojení, obvykle formuláři {*myname}. servicebus.windows.net.*

Jsou následující možnosti parametru řetězce dotazu

| Parametr        | Povinné? | Popis                                                                                                                                                                                        |
|--------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB hc akce | Ano       | Role posluchače parametr musí být **sb hc akce = poslech**                                                                                                                                |
| {Gacutil}       | Ano       | Cesta kódování URL názvů předkonfigurované hybridní připojení k registraci tento posluchače na. Tento výraz připojen k pevné * **$servicebus**/**hybridconnection /*** část cesty. |
| SB hc tokenu  | Ano\*     | Posluchače musí poskytovat platné, kódování URL služby Bus sdílené přístup Token obor názvů nebo hybridní připojení, která uděluje **Poslech** vpravo.                                           |
| SB hc id     | Ne        | Toto ID volitelné předávaných klienta umožňuje diagnostické trasování začátku do konce.                                                                                                                             |

Pokud Web Socket připojení selže kvůli potížím cesty hybridní připojení nebyl registrovaná, nebo token neplatné neobsahují žádnou hodnotu nebo některé chyby, názory chyby poskytovat pomocí běžná modelu názory stav HTTP 1.1. Popis stavu bude obsahovat sledování id chyby, můžete oznamují Azure podpory:

| Kód | Chyba          | Popis                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Nenalezeno      | Hybridní připojení **cesta** není platná nebo základní adresa URL je poškozený |
| 401  | Neoprávněným   | Tokenu zabezpečení je chybějící nebo poškozené nebo neplatné                  |
| 403  | Zakázáno      | Tokenu zabezpečení neplatí pro tyto materiály pro tuto akci          |
| 500  | Vnitřní chyba | Něco se nepovedlo ve službě                                    |

Připojení webové socket úmyslně vypnutí službou po ho původně nastavený, to tak, aby se oznamují pomocí příslušné webové socket protokol kódem chyby spolu s popisný chybovou zprávu, která bude obsahovat také id sledování. Služba nevypne řídicí kanál bez výskytu chybový stav. Vyčistit vypnutí je klient řídit.

| Stav WS | Popis                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Cesta hybridní připojení byl odstraněn nebo zakázané.                           |
| 1008      | Vypršela platnost tokenu zabezpečení a tedy porušení zásad se tak mohli ověřovat. |
| 1011      | Něco se nepovedlo uvnitř službu.                                           |

### <a name="accept-handshake"></a>Přijetí handshake 

Oznámení přijmout odeslaný službou do posluchače kanálem dříve vytvořené ovládací prvek jako JSON zprávu v rámečku Web socket text. Neexistuje žádná odpověď na tuto zprávu.

Zpráva obsahuje JSON objekt s názvem "přijmout", která definuje následující vlastnosti v současné době:

-   **adresa** – řetězec adresy URL pro použití pro vytvoření Socket Web ve službě přijmout příchozí připojení.

-   **id** – jedinečný identifikátor u tohoto připojení. Pokud byl id klienta odesílatele, je hodnota odesílatele předávaných, jinak je hodnota generovat systém.

-   **connectHeaders** – všechna záhlaví HTTP zadaných koncový bod Relay odesílatelem, která zahrnuje i protokolů WebSocket Sec a záhlaví Sec WebSocket rozšíření.

| Přijetí zprávy                                                                     |
|------------------------------------------------------------------------------------|
```json
{                                                                                  
    "accept" : {                                                                                    
        "address" : "wss://168.61.148.205:443/$servicebus/hybridconnection/{path}?...",                                                                          
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736\_G0\_G1",                         
        "connectHeaders" : {                                                                
            "Host" : "…",                                                                       
            "Sec-WebSocket-Protocol" : "…",                                                     
            "Sec-WebSocket-Extensions" : "…"                                                    
        }                                                                                     
    }
}
```

Adresa URL podle zprávy JSON používá posluchače vytvořit Web Socket pro přijetí nebo odmítnutí socket odesílatele.

#### <a name="accepting-the-socket"></a>Přijetí socket

Přijmout, vytvoří posluchače WebSocket připojení k zadané adrese.

Tím, že po dobu náhled adresu URI může použít úplné IP adresu a TLS certifikát poskytovaného serverem se nepovede ověření na tuto adresu. To budou odstraněny během náhledu.

Jestliže zprávu "přijmout" záhlaví "Sec protokolů WebSocket", předpokládá se, že posluchače pouze přijímá WebSocket Pokud podporuje protokolu a že nastaví záhlaví stanovené WebSocket.

Totéž záhlaví "Sec rozšíření WebSocket". Pokud rámci podporuje příponu, nastavte záhlaví na odpověď straně *server* vyžaduje signalizace "Sec WebSocket rozšíření" pro rozšíření.

Adresa URL je nutné použít jako-je určený pro vytvoření socket přijmout, ale obsahuje následující parametry:

| Parametr        | Povinné? | Popis                                                                                                                                                                                                                                                                                   |
|--------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB hc akce | Ano       | Pro příjem socket parametr musí být **sb hc akce = přijmout**                                                                                                                                                                                                                          |
| {Gacutil}       | Ano       | Cesta kódování URL názvů předkonfigurované hybridní připojení k registraci tento posluchače na. Tento výraz připojen k pevné * **$servicebus**/**hybridconnection /*** část cesty.                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The path expression MAY be extended with a suffix and a query string expression that follows the registered name after a separating forward slash.                                                                                                                                             
                                                                                                                                                                                                                                                                                                                           
                            This allows the sender client to pass dispatch arguments to the accepting listener when it is not possible to include HTTP headers.                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The expectation is that the listener framework will parse out the fixed path portion and the registered name from the path and make the remainder, possibly without any query string arguments prefixed by “sb-“, available to the application for deciding whether to accept the connection.  
                                                                                                                                                                                                                                                                                                                           
                            For more detail see the “Sender Protocol” section below.                                                                                                                                                                                                                                       |
| id hc SB – | No        | V tématu Popis **id** .                                                                                                                                                                                                                                                              |

Pokud dojde k chybě, může službu odpověď následujícím způsobem:

| Kód | Chyba          | Popis                         |
|------|----------------|-------------------------------------|
| 403  | Zakázáno      | Adresa URL není platný.               |
| 500  | Vnitřní chyba | Něco se nepovedlo ve službě |

Po připojení ukončí serveru Web socket při socket Web odesílatele vypne, nebo pomocí následujících stavu

| Stav WS | Popis                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Klienta odesílatele vypne připojení                                        |
| 1001      | Cesta hybridní připojení byl odstraněn nebo zakázané.                           |
| 1008      | Vypršela platnost tokenu zabezpečení a tedy porušení zásad se tak mohli ověřovat. |
| 1011      | Něco se nepovedlo uvnitř službu.                                           |

#### <a name="rejecting-the-socket"></a>Odmítnutí socket

Podobně jako handshake odmítnutí socket po podrobná zprávu "přijmout" vyžaduje, aby stavový kód a popis stavu komunikaci důvod zamítnutí můžete tok odesílateli.

Výběr návrhu protokol tady, je použít WebSocket handshake (určený ke končící definovaný chybovém stavu) tak, aby posluchače klienta implementace můžete dál spolehnout WebSocket klienta a nemusíte využívat další, bez nutnosti dodatečného softwaru HTTP klienta.

Klient odmítnout socket, trvá adresu URI ze zprávy "přijmout" a připojí dva řetězce parametry dotazu:

| Parametr             | Povinné? | Popis                             |
|-------------------|-----------|-----------------------------------------|
| statusCode        | Ano       | Číselný kód stav HTTP                |
| Popis_stavu | Ano       | Lidské čitelné důvod, proč se odmítnutí |

Výsledný URI je pak použít navázat spojení WebSocket; znovu nezapomeňte, že certifikát TLS neodpovídají adresu během náhledu tak ověření možná bude potřeba zakázán.

Po dokončení správně, tento handshake úmyslně selže s kódem chyby HTTP 410, od žádné WebSocket. Pokud dojde k chybě tyhle možnosti:

| Kód | Chyba          | Popis                         |
|------|----------------|-------------------------------------|
| 403  | Zakázáno      | Adresa URL není platný.               |
| 500  | Vnitřní chyba | Něco se nepovedlo ve službě |

### <a name="listener-token-renewal"></a>Obnovení tokenu posluchače

Když posluchače token se blíží konec platnosti, ho nahradit ho tak, že textovou zprávu rámeček služby prostřednictvím kontrolních kanálu. Zpráva obsahuje JSON objekt s názvem "renewToken", která definuje následující vlastnost v současné době:

-   **tokenu** – platné, kódování URL služby Bus sdílené přístup Token pro obor nebo hybridní připojení, která uděluje pravém **poslechněte** .

| renewToken zprávy                                                                                                                                                    
|------------------------------------------------------------------------------------------------|
```json
{
    "renewToken" : {      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fHybridConnection1%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
    }                                                                                                                                                            
}
```                                                                                                                                                                      

Pokud tokenu ověření se nezdaří, přístup odepřen a cloudové služby ukončíte websocket kanálu ovládací prvek zobrazí se chybová zpráva, je jinak odpověď.

| Stav WS | Popis                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1008      | Vypršela platnost tokenu zabezpečení a tedy porušení zásad se tak mohli ověřovat. |

<a name="sender-protocol"></a>Protokol odesílatele
---------------

Protokol odesílatele je prakticky shodné s jak zřídit posluchače. Cílem je maximální průhlednost socket Web začátku do konce. Připojení k adrese je stejná jako posluchače, ale "akce" se liší a tokenu potřebuje různých oprávnění:

*wss: / / {názvů adresa} /* *$servicebus* */* *hybridconnection /**{Gacutil}? sb hc akce =... & sb hc id =... \[& sbc hc token =... \]*

*Obor názvů adresu* je plně kvalifikovaný název domény Azure Relay názvů, který je hostitelem hybridní připojení, obvykle formuláři {*myname}. servicebus.windows.net.*

Žádost může obsahovat libovolného navíc záhlaví HTTP, včetně definované aplikací z nich. Všechna záhlaví předaném flow posluchače a se nachází na objekt "connectHeader" zpráva "přijmout" řízení.

Jsou následující možnosti parametru řetězce dotazu

| Parametr        | Povinné? | Popis                                                                                                                                                                                                       |
|--------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB hc akce | Ano       | Role posluchače parametr musí být **Akce = připojení**                                                                                                                                                    |
| {Gacutil}       | Ano       | Cesta kódování URL názvů předkonfigurované hybridní připojení k registraci tento posluchače na.                                                                                                               
                                                                                                                                                                                                                                               
                            The path expression MAY be extended with a suffix and a query string expression to communicate further                                                                                                             
                                                                                                                                                                                                                                               
                            If the Hybrid Connection is registered under the path “hyco”, the path expression can be “**hyco/**suffix?param=value&…” followed by the query string parameters defined here. A complete expression may then be:  
                                                                                                                                                                                                                                               
                            *wss://{ns}/**$servicebus**/**hybridconnection/*** **hyco/**suffix?param=value*& sb-hc-action=…&sb-hc-id=…\[&sbc-hc-token=…\]*                                                                                     
                                                                                                                                                                                                                                               
                            The path expression is passed through to the listener in the address URI contained in the “accept” control message.                                                                                                |
| SB hc token | Yes\*     | Posluchače musí poskytovat platné, kódování URL služby Bus sdílené přístup Token obor názvů nebo hybridní připojení, která uděluje pravém **Odeslat** .                                                            | | id hc SB – | No        | Volitelné ID, které umožňuje diagnostické trasování začátku do konce a je k dispozici posluchače během signalizace přijmout.                                                                                       |

Pokud Web Socket připojení selže kvůli potížím cesty hybridní připojení nebyl registrovaná, nebo token neplatné neobsahují žádnou hodnotu nebo některé chyby, názory chyby poskytovat pomocí běžná modelu názory stav HTTP 1.1. Popis stavu bude obsahovat sledování id chyby, můžete oznamují Azure podpory:

| Kód | Chyba          | Popis                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Nenalezeno      | Hybridní připojení **cesta** není platná nebo základní adresa URL je poškozený |
| 401  | Neoprávněným   | Tokenu zabezpečení je chybějící nebo poškozené nebo neplatné                  |
| 403  | Zakázáno      | Tokenu zabezpečení neplatí pro tyto materiály pro tuto akci          |
| 500  | Vnitřní chyba | Něco se nepovedlo ve službě                                    |

Připojení webové socket úmyslně vypnutí službou po ho původně nastavený, to tak, aby se oznamují pomocí příslušné webové socket protokol kódem chyby spolu s popisný chybovou zprávu, která bude obsahovat také id sledování.

| Stav WS | Popis                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1 000      | Posluchače vypnout socket.                                                 |
| 1001      | Cesta hybridní připojení byl odstraněn nebo zakázané.                           |
| 1008      | Vypršela platnost tokenu zabezpečení a tedy porušení zásad se tak mohli ověřovat. |
| 1011      | Něco se nepovedlo uvnitř službu.                                           |

## <a name="next-steps"></a>Další kroky:

- [Předávací časté otázky](relay-faq.md)
- [Vytvořit obor názvů](relay-create-namespace-portal.md)
- [Začínáme s .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Začínáme s uzel](relay-hybrid-connections-node-get-started.md)