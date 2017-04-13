<properties
   pageTitle="Správa výpočetního výkonu v Azure SQL datový sklad (Azure portál) | Microsoft Azure"
   description="Azure úkoly portálu pro správu výpočetního výkonu. Měřítko výpočet zdroje úpravou DWUs. Nebo pozastavení a obnovení výpočet nákladů na zdroje."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Správa výpočetním power v Azure SQL datový sklad (Azure portál)

> [AZURE.SELECTOR]
- [Základní informace](sql-data-warehouse-manage-compute-overview.md)
- [Portál](sql-data-warehouse-manage-compute-portal.md)
- [Prostředí PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [ZBÝVAJÍCÍ](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Výkon měřítko tak, že rozšiřování vypočítat a materiály pro změnu požadavkům vaší pracovního vytížení paměti. Ušetřit náklady měřítka zpět zdrojů během není Špička nebo pozastavení výpočetním úplně odebrat. 

Tuto kolekci úkolech používá Azure portálu:

- Pro využití měřítko
- Pozastavit výpočetním
- Pro využití životopisu

Další informace najdete v tématu [Správa výpočet přehled][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Měřítko výpočetního výkonu

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Chcete-li změnit výpočetním zdroje:

1. Otevřete [Azure portál][], otevřete databázi a klikněte na **velikost**.

    ![Klikněte na velikost][1]

1. V měřítku zásuvné posunutím posuvníku doleva nebo doprava a změnit nastavení DWU.

    ![Přesuňte posuvník][2]

1. Klikněte na **Uložit**. Zobrazí se potvrzovací zpráva. Klikněte na tlačítko **Ano** potvrďte nebo **bez** zrušit.

    ![Klikněte na tlačítko Uložit][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pozastavit výpočetním

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Chcete-li pozastavit databáze:

1. Otevřete [portál Azure][] a otevřete databázi. Všimněte si, že stav **Online**. 

    ![Stav online][6]

1. Pozastavení výpočetním a prostředky paměti, klikněte na **Pozastavit**a potom potvrzovací zpráva se zobrazí. Klikněte na tlačítko **Ano** potvrďte nebo **bez** zrušit.

    ![Potvrďte pozastavit][7]

1. Během spouštění databáze SQL datový sklad je stav **pozastaveno**.
2. Pokud je stav **pozastaveno**, pozastavit proběhne a vám jsou už účtovány DWUs.

    ![Pozastavit stavu][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Pro využití životopisu

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]Chcete-li obnovit databázi:

1. Otevřete [portál Azure][] a otevřete databázi. Všimněte si, že stav **pozastaveno**. 

    ![Pozastavit databáze][4]

1. Obnovení databáze klikněte na tlačítko **Start**a potom se zobrazí zprávu s potvrzením. Klikněte na tlačítko **Ano** potvrďte nebo **bez** zrušit.

    ![Potvrďte životopisu][5]

1. Během spouštění databáze SQL datový sklad stav je "Resuming".
2. Pokud je stav **online**, je připraven databáze.

    ![Stav online][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Další kroky
Další informace najdete v tématu [Přehled správy][].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Přehled správy]: ./sql-data-warehouse-overview-manage.md
[Správa výpočetním přehled]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portálu]: http://portal.azure.com/
