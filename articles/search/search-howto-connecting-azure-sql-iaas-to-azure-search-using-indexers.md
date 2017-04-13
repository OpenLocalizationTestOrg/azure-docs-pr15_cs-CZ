<properties 
    pageTitle="Konfigurace připojení z indexování vyhledávání Azure SQL Server Azure počítače virtuální | Microsoft Azure | Indexování" 
    description="Povolte šifrované připojení a nakonfigurujte bránu firewall tak, aby připojení k serveru SQL Server Azure virtuálního počítače (OM) z indexování na Azure hledání." 
    services="search" 
    documentationCenter="" 
    authors="jack4it" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="09/26/2016" 
    ms.author="jackma"/>

# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Nastavení připojení z indexování vyhledávání Azure SQL Server Azure OM

Jak je uvedeno v [Připojení databáze SQL Azure pomocí indexování hledání Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md#frequently-asked-questions), vytváření indexování proti **SQL Server na Azure VMs** (nebo **SQL Azure VMs** krátký) podporuje Azure vyhledávání, ale existuje několik související se zabezpečením předpoklady starat o první. 

**Doby trvání úkolu:** 30 minut, za předpokladu, že jste již nainstalovali OM certifikát.

## <a name="enable-encrypted-connections"></a>Povolit šifrované připojení

Azure vyhledávání vyžaduje službu šifrovaný kanál pro všechny žádosti o indexování přes veřejné připojení k Internetu. V této části jsou uvedeny kroky, aby to fungovalo.

1. Zkontrolujte vlastnosti certifikát, který chcete ověřit, že název subjektu je plně kvalifikovaný název domény (FQDN) z Azure OM. Použijte nástroj jako CertUtils nebo modulu snap-in Certifikáty zobrazte vlastnosti. Úplný název domény můžete získat z oddílu Essentials zásuvné služby OM, v poli **názvů veřejnou IP adresu/DNS popisek** [Azure portálu](https://portal.azure.com/).

    - Pro VMs vytvořených pomocí novější šablony **Správce prostředků** , je ve formátu FQDN `<your-VM-name>.<region>.cloudapp.azure.com`. 

    - Pro starší VMs vytvořen jako **klasické** OM, je ve formátu FQDN `<your-cloud-service-name.cloudapp.net>`. 

2. Konfigurace systému SQL Server pomocí certifikátu pomocí Editoru registru (regedit). 

    I když Správce konfigurace systému SQL Server se často používá u tohoto úkolu, nemůže ho používat v tomto scénáři. Importovaný certifikát nenajde protože plně kvalifikovaný název domény OM na Azure neshoduje s plně kvalifikovaný název domény stanovený OM (identifikuje domény místního počítače nebo domény sítě, ke kterému je připojen). Pokud názvy neodpovídají, umožňuje regedit zadat certifikát.

    - V editoru registru, přejděte k následujícímu klíči registru: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
    `[MSSQL13.MSSQLSERVER]` Část se liší v závislosti na verzi a instance jména. 

    - Nastavte hodnotu klávesu **certifikátů** na **miniaturu** certifikát SSL, který jste importovali bude v angličtině.

    Existuje několik způsobů, jak získat Miniatura, některé půjde vám ještě snadněji ostatním. Pokud ho zkopírovat z **certifikáty** snap-in konzoly MMC, je nejspíš vyzvedne skrytý počátečními znak [popsanou v tomto článku podpory](https://support.microsoft.com/kb/2023869/), jehož výsledkem je chyba při pokusu o připojení. Několik řešení existují pro kontrole potíže. Nejjednodušší je backspace myší a opětovným zadáním prvního znaku Miniatura odebrat úvodní znak v poli hodnoty klíče v editoru registru. Můžete taky můžete jiný nástroj Kopírovat miniatura.

3. Udělení oprávnění ke účet služby. 

    Zkontrolujte, že účet služby SQL Server, která odpovídající oprávnění na privátním klíčem certifikát SSL. Pokud přehlédne tohoto kroku nejde spustit SQL Server. Použití modulu snap-in **certifikáty** nebo **CertUtils** pro tento úkol.

4. Znovu spusťte službu SQL Server.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>Konfigurace připojení k SQL serveru v OM

Po nastavení šifrovaného připojení vyžadované Azure hledání jsou další kroky konfigurace vnitřní k serveru SQL Azure VMs. Pokud jste to ještě neudělali, dalším krokem je dokončete konfiguraci pomocí jedné z těchto článků:

- **Správce prostředků** OM najdete v článku [připojení k SQL serveru virtuálního počítače na Azure pomocí Správce prostředků](../virtual-machines/virtual-machines-windows-sql-connect.md). 

- **Classic** OM najdete v článku [připojení k SQL serveru virtuálního počítače na Azure Classic](../virtual-machines/virtual-machines-windows-classic-sql-connect.md).

Zejména přečtěte si část v každém článku "připojení přes internet".

## <a name="configure-the-network-security-group-nsg"></a>Konfigurace skupině zabezpečení sítí (NSG)

Není konfigurace NSG a odpovídající Azure koncový bod nebo seznam řízení přístupu (ACL) pro zpřístupnění Azure OM ostatních účastníků. Je pravděpodobné, že můžete to udělat i před umožňuje vlastní aplikace logiky pro připojení k vaší OM SQL Azure. Ho neliší pro hledání Azure připojení k vaší OM SQL Azure. 

Následující odkazy obsahují pokyny ke konfiguraci NSG OM nasazení. Pomocí těchto pokynů k řízení přístupu koncového Azure hledání na základě jeho IP adresy.

> [AZURE.NOTE] Pozadí, najdete v článku [Co je skupina zabezpečení sítě?](../virtual-network/virtual-networks-nsg.md)

- **Správce prostředků** OM najdete v článku [jak vytvářet NSGs ARM nasazení](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

- **Klasické** OM najdete v článku [jak vytvářet NSGs klasické nasazení](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP adresy můžou být z hlediska několik výzvy, které jsou snadno vyřešit, pokud si byli vědomi problému a možných řešení. V dalších částech obsahují doporučení zvládat problémy související s IP adres v seznamu ACL.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Omezení přístupu k IP adrese vyhledávací služby

Důrazně doporučujeme omezit přístup k IP adrese vyhledávací služby v ACL místo nastavení vašeho SQL Azure VMs otevřená žádné požadavky na připojení. Snadno zjistit IP adresu příkazem ping plně kvalifikovaný název domény (třeba `<your-search-service-name>.search.windows.net`) aplikace Vyhledávací služby.

#### <a name="managing-ip-address-fluctuations"></a>Správa kolísání IP adresa

Pokud vyhledávací služba obsahuje pouze jednu jednotku hledání (to znamená jeden otevřené a jeden oddíl), IP adresa se změní v průběhu běžné služby restartování zrušili platnost existující ACL s adresou IP vyhledávací služby.

Jedním ze způsobů vyhnout chyby následné připojení je použití více otevřené a jeden oddíl v Azure vyhledávání. Tím zvyšuje náklady, ale také řeší potíže IP adres. Do pole hledání Azure IP adresy neměňte Pokud máte víc než jednu jednotku vyhledávání.

Druhý postup je povolit připojení k selhání a potom překonfigurovat ACL v NSG. Průměrně můžete očekávat IP adresy změnit každých několik týdnů. Pro zákazníky, kteří nemají řízené indexování na základě časté může být tento postup životaschopné.

Třetí přístup životaschopné (ale zejména zabezpečené) je můžete určit rozsah IP adres Azure oblasti, kde máte k dispozici Vyhledávací služby. Seznam rozsahů IP ze kterých jsou veřejné adresy IP přidělit Azure zdroje je publikován na [rozsahy IP Azure Datacentra](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Zahrnout IP adresy portálu Azure hledání

Vytvoření indexování pomocí portálu Azure, portálu logiky Azure hledání také potřebuje přístup k vaší SQL Azure OM během údaje o času vytvoření. IP adresy portálu Azure vyhledávání najdete příkazem ping `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Další kroky

Konfigurace zmenšit teď můžete určit SQL Server na Azure OM jako zdroj dat pro vyhledávací Azure indexování. Další informace najdete v tématu [Připojení databáze SQL Azure pomocí indexování hledání Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) .
