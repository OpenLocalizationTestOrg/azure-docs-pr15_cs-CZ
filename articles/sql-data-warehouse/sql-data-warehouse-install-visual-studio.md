<properties
   pageTitle="Instalace aplikace Visual Studio a SSDT SQL datový sklad | Microsoft Azure"
   description="Instalace aplikace Visual Studio a SQL Server vývojového nástroje SSDT () pro datový sklad Azure SQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="install-visual-studio-2015-and-ssdt-for-sql-data-warehouse"></a>Instalace aplikace Visual Studio 2015 a SSDT SQL datový sklad

Můžete vyvíjet aplikace pro systém SQL Data Warehouse, doporučujeme vám použít Visual Studio 2015 s nejnovější verzí systému SQL Server Data nástroje (SSDT).  Visual Studio 2013 aktualizaci 5 s SSDT podporuje i z důvodu zpětné kompatibility.  

Pomocí aplikace Visual Studio SSDT vám umožní Průzkumník objektu SQL serveru umožňuje vizuálně prozkoumání tabulky, zobrazení, uložené procedury a celou řadu dalších objektů v SQL datový sklad i spouštění dotazů.

> [AZURE.NOTE] SQL datový sklad ještě podporuje projekty Visual Studio databáze.  Tato funkce se přidají v budoucích verzích.

## <a name="step-1-install-visual-studio-2015"></a>Krok 1: Instalace aplikace Visual Studio 2015

Tyto odkazy vedou na stažení a instalace aplikace Visual Studio 2015. Pokud už máte Visual Studio 2013 nebo 2015 nainstalovaný, můžete přejít ke kroku 2, instalace SSDT.

1. [Stažení aplikace Visual Studio 2015][].
2. Postupujte podle průvodce [Instalací aplikace Visual Studio][] na webu MSDN a zvolte výchozí konfigurace.

## <a name="step-2-install-ssdt"></a>Krok 2: Instalace SSDT

Instalace SSDT for Visual Studio jednoduše vyhledat aktualizace SSDT z aplikace Visual Studio pomocí následujících kroků.

1. Ve Visual Studiu klikněte na **Nástroje** / **rozšíření a aktualizace...**  /  **Aktualizace**
2. Výběr **Aktualizací** a potom vyhledejte **Nástroje aktualizace Microsoft SQL Server pro databázi**

Pokud není nalezen aktualizaci, byste měli mít nainstalovanou nejnovější verzi.  Abyste si ověřili, SSDT je nainstalovaný, klepněte na **Nápověda** / **O Microsoft Visual Studio** a hledejte SQL Server Data Tools v seznamu.  Nejnovější verzi SSDT je 14.0.60525.0.  Pokud možnost nainstalovat není k dispozici z aplikace Visual Studio, můžete taky můžou navštívit stránky [SSDT stáhnout][] si stáhněte a nainstalujte SSDT ručně.

## <a name="next-steps"></a>Další kroky

Teď, když máte nejnovější verzi SSDT, budete chtít [Připojit][] k datový sklad SQL.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[připojení]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Stažení aplikace Visual Studio 2015]: https://www.visualstudio.com/downloads/
[Instalace aplikace Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[Stažení SSDT]: https://msdn.microsoft.com/library/mt204009.aspx
