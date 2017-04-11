<properties
   pageTitle="Katalog dat Azure podporované zdroje dat | Microsoft Azure"
   description="Specifikace současné době podporované zdroje dat"
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="jstrauss"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-supported-data-sources"></a>Katalog dat Azure podporované zdroje dat

Budou moct uživatelé v katalogu dat Azure publikovat metadat pomocí veřejné API, klikněte na-jednou registrace Nástroje nebo ručním zadáním informace přímo do katalogu dat webové portál. Následující tabulka shrnuje všechny zdroje nepodporuje katalogu dnes a funkce publikování pro jednotlivá pole.  Uvedené taky jsou nástroje externích dat, které jednotlivé zdroje můžete spustit z našich prostředí portál "Otevřít se změnami". Druhá mřížka v článku obsahuje další technické specifikace každé připojení zdroje dat – vlastnosti.


## <a name="list-of-supported-data-sources"></a>Seznam podporovaných zdrojů dat

<table>

    <tr>
       <td><b>Objekt zdroje dat</b></td>
       <td><b>ROZHRANÍ API</b></td>
       <td><b>Ruční zadání</b></td>
       <td><b>Nástroj pro registraci</b></td>
       <td><b>Otevření nástroje</b></td>
       <td><b>Poznámky</b></td>
    </tr>

    <tr>
      <td>Adresář úložiště jezera dat Azure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Soubor úložiště jezera dat Azure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure úložiště objektů Blob</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Adresář Azure úložiště</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabulka Azure úložiště</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>

    <tr>
      <td>HDFS adresáře</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Soubor HDFS</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabulku podregistru</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Aplikace Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Zobrazení podregistru</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Aplikace Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabulka MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Zobrazení MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabulky databáze Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Zobrazení databáze Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Ostatní (Obecné majetku)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabulka serveru SQL datový sklad</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Zobrazení Data Warehouse SQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services dimenze</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services klíčového ukazatele výkonu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services míra</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabulka serveru SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Sestava služby Reporting Services serveru SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Prohlížeče</font></td>
      <td><font size=2>Pouze v nativním režimu servery. Integrovaném režimu sharepointu nepodporuje.</font></td>
    </tr>

    <tr>
      <td>Tabulka serveru SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Zobrazení SQL serveru</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabulka Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Aplikace Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Zobrazení Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Aplikace Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP Hana zobrazení</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2>Výpočet a analytických zobrazení. Atribut zobrazení nejsou podporovaná.</font></td>
    </tr>

    <tr>
      <td>Tabulka Db2</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Zobrazení Db2</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Soubor systému</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Adresář FTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Soubor protokolu FTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Sestava protokolu HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Nastavit informace HTTP koncový bod</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Soubor protokolu HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Sadu entit OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Funkce OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabulka Postgresql</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Zobrazení Postgresql</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Zobrazení Hana SAP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td> Objekty služby Salesforce</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Seznam služby SharePoint </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

</table>

Pokud potřebujete podporu další zdroje, zadejte tam žádost o funkci používání [katalogu dat Azure fórum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


<br>
<br>
## <a name="data-source-reference-specification"></a>Zadání odkaz zdroje dat
> [AZURE.NOTE] Sloupec "DSL strukturu" v následující tabulce pouze seznamy připojení vlastnosti kontejneru "address", které se používají v katalogu dat Azure (tedy "address" kontejneru může obsahovat další vlastnosti připojení zdroje dat, která katalog dat Azure potrvají, ale nepoužívá.)
<table>
    <tr>
       <td><b>Typ zdroje</b></td>
       <td><b>Typ majetku</b></td>
       <td><b>Typy objektů</b></td>
       <td><b>Struktura DSL<b></td>
    </tr>
    <tr>
      <td>Úložiště jezera dat Azure</td>
      <td>Kontejner</td>
      <td>Jezera dat</td>
      <td>
        <font size=2>Protocol (protokol): webhdfs <br>ověření: {základní, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Úložiště jezera dat Azure</td>
      <td>Tabulky</td>
      <td>Adresář, soubor</td>
      <td>
        <font size=2>Protocol (protokol): webhdfs <br>ověření: {základní, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure úložiště</td>
      <td>Kontejner</td>
      <td>Kontejner</td>
      <td>
        <font size=2>Protocol (protokol): objektů BLOB azure <br>ověření: {azure přístupové klávesy} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kontejner</font>
      </td>
    </tr>
    <tr>
      <td>Azure úložiště</td>
      <td>Tabulky</td>
      <td>Kulatý adresáře</td>
      <td>
        <font size=2>Protocol (protokol): objektů BLOB azure <br>ověření: {azure přístupové klávesy} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kontejner <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Jméno</font>
      </td>
    </tr>
    <tr>
      <td>Azure úložiště</td>
      <td>Kontejner</td>
      <td>Kontejner</td>
      <td>
        <font size=2>Protocol (protokol): tabulek azure <br>ověření: {azure přístupové klávesy} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet</font>
      </td>
    </tr>
    <tr>
      <td>Azure úložiště</td>
      <td>Tabulky</td>
      <td>Tabulky</td>
      <td>
        <font size=2>Protocol (protokol): tabulek azure <br>ověření: {azure přístupové klávesy} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Jméno</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Kontejner</td>
      <td>Virtuální obrázku</td>
      <td>
        <font size=2>Protocol (protokol): cosmos <br>ověření: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Tabulky</td>
      <td>Toku, toku nastavení zobrazení</td>
      <td>
        <font size=2>Protocol (protokol): cosmos <br>ověření: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Kontejner</td>
      <td>Web</td>
      <td>
        <font size=2>Protocol (protokol): http <br>ověření: {žádný, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Sestava</td>
      <td>Sestavy řídicího panelu</td>
      <td>
        <font size=2>Protocol (protokol): http <br>ověření: {žádný, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protocol (protokol): db2 <br>ověření: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>Db2</td>
      <td>Tabulky</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protocol (protokol): db2 <br>ověření: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</font>
      </td>
    </tr>
    <tr>
      <td>Systém souborů</td>
      <td>Tabulky</td>
      <td>Soubor</td>
      <td>
        <font size=2>Protocol (protokol): soubor <br>ověření: {žádný, základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cesta</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Tabulky</td>
      <td>Adresář, soubor</td>
      <td>
        <font size=2>Protocol (protokol): ftp <br>ověření: {žádný, základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Kontejner</td>
      <td>Obrázku</td>
      <td>
        <font size=2>Protocol (protokol): webhdfs <br>ověření: {základní, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Tabulky</td>
      <td>Adresář, soubor</td>
      <td>
        <font size=2>Protocol (protokol): webhdfs <br>ověření: {základní, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Podregistru</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protocol (protokol): podregistru <br>ověření: {hdinsight, základní, uživatelské jméno, žádné} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Podregistru</td>
      <td>Tabulky</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protocol (protokol): podregistru <br>ověření: {hdinsight, základní, uživatelské jméno, žádné} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Nastavit informace http</td>
      <td>Kontejner</td>
      <td>Web</td>
      <td>
        <font size=2>Protocol (protokol): http <br>ověření: {žádný, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Nastavit informace http</td>
      <td>Sestava</td>
      <td>Sestavy řídicího panelu</td>
      <td>
        <font size=2>Protocol (protokol): http <br>ověření: {žádný, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Nastavit informace http</td>
      <td>Tabulky</td>
      <td>Koncový bod, soubor</td>
      <td>
        <font size=2>Protocol (protokol): http <br>ověření: {žádný, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protocol (protokol): mysql <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Tabulky</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protocol (protokol): mysql <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Kontejner</td>
      <td>Entita kontejneru</td>
      <td>
        <font size=2>Protocol (protokol): odata <br>ověření: {žádný, základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Tabulky</td>
      <td>Entity sady (funkce)</td>
      <td>
        <font size=2>Protocol (protokol): odata <br>ověření: {žádný, základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;zdroje</font>
      </td>
    </tr>
    <tr>
      <td>Databáze Oracle</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protocol (protokol): oracle <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>Databáze Oracle</td>
      <td>Tabulky</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protocol (protokol): oracle <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protocol (protokol): postgresql <br>ověření: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>Postgresql</td>
      <td>Tabulky</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protocol (protokol): postgresql <br>ověření: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Kontejner</td>
      <td>Web</td>
      <td>
        <font size=2>Protocol (protokol): http <br>ověření: {žádný, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Sestava</td>
      <td>Sestavy řídicího panelu</td>
      <td>
        <font size=2>Protocol (protokol): http <br>ověření: {žádný, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Tabulky</td>
      <td>Hybridní webové aplikace dat</td>
      <td>
        <font size=2>Protocol (protokol): power query <br>ověření: {oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Služby Salesforce</td>
      <td>Tabulky</td>
      <td>Objekt</td>
      <td>
        <font size=2>Protocol (protokol): com služby salesforce <br>ověření: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;třídy <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemName</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Kontejner</td>
      <td>Server</td>
      <td>
        <font size=2>Protocol (protokol): sap hana sql <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Tabulky</td>
      <td>Zobrazení</td>
      <td>
        <font size=2>Protocol (protokol): sap-hana-sql <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>Služby SharePoint</td>
      <td>Tabulky</td>
      <td>Seznam</td>
      <td>
        <font size=2>Protocol (protokol): seznam služby sharepoint <br>ověření: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Datový sklad SQL</td>
      <td>Příkaz</td>
      <td>Uložená procedura</td>
      <td>
        <font size=2>Protocol (protokol): Neuvedeno <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>Datový sklad SQL</td>
      <td>TableValuedFunction</td>
      <td>Funkce vracející tabulku</td>
      <td>
        <font size=2>Protocol (protokol): Neuvedeno <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>Datový sklad SQL</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protocol (protokol): Neuvedeno <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>Datový sklad SQL</td>
      <td>Tabulky</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protocol (protokol): Neuvedeno <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Příkaz</td>
      <td>Uložená procedura</td>
      <td>
        <font size=2>Protocol (protokol): Neuvedeno <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Funkce vracející tabulku</td>
      <td>
        <font size=2>Protocol (protokol): Neuvedeno <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protocol (protokol): Neuvedeno <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Tabulky</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protocol (protokol): Neuvedeno <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimenzionální</td>
      <td>Kontejner</td>
      <td>Model</td>
      <td>
        <font size=2>Protocol (protokol): služby analysis services <br>ověření: {windows, základní, anonymní, žádné} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimenzionální</td>
      <td>KLÍČOVÝ UKAZATEL VÝKONU</td>
      <td>KLÍČOVÝ UKAZATEL VÝKONU</td>
      <td>
        <font size=2>Protocol (protokol): služby analysis services <br>ověření: {windows, základní, anonymní, žádné} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vztah mezi typem objektu: {klíčového ukazatele výkonu}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimenzionální</td>
      <td>Míra</td>
      <td>Míra</td>
      <td>
        <font size=2>Protocol (protokol): služby analysis services <br>ověření: {windows, základní, anonymní, žádné} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vztah mezi typem objektu: {míra}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimenzionální</td>
      <td>Tabulky</td>
      <td>Dimenze</td>
      <td>
        <font size=2>Protocol (protokol): služby analysis services <br>ověření: {windows, základní, anonymní, žádné} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vztah mezi typem objektu: {dimenzi}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabulky</td>
      <td>Kontejner</td>
      <td>Model</td>
      <td>
        <font size=2>Protocol (protokol): služby analysis services <br>ověření: {windows, základní, anonymní, žádné} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabulky</td>
      <td>KLÍČOVÝ UKAZATEL VÝKONU</td>
      <td>KLÍČOVÝ UKAZATEL VÝKONU</td>
      <td>
        <font size=2>Protocol (protokol): služby analysis services <br>ověření: {windows, základní, anonymní, žádné} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vztah mezi typem objektu: {klíčového ukazatele výkonu}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabulky</td>
      <td>Míra</td>
      <td>Míra</td>
      <td>
        <font size=2>Protocol (protokol): služby analysis services <br>ověření: {windows, základní, anonymní, žádné} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vztah mezi typem objektu: {míra}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabulky</td>
      <td>Tabulky</td>
      <td>Tabulky</td>
      <td>
        <font size=2>Protocol (protokol): služby analysis services <br>ověření: {windows, základní, anonymní, žádné} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;vztah mezi typem objektu: {tabulka}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Kontejner</td>
      <td>Server</td>
      <td>
        <font size=2>Protocol (protokol): služby reporting services <br>ověření: {windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Sestava</td>
      <td>Sestava</td>
      <td>
        <font size=2>Protocol (protokol): služby reporting services <br>ověření: {windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cesta <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protocol (protokol): teradata <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Tabulky</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protocol (protokol): teradata <br>ověření: {Protocol (protokol), windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server předlohy datové služby</td>
      <td>Kontejner</td>
      <td>Model</td>
      <td>
        <font size="2">Protocol (protokol): mssql strategie minimálního stahování <br>ověření: {windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server předlohy datové služby</td>
      <td>Tabulky</td>
      <td>Entita</td>
      <td>
        <font size="2">Protocol (protokol): mssql strategie minimálního stahování <br>ověření: {windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Entita</font>
      </td>
    </tr>
    <tr>
      <td>Druhý (není výše uvedené)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>Protocol (protokol): Obecné materiálů <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Id_položky</font>
      </td>
    </tr>
</table>
