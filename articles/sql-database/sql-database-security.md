<properties
   pageTitle="Přehled zabezpečení databáze SQL"
   description="Další informace o zabezpečení databáze SQL Azure a SQL Server, včetně rozdíly mezi cloudu a SQL Server místní až přijde ověření se tak mohli ověřovat, zabezpečení připojení, šifrování a dodržování předpisů."
   services="sql-database"
   documentationCenter=""
   authors="tmullaney"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="06/09/2016"
   ms.author="thmullan;jackr"/>


# <a name="securing-your-sql-database"></a>Zabezpečení databáze SQL

## <a name="overview"></a>Základní informace

Tento článek provede základní informace o zabezpečení osy dat aplikace přes databáze SQL Azure. Zejména v tomto článku vám pomůžou začít pracovat s prostředky pro omezení přístupu, ochrana dat, a sledování aktivity na databáze vytvořené v [začít pracovat s kurz databáze SQL](sql-database-get-started.md). Dokončení základní informace o zabezpečení funkce dostupné ve všech provedeních SQL naleznete v článku [Centrum zabezpečení pro SQL Server databázový stroj a databáze SQL Azure](https://msdn.microsoft.com/library/bb510589). Další informace najdete taky v [zabezpečení a databáze SQL Azure technické dokument white paper](https://download.microsoft.com/download/A/C/3/AC305059-2B3F-4B08-9952-34CDCA8115A9/Security_and_Azure_SQL_Database_White_paper.pdf) (PDF).

## <a name="connection-security"></a>Zabezpečení připojení

Zabezpečení připojení odkazuje na tom, jak omezit a zabezpečené připojení k databázi pomocí pravidel bránu firewall a šifrování připojení.

Pravidla brány firewall používají server a databázi odmítnout pokusy o připojení z IP adresy, které nebyly explicitně povolené. Umožňuje aplikace nebo veřejnou IP adresu klientský počítač se snaží připojení k databázi nové musí nejdřív vytvořit pravidlo brány firewall na úrovni serveru pomocí klasické portál Azure, rozhraní REST API nebo Powershellu. Osvědčený měli byste omezit povolené přes bránu firewall serveru co nejvíc rozsahy IP adres. Další informace najdete v tématu [Firewall databáze SQL Azure](https://msdn.microsoft.com/library/ee621782).

Všechna připojení k databázi SQL Azure šifrování (SSL/TLS) v vždy při dat "na cestě" do a z databáze. Vaše aplikace připojovacího řetězce, je nutné zadat parametry šifrovat připojení a *Ne* s informacemi o důvěryhodnosti certifikátu serveru (to se děje při kopírování připojovací řetězec z portálu klasické Azure), jinak připojení nebude ověřit identitu serveru bude nebezpečí útoků "člověka v – uprostřed". ADO.NET ovladače, jako je například tyto parametry připojovacího řetězce jsou **zašifrovat = True** a **TrustServerCertificate = False**. Další informace najdete v tématu [šifrování připojení databáze SQL Azure a ověřovací certifikát](https://msdn.microsoft.com/library/azure/ff394108#encryption).


## <a name="authentication"></a>Ověřování

Ověřování odkazuje na tom, jak prokázat vaši totožnost při připojení k databázi. Databáze SQL podporuje dva typy ověření:

 - **Ověřování serveru SQL**, který využívá uživatelské jméno a heslo
 - **Ověřování služby azure Active Directory**, které používá identit spravuje Azure Active Directory a je podporovaný pro a domény

Po vytvoření logické serveru databáze zadaná přihlašovací "správce serveru" pomocí jména a hesla. Pomocí těchto přihlašovacích údajů, které lze ověřit všechny databáze na serveru jako vlastník databáze nebo "dbo." Pokud chcete použít ověřování služby Azure Active Directory, musíte vytvořit jiné správce serveru s názvem "správce Azure AD," které bude moct spravovat Azure AD Uživatelé a skupiny. Tento správce lze provést také všechny operace, které můžete běžná server správce. Informace o tom, jak vytvořit správce Azure AD povolit ověřování služby Azure Active Directory najdete v tématu [připojení k SQL databázi tak, že pomocí Azure Active Directory, ověřování](sql-database-aad-authentication.md) .

Osvědčený používejte aplikace na jiný účet ověření – tímto způsobem můžete omezit oprávnění udělena aplikace a snížení rizik škodlivým aktivity v případě kód aplikace nebude ohroženo útok vkládání SQL. Doporučený postup je vytvořit [obsažené uživatel databáze](https://msdn.microsoft.com/library/ff929188), které umožňuje aplikace ověření přímo do jedné databáze. Můžete vytvořit i databáze uživatel, který používá ověřování serveru SQL spuštěním následujícího příkazu T SQL při připojení k databázi uživatele jako správce přihlašovací server:

```
CREATE USER ApplicationUser WITH PASSWORD = 'strong_password'; -- SQL Authentication
```

Pokud jste vytvořili Azure AD správce, můžete vytvořit i databáze uživatel, který používá ověřování služby Azure Active Directory spuštěním následujícího příkazu T SQL při připojení k databázi uživatele jako správce Azure AD:

```
CREATE USER [Azure_AD_principal_name | Azure_AD_group_display_name] FROM EXTERNAL PROVIDER; -- Azure Active Directory Authentication
```

V obou případech aplikace připojovací řetězec zadají tyto přihlašovací údaje uživatele pro, nikoli přihlášení správce serveru, připojení k databázi.

Další informace o ověřování k databázi SQL najdete v článku [Správa databází a přihlášení v databázi SQL Azure](sql-database-manage-logins.md).


## <a name="authorization"></a>Povolení
Se tak mohli ověřovat odkazuje co můžete dělat v databázi SQL Azure, a to se řídí členství svůj uživatelský účet role a oprávnění. Osvědčený udělte uživatelům alespoň oprávnění. Databáze SQL Azure usnadňuje to správa rolí v T-SQL:

```
ALTER ROLE db_datareader ADD MEMBER ApplicationUser; -- allows ApplicationUser to read data
ALTER ROLE db_datawriter ADD MEMBER ApplicationUser; -- allows ApplicationUser to write data
```

Účet správce serveru, ke kterému se připojujete patří do skupiny db_owner, který má oprávnění Hotovo v databázi. Uložte pro nasazení upgradů schématu a dalších operací správy tento účet. Pomocí účtu "ApplicationUser" omezenější oprávnění připojit z aplikace do databáze s nejnižšími oprávněními potřeby tak, že aplikace.

Způsoby dále omezit, co můžete dělat uživatel s databáze SQL Azure:

* [Role databáze](https://msdn.microsoft.com/library/ms189121) než db_datareader a db_datawriter lze vytvářet výkonnější aplikace uživatelské účty nebo méně výkonných správy účty.
* Podrobného [oprávnění](https://msdn.microsoft.com/library/ms191291) umožňují řídit operací, které můžete dělat v jednotlivých sloupců tabulek, zobrazení, postupů a jiných objektů v databázi.
* [Zosobnění](https://msdn.microsoft.com/library/vstudio/bb669087) a [modulu podepisování](https://msdn.microsoft.com/library/bb669102) mohou sloužit k bezpečnému dočasně zvýšení oprávnění.
* [Řádek úrovně zabezpečení](https://msdn.microsoft.com/library/dn765131) mohou sloužit přístup limit řádků uživatele.
* [Data maskování](sql-database-dynamic-data-masking-get-started.md) mohou sloužit k vystaven citlivá data.
* [Uložené procedury](https://msdn.microsoft.com/library/ms190782) mohou sloužit k omezit akce, které můžou brát databázi.

Správa databází a logické servery z portálu Microsoft Azure klasické nebo pomocí rozhraní API Správce prostředků Azure řídí přiřazování rolí portálu uživatelský účet. Další informace o tomto tématu najdete v tématu [řízení přístupu na základě rolí v portálu Azure](../active-directory./role-based-access-control-configure.md).


## <a name="encryption"></a>Šifrování

Databáze SQL Azure ochranu dat zašifrováním datům, když je "zbývající" nebo uložené v soubory databáze a zálohování pomocí [Šifrování průhlednosti](http://go.microsoft.com/fwlink/?LinkId=526242). Postup pro šifrování databáze, připojit jako vlastník databáze a provést:

```
ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

Další způsoby šifrování tajemství dat zvažte možnost:

* [Šifrování úrovni buněk](https://msdn.microsoft.com/library/ms179331.aspx) zašifrovat konkrétních sloupců nebo dokonce buněk s daty pomocí různých šifrovacího klíče.
* Pokud potřebujete hardwaru zabezpečení modulu nebo Centrální správa šifrovacího klíče hierarchie, zvažte použití [Azure klíč trezoru se serverem SQL Server v Azure OM](http://blogs.technet.com/b/kv/archive/2015/01/12/using-the-key-vault-for-sql-server-encryption.aspx).
* [Vždy zašifrovaných](https://msdn.microsoft.com/library/mt163865.aspx) (v náhledu) umožňuje šifrování průhledné aplikací a umožňuje klientům zašifrovat citlivá data v klientských aplikacích bez šifrovacího klíče nasdílet databáze SQL.

## <a name="auditing"></a>Sestavy auditování

Sestavy auditování a sledování databáze události můžete udržovat dodržování předpisů a identifikovat podezřelé aktivity. Sestavy auditování databáze SQL umožňuje že záznamu událostí v databázi s auditem přihlášení svůj účet Azure úložiště. Sestavy auditování databáze SQL také lze integrovat s Microsoft Power BI k usnadnění přechodu na podrobnosti sestavy a analýzy. Další informace najdete v tématu [Začínáme s SQL databáze auditování](sql-database-auditing-get-started.md).

Zjišťování hrozbou, že databáze SQL poskytuje další úroveň zabezpečení nad auditování. Umožňuje odpovědět na hrozeb při jejich vzniku zadáním výstrah zabezpečení na neobvyklých aktivity. Další informace najdete v tématu [Začínáme s zjišťování hrozbou, že databáze SQL](sql-database-threat-detection-get-started.md).  

## <a name="compliance"></a>Dodržování předpisů

Kromě výše uvedených funkcí a funkcí, které můžou pomoct aplikace požadavkům různých zabezpečení dodržování předpisů, databáze SQL Azure také se účastní běžná audity a získalo proti počet organizace pro standardy dodržování předpisů. Další informace najdete v tématu [Centrum zabezpečení aplikace Microsoft Azure](https://azure.microsoft.com/support/trust-center/), kde najdete nejnovější aktualizaci seznamu [certifikace dodržování předpisů databáze SQL](https://azure.microsoft.com/support/trust-center/services/).
