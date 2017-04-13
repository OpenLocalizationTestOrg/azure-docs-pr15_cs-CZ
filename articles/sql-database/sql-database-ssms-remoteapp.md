<properties
    pageTitle="Připojení k databázi SQL pomocí SQL Server Management Studio v Azure RemoteApp | Microsoft Azure"
    description="Pomocí tohoto kurzu se dozvíte, jak můžete SQL Server Management Studio v Azure RemoteApp u zabezpečení a výkonu při připojení k databázi SQL"
    services="sql-database"
    documentationCenter=""
    authors="adhurwit"
    manager="jhubbard"/>

<tags
    ms.service="sql-database"
    ms.workload="data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>Použití SQL Server Management Studio v RemoteApp připojení k databázi SQL Azure

## <a name="introduction"></a>Úvod  
Tento kurz se dozvíte, jak používat SQL Server Management Studio (SSMS) v Azure RemoteApp pro připojení k databázi SQL. Provede vás proces instalace SQL Server Management Studio v Azure RemoteApp, vysvětluje výhody a zobrazí funkce zabezpečení, které můžete použít v Azure Active Directory.

**Předpokládaná doba dokončete:** 45 minut

## <a name="ssms-in-azure-remoteapp"></a>SSMS v Azure RemoteApp

Azure RemoteApp je služba služby Remote Data Service v Azure, která zajišťuje aplikace. Další informace o ho tady: [Co je RemoteApp?](../remoteapp/remoteapp-whatis.md)

SSMS spuštěné v Azure RemoteApp vám stejné možnosti jako místně spuštěné SSMS.

![Snímek obrazovky zobrazující SSMS spuštěné v Azure RemoteApp][1]



## <a name="benefits"></a>Výhody

Existuje mnoho výhod používání SSMS v Azure RemoteApp, včetně:

- Port 1433 na serveru SQL Azure nemá zveřejněn externě (mimo Azure).
- Není nutné zachovat přidávání a odebírání IP adres v bráně firewall Azure SQL serveru.
- Všechna připojení Azure RemoteApp prováděn přes protokol HTTPS port 443 pomocí zašifrovaných vzdálené plochy Protocol (protokol)
- Je více uživatelů a můžete změnit velikost.
- Existuje zvýšení výkonu ušetřil práci SSMS ve stejné oblasti jako databáze SQL.
- Použití služby Azure RemoteApp s Premium verzi Azure Active Directory, která obsahuje sestavy aktivit uživatelů můžete auditování.
- Můžete povolit vícefaktorové ověřování (MFA).
- Přístup SSMS kdekoli při použití libovolného příkazu pro Podporovaní klienti Azure RemoteApp, která obsahuje iOS, Android, Mac, Windows Phone a Windows PC.


## <a name="create-the-azure-remoteapp-collection"></a>Vytvořit kolekci Azure RemoteApp

Tady je postup vytvoření kolekci Azure RemoteApp s SSMS:


### <a name="1-create-a-new-windows-vm-from-image"></a>1. Vytvořte nové OM Windows z obrázku
Umožňuje "Windows Server vzdálené plochy relace hostitele Windows serveru 2012 R2" obrázek z Galerie vytvořit nové OM.


### <a name="2-install-ssms-from-sql-express"></a>2. nainstalovat SSMS z SQL Express

Přejděte na nový OM a přejděte na této stránce ke stažení: [Express Microsoft® SQL Server® 2014](https://www.microsoft.com/en-us/download/details.aspx?id=42299)

Je k dispozici možnost chcete jenom stáhnout SSMS. Po stažení přejděte do adresáře instalace a spuštění instalačního programu k instalaci SSMS.

Bude potřeba nainstalovat SQL Server 2014 Service Pack 1. Můžete si ji zde: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/en-us/download/details.aspx?id=46694)

SQL Server 2014 Service Pack 1 zahrnuje základní funkce pro práci s databáze SQL Azure.


### <a name="3-run-validate-script-and-sysprep"></a>3. Spusťte ověřit skript a Sysprep

Na ploše OM nazývá skript Powershellu ověřit. Spusťte dvojím kliknutím. Ověří, že OM je připravená k použití pro vzdálené hostování aplikací. Po dokončení ověření bude požádat o spuštění sysprep – zvolit ho spusťte.

Po dokončení sysprep ukončí OM.

Další informace o vytváření Azure RemoteApp obrázku najdete v tématu: [Postup vytvoření obrázku RemoteApp šablony v Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)


### <a name="4-capture-image"></a>4. můžete udělat snímek

Když OM zastavila, najděte ji v portálu aktuální a zachyťte jej.

Další informace o zachycení obrázku najdete v tématu [zachyťte libovolný obraz z Azure Windows virtuálního počítače vytvořená pomocí klasické nasazení modelu](../virtual-machines/virtual-machines-windows-classic-capture-image.md)


### <a name="5-add-to-azure-remoteapp-template-images"></a>5. přidat obrázky šablon RemoteApp Azure

V části Azure RemoteApp portálu aktuální přejděte na kartu obrázky šablon a klikněte na Přidat. V dialogovém okně místní vyberte "Import obrázek z knihovny virtuálních počítačích" a pak zvolte obrázek, který jste právě vytvořili.



### <a name="6-create-cloud-collection"></a>6. kolekci cloudu vytvořit

Na portálu aktuální vytvoří novou kolekci cloudu RemoteApp Azure. Zvolte obrázek šablony, které jste importovali s nainstalovaným SSMS.

![Vytvořit novou kolekci cloudu][2]


### <a name="7-publish-ssms"></a>7. SSMS publikovat

Na stránce publikování na kartě nové kolekce cloudu, vyberte publikovat aplikace z nabídky Start a pak zvolte SSMS ze seznamu.

![Publikování aplikace][5]

### <a name="8-add-users"></a>8 přidat uživatele

Na kartě přístupu uživatelů můžete vybrat uživatele, kteří mají přístup do této kolekce Azure RemoteApp, která obsahuje pouze SSMS.

![Přidání uživatele][6]


### <a name="9-install-the-azure-remoteapp-client-application"></a>9. Instalace klientské aplikaci Azure RemoteApp

Stažení a instalace klienta Azure RemoteApp tady: [Stáhnout | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)



## <a name="configure-azure-sql-server"></a>Konfigurace Azure SQL serveru

Pouze konfigurace potřebná je povolena služby Azure brány firewall. Pokud používáte toto řešení, není třeba přidat všechny adresy IP otevřete bránu firewall. V síti, která bude moct SQL Server je z jiných služeb, Azure.


![Povolit Azure][4]



## <a name="multi-factor-authentication-mfa"></a>Vícefaktorové ověřování (MFA)

MFA může být užitečné pro tuto aplikaci zvlášť. Přejděte na kartu aplikace služby Azure Active Directory. Pro Microsoft Azure RemoteApp bude hledat položky. Pokud klikněte na aplikace a potom i konfiguraci, zobrazí se stránka pod místo, kam můžete povolit MFA k této aplikaci.

![Povolení MFA][3]



## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Činnosti uživatelů auditování s Azure Active Directory Premium

Pokud nemáte Azure AD Premium, budete muset zapnout v části licence adresáře. S podporou Premium můžete přiřadit uživatelům na úroveň Premium.

Když přejdete do uživatele v Azure Active Directory, můžete potom přejděte k části díky kartě aktivita zobrazíte přihlašovací údaje k Azure RemoteApp.



## <a name="next-steps"></a>Další kroky

Po dokončení všech výše uvedené kroky, budete moct spustit Azure RemoteApp klienta a protokol přihlášení k aplikaci přiřazeného uživatele. Zobrazí se SSMS jako jednu z aplikací a můžete spustit, jako kdybyste byly nainstalované v počítači s přístupem k serveru SQL Azure.

Další informace o tom, jak vytvořit připojení k databázi SQL najdete v tématu [připojení k databázi SQL s SQL Server Management Studio a dělat ukázkový T-SQL dotaz](sql-database-connect-query-ssms.md).


Je to všechno nyní. Využijte!



<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png
