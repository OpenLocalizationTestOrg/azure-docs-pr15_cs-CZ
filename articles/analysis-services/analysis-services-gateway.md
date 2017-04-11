<properties
   pageTitle="Místní data brány | Microsoft Azure"
   description="Na místní brána je nutné, pokud serveru služby Analysis Services v Azure připojí k místním zdrojům dat."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="on-premises-data-gateway"></a>Místní brána dat

Místní brána dat slouží jako most, poskytnutí zabezpečeného datového přenosu mezi místních zdrojů dat a váš server Azure Analysis Services v cloudu.

Brána je nainstalovaný v počítači v síti. Jedné brány musí být nainstalovaný každé služby Azure Analysis Services serveru, ke kterým máte ve vašem předplatném Azure. Například pokud máte dvě servery předplatné Azure, která se připojují k místním zdrojům dat, brány musí být nainstalovaný ve dvou samostatných počítačích v síti.

## <a name="requirements"></a>Požadavky

**Minimální požadavky:**

- .NET 4.5 framework
- 64bitová verze systému Windows 7 nebo Windows Server 2008 R2 (nebo novější)

**Doporučujeme:**

- 8 core CPU
- 8 GB paměti
- 64bitová verze Windows 2012 R2 (nebo novější)

**Důležité informace:**

- U řadiče domény nejde nainstalovat bránu.

- Pouze jednu bránu možné nainstalovat na jednom počítači.

- Instalace brány na počítač, na kterém zůstane v a nepřekročí spánek. Pokud počítač není na, serveru služby Analysis Services Azure nemůže připojit k vaší místní zdroje dat aktualizovat data.

- Neinstalovat brány na počítači bezdrátového připojení k síti. Výkon můžete rozměry.

- Změna názvu serveru pro brány, která už nakonfigurovali, potřebujete přeinstalovat a konfigurace Nová brána.

- V některých případech může tabulkovém modelu služby připojení ke zdrojům dat pomocí nativního poskytovatelů třeba SQL Server Native Client (SQLNCLI11) vrátí chybu. Další informace najdete v tématu [připojení zdroje dat](analysis-services-datasource.md).

## <a name="supported-on-premises-data-sources"></a>Podporované místní zdroje dat
(Verze Preview) bránu podporuje připojení mezi váš server Azure Analysis Services a následující místních zdrojů dat:

- SQL Server
- Datový sklad SQL
- APLIKACE APPS
- Oracle
- Teradata


## <a name="download"></a>Ke stažení
 [Stáhněte si brány](https://aka.ms/azureasgateway)


## <a name="install-and-configure"></a>Instalace a konfigurace

1. Spuštění instalačního programu.

2. Výběr umístění instalace a přijměte licenční podmínky.

3. Přihlaste se k Azure.

4. Zadejte název svého serveru analýzy Azure. Můžete zadat pouze jeden server za brány. Klikněte na **Konfigurovat** a můžete začít.

    ![Přihlaste se k azure](./media\analysis-services-gateway\aas-gateway-configure-server.png)


## <a name="how-it-works"></a>Jak to funguje
Bránu se spustí jako Windows službu **brány místních dat**do počítače v síti vaší organizace. Brány, který nainstalujete pro použití se službou Azure Analysis Services je založená na stejné bráně použit pro další služby, jako je Power BI, ale s některé rozdíly ve způsobu je nakonfigurované.

![Jak to funguje](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Dotazy a data toku práce takto:

1.  Dotaz se vytvořil cloudovou službu s šifrované pověření pro zdroj dat místní. Potom se odesílá do fronty brány zpracuje.

2.  Cloudová služba brány analyzuje dotaz a posune žádost [Bus služby Azure](https://azure.microsoft.com/documentation/services/service-bus/).

3.  Brána dat místní zjišťuje Bus služby Azure pro až do žádosti.

4.  Brána získává dotaz, dešifruje přihlašovací údaje a připojení ke zdrojům dat pomocí těchto přihlašovacích údajů.

5.  Brány odešle dotazu na zdroj dat pro spuštění.

6.  Výsledky se odesílají ze zdroje dat zpátky na brány a potom do cloudové služby.

## <a name="windows-service-account"></a>Účet služby Windows

Místní data bránu pro účely přihlašovací pověření služby systému Windows *NT SERVICE\PBIEgwService* . Ve výchozím nastavení obsahuje vpravo od přihlášení jako služba; v kontextu počítač, který při instalaci brány. Toto pověření není stejný účet používá pro připojení ke zdrojům dat místní nebo účet Azure.  

Pokud narazíte na problémy s proxy serveru kvůli ověření, můžete chtít změnit účet služby Windows uživateli domény nebo spravovat účet služby.

## <a name="ports"></a>Porty

Brány vytvoří odchozí připojení Bus služby Azure. Ho informuje uživatele o odchozí porty: TCP 443 (výchozí), 5671, 5672, 9350 prostřednictvím 9354.  Brána není nutné zadávat příchozí porty.

Doporučujeme můžete povolených IP adres pro oblast dat v bráně firewall. [Microsoft Azure Datacentra IP seznamu](https://www.microsoft.com/download/details.aspx?id=41653)je možné stáhnout. Tento seznam se aktualizuje týdně.

> [AZURE.NOTE]  V zápisu CIDR nejsou IP adresy uvedené v seznamu adres IP Azure Datacentra. Například 10.0.0.0/24 neznamená 10.0.0.0 prostřednictvím 10.0.0.24. Další informace o [CIDR zápisu](http://whatismyipaddress.com/cidr).

Následují plně kvalifikované názvy domén brána.

|Názvy domén|Odchozí porty|Popis|
|---|---|---|
|*. powerbi.com|80|HTTP použít ke stahování instalačního programu.|
|*. powerbi.com|443|PROTOKOL HTTPS|
|*. analysis.windows.net|443|PROTOKOL HTTPS|
|*. login.windows.net|443|PROTOKOL HTTPS|
|*. servicebus.windows.net|5671 5672|Rozšířené Protocol (AMQP) pro zpracování zpráv|
|*. servicebus.windows.net|443, 9350 9354|Posluchačů na Relay Service Bus prostřednictvím protokolu TCP (vyžaduje 443 pro pořízení tokenu řízení přístupu)|
|*. frontend.clouddatahub.net|443|PROTOKOL HTTPS|
|*. core.windows.net|443|PROTOKOL HTTPS|
|Login.microsoftonline.com|443|PROTOKOL HTTPS|
|*. msftncsi.com|443|Slouží k testování připojení k Internetu, pokud brána nedostupná pro službu Power BI.|
|*.microsoftonline p.com|443|Použít pro ověřování v závislosti na konfiguraci.|


### <a name="forcing-https-communication-with-azure-service-bus"></a>Vynucení HTTPS komunikace s Bus služby Azure

Platnost brány komunikovat s Bus služby Azure pomocí HTTPS místo přímého TCP; To však značně snížit výkonu. Potřebujete upravit soubor *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* . Změňte hodnotu z `AutoDetect` k `Https`. Tento soubor je umístěn ve výchozím nastavení *zákulisí brány Files\On místní C:\Program dat*od.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```


## <a name="troubleshooting"></a>Řešení potíží
Rozšířená místní brána dat používá pro připojení služby Azure Analysis Services pro místní zdroje dat je stejné bráně použit s Power BI.

Pokud se vám nedaří při instalaci a konfiguraci brány, je potřeba najdete v článku [řešení problémů s bránou Power BI](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem-tshoot/). Pokud nevíte, jestli že máte problém s bránu firewall, naleznete v částech bránu firewall nebo proxy serveru.

Pokud si myslíte, že dochází k proxy problémech se dočtete víc s brány, najdete v článku [Konfigurace nastavení proxy bran Power BI](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy.md).

## <a name="next-steps"></a>Další kroky
- [Správa služby Analysis Services](analysis-services-manage.md)
- [Načtení dat z Azure Analysis Services](analysis-services-connect.md)
