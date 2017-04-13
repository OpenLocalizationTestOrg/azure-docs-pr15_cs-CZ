<properties
   pageTitle="Pomocí datový sklad SQL Azure toku analýzy | Microsoft Azure"
   description="Tipy pro použití analýzy toku Azure s Azure SQL datový sklad k vývoji řešení."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Pomocí datový sklad SQL Azure toku analýzy

Azure analýzy toku je plně spravovaných služba poskytuje zpracování nízkou latencí vysoce dostupné, scalable složité události nad přenos dat v cloudu. Naučte se základy v tématu [Úvod do technologie pro analýzu toku Azure][]. Je můžete pak Naučte se vytvářet řešení začátku do konce s toku analýzy pomocí následujících kurz [začít používat Azure toku analýzy][] .

V tomto článku se naučíte používat databázi SQL Azure datový sklad jako jímky výstup páry analýzy projektů.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Nejdřív spusťte následujícími kroky v tomto kurzu [začít používat Azure toku analýzy][] .  

1. Vytvoření události centrální vstup
2. Konfiguraci a spuštění aplikace generátor události
3. Zřízení úlohy toku analýzy
4. Zadejte úlohu vstupní a dotaz

Vytvořte databázi SQL Azure datový sklad

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Určení výstup projektu: datový sklad Azure SQL databáze

### <a name="step-1"></a>Krok 1

V toku analýzy práce klikněte **výstup** v horní části stránky a potom klikněte na **Přidat výstup**.

### <a name="step-2"></a>Krok 2

Vyberte databázi SQL a klikněte na další.

![][add-output]

### <a name="step-3"></a>Krok 3
Na další stránce zadejte následující hodnoty:

- *Výstup Alias*: Zadejte popisný název pro tento výstup projektu.
- *Předplatné*:
    - Pokud je databáze SQL datový sklad ve stejném předplatném jako úlohu toku analýzy, vyberte použít SQL databázi z aktuálního předplatného.
    - Pokud je databáze v jiné předplatné, vyberte použít SQL databázi jiné předplatné.
- *Databáze*: Zadejte název cílové databázi.
- *Název serveru*: Zadejte název serveru pro databázi jste zadali. Klasický portálu Azure umožňuje našli toto.

![][server-name]

- *Uživatelské jméno*: Zadejte uživatelské jméno účtu, který má oprávnění k zápisu pro databázi.
- *Heslo*: zadat heslo pro zadaný uživatelský účet.
- *Tabulka*: Zadejte název cílové tabulky v databázi.

![][add-database]

### <a name="step-4"></a>Krok 4

Klikněte na tlačítko Kontrola přidat tento výstup projektu a ověřte, že toku analýzy můžete úspěšně se připojit k databázi.

![][test-connection]

Po úspěšném připojení k databázi, zobrazí se oznámení v dolní části na portálu. Klikněte na Testovat připojení můžete v dolní části test připojení k databázi.

## <a name="next-steps"></a>Další kroky

Přehled integrace naleznete v tématu [Přehled integrace SQL datový sklad][].

Další tipy vývoj najdete v tématu [Přehled vývoje SQL datový sklad][].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Úvod do technologie pro analýzu Azure toku]: ../stream-analytics/stream-analytics-introduction.md
[Začínáme s používáním analýzy toku Azure]: ../stream-analytics/stream-analytics-get-started.md
[Přehled vývoje SQL datový sklad]:  ./sql-data-warehouse-overview-develop.md
[Přehled integrace SQL datový sklad]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
