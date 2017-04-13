<properties
   pageTitle="SSMS podpory pro MFA Azure AD pomocí databáze SQL a SQL datový sklad | Microsoft Azure"
   description="Použití více promítnou ověřování v SSMS pro databáze SQL a SQL datový sklad."
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
   ms.date="10/04/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="ssms-support-for-azure-ad-mfa-with-sql-database-and-sql-data-warehouse"></a>Podpora SSMS MFA Azure AD pomocí databáze SQL a SQL datový sklad

Azure databáze a SQL Azure SQL datový sklad podporuje připojení ze serveru SQL Server Management Studio (SSMS) pomocí *Univerzální ověřování služby Active Directory*. Univerzální ověřování služby Active Directory je interaktivní pracovní postup, který podporuje *Azure Vícefaktorové ověřování* (MFA). Azure MFA pomáhá chránit přístup k datům a aplikace během schůzky požádání uživatele pro jednoduchý proces přihlášení. Poskytuje silné ověřování s celou řadu možností snadno ověření – telefonní hovor, textovou zprávu, čipové karty se PIN kód nebo mobilní aplikaci oznámení – umožňuje uživatelům zvolte způsob jejich upřednostňujete. Popis Vícefaktorové ověřování najdete v tématu [Vícefaktorové ověřování](../multi-factor-authentication/multi-factor-authentication.md).

SSMS nyní podporuje:

- Interaktivní MFA s Azure AD se potenciální pro ověření pole automaticky otevíraná okna.
- Interaktivní nejsou Active Directory heslo a integrované ověřování služby Active Directory metody, které lze použít v mnoha různých aplikacích (ADO.NET JDBC, ODBC, atd.). Tyto dva způsoby nikdy mít za následek místní dialogových oknech.

Když uživatelský účet nakonfigurovaný pro MFA interaktivní ověřování pracovní postup vyžaduje další interakce s uživatelem přes místní dialogových oknech, použití čipové karty atd. Pokud je uživatelský účet nakonfigurovaný pro MFA, musí uživatel vybrat Azure univerzální ověřování pro připojení. Pokud je uživatelský účet nevyžaduje MFA, uživatel můžete dál používat další dvě možnosti ověřování služby Azure Active Directory.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Univerzální omezení ověřování pro databáze SQL a SQL datový sklad

- SSMS je nástroj pouze MFA prostřednictvím univerzální ověřování služby Active Directory v současné době povolenou.
- Pouze jeden účet Azure Active Directory může přihlásit k instanci SSMS pomocí univerzální ověřování. Pokud chcete se přihlásit jako jiný účet Azure AD, je nutné použít jiné instanci aplikace SSMS. (Toto omezení se omezí na univerzální ověřování služby Active Directory, se můžete přihlásit k jiné servery pomocí ověřování hesla služby Active Directory, integrované ověřování služby Active Directory nebo ověřování serveru SQL Server).
- SSMS podporuje univerzální ověřování služby Active Directory pro vizualizaci Průzkumník objektů, editoru dotazů a úložiště přihlašovacích údajů dotazu.
- DacFx ani návrháře schématu podporuje univerzální ověřování.
- Účty MSA nejsou podporované pro univerzální ověřování služby Active Directory.
- Univerzální ověřování služby Active Directory nepodporuje SSMS uživatelům, kteří jsou importují do aktuální služby Active Directory z ostatních Azure Active adresářů. Tito uživatelé nejsou podporovány, protože bude vyžadovat ID klienta pro ověřování účty a neexistuje žádný mechanismus k tomu, že.
- Kromě toho, že je nutné použít podporovanou verzi aplikace SSMS existuje žádné další požadavky na software pro univerzální ověřování služby Active Directory.

## <a name="configuration-steps"></a>Kroky konfigurace

Implementace Vícefaktorové ověřování vyžaduje čtyřmi základními kroky.

1. **Konfigurovat služby Azure Active Directory** – Další informace najdete v tématu [Integrace místních identit s Azure Active Directory](../active-directory/active-directory-aadconnect.md), [přidáte vlastní název domény Azure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure nyní podporuje federaci s služby Active Directory pro Windows Server](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Správa adresáři Azure AD](https://msdn.microsoft.com/library/azure/hh967611.aspx)a [Spravovat Azure AD pomocí Windows Powershellu](https://msdn.microsoft.com/library/azure/jj151815.aspx).

2. **Konfigurace MFA** – podrobné pokyny najdete v tématu [Konfigurace Azure Vícefaktorové ověřování](../multi-factor-authentication/multi-factor-authentication-whats-next.md). 

3. **Konfigurace databáze SQL nebo datový sklad SQL Azure AD ověřování** – podrobné pokyny najdete v tématu [připojení k databázi SQL nebo SQL datový sklad tak, že pomocí Azure Active Directory, ověřování](sql-database-aad-authentication.md).

4. **Stáhněte si SSMS** – v klientském počítači, stáhněte si nejnovější SSMS (aspoň srpen 2016), z [Stáhnout SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Připojení pomocí SSMS univerzální ověřování

Podle těchto kroků ukazují, jak se připojit k databázi SQL nebo SQL datový sklad pomocí nejnovější SSMS.

1. Připojení pomocí univerzální ověřování, v dialogovém okně **připojení k serveru** , vyberte **Univerzální ověřování služby Active Directory**.
![1mfa univerzální spojení][1]

2. Jako obvykle pro databáze SQL a datový sklad SQL musíte klikněte na **Možnosti** a zadejte databázi, v dialogovém okně **Možnosti** . Klepněte na tlačítko **Připojit**.
3. Dialogové okno **přihlásit ke svému účtu** , poskytněte účet a heslo vaši identitu Azure Active Directory.
![2mfa-přihlášení][2]

    > [AZURE.NOTE] Univerzální ověření pomocí účtu, který nevyžaduje MFA připojení v tomto okamžiku. Pro uživatele vyžadující MFA pokračujte následujícími kroky.
 
4. Dva MFA dialogových oken instalačního programu se může zobrazit. Tento daném čase operace závisí na MFA správce nastavení a proto mohou být nepovinné. Pro MFA povolené domény tento krok je někdy předdefinovaných (například domény vyžaduje, aby uživatelé používat čipové karty a PIN kód).  
![nastavení 3mfa][3]

5. Druhá možné jednorázové dialogové okno umožňuje vybrat podrobnosti o metodě ověřování. Dostupné možnosti jsou nakonfigurované tak, že váš správce.
![4mfa ověření 1][4]
 
6. Azure Active Directory potvrzení informace vám pošle. Když dostanete ověřovací kód, zadejte do pole **Zadejte ověřovací kód** a klikněte na **přihlásit**.
![5mfa – ověření 2][5]

Po dokončení ověření SSMS připojí, obvykle za předpokladu platných přihlašovacích údajů a brány firewall přístup.

##<a name="next-steps"></a>Další kroky  

Udělovat ostatním uživatelům přístup k databázi: [SQL databáze a tak mohli ověřovat: udělení přístupu k](sql-database-manage-logins.md)  
Přesvědčte se, zda ostatní můžou připojit přes bránu firewall: [Konfigurace brány firewall na úrovni serveru pravidlo databáze SQL Azure pomocí portálu Azure](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

