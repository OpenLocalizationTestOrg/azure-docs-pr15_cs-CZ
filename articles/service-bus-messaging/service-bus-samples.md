<properties 
    pageTitle="Zasílání zpráv služby Bus vzorky přehled | Microsoft Azure"
    description="Rozdělíte do kategorií a popis služby Bus zpráv vzorky s odkazy na jednotlivé."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-messaging-samples"></a>Zasílání zpráv ukázky Bus služby

Ukázky zasílání zpráv služby Bus demonstrují klíčové funkce [zasílání zpráv služby Bus](https://azure.microsoft.com/services/service-bus/) (Cloudová služba) a [Bus služby systému Windows Server](https://msdn.microsoft.com/library/dn282144.aspx). Tento článek rozdělíte do kategorií a popisuje dostupné s odkazy na jednotlivé ukázky.

>[AZURE.NOTE] Ukázky Bus služby nejsou nainstalovány s SDK. Ukázky, získáte na [stránce ukázek Azure SDK](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Kromě toho je aktualizovanou sadu Bus služeb zasílání zpráv ukázky [tady](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) (k zápisu, že nejsou popisované v tomto článku).  

Předávací výběry najdete v článku [Ukázky relay Service Bus](../service-bus-relay/service-bus-relay-samples.md).

## <a name="service-bus-messaging"></a>Zasílání zpráv služby Bus

Následující příklady ukazují, jak psát aplikace, které používat službu Bus zprávy.

Všimněte si, že zpráv ukázky vyžadují připojovací řetězec pro přístup k služby Bus názvů.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Získání připojovacího řetězce pro Bus služby Azure

1. Přihlaste se k [portálu Azure](http://portal.azure.com).

1. V levém sloupci klikněte na **Bus služby**.

1. Klikněte na název svého názvů v seznamu.

3. V zásuvné názvů klikněte na **sdílené zásady přístupu**.

4. V zásuvné **zásady přístupu sdílené** klikněte na **RootManageSharedAccessKey**.

6. Zkopírujte připojovací řetězec do schránky.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Získání připojovacího řetězce pro Bus služby systému Windows Server

1. Spusťte následující rutinu Powershellu:

    ```
    get-sbClientConfiguration
    ```

2. Zkopírovat připojovací řetězec do konfiguračního souboru pro vzorku.

### <a name="getting-started-samples"></a>Získání Začínáme vzorky

Tyto příklady popisují základní funkce zasílání zpráv.

|Ukázkový název|Popis|Minimální SDK verze|Dostupnost|
|---|---|---|---|
|[Začínáme: Zpráv s fronty](http://code.msdn.microsoft.com/Getting-Started-Brokered-aa7a0ac3)|Ukazuje, jak používat Bus služby Microsoft Azure k odesílání a přijímání zpráv z fronty.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Začínáme: Zpráv s témata](http://code.msdn.microsoft.com/Getting-Started-Brokered-614d42e5)|Ukazuje, jak používat Bus služby Microsoft Azure k odesílání a přijímání zpráv od téma s víc předplatných.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|

### <a name="exploring-features"></a>Představení funkcí

Následující příklady ukazují různé funkce Bus služby.

|Ukázkový název|Popis|Minimální SDK verze|Dostupnost|
|---|---|---|---|
|[Poskytovatelé HTTP Token](http://code.msdn.microsoft.com/Service-Bus-HTTP-Token-38f2cfc5)|Ukazuje různé způsoby ověřování protokolu HTTP/ZBÝVAJÍCÍ klient s Bus služby.|2.1|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Klient HTTP Bus služby](http://code.msdn.microsoft.com/Service-Bus-HTTP-client-fe7da74a)|Ukazuje, jak zpráv do odesílání a přijímání zpráv ze služby Bus prostřednictvím protokolu HTTP/ZBÝVAJÍCÍ.|2.3|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Služba Bus Autoforwarding](http://code.msdn.microsoft.com/Service-Bus-Autoforwarding-b9df470b)|Ukazuje, jak automatické předávání zpráv dál z fronty, předplatné nebo tato fronta do jiného fronty nebo tématu. Také ukazuje, jak můžete poslat zprávu do fronty nebo tématu prostřednictvím fronty přenos.|2.3|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: Ukázkový kanál WCF relace](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-0a526451)|Ukazuje, jak používat Bus služby Microsoft Azure pomocí kanálů Windows Communication Foundation (WCF). Vzorku ukazuje použití kanálů WCF a odesílání a přijímání zpráv prostřednictvím služby Bus fronty. Vzorku ukazuje relace a bez relace komunikace přes Bus služby.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: transakce](http://code.msdn.microsoft.com/Brokered-Messaging-8cd41d1e)|Demonstruje použití Bus služby Microsoft Azure zpráv funkcí v rozsahu transakce zajistit tak, že jsou listy zasílání zpráv operace potvrzené atomicky.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: Operace správa pomocí REST](http://code.msdn.microsoft.com/Brokered-Messaging-569cff88)|Ukazuje, jak provádět správu operace s služby Bus pomocí ZBÝVAJÍCÍ.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Zprostředkovatele prostředků rozhraní REST API](http://code.msdn.microsoft.com/Service-Bus-Resource-5d887203)|Ukazuje, jak používat novou RDFE REST API Bus služby ke správě obory názvů a zpráv entity.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: Ukázka relace služby WCF](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-db4262c2)|Ukazuje, jak používat Bus služby Microsoft Azure pomocí modelu služby WCF. Vzorku ukazuje použití modelu služby WCF provádění komunikaci prostřednictvím služby Bus fronty založené na relace.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: Žádost o odpověď](http://code.msdn.microsoft.com/Brokered-Messaging-Request-2b4ff5d8)|Ukazuje, jak používat Bus služby Microsoft Azure a funkci žádostí a odpovědí. Vzorku zobrazuje jednoduchý klienty a servery komunikaci prostřednictvím služby Bus fronty.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: fronty nepoužívaných dopisů](http://code.msdn.microsoft.com/Brokered-Messaging-Dead-22536dd8)|Ukazuje, jak používat Microsoft Azure služby Bus a funkce zasílání zpráv "fronty nepoužívaných dopisů". Vzorku zobrazuje jednoduchý odesílatele a příjemce komunikaci prostřednictvím služby Bus fronty.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: Pozdržená zprávy](http://code.msdn.microsoft.com/Brokered-Messaging-ccc4f879)|Ukazuje, jak používat funkci zprávy vyloučení DPH služby Microsoft Azure. Vzorku zobrazuje jednoduchý odesílatele a příjemce komunikaci prostřednictvím služby Bus fronty.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: Relaci zprávy](http://code.msdn.microsoft.com/Brokered-Messaging-Session-41c43fb4)|Ukazuje, jak používat Microsoft Azure služby Bus a funkce zasílání zpráv relace. Vzorku zobrazuje jednoduchý odesílatelé a příjemci komunikaci prostřednictvím služby Bus fronty.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: Téma žádost o odpověď](http://code.msdn.microsoft.com/Brokered-Messaging-Request-6759a36e)|Ukazuje, jak implementovat vzorek žádostí a odpovědí pomocí Microsoft Azure služby Bus témata a předplatná. Vzorku zobrazuje jednoduchý klienty a servery komunikace přes tematickým Bus služby.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: Fronty odpověď požadavků](http://code.msdn.microsoft.com/Brokered-Messaging-Request-0ce8fcaf)|Ukazuje, jak používat Microsoft Azure služby Bus a funkci žádostí a odpovědí. Vzorku zobrazuje jednoduchý klienty a servery komunikaci prostřednictvím dvě fronty Bus služby.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: Duplicit](http://code.msdn.microsoft.com/Brokered-Messaging-c0acea25)|Ukazuje, jak zjišťování duplicitní zprávu Bus služby Microsoft Azure pomocí služby fronty. Vytvoří dva fronty, jedna z nich duplicit povolené a další bez duplicit.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: Asynchronní zasílání zpráv](http://code.msdn.microsoft.com/Brokered-Messaging-Async-211c1e74)|Ukazuje, jak používat Bus služby Microsoft Azure k odesílání a přijímání zpráv asynchronní z fronty. Ve frontě poskytuje oddělené, asynchronní komunikaci mezi odesílatele nebo libovolný počet příjemci (tady jednoho příjemce).|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: Pokročilé filtry](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)|Ukazuje, jak používat Microsoft Azure služby Bus publikovat nebo přihlášení k odběru rozšířené filtry. Vytvoří tématu a 3 předplatná s definice různých filtrů, zasílání zpráv v tématu a obdrží všechny zprávy z předplatného.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Brokered zasílání zpráv: Předběžné zprávy](http://code.msdn.microsoft.com/Brokered-Messaging-be2dac1d)|Ukazuje, jak používat funkci Bus služby Microsoft Azure zprávy předběžné. Ukazuje, jak používat funkci předběžné zprávy při přijímání.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|

## <a name="service-bus-reference-tools"></a>Nástroje odkazu Bus služby

Následující příklady ukazují různé funkce služby.

|Ukázkový název|Popis|Minimální SDK verze|Dostupnost|
|---|---|---|---|
|[Průzkumník Bus služby](http://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)|Průzkumník Bus služby umožňuje uživatelům připojení k obor názvů služby služby Bus a spravovat zpráv entity snadno způsobem. Nástroj poskytuje pokročilé funkce, jako jsou funkce pro import nebo export a možnost testování zpráv entity a předávací služby.|1.8|Bus služby Microsoft Azure; Bus služby systému Windows Server|
|[Povolení: SBAzTool](http://code.msdn.microsoft.com/Authorization-SBAzTool-6fd76d93)|Tento příklad ukazuje, jak vytvářet a spravovat služby identit v Microsoft Azure Active Directory řízení přístupu (označovaná taky jako služba Řízení přístupu nebo ACS) pro použití s Bus služby.|NENÍ K DISPOZICI|Bus služby Microsoft Azure|

## <a name="next-steps"></a>Další kroky

V následujících tématech pro konceptuální přehledy Bus služby.

- [Přehled služeb Bus zasílání zpráv](service-bus-messaging-overview.md)
- [Služba Bus architektura](service-bus-architecture.md)
- [Základy Bus služby](service-bus-fundamentals-hybrid-solutions.md)
