<properties
   pageTitle="Databáze SQL Azure pomocí Azure RemoteApp | Microsoft Azure"
   description="Zjistěte, jak SQL Azure pomocí služby Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="ericorman"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="sql-azure-with-azure-remoteapp"></a>Databáze SQL Azure pomocí Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Často-li zákazníci hostovat jejich aplikací Windows v cloudu pomocí Azure RemoteApp chtějí také přenést data například servery SQL do cloudu pro nasazení celý cloudu. Tato funkce umožňuje celý cloudu hostované řešení, které můžete k nim získat přístup kdykoliv podle libovolného zařízení, odkudkoli pomocí Azure RemoteApp. Tady jsou odkazy spolu s pokyny vám pomůže při tomto procesu.  

## <a name="migrate-your-sql-data"></a>Přenést SQL data

Začněte s [migrací databáze SQL serveru k databázi SQL Azure](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Konfigurace Azure RemoteApp
Hostovat aplikace Windows Azure RemoteApp. Níže je velmi vysoké úrovni podrobné pokyny:

1.     Vytvoření [šablony Azure RemoteApp OM](remoteapp-imageoptions.md). 
2.     Vyžaduje aplikaci nainstalujte OM.
3.     Konfigurace aplikace tak, aby se připojuje k SQL databáze a potvrďte, že funguje.
4.     Sysprep a vypnutí OM. Zachycení to jako obrázek pro použití s Azure. **Poznámka:** Je třeba zajistit, aby byla aplikace možnost ponechat si informace o připojení DB procesem sysprep. Pokud je aplikace nelze zachovat informace o připojení databáze, můžete zapojit dodavatele aplikace zjistit, jak jsme můžete zadat připojovací řetězec.
5.     Importujte vlastní obrázek do knihovny Azure RemoteApp výběr správné zeměpisná oblast, která jsou uložená nasazením SQL Azure. 
6.     Nasazení kolekce RemoteApp v centru dat jako nasazení databáze SQL Azure pomocí výše uvedených šablony a publikovat aplikace. Nasazení Azure RemoteApp v centru dat jako SQL Azure nasazení pomůže zajistit nejrychlejší rychlost připojení a sníží latence. 

## <a name="app-and-sql-configuration-considerations"></a>Aspektech konfiguraci aplikace a SQL:
Existuje několik bodů potřeba zvážit při Azure SQL pomocí RemoteApp:

Zjistěte, [jak konfigurovat bránu firewall databáze Azure SQL](../sql-database/sql-database-firewall-configure.md). Výpisem z článku států, "původně, všechny přístup k Azure databázovým serverem SQL je blokován bránou firewall. Abyste mohli začít používat serveru databáze SQL Azure, musíte přejděte na portál klasické a určit jeden nebo více pravidla úrovni serveru brány firewall, která umožňují přístup k databázi SQL Azure serveru. Pomocí pravidel brány firewall můžete určit jsou povoleny které rozsahy IP adres z Internetu a jestli Azure aplikace můžete pokuste se připojit k serveru databáze SQL Azure."

Také při počítače pro připojení k databázovému serveru z Internetu, bránu firewall zkontroluje, původní IP adresa požadavek na úplné sady úrovni serveru a (v případě potřeby) úrovni databáze pravidla brány firewall. "-Li IP adresu žádost v jedné oblasti zadané v pravidlech brány firewall úrovni serveru, připojení, která k serveru databáze SQL Azure." Proto jsme může být použití rozsahy IP adres a nejen ty jednotlivé zdroje IP adres.

Postupujte podle pokynů krok za krokem [jak: Nakonfigurujte nastavení brány firewall na databáze SQL pomocí portálu Azure](../sql-database/sql-database-configure-firewall-settings.md) k určení rozsahu IP. Až budete konfigurovat pravidla brány Firewall SQL zadejte rozsah IP adres podsítí zadané kolekce Azure RemoteApp. To měli povolit serverům ARA připojení k SQL databáze, i když se bude dynamicky přiřadili IP adres.

## <a name="troubleshooting"></a>Řešení potíží
Pokud se možnosti použití klientské aplikaci hostované v Azure RemoteApp, který se připojuje k SQL databázi kde hostitelem Azure nebo místní pomalé tam může být několik důvodů, proč.  

- Latence síť z vašeho zařízení na Azure nastavená dostatečně vysoká. Přejděte nejlepší a nejrychlejší připojení k síti, můžete pro dosažení nejlepších výsledků. Otestujte vaše zařízení latence Azure datacentrem pomocí [azurespeed.com](http://azurespeed.com/) jako prostředek obecné.  
- Klient aplikace hostované v Azure RemoteApp je vysokém. Výběr jiného fakturační plánu třeba Premium fakturace zlepšíte výkon. Jiné Vtip je sledování aplikace spotřebovává zdroje: během aktivní relace provést některou klávesovou zkratku ctrl konce alt, která spuštění obrazovce přidružení zabezpečení, vyberte Správce úloh a sledovat využití prostředků aplikace.
- SQL server je vysokém nebo nejsou optimalizované. Postupujte podle SQL pokyny pro řešení potíží. 

