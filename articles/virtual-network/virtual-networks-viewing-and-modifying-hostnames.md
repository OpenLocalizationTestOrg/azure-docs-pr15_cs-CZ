<properties 
   pageTitle="Prohlížení a úpravy názvy hostitelů | Microsoft Azure"
   description="Jak zobrazit a změnit názvy hostitelů pro Azure virtuálních počítačích, webu a pracovní role pro překlad"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="viewing-and-modifying-hostnames"></a>Prohlížení a úpravy názvy hostitelů

Pokud chcete, aby vaše role instance odkazuje podle názvu hostitele, nastavte hodnotu název hostitele v souboru konfigurace služby pro každou roli. To uděláte přidáním názvu požadovaného hostitele atribut **vmName** elementu **Role** . Hodnota atributu **vmName** slouží jako základ pro název hostitele pokaždé role. Například pokud existují tři instance určitou roli **vmName** je *webrole* , názvy hostitelů instancí byly *webrole0*, *webrole1*a *webrole2*. Nepotřebujete zadáte název hostitele virtuálních počítačích v souboru konfigurace, protože název hostitele virtuálního počítače je vyplněné podle názvu virtuálního počítače. Další informace o konfiguraci služby Microsoft Azure najdete v článku [Konfigurace schématu služby Azure (.cscfg souboru)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Zobrazení názvy hostitelů

Názvy hostitelů virtuálních počítačích a rolí instancí můžete zobrazit v cloudové službě pomocí některého z následujících nástroje.

### <a name="azure-portal"></a>Azure portálu

[Azure portál](http://portal.azure.com) umožňuje zobrazit názvy hostitelů virtuálních počítačích na zásuvné přehled virtuálního počítače. Mějte na paměti, že zásuvné zobrazuje hodnoty **název** a **Název hostitele**. Jsou sice původně stejná, změna názvu hostitele nezmění název virtuálního počítače nebo instanci rolí.

Role instance lze zobrazit také v portálu Azure, ale když seznam instancí v do cloudové služby, není zobrazen název hostitele. Zobrazí se název pokaždé, ale tohoto názvu nepředstavuje název hostitele.

### <a name="service-configuration-file"></a>Soubor konfigurační služby

Soubor konfigurace služby pro nasazení služeb si můžete stáhnout z zásuvné **Konfigurovat** služby Azure portálu. Pak můžete vyhledávat **vmName** atribut element **Název Role** zjistíte název hostitele. Mějte na paměti, že tento název hostitele slouží jako základ pro název hostitele pokaždé role. Například pokud existují tři instance určitou roli **vmName** je *webrole* , názvy hostitelů instancí byly *webrole0*, *webrole1*a *webrole2*.

### <a name="remote-desktop"></a>Vzdálená plocha

Po povolení Vzdálená plocha (Windows), prostředí Windows PowerShell Vzdálená (Windows) nebo připojení SSH (Linux a Windows) virtuálních počítačích nebo role instance, můžete zobrazit název hostitele z active připojení ke vzdálené ploše různými způsoby:

- Na příkazovém řádku nebo SSH terminálu zadejte hostname.

- Zadáním ipconfig/všechny do příkazového řádku (pouze Windows).

- Zobrazte název počítače v nastavení systému (pouze Windows).

### <a name="azure-service-management-rest-api"></a>Správa služby Azure rozhraní REST API

Z ostatních klienta postupujte podle těchto pokynů:

1. Ujistěte se, jestli máte certifikát klienta pro připojení k portálu Azure. Získejte certifikát klienta, postupujte podle pokynů prezentovat [jak: soubor ke stažení a importovat nastavení publikování a informace o předplatném](https://msdn.microsoft.com/library/dn385850.aspx). 

1. Nastavte záhlaví položku s názvem ms verze x hodnotou 2013-11 – 01.

1. Pošlete žádost o v v tomto formátu: https://management.core.windows.net/\<subscrition id\>/služby/hostedservices/\<název služby\>?embed podrobností = true

1. Vyhledejte element **HostName** pro každý prvek **RoleInstance** .

>[AZURE.WARNING] Přípona vnitřní domény můžete taky zobrazit cloudové službě z volání odpovědi ostatních kontrolou **InternalDnsSuffix** element nebo spuštěním příkazu ipconfig/všechny z příkazového řádku v relaci Vzdálená plocha (Windows), nebo pracovního kočka /etc/resolv.conf z SSH terminálu (Linux).

## <a name="modifying-a-hostname"></a>Úprava název hostitele

Název hostitele virtuálního počítače nebo instanci rolí můžete upravit tak, že nahrávání souboru konfigurace změněné služby nebo přejmenováním počítače z relaci vzdálené plochy.

## <a name="next-steps"></a>Další kroky

[Překládání (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Schéma konfigurace služby Azure (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Schéma konfigurace Azure virtuální sítě](http://go.microsoft.com/fwlink/?LinkId=248093)

[Určení nastavení DNS pomocí síťové konfigurace soubory](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)
