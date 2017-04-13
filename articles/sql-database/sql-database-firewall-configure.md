<properties
   pageTitle="Konfigurace Přehled brány firewall pro SQL server | Microsoft Azure"
   description="Zjistěte, jak nakonfigurovat bránu firewall databáze SQL s pravidly server a databázi úrovně bránu firewall a spravovat přístup."
   keywords="Brána firewall databáze"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor="cgronlun"
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/14/2016"
   ms.author="rickbyh"/>

# <a name="configure-azure-sql-database-firewall-rules---overview"></a>Její konfiguraci databáze SQL Azure \- základní informace


> [AZURE.SELECTOR]
- [Základní informace](sql-database-firewall-configure.md)
- [Azure portálu](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [Prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [ROZHRANÍ REST API](sql-database-configure-firewall-settings-rest.md)


Databázi Microsoft Azure SQL obsahuje relační databáze služby Azure nebo jiných aplikacích pro internetové. Abychom ochránili vaše data, brány firewall úplně zabránit přístupu k databázovému serveru, dokud zadáte počítače, které smíte. Brána firewall uděluje přístup k databázím založený na původní IP adresa každý požadavek.

Konfigurace brány firewall, vytvoříte pravidla brány firewall, které určují rozsahy přijatelné IP adres. Vytvoření pravidla brány firewall na úrovni serveru a databáze.

- **Pravidla brány firewall úrovni serveru:** Tato pravidla umožňují klientům umožnit přístup k celé Azure SQL serveru, tedy všech databází ve stejném logické serveru. Tato pravidla jsou uložena v **hlavní** databáze. Pravidla brány firewall úrovni serveru je možné konfigurovat tak, že na portálu nebo pomocí příkazů jazyce Transact-SQL.
- **Pravidla brány firewall úrovni databáze:** Tato pravidla umožňují klientům umožnit přístup k jednotlivé databází v rámci serveru databáze SQL Azure. Vytváření pravidel pro každou databázi a jsou uloženy v jednotlivých databází. (Můžete vytvořit pravidla úrovni databáze brány firewall pro **hlavní** databáze.) Tato pravidla může být užitečné pro omezení přístupu k určité (zabezpečení) databází ve stejném logické serveru. Pravidla brány firewall úrovni databáze je možné konfigurovat pouze pomocí příkazů jazyce Transact-SQL.

**Doporučení:** Microsoft doporučuje používat databázi brána firewall pravidla kdykoli je to možné, můžete zvýšit zabezpečení a tím obecnější databáze. Používejte pravidla brány firewall úrovni serveru pro správce a máte mnoho databází, které mají stejné požadavky přístup a nechcete, aby se vám konfigurace každou databázi jednotlivě.


## <a name="firewall-overview"></a>Přehled brány firewall

Nejprve je blokován všechny jazyce Transact-SQL přístup k serveru Azure SQL bránou firewall. Pokud chcete začít používat Azure SQL serveru, musíte přejděte na portál Azure a zadat jeden nebo více pravidla serveru Brána firewall, která povolení přístupu k serveru Azure SQL. Můžete zadat jsou povoleny které rozsahy IP adres z Internetu a zda Azure aplikace můžete pokuste se připojit k serveru Azure SQL pomocí pravidel bránu firewall.

Selektivně udělit přístup jenom jeden z databáze na serveru Azure SQL, musíte vytvořit pravidlo úrovni databáze pro požadované databázi. Určete rozsah IP adres pro pravidlo brány firewall databáze, který přesahuje rozsah IP adres podle pravidlo brány firewall úrovni serveru a zrušte zaškrtnutí, IP adresa klienta spadá do rozsah popsaný v pravidlech úrovni databáze.

Pokusy o připojení z Internetu a Azure nejdřív dospět přes bránu firewall mohli kontaktovat Azure SQL server nebo SQL databáze, jak je znázorněno na následujícím obrázku.

   ![Diagram s popisem konfigurace brány firewall.][1]

## <a name="connecting-from-the-internet"></a>Připojení z Internetu

Když počítači se pokusí připojení k databázovému serveru z Internetu, bránu firewall nejprve zkontroluje původní IP adresa požadavek na úplné sady pravidel brány firewall:

- Nachází-li IP adresu žádost v jedné oblasti zadané v pravidlech brány firewall úrovni serveru, uděleno připojení k databázi SQL Azure serveru.

- Nejsou-li IP adresu žádost v jedné oblasti zadané v pravidle brány firewall úrovni serveru, zkontrolují pravidla databáze brána firewall. Nachází-li IP adresu žádost v jedné oblasti zadané v pravidlech brány firewall úrovni databáze, připojení uděleno pouze databázi obsahující pravidlo odpovídající úrovni databáze.

- Nejsou-li IP adresu žádost v oblasti zadané v libovolnému pravidlu úrovni serveru nebo databáze bránu firewall, dojde k chybě žádost o připojení.

> [AZURE.NOTE] Přístup k databázi SQL Azure z místního počítače, zajistěte, aby že bránu firewall na síť a místním počítači umožňuje odchozí komunikaci na TCP port 1433.


## <a name="connecting-from-azure"></a>Připojení z Azure

Když aplikace z Azure pokusí připojení k databázovému serveru, bránu firewall ověřuje, zda je povolen Azure připojení. Brána firewall nastavení s počátečním a koncovým adresu rovno 0.0.0.0 označuje, že jsou povoleny tato připojení. Pokud není povolená pokus o připojení, žádost kontaktovat není server databáze SQL Azure.

> [AZURE.IMPORTANT] Tato možnost nastaví bránu firewall na Povolit všechna připojení z Azure včetně připojení z předplatných od ostatních zákazníků. Pokud vyberete tuto možnost, zkontrolujte přihlašovací jméno a uživatelská oprávnění omezit přístup jenom uživatelé.

Můžete povolit připojení z Azure dvěma způsoby:

- Na [portálu Microsoft Azure](https://portal.azure.com/)zaškrtněte políčko **Povolit, aby access server azure služby** při vytváření nového serveru.

- V [klasickém portál](http://go.microsoft.com/fwlink/p/?LinkID=161793), na kartě **Konfigurovat** na serveru, v části **Povolené služby** klikněte na **Ano** **Služeb**společnosti Microsoft Azure.

## <a name="creating-the-first-server-level-firewall-rule"></a>Vytvoření první pravidlo brány firewall úrovni serveru

První nastavení brány firewall úrovni serveru se dají vytvořit pomocí [Azure portál](https://portal.azure.com/) nebo programové rozhraní REST API nebo Azure Powershellu. Pravidla následné brány firewall úrovni serveru se dají vytvářet a spravovat pomocí těchto metod i jako prostřednictvím jazyce Transact-SQL. Aby se zvýšil výkon, jsou pravidla brány firewall úrovni serveru dočasně mezipaměti na úrovni databáze. Abyste mohli aktualizovat mezipaměti, najdete v článku [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). Další informace na úrovni serveru firewall pravidlech najdete v tématu [jak: konfigurovat bránu firewall server Azure SQL pomocí portálu Azure](sql-database-configure-firewall-settings.md).

## <a name="creating-database-level-firewall-rules"></a>Vytváření pravidel brány firewall úrovni databáze

Po konfiguraci brány první úrovni serveru můžete omezit přístup k určitým databází. Pokud zadáte rozsah IP adres v pravidlech databáze brána firewall, která je mimo rozsah popsaný v pravidle brány firewall úrovni serveru, pouze klienty, kteří mají IP adres v oblasti úrovni databáze máte přístup k databázi. Můžete mít maximálně pravidla 128 úrovni databáze brány firewall pro danou databázi. Pravidla úrovni databáze brány firewall pro předlohy a uživatel databáze můžete vytváří a spravuje přes jazyce Transact-SQL. Další informace o konfiguraci databáze brána firewall pravidla najdete v článku [sp_set_database_firewall_rule (databáze SQL Azure)](https://msdn.microsoft.com/library/dn270010.aspx).

## <a name="programmatically-managing-firewall-rules"></a>Programově Správa pravidel brány firewall

Kromě portálu Azure lze spravovat pravidla brány firewall programově pomocí jazyce Transact-SQL, rozhraní REST API a Azure Powershellu. Následující tabulka popisuje sady příkazů umožňující obou metod.


### <a name="transact-sql"></a>Skalární funkce

| Zobrazení katalogu nebo uložená procedura                                                           | Úroveň     | Popis                                          |
|--------------------------------------------------------------------------------------------|-----------|------------------------------------------------------|
| [Sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx)                   | Server    | Zobrazuje aktuální pravidla serveru Brána firewall     |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx)             | Server    | Vytvoří nebo aktualizuje pravidla brány firewall úrovni serveru       |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx)          | Server    | Odstraní pravidla serveru Brána firewall                  |
| [Sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx)        | Databáze  | Zobrazuje aktuální databáze brána firewall pravidla   |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx)    | Databáze  | Vytvoří nebo aktualizuje pravidla databáze brána firewall |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) | Databáze | Odebere databáze úrovně pravidla brány firewall                |

### <a name="rest-api"></a>ROZHRANÍ REST API

| ROZHRANÍ API                  | Úroveň  | Popis                                                      |
|----------------------|--------|------------------------------------------------------------------|
| [Seznam pravidla brány Firewall](https://msdn.microsoft.com/library/azure/dn505715.aspx)  | Server | Zobrazuje aktuální pravidla serveru Brána firewall                 |
| [Vytvořit pravidlo brány Firewall](https://msdn.microsoft.com/library/azure/dn505712.aspx) | Server | Vytvoří nebo aktualizuje pravidla brány firewall úrovni serveru                   |
| [Nastavit pravidlo brány Firewall](https://msdn.microsoft.com/library/azure/dn505707.aspx)    | Server | Aktualizace vlastností existujícího pravidla serveru Brána firewall |
| [Odstranění pravidla brány Firewall](https://msdn.microsoft.com/library/azure/dn505706.aspx) | Server | Odstraní pravidla serveru Brána firewall                              |


### <a name="azure-powershell"></a>Azure Powershellu

| Rutina                                                                                              | Úroveň  | Popis                                                      |
|-----------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------------|
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx)    | Server | Vrátí aktuální pravidla serveru Brána firewall                  |
| [Nové AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx)    | Server | Vytvoří nové pravidlo brány firewall úrovni serveru                         |
| [Nastavení AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx)    | Server | Aktualizace vlastností existujícího pravidla serveru Brána firewall |
| [Odebrat AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) | Server | Odstraní pravidla serveru Brána firewall                              |

> [AZURE.NOTE] Je možné nahoru velmi podobným způsobem jako pět minut zpoždění pro změny nastavení se projeví brány firewall.

## <a name="troubleshooting-the-database-firewall"></a>Poradce při potížích s bránu firewall databáze

Při přístupu ke službě Microsoft Azure SQL databáze nechová podle očekávání, zvažte následující skutečnosti:

- **Místní brány firewall konfigurace:** Pokud váš počítač získat přístup k databázi SQL Azure, budete muset vytvořit výjimku brány firewall na svém počítači pro TCP port 1433. Pokud vytváříte připojení uvnitř hranici Azure cloudu, bude pravděpodobně zobrazíte další porty. Další informace najdete v tématu **V12 SQL databáze: mimo a uvnitř** část [porty za 1433 ADO.NET 4.5 a V12 databáze SQL](sql-database-develop-direct-route-ports-adonet-v12.md).

- **Sítě adresu překladu:** Z důvodu překladu síťových adres se liší od na IP adresu zobrazenou v nastavení počítače IP konfigurace IP adresu tak, že váš počítač používá pro připojení k databázi SQL Azure. Zobrazení IP adresu, kterou počítač používá pro připojení k Azure, přihlaste se k portálu a přejděte na kartu **Konfigurovat** na serveru, který je hostitelem vaší databáze. V části **Povolené IP adresy** se zobrazí na **Aktuální IP adresa klienta** . Klikněte na **Přidat** do **Povolené IP adresy** povolit na tomto počítači pro přístup k serveru.

- **Změny v seznamu Akce povolené neprojevila ještě:** Doména může obsahovat velmi podobným způsobem jako pět minut zpoždění pro změny konfigurace brány firewall databáze SQL Azure se projeví.

- **Nemá oprávnění přihlášení ani ho nepoužíval nesprávné heslo:** Pokud přihlášení nemá oprávnění na serveru databáze SQL Azure nebo použít heslo, je nesprávné, byl odepřen připojení k databázi SQL Azure serveru. Vytvoření nastavení brány firewall pouze poskytuje klientů příležitost pokusu o připojení k serveru. každého klienta musí poskytovat potřebné zabezpečovací přihlašovací údaje. Další informace o přípravě přihlášení najdete v tématu Správa databáze, přihlášení a uživatelů v databázi SQL Azure.

- **Dynamické IP adresa:** Pokud máte připojení k Internetu se dynamické adres IP a máte potíže s nastavením přes bránu firewall, může použijte jeden z následujících způsobů:

 - Požádejte poskytovatele internetových služeb (ISP) pro rozsah IP adres přiřazená klientské počítače, které přístup k databázi SQL Azure serveru a zadejte rozsah IP adres zpravidla bránu firewall.

 - Získat statické IP adresy místo pro klientské počítače a pak přidejte IP adresy pravidla brány firewall.

## <a name="next-steps"></a>Další kroky

Jak články o vytváření pravidel serveru a databáze úrovně brány firewall, najdete v článku:

- [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí portálu Azure](sql-database-configure-firewall-settings.md)
- [Konfigurace pravidel serveru a databáze úrovně brány firewall databáze SQL Azure pomocí T-SQL](sql-database-configure-firewall-settings-tsql.md)
- [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí prostředí PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Konfigurace pravidel brány firewall úrovni serveru databáze SQL Azure pomocí rozhraní REST API](sql-database-configure-firewall-settings-rest.md)

Kurz týkající se vytváření databáze najdete v článku [vytvoření databáze SQL Azure portálu minut](sql-database-get-started.md).
Nápověda ke službě připojení k databázi Azure SQL z otevřít zdroj nebo aplikací jiných výrobců najdete v článku [klienta rychlého ukázky k SQL databázi](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Jak přejít do databáze, najdete v tématu [Správa databáze aplikace access a přihlašovací zabezpečení](https://msdn.microsoft.com/library/azure/ee336235.aspx).



## <a name="additional-resources"></a>Další zdroje informací

- [Zabezpečení databáze](sql-database-security.md)
- [Centrum zabezpečení pro SQL Server databázový stroj a databáze Azure SQL](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
