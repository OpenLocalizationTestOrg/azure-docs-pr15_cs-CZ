<properties
   pageTitle="Povolit šifrování průhledné dat (TDE) pro roztáhnout databáze systému SQL Server na Azure | Microsoft Azure"
   description="Povolit šifrování průhledné dat (TDE) pro roztáhnout databáze systému SQL Server na Azure"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Povolit šifrování průhledné dat (TDE) pro roztáhnout databáze na Azure
> [AZURE.SELECTOR]
- [Azure portálu](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Šifrování průhledné dat (TDE) pomáhá chránit před hrozbou, že škodlivým aktivitám tak, že provedete v reálném čase šifrování a dešifrování databáze, přidružené zálohování a souborů protokolu transakce v klidu bez nutnosti změny aplikace.

TDE šifruje úložiště pro celou databázi pomocí symetrickou klíče s názvem šifrovací klíč databáze. Klíč šifrování databáze se po zamknutí tak, že certifikát předdefinované serveru. Certifikát předdefinované serveru je jedinečné pro každý Azure server. Microsoft automaticky: otočí tato poukázky minimálně každých 90 dní. Obecný popis TDE najdete v článku [Šifrování průhledné dat (TDE)].

##<a name="enabling-encryption"></a>Povolení šifrování

Povolení TDE pro Azure migrovat databázi, která je ukládání dat z databáze SQL serveru roztáhnout s podporou, proveďte následující akce:

1. Otevřete databázi v [Azure portálu](https://portal.azure.com)
2. V zásuvné databáze klikněte na tlačítko **Nastavení**
3. Vyberte možnost **průhledné šifrování**![][1]
4. Vyberte nastavení **na** a potom vyberte **Uložit**
![][2]


##<a name="disabling-encryption"></a>Zakázání šifrování

Zakázání TDE pro Azure migrovat databázi, která je ukládání dat z databáze SQL serveru roztáhnout s podporou, proveďte následující akce:

1. Otevřete databázi v [Azure portálu](https://portal.azure.com)
2. V zásuvné databáze klikněte na tlačítko **Nastavení**
3. Vyberte možnost **průhledné šifrování**
4. Vyberte **vypnout** nastavení a pak vyberte **Uložit**




<!--Anchors-->
[Šifrování průhledné dat (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
