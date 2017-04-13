<properties
   pageTitle="Připojení k databázi SQL nebo SQL datový sklad pomocí ověřování služby Azure Active Directory | Microsoft Azure"
   description="Zjistěte, jak připojit k databázi SQL pomocí ověřování služby Azure Active Directory."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/16/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="connecting-to-sql-database-or-sql-data-warehouse-by-using-azure-active-directory-authentication"></a>Připojení k databázi SQL nebo SQL datový sklad pomocí ověřování služby Azure Active Directory

Azure Active Directory authentication je mechanismus připojení k databázi Microsoft Azure SQL a [SQL datový sklad](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) pomocí identit v Azure Active Directory (Azure AD). Ověřování služby Azure Active Directory centrální Správa identit uživatelů databáze a další služby od Microsoftu na jednom centrálním místě. Centrální správa ID obsahuje mít na jednom místě ke správě databáze uživatelů a zjednodušuje správy oprávnění. Výhody patřit následující úkoly:

- Poskytuje alternativy ověřování serveru SQL Server.
- Pomáhá zastavit šíření identity uživatele na serverech databáze.
- Umožňuje heslo otočení na jednom místě
- Zákazníci můžete spravovat oprávnění k databázi pomocí vnější skupiny (AAD).
- Ukládání hesel ho můžete odstranit povolením integrované ověřování systému Windows a jinými formami ověřování podporovaných službou Azure Active Directory.
- Azure Active Directory ověřování používá obsažené databázi uživatelům ověření identity na úrovni databáze.
- Azure Active Directory podporuje tokeny ověřování pro připojení k databázi SQL aplikace.
- Azure Active Directory authentication podporuje služby AD FS (federace domén) nebo ověřování nativní uživatele a hesla pro místní Azure Active Directory bez synchronizaci domény.  
- Azure Active Directory podporuje připojení z SQL Server Management Studio používající univerzální ověřování služby Active Directory, která obsahuje Multi-Factor Authentication (MFA).  MFA obsahuje silné ověřování s celou řadu možností snadno ověření – telefonní hovor, textovou zprávu, čipové karty se PIN kód nebo mobilní aplikaci oznámení. Další informace najdete v tématu [SSMS podpory pro MFA Azure AD pomocí databáze SQL a SQL datový sklad](sql-database-ssms-mfa-authentication.md).

Postup konfigurace patří následující postupy a nakonfigurujte ověřování služby Azure Active Directory.

1. Vytvoření a naplňte Azure Active Directory.
2. Zkontrolujte, zda že je databáze v V12 databáze SQL Azure. (Není nutné SQL datový sklad.)
3. Volitelné: Přidružení nebo změna active directory, která je aktuálně přidružený k předplatnému Azure.
4. Vytvoření správce služby Azure Active Directory pro Azure SQL Server nebo [Datový sklad SQL Azure](https://azure.microsoft.com/services/sql-data-warehouse/).
5. Konfigurace klientské počítače.
6. Vytvoření uživatelů obsažené databáze v databázi namapované Azure AD identity.
7. Připojení k databázi pomocí Azure AD identity.


## <a name="trust-architecture"></a>Architektura zabezpečení

Následující uceleném schéma shrnuje architektury řešení ověřování Azure AD pomocí databáze SQL Azure. Platí pro SQL datový sklad koncepty. K podpoře nativní heslo Azure Active Directory, se považuje za jenom část cloudu a databáze SQL Azure AD/Azure. Pro vedení soudního federovaný ověřování (nebo uživatele a hesla pro přihlašovací údaje Windows) požaduje komunikace blokem služby AD FS. Šipky označuje komunikace cest.

![aad auth diagram][1]

Následující diagram označuje federace, zabezpečení a hostingu relace, které umožňují klientům připojení k databázi odesláním token. Tokenu ověřen službou Azure AD a důvěřovat databázi. Zákazník 1 může výkres znázorňovat Azure Active Directory s nativní uživateli nebo služby Azure Active Directory s federovaní uživatelé. Zákazník 2 představuje možná řešení včetně importovaných uživatelů. v tomto příkladu pocházejících z federované Azure Active Directory pomocí služby AD FS synchronizuje se službou Azure Active Directory. Je důležité pochopit, přístup k databázi pomocí ověřování Azure AD vyžaduje, aby hostingu předplatné přidružené pro službu Azure Active Directory. Stejném předplatném musí být použity k vytvoření hostingu databáze SQL Azure nebo datový sklad SQL serveru SQL Server.

![předplatné relace][2]

## <a name="administrator-structure"></a>Struktura správce

Při použití Azure AD ověřování, existují dva účty správců pro databáze SQL serveru. původní správce serveru SQL Server a Azure AD správce. Platí pro SQL datový sklad koncepty. Pouze správci založený na klientovi Azure AD můžete vytvořit první uživatel databáze Azure AD obsažené v databázi uživatele. Přihlášení Azure AD správce může být Azure AD uživatele nebo skupinu Azure AD. Je-li správce účet skupiny, lze použít tak, že každý člen skupiny povolení více Azure AD správců instanci systému SQL Server. Pomocí účtu skupiny jako správce rozšiřuje možnosti správy umožňuje centrálně přidat nebo odebrat členy skupiny v Azure AD beze změny uživatelé a oprávnění v databázi SQL. Kdykoli je možné konfigurovat pouze jeden Azure AD správce (uživatele nebo skupiny).

![Struktura správce][3]

## <a name="permissions"></a>Oprávnění

Vytvoření nových uživatelů, musíte mít `ALTER ANY USER` oprávnění v databázi. `ALTER ANY USER` Všichni uživatelé databázi lze udělit oprávnění. `ALTER ANY USER` Oprávnění směřuje taky tak, že účty správce serveru a databáze uživatelé, kteří mají `CONTROL ON DATABASE` nebo `ALTER ON DATABASE` oprávnění pro tuto databázi a členové `db_owner` role databáze.

Vytvoření uživatele obsažené databáze v databázi SQL Azure nebo SQL datový sklad, musíte připojení k databázi pomocí identitu Azure AD. Vytvoření prvního uživatele obsažené databáze, musíte připojení k databázi pomocí Správce služby Azure Active Directory (vnímat vlastníka databáze). Tento postup je znázorněn v krocích 4 a 5 níže. Všechny Azure Active Directory authentication lze pouze pokud správce služby Azure Active Directory byla vytvořená pro databázi SQL Azure nebo datový sklad SQL server. Pokud byl správce služby Azure Active Directory odebrat ze serveru, stávajícím uživatelům Azure Active Directory vytvořili dříve uvnitř SQL serveru můžete už připojení k databázi pomocí svých přihlašovacích údajů Azure Active Directory.

## <a name="azure-ad-features-and-limitations"></a>Azure AD funkce a omezení

Následující členy Azure Active Directory můžete v Azure SQL Server nebo SQL datový sklad zřízení:

- Nativní členů: člena vytvořené v Azure AD v seznamu spravované doména nebo v doméně zákazníka. Další informace najdete v tématu [Přidání vlastní název domény Azure AD](../active-directory/active-directory-add-domain.md).

- Federované domény členů: člena vytvořené v Azure AD pomocí z federované domény. Další informace najdete v tématu [Microsoft Azure nyní podporuje federaci s Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/).

- Importované členů z ostatních Azure Active adresářů, kteří jsou členy nativní nebo federované domény.

- Active Directory, skupiny vytvořen jako skupiny zabezpečení.

Účty Microsoft (například outlook.com, hotmail.com, live.com) nebo jiných účtů hosta (například – gmail.com, yahoo.com) nejsou podporovaná. Pokud se můžete přihlásit k [https://login.live.com](https://login.live.com) pomocí účtu a hesla, používáte účet Microsoft, který není podporována pro ověřování Azure AD pro databázi SQL Azure nebo datový sklad SQL Azure.

### <a name="additional-considerations"></a>Další informace

- K vylepšení správy, doporučujeme, že zřízení skupině Azure Active Directory jako správce.
- Pouze jeden Azure AD správce (uživatele nebo skupiny) lze nakonfigurovat pro Azure SQL Server nebo Azure SQL datový sklad kdykoli.
- Pouze správce služby Azure Active Directory pro systém SQL Server můžete původně připojit k serveru SQL Azure nebo datový sklad SQL Azure pomocí účtu služby Azure Active Directory. Správce služby Active Directory můžete nakonfigurovat následující databázi uživatelům Azure Active Directory.
- Doporučujeme nastavení časového limitu připojení na 30 sekund.
- SQL Server 2016 Management Studio a SQL Server Data Tools pro Visual Studio 2015 (verze 14.0.60311.1April 2016 nebo novější) podporuje ověřování Azure Active Directory. (Ověřování služby azure Active Directory je podporována **Zprostředkovatel dat .NET Framework pro SQL Server**; na nejnižších verze .NET Framework 4.6). Proto nejnovější verze těchto nástrojů a aplikací osy dat (DAC a .bacpac) můžete použít ověřování služby Azure Active Directory.
- [Rozhraní ODBC 13.1](https://www.microsoft.com/download/details.aspx?id=53339) podporuje ověřování služby Azure Active Directory však `bcp.exe` nelze se připojit pomocí ověřování Azure Active Directory, protože ho používá starší poskytovatele ODBC.
- `sqlcmd`podporuje Azure Active Directory authentication začínající verze 13.1 dostupné na [Webu služby Stažení softwaru](http://go.microsoft.com/fwlink/?LinkID=825643).  
- SQL Server Data Tools pro Visual Studio 2015 vyžaduje aspoň 2016 s dubnovou verzi datové nástroje (verze 14.0.60311.1). Uživatelům Azure Active Directory nejsou aktuálně zobrazený v prohlížeči SSDT objektů. Jako alternativu zobrazte uživatele [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).
- [Microsoft JDBC ovladač 6.0 pro systém SQL Server](https://www.microsoft.com/en-us/download/details.aspx?id=11774) podporuje ověřování služby Azure Active Directory. Viz také [Nastavení vlastnosti připojení](https://msdn.microsoft.com/library/ms378988.aspx).
- PolyBase nemůže ověřit pomocí ověřování služby Azure Active Directory.
- Některé nástroje jako BI a aplikace Excel nepodporuje.
- Azure Active Directory authentication podporuje databáze SQL Azure portálu **Import** a **Export databáze** listy. Import a export pomocí ověřování služby Azure Active Directory podporuje i od příkazu Powershellu.


## <a name="1-create-and-populate-an-azure-ad"></a>1. Vytvořte a naplnění Azure AD

Vytvoření Azure Active Directory a doplníte uživatelé a skupiny. Azure Active Directory může být počáteční Azure AD spravované domény. Azure Active Directory lze také místní Active Directory Domain Services, které je federované se službou Azure Active Directory.

Další informace najdete v tématu [Integrace místních identit s Azure Active Directory](../active-directory/active-directory-aadconnect.md), [přidáte vlastní název domény Azure AD](../active-directory/active-directory-add-domain.md), [Microsoft Azure nyní podporuje federaci s služby Active Directory pro Windows Server](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Správa adresáři Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx)a [Spravovat Azure AD pomocí Windows Powershellu](https://msdn.microsoft.com/library/azure/jj151815.aspx).

## <a name="2-ensure-your-sql-database-is-version-12"></a>2. Zajistěte, aby že databázi SQL je verze 12

Nejnovější V12 databáze SQL Azure Active Directory authentication podporují. Informace o V12 databáze SQL a zjistěte, jestli je k dispozici ve vaší oblasti najdete v článku [Co je nového v nejnovější V12 aktualizace databáze SQL](sql-database-v12-whats-new.md). Tento krok není nutné pro datový sklad SQL Azure, protože SQL datový sklad je dostupná jenom v V12.

Pokud máte existující databázi, ověřte, že je hostovaný v SQL databázi V12 po připojení k databázi (například pomocí SQL Server Management Studio) a provádění `SELECT @@VERSION;`. Očekávaný výstup pro danou databázi SQL databáze V12 je aspoň **Microsoft SQL Azure (RTM) - 12.0**. Pokud databázi není použitý ve V12 databáze SQL, přečtěte si článek [plánování a příprava na upgrade na SQL databáze V12](sql-database-v12-plan-prepare-upgrade.md)a potom navštivte Azure klasické portálu migrovat databázi SQL databáze V12.

Případně můžete vytvořit novou databázi v SQL databázi V12 pomocí následujících kroků uvedených v tématu [vytvoření první databáze Azure SQL](sql-database-get-started.md). **Tip**: před vyberte předplatné pro novou databázi si přečtěte další krok.

## <a name="3-optional-associate-or-change-the-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>3. volitelné: Přidružení nebo změna active directory, která je aktuálně přidružený k předplatnému Azure

Databáze přidružit adresáři Azure AD pro vaši organizaci, aby v adresáři důvěryhodných adresář pro Azure předplatného, který je hostitelem databáze. Další informace najdete v tématu [jak Azure předplatná souvisí s Azure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Další informace:** Každý Azure předplatné má vztah důvěryhodnosti s instance služby Azure AD. To znamená, že důvěryhodnosti tento adresář ověření uživatele, služby a zařízení. Víc předplatných můžete důvěřovat stejného adresáře, ale předplatné důvěryhodnosti pouze jeden adresář. Uvidíte, jaký directory je důvěryhodná předplatného klikněte v části **Nastavení** na [https://manage.windowsazure.com/](https://manage.windowsazure.com/). Tento vztah důvěryhodnosti, který má předplatné s adresářem se liší od vztah, který má předplatné s všech zdrojů v Azure (weby, databází a tak dál), které jsou jako zdroje podřízené předplatného. Když skončí platnost předplatného, pak přístup k těmto dalších zdrojů přidružený k předplatnému také zastaví. Ale adresáři zůstane v Azure a jiné předplatné přidružit tento adresář a pokračovat ve správě uživatelů adresář. Další informace o zdrojích najdete v článku [Principy přístupu k prostředkům ve Azure](https://msdn.microsoft.com/library/azure/dn584083.aspx).

Následující postup poskytuje podrobné informace o tom, jak změnit přidružené adresář pro dané předplatné.

1. Připojení k [Portálu klasické Azure](https://manage.windowsazure.com/) pomocí Správce Azure předplatného.
2. Na panelu vlevo vyberte **Nastavení**.
3. Předplatné se na obrazovce nastavení. Pokud požadované předplatné nezobrazí, klikněte na **předplatná** v horní rozevírací seznam dialogovém okně **Filtr podle adresář** a vyberte adresář, který obsahuje vaše předplatné a pak klikněte na **použít**.

    ![Vyberte předplatné][4]
4. V oblasti **Nastavení** klikněte na předplatné a pak klikněte na **Upravit adresáře** v dolní části stránky.

    ![AD nastavení portálu][5]
5. V dialogovém okně **Upravit adresáře** vyberte Azure Active Directory, která je přidružená k SQL Server nebo SQL datový sklad a potom klikněte na šipku pro další.

    ![Vyberte možnost upravit adresáře][6]
6. V dialogovém okně **POTVRDIT** adresáře mapování potvrdit, že "**všech dalších správců se odeberou.**"

    ![Potvrďte adresáře upravit][7]
7. Klikněte na zkontrolovat načtěte znovu na portálu.

> [AZURE.NOTE] Při změníte v adresáři přístup ke všem dalších správců, Azure AD Uživatelé a skupiny, odeberou se uživatelé adresáře podporované zdroje a už mají přístup k toto předplatné nebo jeho zdroje. Pouze, jako správce služby, můžete nakonfigurovat přístup pro objekty založené na nový adresář. Tato změna může trvat značné množství času se rozšíří do všech zdrojů. Změna adresáře, taky změní správce Azure AD pro databáze SQL a SQL datový sklad a disallow přístup k databázi pro všechny stávajícím uživatelům Azure AD. Správce Azure AD musí být obnovení (viz dále) a nové Azure AD Uživatelé musí být vytvořen.

## <a name="4-create-an-azure-ad-administrator-for-azure-sql-server"></a>4. Vytvoření správce Azure AD pro systém SQL Server Azure

Každý Azure SQL serveru (hostí SQL databáze nebo SQL datový sklad) začíná účtem správce jednoho serveru, který je správce celý Server SQL Azure. Druhá správce serveru SQL Server je třeba vytvořit, je účet Azure AD. Tento jistinu se vytvoří jako uživatel databáze obsažené v hlavní databázi. Jako správce správce účtů serveru jsou členy této role **db_owner** každý uživatel databáze a zadejte každý uživatel databáze jako uživatel **dbo** . Další informace o účtech správce serveru najdete v tématu [Správa databáze a přihlášení v databázi SQL Azure](sql-database-manage-logins.md) a **přihlášení a uživatelé** část [pravidla zabezpečení databáze SQL Azure a omezení](sql-database-security-guidelines.md).

Pokud chcete použít Azure Active Directory s Geo replikace, Správce služby Azure Active Directory pro primární a sekundární servery nakonfigurovaný. Pokud server nemá Správce služby Azure Active Directory, pak přihlášení Azure Active Directory a uživatelé dostávat "nelze se připojit" ke chyba serveru.

> [AZURE.NOTE] Uživatelé, kteří nejsou založený na klientovi Azure AD (včetně účet správce serveru SQL Azure), nelze vytvořit uživatelům Azure založené na AD, protože nemáte oprávnění k ověření navrhované databázi uživatelům Azure AD.

### <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server-by-using-the-azure-portal"></a>Zřízení Správce služby Azure Active Directory pro systém SQL Server Azure pomocí portálu Azure

1. Na [portálu Azure](https://portal.azure.com/), v pravém horním rohu klikněte na připojení k rozevírací seznam možných aktivní adresářů. Jako výchozí Azure AD zvolte správný Active Directory. Tento krok odkazy přidružení předplatného se službou Active Directory se serverem SQL Azure a ujistěte se, že stejném předplatném se používá pro obě Azure AD a SQL Server. (Azure SQL Server můžete hostovat databáze SQL Azure nebo Azure SQL datový sklad.)

    ![Zvolte ad][8]
2. V levém panelu vyberte **SQL servery**, vyberte **SQL serveru**a v **SQL Server** zásuvné nahoře klikněte na **Nastavení**.

    ![nastavení služby AD][9]
3. V **Nastavení** zásuvné, klikněte na ** správce služby Active Directory.
4. V zásuvné **Správce služby Active Directory** klikněte na **Správce služby Active Directory**a nahoře, klepněte na **Nastavení správy**.
5. V zásuvné **Přidat správy** hledání pro uživatele, vyberte uživateli nebo skupině provádět jenom správce a potom klikněte na **Výběr**. (Zásuvné Správce služby Active Directory zobrazuje všechny členy a skupiny služby Active Directory. Uživatele nebo skupiny, které jsou neaktivní, nemůžete vybrat, protože nejsou podporované jako správci služby Azure AD. (Viz seznam podporovaných správci výše v **Azure AD funkce a omezení** .) Řízení přístupu na základě rolí (RBAC) se týká jenom portál a není rozšíří do SQL serveru.
6. V horní části zásuvné **Správce služby Active Directory** klikněte na **Uložit**.
    ![Vyberte správce][10]

    Změny správce může trvat několik minut. Nový správce zobrazí v poli **Správce služby Active Directory** .

> [AZURE.NOTE] Při nastavování správy Azure AD správce názvu nové (uživatel nebo skupina) nelze již být v virtuální hlavní databáze jako uživatel ověřování serveru SQL Server. Pokud je k dispozici, se nepovede nastavení správce Azure AD; vrácení zpět jeho vytvoření a o tom, které tyto správce (jméno), již existuje. Od tohoto serveru SQL Server ověřování uživatele není součástí Azure AD, všechny plánování řízené úsilí pro připojení k serveru pomocí ověřování Azure AD se nezdaří.

Později odebrat správce v horní části zásuvné **Správce služby Active Directory** , klikněte na tlačítko **odebrat správce**a potom klikněte na **Uložit**.

### <a name="provision-an-azure-ad-administrator-for-azure-sql-server-by-using-powershell"></a>Zřízení správce Azure AD pro Azure SQL Server pomocí prostředí PowerShell

Abyste mohli spouštět rutiny prostředí PowerShell, musíte mít nainstalovanou a spuštěnou Azure PowerShell. Podrobné informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md).

Zřízení Azure AD správce, proveďte následující příkazy Powershellu Azure:

- Přidání AzureRmAccount
- Vyberte AzureRmSubscription


Použít můžete vytvořit a spravovat Azure AD správce:

| Název rutiny                                       | Popis                                                                                                     |
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [Nastavení AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603544.aspx)    | Zřídí Správce služby Azure Active Directory pro Azure SQL Server nebo datový sklad SQL Azure. (Musí být z aktuálního předplatného.) |
| [Odebrat AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt619340.aspx) | Odebere Správce služby Azure Active Directory pro Azure SQL Server nebo datový sklad SQL Azure. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](https://msdn.microsoft.com/library/azure/mt603737.aspx)    | Vrátí informace o Správci Azure Active Directory konfigurovaná pro Azure SQL Server nebo datový sklad SQL Azure. |

Pomocí prostředí PowerShell příkaz Nápověda zobrazíte další informace u každé z následujících příkazů, například ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Následující ustanovení skript **DBA_Group** s názvem skupiny správců Azure AD (id objektu `40b79501-b343-44ed-9ce7-da4c8cc7353f`) **demo_server** serveru ve skupině zdroje s názvem **skupiny 23**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group"
```

Vstupní parametry **DisplayName** přijme Azure AD zobrazované jméno nebo hlavního názvu uživatele. Například ``DisplayName="John Smith"`` a ``DisplayName="johns@contoso.com"``. Zobrazovaný název skupiny Azure AD pouze Azure AD je podporované.

> [AZURE.NOTE] Azure PowerShell command ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` nezabrání zřizování správci Azure AD pro nepodporované uživatele. Nepodporované uživatel může zřízení, ale nemůže připojit k databázi. (Viz seznam podporovaných správci výše v **Azure AD funkce a omezení** .)

Volitelné **objektu**v následujícím příkladu:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23"
–ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [AZURE.NOTE] Pokud **DisplayName** není jedinečné, je nutné Azure AD **objektu** . K načtení hodnot **objektu** a **DisplayName** pomocí část klasické portál Azure Active Directory a zobrazte vlastnosti uživatele nebo skupiny.

Následující příklad vrátí informace o aktuální správce Azure AD pro systém SQL Server Azure:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator –ResourceGroupName "Group-23" –ServerName "demo_server" | Format-List
```

Následující příklad odebere Azure AD správce:
```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" –ServerName "demo_server"
```

Můžete taky zřizujete správce Azure Active Directory pomocí rozhraní REST API. Další informace najdete v tématu [služby správy REST API odkaz a operace pro operace databáze SQL Azure pro databáze SQL Azure](https://msdn.microsoft.com/library/azure/dn505719.aspx)

## <a name="5-configure-your-client-computers"></a>5. konfigurace klientské počítače

Ve všech klientských počítačích, ze kterých aplikací ani uživatelům připojení k databázi SQL Azure nebo datový sklad SQL Azure pomocí identit Azure AD je třeba nainstalovat následujícím softwarem:

- .NET framework 4.6 nebo novější z [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
- Azure Active Directory Authentication Library pro systém SQL Server (**ADALSQL. Knihovna DLL**) je k dispozici ve víc jazycích (x86 i amd64) ze služby Stažení softwaru v [Microsoft Active Directory Authentication Library pro Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

### <a name="tools"></a>Nástroje

- Instalace [SQL serveru 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) nebo [SQL Server Data Tools pro Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) musí vyhovovat požadavkům .NET Framework 4.6.
- SSMS instalace x86 verzi **ADALSQL. Knihovna DLL**.
- SSDT nainstaluje amd64 verze **ADALSQL. Knihovna DLL**.
- Nejnovější Visual Studio z [Ke stažení pro aplikace Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs) musí vyhovovat požadavkům .NET Framework 4.6, ale není stránce instalovat požadovaných amd64 **ADALSQL. Knihovna DLL**.

## <a name="6-create-contained-database-users-in-your-database-mapped-to-azure-ad-identities"></a>6. v databázi namapované Azure AD identit vytvořte uživatelů obsažené databáze

### <a name="about-contained-database-users"></a>O uživatelích obsažené databáze

Azure Active Directory authentication vyžaduje, aby uživatelé databázi vytvořit jako uživatelů obsažené databáze. Uživatel obsažené databáze podle identitu Azure AD je uživatel databáze, ve kterém není přihlašovacího jména v databázi předlohy a které mapy identitě v adresáři Azure AD, který je spojený s databází. Identita Azure AD může být jednotlivé uživatele nebo skupiny. Další informace o uživatelích obsažené databáze najdete v článku [Obsažené databázi uživatelům provádění svůj přenosný databáze](https://msdn.microsoft.com/library/ff929188.aspx).

> [AZURE.NOTE] Uživatelé databáze (s výjimkou správci) nelze vytvořit pomocí portálu. Role RBAC nejsou rozšíří do SQL serveru, databázi SQL nebo SQL datový sklad. Azure role RBAC slouží ke správě zdrojů Azure a neplatí pro databáze oprávnění. Například role **Přispěvatele SQL serveru** není udělit přístup k připojení k databázi SQL nebo SQL datový sklad. Oprávnění přístupu musí mít udělený přímo z databáze pomocí příkazů jazyce Transact-SQL.

### <a name="connect-to-the-user-database-or-data-warehouse-by-using-sql-server-management-studio-or-sql-server-data-tools"></a>Připojení k uživatel databáze nebo datový sklad pomocí SQL Server Management Studio nebo SQL Server Data Tools

Potvrďte správce Azure AD je správně nastavit připojení k databázi **předlohy** pomocí účtu správce Azure AD.
Zřízení Azure AD na základě obsažené databáze uživatel (jiné než správce serveru, která vlastní databáze), připojení k databázi k identitě Azure AD, který má přístup k databázi.

> [AZURE.IMPORTANT] Nastavení vícefaktorového ověřování Azure Active Directory authentication je dostupný u [SQL serveru 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) a [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) ve Visual Studiu 2015. Verze srpen 2016 na trh SSMS taky podporuje univerzální ověřování služby Active Directory, která umožňuje správcům vyžadují Vícefaktorové ověřování pomocí telefonní hovor, textová zpráva čipové karty se PIN kód nebo mobilní aplikaci oznámení.

#### <a name="connect-using-active-directory-integrated-authentication"></a>Připojit pomocí integrovaného ověřování služby Active Directory

Tento způsob použijte, pokud jste se přihlásili k Windows pomocí svých přihlašovacích údajů Azure Active Directory z federované domény.

1. Otevřete Management Studio nebo datové nástroje a v dialogovém okně **připojit k serveru** (nebo **připojit k databázový stroj**) v dialogovém okně **ověření** vyberte **Integrované ověřování služby Active Directory**. Žádné heslo není potřeba nebo můžete zadat, protože zobrazí přihlašovacích údajů existující připojení.
    ![Vyberte AD ověřování][11]

2. Klikněte na tlačítko **Možnosti** a na stránce **Vlastnosti připojení** v dialogovém okně **připojení k databázi** zadejte název, který chcete připojit k databázi uživatelů.
    ![Vyberte název databáze][13]


#### <a name="connect-using-active-directory-password-authentication"></a>Připojení pomocí ověřování hesla služby Active Directory

Tento způsob použijte, když připojení s názvem hlavní Azure AD pomocí Azure AD spravovat domény. Taky můžete ho federované účtu bez přístupu na doménu, například když pracuje na dálku.

Tento způsob použijte, pokud jste se přihlásili k Windows pomocí přihlašovacích údajů z domény, který není federovaný s Azure nebo pokud pomocí ověřování Azure AD pomocí Azure AD na základě počáteční nebo domény klienta.

1. Otevřete Management Studio nebo datové nástroje a v dialogovém okně **připojit k serveru** (nebo **připojit k databázový stroj**) v dialogovém okně **ověření** vyberte **Active Directory, ověřování hesla**.
2. Do pole **uživatelské jméno** zadejte své uživatelské jméno Azure Active Directory ve formátu **username@domain.com**. Musí se jednat o účet ze služby Azure Active Directory nebo účtu z domény vytvořit federaci s Azure Active Directory.
3. Do pole **heslo** zadejte heslo pro účet služby Azure Active Directory nebo federované doménovým účtem.
    ![Vyberte ověřování AD hesla][12]

4. Klikněte na tlačítko **Možnosti** a na stránce **Vlastnosti připojení** v dialogovém okně **připojení k databázi** zadejte název, který chcete připojit k databázi uživatelů. (Viz obrázek v předchozí možnosti).


### <a name="create-an-azure-ad-contained-database-user-in-a-user-database"></a>Vytvoření databáze uživatele služby Azure AD obsažené v databázi uživatele

Pokud chcete vytvořit Azure AD na základě obsažené databáze uživatel (jiné než správce serveru, který vlastní databáze), připojení k databázi k identitě Azure AD jako uživatel s alespoň oprávnění k **ÚPRAVÁM libovolný uživatel** . Potom pomocí následující syntaxe jazyce Transact-SQL:

    CREATE USER <Azure_AD_principal_name>
    FROM EXTERNAL PROVIDER;


*Azure_AD_principal_name* může být hlavní uživatelské jméno uživatele služby Azure AD nebo zobrazovaný název pro skupinu Azure AD.

**Příklady:** Vytvořit databázi i uživatel představující Azure AD federované nebo spravovat domény uživatele:

    CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
    CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;

Vytvoření uživatele obsažené databáze představující Azure AD nebo federované domény skupinu, zadejte zobrazované jméno skupiny zabezpečení:

    CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;

Pokud chcete vytvořit i databáze uživatele představující aplikace, ke kterému je připojen token Azure AD pomocí:

    CREATE USER [appName] FROM EXTERNAL PROVIDER;

Další informace o vytváření založené na Azure Active Directory identit uživatelů obsažené databáze najdete v článku [Vytvoření uživatele (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).


> [AZURE.NOTE] Odebrání správce služby Azure Active Directory pro systém SQL Server Azure zabrání všichni ověření uživatelé Azure AD připojení k serveru. Pokud potřeby nelze použít uživatelům Azure AD můžete ručně nezobrazí, správce databáze SQL.

Při vytváření uživatel databáze tento uživatel obdrží oprávnění **Připojit** a můžete připojit k této databázi jako člen jejího **veřejné** role. Původně uživateli k dispozici pouze oprávnění jsou oprávněním **veřejné** role nebo oprávnění udělena žádné skupiny Windows, které jsou členem. Po zřízení Azure AD na základě obsažené databáze uživatel můžete udělit uživatele další oprávnění, stejně jako udělit oprávnění jiný typ uživatele. Obvykle udělit oprávnění k rolím databáze a přidání uživatelů k rolím. Další informace najdete v tématu [Základy oprávnění Engine databáze](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Další informace o rolích zvláštní SQL databáze v tématu [Správa databáze a přihlášení v databázi SQL Azure](sql-database-manage-logins.md).
Federované domény uživatele, který je importovat do spravovat domény, musíte použít identita spravované domény.

> [AZURE.NOTE] Azure AD Uživatelé jsou označené v metadatech databáze s typem E (EXTERNAL_USER) a skupin typ X (EXTERNAL_GROUPS). Další informace najdete v tématu [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).


## <a name="7-connect-by-using-azure-ad-identities"></a>7. připojit pomocí Azure AD identity

Azure Active Directory authentication podporuje připojení k databázi identit Azure AD pomocí následujících způsobů:

- Pomocí integrovaného ověřování systému Windows
- Použití hlavního názvu Azure AD a hesla
- Pomocí aplikace tokenu ověřování

### <a name="71-connecting-using-integrated-windows-authentication"></a>7.1. Připojení pomocí integrovaného ověřování (Windows)

Pomocí integrovaného ověřování systému Windows, musí být služby Active Directory vaší domény federovaní Azure Active Directory. Klientská aplikace (nebo do služby) připojení k databázi musí běžet na počítač připojen doménu v části přihlašovací údaje uživatele domény.

Připojení k databázi pomocí ověřování a identitu Azure AD pomocí ověřování klíčových slov v připojovacím řetězci databáze musíte nastavit Active Directory integrované. Následující příklad kódu C# používá ADO .NET.

    string ConnectionString =
    @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Poznámka připojovací řetězec klíčové slovo ``Integrated Security=True`` nepodporuje připojení k databázi SQL Azure.
Nezapomeňte, že při připojení ODBC se musí odebrat mezery a nastavení ověřování "ActiveDirectoryIntegrated".

### <a name="72-connecting-with-an-azure-ad-principal-name-and-a-password"></a>7.2. Připojení pomocí hlavního názvu Azure AD a hesla
Připojení k databázi pomocí ověřování a identitu Azure AD klíčového slova ověřování musí nastavit heslo Active Directory. Připojovací řetězec musí obsahovat uživatelské ID a UID a klíčových slov pro heslo/PWD a hodnoty. Následující příklad kódu C# používá ADO .NET.

    string ConnectionString =
      @"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
    SqlConnection conn = new SqlConnection(ConnectionString);
    conn.Open();

Další informace o metody ověřování Azure AD pomocí ukázek kódu ukázku k dispozici v [Azure AD ověřování GitHub ukázku](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).


### <a name="73-connecting-with-an-azure-ad-token"></a>7.3 připojením pomocí token Azure AD
Tuto metodu ověřování umožňuje vícevrstvé služby připojení k databázi SQL Azure nebo Azure SQL datový sklad získáním token z Azure Active Directory (AAD). Umožňuje sofistikované scénáře včetně ověřování na základě certifikát. Je nutné provést čtyři základní kroky pro použití tokenu ověřování Azure AD:

1. Registrace aplikace pomocí služby Azure Active Directory a získat kód klienta pro váš kód. 
2. Vytvoření databáze uživatele představující aplikace. (Dokončeno dříve v kroku 6)
3. Vytvořte certifikát v počítači spustí klienta aplikace.
4. Přidání certifikátu jako klíč pro aplikaci.

Ukázka připojovací řetězec:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Další informace najdete v tématu [Blog zabezpečení SQL Server](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/).

### <a name="connecting-with-sqlcmd"></a>Připojení pomocí sqlcmd  
Následující příkazy Připojit pomocí verze 13.1 sqlcmd, která je dostupná z [Webu služby Stažení softwaru](http://go.microsoft.com/fwlink/?LinkID=825643).

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="see-also"></a>Viz taky

[Správa databází a přihlášení v databázi Azure SQL](sql-database-manage-logins.md)

[Uživatelé obsažené databáze](https://msdn.microsoft.com/library/ff929071.aspx)

[VYTVOŘENÍ uživatele (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx)


<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[9]: ./media/sql-database-aad-authentication/9ad-settings.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/11connect-using-int-auth.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db.png

