<properties
   pageTitle="Obecný SQL spojnice krok za krokem | Microsoft Azure"
   description="Tento článek se je rozbor jednoduchý systém HR podrobné pomocí konektoru obecný SQL."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-sql-connector-step-by-step"></a>Obecný SQL spojnice podrobné
Toto téma je podrobného průvodce. Vytvoří databázi jednoduchý příklad HR a použít při importu některých uživatelů a jejich členství.

## <a name="prepare-the-sample-database"></a>Příprava ukázkovou databázi
Na serveru SQL Server následujícím způsobem SQL v [dodatku A](#appendix-a)nalezena. Tento skript vytvoří databázi ukázkové s názvem GSQLDEMO. Objektový model vytvořený databáze vypadá jako na tomto obrázku:  
![Objektový Model](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\objectmodel.png)

Vytvořte také uživatele, kterého chcete použít pro připojení k databázi. V tomto návodu uživatel s názvem FABRIKAM\SQLUser se nachází v doméně.

## <a name="create-the-odbc-connection-file"></a>Vytvoření souboru připojení ODBC
Obecný spojnice SQL používá ODBC se připojit k vzdálený server. Nejdřív potřeba vytvořit soubor s informacemi o připojení ODBC.

1. Spusťte nástroj pro správu ODBC na serveru:  
![ROZHRANÍ ODBC](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc.png)
2. Klikněte na kartu **Soubor DSN**. Klepněte na tlačítko **Přidat**.
![ODBC1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc1.png)
3. Ovladač mimo pole funguje graf tak vyberte ho a klikněte na **Další >**.  
![ODBC2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc2.png)
4. Pojmenujte soubor, například **GenericSQL**.  
![ODBC3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc3.png)
5. Klikněte na **Dokončit**.  
![ODBC4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc4.png)
6. Čas, který nakonfiguruje připojení. Dejte dobrý popis zdroje dat a jako název serveru, na kterém běží SQL Server.  
![ODBC5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc5.png)
7. Vyberte, jak ověření s SQL. V tomto případě jsme pomocí ověřování Windows.  
![ODBC6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc6.png)
8. Zadejte název databáze ukázkové **GSQLDEMO**.  
![ODBC7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc7.png)
9. Zachovat všechno výchozí na této obrazovce. Klikněte na **Dokončit**.  
![ODBC8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc8.png)
10. Ověřte, jestli že všechno funguje očekávaným způsobem, klikněte na **Zdroj dat testu**.  
![ODBC9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc9.png)
11. Ujistěte se, že test proběhne úspěšně.  
![ODBC10](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc10.png)
12. Konfigurační soubor ODBC by se měla v souboru DSN.  
![ODBC11](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc11.png)

Nyní je soubor budeme potřebovat a můžete začít vytvářet spojnice.

## <a name="create-the-generic-sql-connector"></a>Vytvoření spojnice obecný SQL

1. V uživatelském rozhraní Správce služby synchronizace vyberte **spojnic** a **vytvořit**. Vyberte **Obecné SQL (Microsoft)** a zadejte popisný název.  
![Connector1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector1.png)
2. Vyhledání kódu oznámení o doručení soubor, který jste vytvořili v předchozí části a nahrát na server. Zadejte přihlašovací údaje pro připojení k databázi.  
![Connector2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector2.png)
3. V tomto návodu jsou usnadňuje nám jsme, že, existují dva typy objektů, **uživatelů** a **skupin**.
![Connector3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector3.png)
4. Atributy najdete chceme konektoru zjišťování tyto atributy pohledem na tabulku. Protože **Uživatelé** je rezervovaná slova SQL, potřebujeme poskytují v hranaté závorky [].  
![Connector4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector4.png)
5. Definování atribut ukotvení a atribut DN je čas. **Uživatelé**používáme kombinace dva atributy uživatelské jméno a číslo zaměstnance. **Skupina**používáme název skupiny (ne reálné v reálné, ale v tomto návodu funguje).
![Connector5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector5.png)
6. Ne všechny typy atributů můžete zjišťování v databázi SQL. Typ atributu odkaz zejména nemůžete. Typ objektu skupiny potřebujeme změnit ID vlastníka a ID členu odkazovat.  
![Connector6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector6.png)
7. Atributy, které jsme vybraný odkaz atributy v předchozím kroku vyžadují typ objektu jsou tyto hodnoty odkaz. V našem případě typ objektu uživatele.  
![Connector7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector7.png)
8. Na stránce globální parametry vyberte **vodoznak** jako strategie delta. Taky nejdřív napište formát data a času **rrrr MM-dd hh**.
![Connector8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector8.png)
9. Na stránce **Konfigurovat oddíly a hierarchie** vyberte oba typy objektů.
![Connector9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector9.png)
10. **Vyberte typy objektů** a **Vyberte atributy**vyberte typy objektů a všechny atributy. Na stránce **Konfigurace ukotvení** klikněte na **Dokončit**.

## <a name="create-run-profiles"></a>Vytvoření profilů

1. V uživatelském rozhraní Správce služby synchronizace vyberte **spojnic**a **Konfigurace profilů**. Klikněte na **Nový profil**. Začneme tím se **Úplný Import**.  
![Runprofile1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile1.png)
2. Vyberte požadovaný typ **Úplný Import (pouze fázi)**.  
![Runprofile2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile2.png)
3. Vyberte požadovaný oddíl **OBJEKT = uživatel**.  
![Runprofile3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile3.png)
4. Vyberte **tabulku** a zadejte text **[uživatelé]**. Přejděte dolů do části typu objektu s více hodnotami a zadání dat jako na následujícím obrázku. Vyberte uložit v kroku **Dokončit** .
![Runprofile4a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4a.png)  
![Runprofile4b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4b.png)  
5. Vyberte **nový krok**. Tentokrát vyberte **OBJEKT = skupina**. Na poslední stránky pomocí konfigurace jako na následujícím obrázku. Klikněte na **Dokončit**.  
![Runprofile5a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5a.png)  
![Runprofile5b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5b.png)  
6. Volitelné: Pokud budete chtít, můžete nakonfigurovat další profilů. V tomto návodu se používá pouze úplný Import.
7. Klikněte na **OK** dokončete změna profilů.

## <a name="add-some-test-data-and-test-the-import"></a>Přidání některá data a testujte importu
Vyplňte některé testovací data v ukázkové databázi. Jakmile budete připraveni, vyberte **Spustit** a **úplný import**.

Tady je uživatel s dvěma telefonní čísla a skupinou s některé členy.  
![cs1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs1.png)  
![CS2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs2.png)  

## <a name="appendix-a"></a>Dodatek A
**Skript SQL k vytvoření databáze ukázkové**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
