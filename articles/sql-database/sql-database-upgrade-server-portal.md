<properties
    pageTitle="Upgrade na V12 databáze SQL Azure pomocí portálu Azure | Microsoft Azure"
    description="Vysvětluje, jak upgradovat na V12 databáze SQL Azure včetně upgrade webu a Business databází a upgrade serveru V11 migrace své databáze přímo do fondu pružná databáze pomocí portálu Azure."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/08/2016"
    ms.author="sstein"/>


# <a name="upgrade-to-azure-sql-database-v12-using-the-azure-portal"></a>Upgrade na V12 databáze SQL Azure pomocí portálu Azure


> [AZURE.SELECTOR]
- [Azure portálu](sql-database-upgrade-server-portal.md)
- [Prostředí PowerShell](sql-database-upgrade-server-powershell.md)


V12 databáze SQL je nejnovější verze, takže upgrade stávající servery k SQL databázi V12 se doporučuje.
V12 SQL databáze obsahuje mnoho včetně [výhody předchozí verze](sql-database-v12-whats-new.md) :

- Lepší kompatibilitu se serverem SQL Server.
- Vylepšené premium výkon a nové úrovně výkonu.
- [Pružná databáze fondů](sql-database-elastic-pool.md).

Tento článek obsahuje pokyny pro upgrade stávající servery V11 databáze SQL a databází V12 databáze SQL.

Během procesu upgradu a V12 můžete upgraduje Web a Business databází nové vrstvy služeb, pokyny pro upgrade webu a Business databáze jsou však započítávány.

Kromě toho migraci do [fondu pružná databázi](sql-database-elastic-pool.md) lze nákladů efektivnější než upgradu na jednotlivé výkonu úrovně (cena za úrovní) pro jednoho databáze. Fondů taky zjednodušení správy databáze vzhledem k tomu potřebujete spravovat nastavení výkonu pro fondu ne odděleně Správa úrovní výkonu jednotlivé databází. Pokud máte databází ve více serverů zvažte přesunutí do stejného serveru a využijete uvedení do fondu. Můžete snadno [automatického přenést databáze z V11 servery přímo do skupin pružná databáze pomocí Powershellu](sql-database-upgrade-server-powershell.md). Můžete také portálu migrovat V11 databáze do fondu, ale na portálu je nutné V12 serveru vytvoření fondu. Pokyny jsou k dispozici dál v tomto článku k vytvoření fondu po upgradu serveru, pokud máte [databází, které můžete využít fond](sql-database-elastic-pool-guidance.md).

Všimněte si, že databáze zůstat online a pokračovat v práci během operace upgradu. V době skutečné přechodu na nový výkonu úroveň dočasné přetažením připojení k databázi může dojít po příliš malý dobu, obvykle se asi 90 sekund ale může mít až 5 minut. Pokud má vaše aplikace [přechodná poruch zpracování pro ukončení připojení](sql-database-connectivity-issues.md) je dostatečně vysoká pro ochranu proti zamítnuté připojení na konci upgradu.

Upgrade na SQL databáze V12 nelze je vrátit zpět. Po upgradu nelze serveru vrátit do V11.

Po upgradu na V12, [doporučení osy služby](sql-database-service-tier-advisor.md) a [Důležité informace o výkonu pružná fondu](sql-database-elastic-pool-guidance.md) není možné okamžitě dokud službu nějaký čas na vyhodnocení úloh na novém serveru. V11 serveru doporučení historie, nebudou mít vliv na V12 servery tak, aby se zachovají.

## <a name="prepare-to-upgrade"></a>Příprava na upgrade

- **Upgrade všechny databáze pro Web a firmy**: najdete v článku [Upgrade všechny databáze pro Web a obchodní](sql-database-upgrade-server-portal.md#upgrade-all-web-and-business-databases) oddílu pod nebo najdete v článku [sledování a správa fondu pružná databáze (Powershellu)](sql-database-elastic-pool-manage-powershell.md).
- **Revize a pozastavení Geo replikace**: Pokud databázi Azure SQL nakonfigurovaný pro Geo replikace by měl dokumentu jeho aktuální konfigurace a [Zastavit Geo replikace](sql-database-geo-replication-portal.md#remove-secondary-database). Po dokončení upgradu překonfigurujte Geo replikace databáze.
- **Otevřete tyto porty, pokud máte klienty v Azure OM**: Pokud klientským programem připojuje k SQL databázi V12 v průběhu svému klientovi na Azure virtuálního počítače (OM), musíte otevřít rozsahy portů 11000 11999 a 14000 14999 na OM. Podrobnosti najdete v tématu [porty V12 databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md).



## <a name="start-the-upgrade"></a>Spuštění upgradu

1. V [Azure portál](https://portal.azure.com/) Procházet k serveru chcete upgradovat tak, že vyberete **Procházet** > **servery SQL**a výběrem server verze 2.0 chcete upgradovat.
2. Vyberte **Nejnovější aktualizace databáze SQL**a potom vyberte možnost **Upgrade tento server**.

      ![Upgrade serveru][1]

3. Upgrade na nejnovější aktualizace databáze SQL serveru je trvalý a nevratné. Abyste si ověřili upgradu, zadejte název serveru a klikněte na **OK**.

## <a name="upgrade-all-web-and-business-databases"></a>Upgrade všechny databáze Web pro podnikatele

Pokud váš server má Web nebo Business databáze je třeba upgradovat. Během procesu upgradu a V12 SQL databáze se aktualizuje všechny databáze pro Web a obchodní nové vrstvy služeb.    

Která vám pomůže s upgradu, doporučuje služby SQL databáze příslušné služby osy a výkonu úroveň (cena za úrovně) na každou databázi. Služba doporučuje nejlepší vrstvy pro spuštění pracovního vytížení existující databázi analýzou historické použití databáze.

3. V zásuvné **Upgrade tento server** vyberte každý databáze kontroloval a doporučené ceny úroveň upgradovat. Vždy můžete procházet dostupných ceny úrovní a vyberte účet, který nejlíp vyhovuje prostředí.


     ![databáze][2]


7. Po kliknutí na navrhované osy zobrazí s zásuvné **Zvolit ceny osy** , kde můžete vybrat vrstva a potom klikněte na tlačítko **Vybrat** přejdete k této osy. Vyberte nové osy na každou databázi Web nebo Business.

    Co je důležité mít na paměti je, že databázi SQL není do libovolné konkrétní službu osy nebo výkonu úrovni tak jako požadavky uložte změnu databáze můžete heslo snadno změnit mezi dostupnými službami, které úrovní a výkonu úrovně. Ve skutečnosti Basic, standardních a Premium SQL databáze je faktura vystavená hodiny a máte možnost zobrazit každou databázi nahoru nebo dolů 4 během 24 hodin.

    ![doporučení][6]


Po všechny databáze na serveru se dá použít budete chtít spustit upgrade

## <a name="confirm-the-upgrade"></a>Potvrďte upgradu

3. Přestože všechny databáze na serveru pro upgrade potřebujete **Typ název serveru** pro ověření, že chcete upgradu a pak klikněte na **OK**.

    ![ověření upgrade][3]


4. Upgrade spustí a zobrazí se v průběhu oznámení. Proces upgradu aktivováno. V závislosti na podrobností o konkrétní databází upgradu na V12 můžete nějakou dobu trvat. Během této doby zůstane všechny databáze na serveru online, ale server a databázi správy akce budou omezeny.

    ![Upgrade probíhá][4]

    V době skutečné přechodu na nový výkonu může dojít úroveň dočasné přetažením připojení k databázi příliš malý dobu (obvykle měřeno v sekundách). Pokud aplikace přechodná poruch zpracování (Logika opakování) pro ukončení připojení a pak stačí chránit před zamítnuté připojení na konci upgradu.

5. Po dokončení operace upgradu se prezentace zobrazí **povoleno**zásuvné **Nejnovější aktualizace** .

    ![V12 povolena][5]  

## <a name="move-your-databases-into-an-elastic-database-pool"></a>Přesuňte databází aplikace do fondu pružná databáze

[Azure portál](https://portal.azure.com/) vyhledejte V12 serveru a klikněte na **Přidat fondu**.

– nebo –

Pokud se zobrazí zpráva, **Kliknutím sem zobrazíte fondů doporučené pružná databáze pro tento server**, klikněte na a snadno vytvořit skupinu, která je optimalizovaná pro váš server databáze. Podrobnosti najdete v tématu [aspektech cena a výkonu pro fond pružná databáze](sql-database-elastic-pool-guidance.md).

![Přidání fondu na server][7]

Postupujte podle pokynů v článku [Vytvoření fondu pružná databáze](sql-database-elastic-pool.md) dokončete vytváření vašeho fondu.

## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Sledování databází po upgradu na SQL databáze V12

>[AZURE.IMPORTANT] Upgradujte na nejnovější verzi systému SQL Server Management Studio (SSMS) využít nové funkce v12. [Stáhnout SQL Server Management Studio] (https://msdn.microsoft.com/library/mt238290.aspx).

Po upgradu, aktivně sledovat databáze, kterou chcete zajistit, aby aplikace spuštěné na požadovaný výkon a optimalizujte nastavení podle potřeby.

Kromě sledování jednotlivé databází, můžete sledovat pružná databáze fondů [monitoru, spravovat a velikost fondu pružná databáze pomocí portálu Azure](sql-database-elastic-pool-manage-portal.md) nebo pomocí [Powershellu](sql-database-elastic-pool-manage-powershell.md).


**Zdroje dat spotřebu:** Pro Basic, Standard a Premium databáze je k dispozici až [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV v databázi uživatelů daty spotřebu zdrojů. Tento DMV poskytuje v reálném čase spotřebu informace o zdroji na 15 druhý granularity pro předchozí hodiny operace. Spotřebu DTU procentuální hodnotu pro interval počítá jako maximální procento využití procesoru, vstupu a výstupu a protokolu rozměry. Tady je dotazu pro výpočet průměru spotřebu procento DTU přes poslední hodiny:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Další informace o sledování:

- [Databáze SQL azure výkonu pokyny pro jeden databáze](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Ceny a výkonu aspektech pro fondu pružná databáze](sql-database-elastic-pool-guidance.md).
- [Sledování databáze SQL Azure pomocí Správa dynamických zobrazení](sql-database-monitoring-with-dmvs.md)




**Upozornění:** Nastavte "upozornění na portálu Azure oznámí, že spotřeba DTU pro databázi upgradovaném blíží určitých vysokou úroveň. Oznámení databáze může být instalace Azure portálu pro různé měřítka jako DTU, využití procesoru, vstupu a výstupu a protokolu. Přejděte do vaší databáze a vyberte **upozornění pravidla** v zásuvné **Nastavení** .

Například můžete nastavit e-mailovém upozornění na "DTU procentuální hodnotu" Pokud průměrná hodnota procenta DTU přesahuje 75 % přes poslední 5 minut. Další informace o tom, jak konfigurovat oznámení v nápovědě k [dostávat oznámení](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) .





## <a name="next-steps"></a>Další kroky

- [Vyhledat fondu doporučení a vytvoření fondu](sql-database-elastic-pool-create-portal.md).
- [Změna služby osy a výkonu úrovně databáze](sql-database-scale-up.md).



## <a name="related-links"></a>Související odkazy

- [Co je nového v SQL databázi V12](sql-database-v12-whats-new.md)
- [Plánování a příprava na upgrade na SQL databáze V12](sql-database-v12-plan-prepare-upgrade.md)


<!--Image references-->
[1]: ./media/sql-database-upgrade-server-portal/latest-sql-database-update.png
[2]: ./media/sql-database-upgrade-server-portal/upgrade-server2.png
[3]: ./media/sql-database-upgrade-server-portal/upgrade-server3.png
[4]: ./media/sql-database-upgrade-server-portal/online-during-upgrade.png
[5]: ./media/sql-database-upgrade-server-portal/enabled.png
[6]: ./media/sql-database-upgrade-server-portal/recommendations.png
[7]: ./media/sql-database-upgrade-server-portal/new-elastic-pool.png
