<properties 
    pageTitle="Přehled služeb Bus AMQP s Java | Microsoft Azure" 
    description="Zjistěte, jak používat Java s rozšířený zpráva řízení fronty Protocol (AMQP) 1.0 v Azure." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="amqp-10-support-in-service-bus"></a>Podpora AMQP 1.0 v Bus služby

Bus služby Azure Cloudová služba a místní [Bus služby systému Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) podporují rozšířený zprávy přidávání do fronty Protocol (AMQP) 1.0. AMQP umožňuje vytvářet různé platformy, hybridní aplikací pomocí otevřený standardní protokol. Můžete vytvářet aplikace, které používají součástí, které jsou vytvořené pomocí různých jazycích a rámce a které běží v různých operačních systémech. Všechny tyto komponenty se můžete připojit Bus služby a bezproblémově efektivní a u celé věrností vyměňovat strukturované obchodní zprávy.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Úvod: Co je AMQP 1.0 a proč je důležité?

Obvykle používají orientovaného zprávy middlewarových produktů speciální protokoly pro komunikaci mezi klientskými aplikacemi a zprostředkovatelů. To znamená, že po výběru zpráv zprostředkovatele určitým dodavatelem, musíte pomocí knihoven prodejce připojení vaší klientské aplikace k této zprostředkovatele. Výsledkem stupeň závislost na tuto dodavatelem, protože přenos aplikace k jinému produktu vyžaduje kód změn ve všech propojených aplikací. 

Kromě toho je připojení zpráv zprostředkovatelů z různých dodavatelů záludné. Obvykle vyžaduje úrovni aplikace přemostění přesunout zprávy z jednoho systému a překládat mezi jejich vlastních formátů. Toto je požadavek na běžné; třeba při musí přináší nové jednotné rozhraní na starší různorodé systémy nebo integrace systémů sledovat spojení.

Odvětví software je rychlý přesunutí business; nové jazyky a aplikace rámce jsou zavádí někdy nepřeberné tempem. Podobně požadavky počítačové systémy v průběhu času vyvíjejí a vývojáři chcete využít nejnovější funkce platformy. Ale někdy vybraného zpráv dodavatele nepodporuje tyto platformy. Protože jsou speciální zpráv protokoly, není možné pro jiné uživatele k zadání knihoven pro tyto nové platformy. Proto je nutné použít přístupy například vytváření bran nebo mosty, které vám umožní dál používat zasílání produktu.

Vývoj systému Upřesnit zpráva řízení fronty protokol (AMQP) 1.0 bylo motivováno těchto problémů. Vznikl v Chase Nováková CF, kteří jako nejčastěji finanční služby firmami, jsou náročné uživatele middleware orientované zprávy. Cílem je jednoduchá: vytvoření otevřít standardní zpráv protokol, který umožňuje vytvářet zprávy aplikacím komponent vytvořených pomocí různých jazycích, rámců a operační systémy, nejlepší svého druhu součásti z oblasti dodavatele použít u všech.

## <a name="amqp-10-technical-features"></a>Funkce technické AMQP 1.0

AMQP 1.0 je efektivně spolehlivé, drátěný úrovni zpráv protokol, který slouží k vytváření robustních, různé platformy aplikace pro zasílání zpráv. Protokol má jednoduchého cíle: Definujte mechanismy zabezpečené spolehlivý a efektivně přenosem zpráv mezi dvěma stranami. Samotné zprávy kódování pomocí znázornění portable dat, který umožňuje nesourodými odesílatelé a příjemci exchange strukturované obchodní zpráv v celé věrností. Následuje přehled nejdůležitějších funkcí:

*    **Efektivní**: AMQP 1.0 je připojení určený protokol používající binární kódování protokol pokyny a zprávy firmy přenášena. Zahrne sofistikované řízení toku schémata maximální využití sítě a připojené součásti. Který říká, protokol je určen narazila poměr efektivity, flexibilitu a spolupráce.
*    **Spolehlivost**: protokol AMQP 1.0 umožňuje výměnu s rozsahem spolehlivost záruky, fire a zapomenete spolehlivé, přesně zpráv – jednou potvrzeno doručení.
*    **Flexibilní**: AMQP 1.0 je flexibilní protokol, který slouží ke podporují různé topologií. Stejný protokol lze použít pro klienta klienta, klienta zprostředkovatel a zprostředkovatel Zprostředkovatel komunikaci.
*    **Zprostředkovatel modelu nezávislé**: specifikace 1.0 AMQP abyste požadavky na modelu zasílání zpráv používaný zprostředkovatele. To znamená, že je možné snadno přidat AMQP 1.0 podporu existující zprostředkovatelů zasílání zpráv.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 je Standard (s na velké ")

AMQP 1.0 byl ve vývoji od 2008 skupinou core víc než 20 společností technologii dodavatele a koncových uživatelů podniky. Během tohoto období uživatele podniky přispět svých praktických obchodní požadavky a dodavatelé technologii vývojem protokolu splňovat tyto požadavky splnit. Během procesu dodavatelé zúčastnili cvičení, ve kterých jsou spolupracovali ověřit vzájemnou jejich implementace.

V Říjen 2011 vývojářů, které přešly technické výboru v rámci organizace postupu z strukturované informace organizace pro standardy OASIS () a OASIS AMQP 1.0 standardní vydání v říjen 2012. Následující podniky se zúčastnili technický výbor průběhu vývoje standardu:

*    **Dodavatelé technologie**: Axway Software, Huawei technologií, IIT Software, INETCO systémy, Kaazing, Microsoft, Mitre Corporation, Primeton technologií, průběh Software, červené klobouk, SITA, Software AG, Solace systémy, VMware, WSO2, Zenika.
*    **Uživatel podniky**: banky Amerika platební Suisse, německá Boerse, Goldman Sachs, JPMorgan Chase.

Mezi běžně uvedeným výhody otevřít standardy patří:

*    Nižší pravděpodobnost, že dodavatele lock se změnami
*    Spolupráce
*    Obecných dostupnost knihoven a nástrojů
*    Ochrana proti životnosti
*    Dostupnost zkušení zaměstnanců
*    Malá písmena a spravovatelné rizika

## <a name="amqp-10-and-service-bus"></a>Bus AMQP 1.0 a služby

Podpora AMQP 1.0 v Bus služby Azure znamená, že teď můžete využít služby Bus shromažďování zpráv a publikovat a přihlášení k odběru brokered funkce zasílání zpráv z oblasti platformách pomocí efektivně binární protokolu. Kromě toho je možné vytvářet aplikace skládá součástí vytvořené pomocí kombinací jazyky, rámců a operační systémy.

Následující obrázek znázorňuje nasazení příklad, ve kterém Java klienty na Linux vytvořených pomocí standardní Java zprávy služby (JMS) rozhraní API a .NET klienti v systému Windows, vymění prostřednictvím služby Bus pomocí AMQP 1.0.

![][0]

**Obrázek 1: Příklad scénáře zobrazující a platformách zasílání zpráv pomocí služby Bus a AMQP 1.0**

V současné době označují následující knihoven klienta pro práci s Bus služby:

| Jazyk | Knihovna                                                                       |
|----------|-------------------------------------------------------------------------------|
| Java     | Klient Apache Qpid Java zprávy služby (JMS)<br/>Klient IIT Software SwiftMQ Java |
| C        | Apache Qpid kanálem C                                                          |
| PHP      | Apache Qpid kanálem PHP                                                        |
| Python   | Apache Qpid kanálem Python                                                     |


**Obrázek 2: Tabulky knihoven klienta AMQP 1.0**

Další informace o tom, jak získat a těchto knihoven pomocí služby Bus najdete v tématu [služby Bus AMQP Příručka pro vývojáře][]. Odkazy na další informace v části [Další kroky](service-bus-java-amqp-overview.md#next-steps) .

## <a name="summary"></a>Souhrn

*    AMQP 1.0 je otevřít, spolehlivé zpráv protokol, který slouží k vytvoření různé platformy, hybridní aplikací. AMQP 1.0 je OASIS standard.
*    Podpora AMQP 1.0 je teď dostupná v Bus služby Azure, jakož i Bus služby systému Windows Server (Service Bus 1.1). Ceny je stejné jako u stávajících protokoly.

## <a name="next-steps"></a>Další kroky

Navštivte následující odkazy na další informace o podpoře AMQP v Bus služby.

*    [Jak pomocí rozhraní API služeb Bus .NET AMQP 1.0](service-bus-dotnet-advanced-message-queuing.md)
*    [Používání jazyka Java zprávy služby (JMS) rozhraní API aplikace pomocí služby Bus & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Služby průvodce Bus AMQP vývojář][]
*    [Specifikace OASIS rozhraní Upřesnit zpráva řízení fronty Protocol (AMQP) verze 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)

[0]: ./media/service-bus-java-amqp-overview/Example1.png
[Služby průvodce Bus AMQP vývojář]: service-bus-amqp-dotnet.md

 
