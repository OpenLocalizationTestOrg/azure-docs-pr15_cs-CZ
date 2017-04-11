<properties
   pageTitle="Zavedení SQL data do Azure události rozbočovače | Microsoft Azure"
   description="Základní informace o události rozbočovače import z ukázkové SQL"
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

# <a name="pulling-data-from-sql-into-an-azure-event-hub"></a>Zavedení dat z SQL do rozbočovači události Azure

Typické architektura pro aplikaci pro zpracování data v reálném čase, musíte nejdřív předání k rozbočovači Azure události. Může být IoT scénář, například sledování provozu v různých úsecích komunikace, nebo scénáři herní monitorování akce shromažďujte frenzied contestants nebo scénáři enterprise, například sledování držby stavební. V těchto případech výrobci data jsou obvykle externí objekty vytváření časových řad dat, která je potřeba shromáždit, analyzovat, ukládání a zpracovat a může mít investovat hodně plánování řízené úsilí do vytvoření infrastruktury těchto procesů. Co můžete dělat, když chcete shromáždit data od něco jako databáze spíše než zdroj dat datových proudů a použití ve spojení s dalších datových proudů datům? Předpokládejme, že místo, kam chcete použít Azure toku analýzy, Průzkumníka Vzdálená Data (RDX) nebo jiný nástroj pro analýzu a pracovat na pomalu změně dat z Microsoft Dynamics AX nebo systém vlastní správy zařízení? Tento problém můžete vyřešit, jste napsali jsme otevřít získaná výběru malé cloudu, můžete upravovat a nasazení, které bude načítat data z tabulky SQL a nabízená k rozbočovači Azure události používat jako vstup v podřízené analytických aplikací. Třeba si uvědomíte, že se jedná scénáři méně častých a opakem co obvyklým způsobem s rozbočovači události. Pokud nenajdete sami situace, kdy je to co musíte udělat, můžete v Azure Galerii ukázek, [tady](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-import-from-sql/)můžete najít kód.  

Všimněte si, že kód v tomto příkladu je právě, výběru. **Není** by měla být praxi a bylo provedeno žádné pokusů usnadnit vhodný pro použití v takovém prostředí – je stricly DIY, zaměřené na developer příklad. Potřebujete zkontrolovat všem druhům zabezpečení, výkonu, funkce a náklady faktory před spuštěním na začátku do konce architektury.

## <a name="application-structure"></a>Struktura aplikace

Aplikace je napsáno v jazyce C# a soubor readme.md ve výběru obsahuje všechny informace, které potřebujete k úpravám, vytváření a publikování aplikace. Následující části obsahují nejvyšší úrovně základní informace o možnostech aplikace.

Začneme tím se za předpokladu, že máte přístup k tabulky databáze SQL Azure. Budete taky muset nastavili rozbočovači Azure událostí a vědět připojení řetězec potřebné k němu přístup.

Při řešení SqlToEventHub spuštění systému, bude číst konfigurační soubor (App) získat počet akcí, jak je uvedeno v souboru readme.md. Toto jsou poměrně zřejmé, jako je název tabulky dat atd a potřeba rehash vysvětlení tady. 

Jakmile aplikace četl konfiguračního souboru, ho přejde do smyčky, čtení tabulky SQL a předání záznamů do centra události a potom čekání interval uživatelem definovaných spánku předtím ho úplně znovu. Co je potřeba je vhodné poznamenat:

1. Aplikace je založena na za předpokladu, že v tabulce SQL je aktualizován některé externí procesem a chcete-li odeslat všechny a pouze aktualizace rozbočovači události.
2. Tabulky SQL, musí mít pole, které má jedinečný a rostoucí číslo, například počet záznamů. To můžou skládat například pole s názvem "Identifikátor" nebo něco, co je zvýšen jako něco jiného, co se aktualizuje v databázi přidá záznamy jako "Creation_time" nebo "Sequence_number". Aplikace poznámek a ukládá hodnotu tohoto pole v každém opakování. V každé následující předávací opakovat aplikace v podstatě dotazy v tabulce pro všechny záznamy, které hodnotu tohoto pole překročí hodnotu viděli posledního procházet opakovat. Voláte jsme tuto poslední hodnotu "posun".
3. Aplikace vytvoří tabulku "TableOffsets" při spuštění systému, k ukládání pořadová čísla. V tabulce se vytvoří pomocí dotazu, který "CreateOffsetTableQuery" definice v souboru konfigurace. 
4. Existuje několik dotazů používaných při práci s posun tabulce určené v souboru konfigurace jako "OffsetQuery", "UpdateOffsetQuery" a "InsertOffsetQuery". Neměňte tyto.
5. Nakonec dotaz, který "dotaz na data" definice v souboru konfigurace je dotaz, který se spustí můžete načítat záznamy z tabulky SQL. Je aktuálně k záznamům horní 1 000 v jednotlivých předávací smyčka pro účely optimalizace – Pokud například přidali 25 000 záznamů do databáze od posledního dotazu může trvat při spuštění dotazu. Omezením dotazu 1 000 záznamů pokaždé, když, dotazy jsou mnohem rychlejší. Výběr horní 1000 jednoduché posune po sobě jdoucí listy 1 000 záznamů k rozbočovači události.    

## <a name="next-steps"></a>Další kroky

Nasazení řešení, klonovat nebo stáhněte si aplikaci SqlToEventHub upravit konfiguračního souboru, vytvořili a nakonec publikujte. Po publikování aplikace můžete vidět spuštěné v portálu Azure klasické v části Cloudovým službám a sledovat události, které přichází do rozbočovače události. Všimněte si, že bude četnost určují dvě věci: Frekvence aktualizací k tabulce SQL a interval spánku jste zadali v souboru konfigurace aplikace.