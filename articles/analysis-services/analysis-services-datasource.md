<properties
   pageTitle="Připojení zdroje dat | Microsoft Azure"
   description="Popisuje připojení ke zdrojům dat pro datové modely v Azure Analysis Services."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="datasource-connections"></a>Připojení zdroje dat

Datové modely v Azure Analysis Services může vyžadovat různé datové poskytovatelů při připojování k určitým zdrojům dat. V některých případech může tabulkovém modelu služby připojení ke zdrojům dat pomocí nativního poskytovatelů třeba SQL Server Native Client (SQLNCLI11) vrátí chybu.

Příklad Pokud máte v paměti nebo přímý dotaz datového modelu, ke kterému je připojen cloudové zdroje dat jako jsou databáze SQL Azure, použijete nativní poskytovateli služeb, než SQLOLEDB, může zobrazit chybová zpráva: **"Zprostředkovateli, který není registrovaný"SQLNCLI11.1""**.

Nebo pokud máte modelu DirectQuery připojování k místním zdrojům dat, pokud používáte nativní poskytovatelů může zobrazit chybová zpráva: **"Chyba při vytváření sada řádku OLE DB. Chybná syntaxe u 'LIMIT' "**.

## <a name="data-source-providers"></a>Zprostředkovatele zdrojů dat

Těchhle poskytovatelů zdroj dat je podporované v paměti nebo přímé dotazu datové modely při připojování k místního nebo cloudového zdroje dat:

|               | **Zdroj dat**                     | **V paměti**                            |  **Přímý dotaz**                                           |
|---------------------------|-------------------------------|---------------------------------------------|---------------------------------------------|
| **Cloud**                     | Datový sklad Azure SQL      | Zprostředkovatel dat .NET framework pro systém SQL Server | Zprostředkovatel dat .NET framework pro systém SQL Server |
|                           | Databáze Azure SQL            | Zprostředkovatel dat .NET framework pro systém SQL Server | Zprostředkovatel dat .NET framework pro systém SQL Server |
| **Místní** (přes brány) | SQL Server                    | SQL Server Native 11.0 Client               | Zprostředkovatel dat .NET framework pro systém SQL Server |
|                           |  SQL Server                             | Zprostředkovatel Microsoft OLE DB pro systém SQL Server    |   Zprostředkovatel dat .NET framework pro systém SQL Server                                          |
|                           |  SQL Server                             | Zprostředkovatel dat .NET framework pro systém SQL Server |  Zprostředkovatel dat .NET framework pro systém SQL Server                                           |
|                           | Oracle                        | Zprostředkovatel Microsoft OLE DB pro Oracle        | Zprostředkovatel dat Oracle pro .NET               |
|                           |  Oracle                             | Zprostředkovatel dat Oracle pro .NET               | Zprostředkovatel dat Oracle pro .NET                                            |
|                           | Teradata                      | Zprostředkovatel OLE DB pro Teradata                | Zprostředkovatel dat Teradata pro .NET             |
|                           |  Teradata                             | Zprostředkovatel dat Teradata pro .NET             |  Zprostředkovatel dat Teradata pro .NET                                            |
|                           | Technologie pro analýzu platformu systému | Zprostředkovatel dat .NET framework pro systém SQL Server | Zprostředkovatel dat .NET framework pro systém SQL Server |


> [AZURE.NOTE] Zajistěte, aby poskytovatelů 64bitových nainstalovaných při použití místního brány.

Když migrace místní tabulkového modelu služby SQL Server Analysis Services na Azure Analysis Services, může být nutné změnit poskytovatele.

**Chcete-li zadat zprostředkovatele zdroj dat**

1. V SSDT > **Tabulkovém modelu Explorer** > **Zdroje dat**, klikněte pravým tlačítkem myši připojení zdroje dat a potom klikněte na **Upravit zdroj dat**.

2. V okně **Upravit připojení**klikněte na **Upřesnit** a otevřete okno Vlastnosti zálohy.

3. V **Nastavení upřesňujících vlastnostech** > **Zprostředkovatelé**a pak vyberte příslušný poskytovatel.

## <a name="impersonation"></a>Zosobnění
V některých případech může být nutné zadat jinou zosobnění účet. Účet zosobnění může být zadán v SSDT nebo SSMS.

Pro místní zdroje dat:

- Pokud používáte ověřování serveru SQL, zosobnění by měl být účet služby.
- Pokud pomocí ověřování Windows, nastavte Windows uživatele a hesla. Pro systém SQL Server ověřování systému Windows pod svým účtem konkrétní zosobnění podporované jenom pro v paměti datových modelů.

Pro cloudové zdroje dat:

- Pokud používáte ověřování serveru SQL, zosobnění by měl být účet služby.


## <a name="next-steps"></a>Další kroky

Pokud máte místní zdroje dat, je potřeba nainstalovat [místní brány](analysis-services-gateway.md). Další informace o správě serveru v SSDT nebo SSMS najdete v tématu [Správa serveru](analysis-services-manage.md).
