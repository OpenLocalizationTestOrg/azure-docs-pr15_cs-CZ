<properties
    pageTitle="Zavedení ve veřejných datech do Azure události rozbočovače | Microsoft Azure"
    description="Základní informace o události rozbočovače import z webová ukázka"
    services="event-hubs"
    documentationCenter="na"
    authors="spyrossak"
    manager="timlt"
    editor=""/>

<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/25/2016"
    ms.author="spyros;sethm" />

# <a name="pulling-public-data-into-azure-event-hubs"></a>Zavedení ve veřejných datech do rozbočovače události Azure

Obvyklé scénáře Internet věcí (IoT) vám dává zařízení, která můžete vložit data do Azure, buď rozbočovači Azure událost nebo rozbočovači IoT program. Jsou obě tyto rozbočovače vstupní body do Azure pro ukládání, analýza a vizualizace se velkého počtu nástroje na Microsoft Azure k dispozici. Však obě vyžadují nabízená dat na ně ve formátu JSON a zabezpečený konkrétní způsoby. Tím se vyvolá následující otázky. Co můžete dělat, když chcete shromáždit data z veřejné nebo soukromé zdrojů kde dat vystavit jako datový kanál určitý druh a webové služby, ale nemáte možnost změnit, jak je publikován data? Zvažte počasí nebo přenosy nebo akcií - neumí určit NOAA, nebo WSDOT nebo NASDAQ konfigurace nabízených k rozbočovači události. Tento problém můžete vyřešit, jste napsali jsme otevřít získaná výběru malé cloudu, můžete upravovat a nasazení, které bude načítat data z některé z těchto zdrojů a použít pro vaše Centrum události. Odtud můžete udělat něco jiného, co máte zájem, předmět, samozřejmě na licenční podmínky od výrobce. Vyhledání aplikace [tady](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/).

Všimněte si, že kód v tomto příkladu pouze ukazuje, jak můžete načítat data z webu typické informační kanály a jak psát k rozbočovači Azure události. Toto není má být praxi a žádné pokusy přináší usnadnit vhodný pro použití v takovém prostředí – je strictfly DIY, zaměřené vývojář příklad pouze. Kromě toho existenci v tomto příkladu je jako doporučení byste měli **získat** data do Azure spíše než **nabízená** ho. Zkontrolujte zabezpečení, výkon a funkce a náklady faktory před spuštěním na začátku do konce architektury.

## <a name="application-structure"></a>Struktura aplikace

Aplikace je napsáno v jazyce C# a [popis ukázky](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) obsahuje všechny informace, které potřebujete k úpravám, vytváření a publikování aplikace. Následující části obsahují podrobný přehled udělá tohle aplikace.

Začneme tím se za předpokladu, že máte přístup k datovému kanálu. Můžete třeba vyžádat přenosy dat z stavu oddělení dopravy WA nebo počasí data z NOAA, chcete-li zobrazit vlastní sestavy nebo ke sloučení dat pomocí dalších dat v aplikaci. Budete taky muset nastavili rozbočovači Azure událostí a vědět připojení řetězec potřebné k němu přístup.

Při řešení GenericWebToEH spuštění systému, bude číst konfigurační soubor (App) získat několik věcí:

1. Adresa URL nebo seznam adres URL, pro web publikování data. V ideálním případě toto je web, který publikuje data ve formátu JSON, třeba můžou být odkazováno WSDOT [zde](http://www.wsdot.wa.gov/Traffic/api/). 
2. Přihlašovací údaje pro adresu URL, v případě potřeby. Mnoho veřejné zdroje nevyžadují přihlašovací údaje nebo pověření můžete dát na řetězec adresy URL. Jiné vyžadují zadat samostatně. (Vezměte v úvahu, že můžete zadat pouze jednu sadu přihlašovacích údajů v této aplikaci tak, aby je funkční pouze v případě zadáte jedinou adresu URL, ne seznam adres URL.)
3. Připojovací řetězec a název Centru událostí v této události rozbočovače názvů, do kterého budou nabízená data. Tyto informace najdete v portálu Azure.
4. Jako spánku interval, v milisekundách pro interval mezi dotazování na web ve veřejných datech. Toto nastavení vyžaduje některé myšlenky. Pokud hlasování příliš často můžete zmeškat data. na druhé straně hlasování příliš často velké množství dat opakující se může zobrazit, zda horší ještě, které můžou blokovat jako nefarious bot. Zvažte frekvenci aktualizací zdroje dat – počasí nebo přenosy dat může být aktualizována každých 15 minut, ale akcií možná každých několik sekund, v závislosti na místo, kam se zobrazí. 
5. Příznak aplikace zjistit, zda je tu data formátu JSON nebo XML. Vzhledem k tomu potřebujete nabízená data k rozbočovači události, má aplikace modul pro převod XML na JSON před odesláním.

Po přečtení konfiguračního souboru, přejde aplikace do cyklus – otevření veřejného webu převodu dat v případě potřeby zápisu do rozbočovače události a potom čekat intervalu spánku předtím ho úplně znovu. Konkrétně:

  * Čtení na veřejný web. Pro příjem jste připravení odeslat data instance třídy RawXMLWithHeaderToJsonReader používat z Azure/GenericWebToEH/ApiReaders/RawXMLWithHeaderToJsonReader.cs. Přečte stream zdroje v metodu GetData() a potom rozdělí na menší části (tedy záznamy) pomocí GetXmlFromOriginalText. 
  Tento způsob přečte XML i ve správném formátu JSON nebo JSON pole. Potom zpracování je spuštěn pomocí konfigurace MergeToXML z App.config (výchozí = prázdné).
  * Přijímání a odesílání dat je jako smyčku v metodě Process() v Program.cs implementovaná. 
  Po přijetí výstup z GetData(), enqueues metoda s oddělovači k rozbočovači události.

## <a name="next-steps"></a>Další kroky

Nasazení řešení, klonovat nebo stáhněte si aplikaci [GenericWebToEH](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) upravit konfiguračního souboru, vytvořili a nakonec publikujte. Po publikování aplikace uvidíte ho spuštěné v portálu Azure klasické v části Cloudovým službám a změnit některé nastavení (například cílové centrální událostí a interval spánku) na kartě **Konfigurovat** .

Zobrazit další příklady rozbočovače událostí v [Galerii Azure ukázek](https://azure.microsoft.com/documentation/samples/?service=event-hubs) a na [webu MSDN](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5).
