<properties 
    pageTitle="Porty za 1433 databáze SQL | Microsoft Azure"
    description="Připojení klientů z ADO.NET k V12 databáze SQL Azure někdy používat proxy server a interaktivně pracovat přímo s databází. Porty než 1433 stane důležité."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="" />


<tags 
    ms.service="sql-database" 
    ms.workload="drivers"
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016"
    ms.author="annemill"/>


# <a name="ports-beyond-1433-for-adonet-45-and-sql-database-v12"></a>Porty za 1433 ADO.NET 4.5 a V12 databáze SQL


Toto téma popisuje změny, které přináší V12 databáze SQL Azure chování připojení klientů používajících ADO.NET 4.5 nebo jeho novější verzí.


## <a name="v11-of-sql-database-port-1433"></a>V11 SQL databáze: Port 1433


Pokud klientským programem používá ADO.NET 4.5 se můžete připojit a dotaz s V11 databáze SQL, vnitřní posloupnost vypadá takto:


1. ADO.NET pokusí o připojení k databázi SQL.

2. ADO.NET používá port 1433 volání modulu middleware a middleware připojení k databázi SQL.

3. Databáze SQL odešle jeho odpověď middleware, odkud odpověď ADO.NET port 1433.


**Terminologie:** Popis předchozích pořadí podle informací, že ADO.NET pracuje s databáze SQL pomocí *proxy serveru směrovat*. Pokud žádná middleware zahrnuté, říkáme by, že byl použit *přímý směrování* .


## <a name="v12-of-sql-database-outside-vs-inside"></a>V12 SQL databáze: mimo a uvnitř


Připojení k V12 musí požádáme, zda klientským programem spustí *mimo* nebo *uvnitř* hranici Azure cloudu. Pododdílu jsou uvedeny dva běžné scénáře.


#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Vnější:* Klient běží na stolním počítači


1433 je pouze port, který musí být otevřena ve stolním počítači, který je hostitelem vaší databáze SQL klientské aplikace.


#### <a name="inside-client-runs-on-azure"></a>*Uvnitř:* Klient běží na Azure


Pokud klient získáte uvnitř hranici Azure cloudu použije můžete označujeme *přímé směrování* můžete komunikovat s databáze SQL serveru. Po vytvoření připojení dalších interakce mezi klientem a databáze zahrnovat žádné middleware proxy.


Pořadí vypadá takto:


1. ADO.NET 4,5 (nebo novější) zahájí stručný interakce s Azure cloudu, volání a přijímání dynamicky identifikované port číslo.
 - Číslo portu dynamicky identifikované je v oblasti 11000 11999 nebo 14000 14999.

2. ADO.NET připojí se k databázi SQL serveru přímo, s žádné middleware mezi.

3. Dotazy jsou odeslány přímo do databáze a výsledky jsou vráceny přímo k desktopovému klientovi.


Zkontrolujte, zda port, který oblastí 11000 11999 a 14000 14999 v Azure klientském počítači zůstanou dostupné pro interakcí klient ADO.NET 4.5 s V12 databáze SQL.

- Zejména v oblasti musí být bez dalších odchozí blokování.

- Na vaše OM Azure mezery nastavuje **Brána Windows Firewall s pokročilým zabezpečením** nastavení portu.
 - V [uživatelském rozhraní na brány firewall](http://msdn.microsoft.com/library/cc646023.aspx) slouží k přidání pravidlo, pro kterou nastavíte protokolu **TCP** spolu s portem oblasti se syntaxí jako **11000 11999**.


## <a name="version-clarifications"></a>Vysvětlení verze


Tento oddíl vysvětluje zástupných názvů, které odkazují na verze produktu. V něm také některé párování verze mezi produkty.


#### <a name="adonet"></a>ADO.NET


- ADO.NET 4.0 podporuje protokol Neuvedeno 7.3, ale ne 7.4.
- ADO.NET 4,5 a novější podporuje protokol 7.4 Neuvedeno.


#### <a name="sql-database-v11-and-v12"></a>V11 databáze SQL a V12


Klient připojení rozdíly mezi V11 databáze SQL a V12 zvýrazněné v tomto tématu.


*Poznámka:* Příkaz Transact-SQL `SELECT @@version;` vrátí hodnotu, které začínají číslo, třeba "11". nebo "12." a tyto podle našeho verze názvů V11 a V12 databáze SQL.


## <a name="related-links"></a>Související odkazy


- ADO.NET 4.6 byla vydána 20 červenec 2015. Oznámení blogu od týmu služeb .NET je k dispozici [v tomto poli](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).


- ADO.NET 4.5 byla vydána 15 srpen 2012. Oznámení blogu od týmu služeb .NET je k dispozici [v tomto poli](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
 - Příspěvek na blogu o ADO.NET 4.5.1 je k dispozici [v tomto poli](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).


- [Seznam verzí Neuvedeno Protocol (protokol)](http://www.freetds.org/userguide/tdshistory.htm)


- [Přehled vývoje databáze SQL](sql-database-develop-overview.md)


- [Firewall databáze SQL Azure](sql-database-firewall-configure.md)


- [Postup: Nakonfigurujte nastavení brány firewall na databáze SQL](sql-database-configure-firewall-settings.md)

