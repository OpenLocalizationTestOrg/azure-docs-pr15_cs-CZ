<properties 
   pageTitle="Správa dat z Azure automatizaci | Microsoft Azure"
   description="Tento článek obsahuje více témat pro správu prostředí Azure automatizaci.  Obsahuje právě uchovávání dat a zálohování Azure automatizaci havárie obnovení v Azure automatizaci."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren;sngun" />

# <a name="managing-azure-automation-data"></a>Správa dat z Azure automatizaci

Tento článek obsahuje více témat pro správu prostředí Azure automatizaci.

## <a name="data-retention"></a>Uchovávání informací

Při odstraňování zdrojů v Azure automatizaci se zachovají 90 dní pro účely auditování před trvale odebrány.  Nelze zobrazit ani použít zdroj během tohoto období.  Tuto zásadu se týká rovněž prostředky, které patří k automatizaci účtu, který se odstraní.

Azure automatizace automaticky odstraní a trvale odebere úlohy starší než 90 dní.

Následující tabulka shrnuje zásady uchovávání informací u různých zdrojů.

|Data|Zásady|
|:---|:---|
|Účty|Trvale odebrány 90 dní po odstranění účtu uživatelem.|
|Prostředky|Trvale odebrány 90 dní po odstranění majetku uživatelem, nebo 90 dní od účtu, který obsahuje že majetek odstraněna uživatelem.|
|Moduly|Trvale odebrány 90 dní po odstranění modulu uživatelem, nebo 90 dní od účtu, který obsahuje, že modul odstraněna uživatelem.|
|Runbooks|Trvale odebrány 90 dní po odstranění zdroje uživatelem, nebo 90 dní od účtu, který obsahuje že daný zdroj odstraněna uživatelem.|
|Úlohy|Odstraněné a trvale odebrány 90 dní od naposledy upravuje. Může to být po úlohy dokončí, je zastaveno nebo je pozastavené.|
|Soubory konfigurace/MOF uzel| Konfigurace staré uzel se trvale odebere 90 dní po vygenerování nové konfigurace uzel.|
|DSC uzlů| Trvale odebrány 90 dní po uzel registrace z účtu automatizaci pomocí Azure portál nebo [Unregister AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) rutiny prostředí Windows PowerShell. Uzly se také odeberou 90 dní od účtu, který obsahuje že uzel odstraněna uživatelem. |
|Sestavy uzel| Trvale odebrány 90 dní po vygeneruje se nové sestavy pro uzel|

Zásady uchovávání informací platí pro všechny uživatele které aktuálně nelze upravovat.

## <a name="backing-up-azure-automation"></a>Zálohování automatizaci Azure

Při odstranění účtu automatizaci v Microsoft Azure budou odstraněny všechny objekty v okně účet včetně runbooks modulů kontroly, konfigurace, nastavení, úlohy a prostředky. Objekty nelze obnovit po odstranění účtu.  Zálohování obsahu účtu automatizaci před odstraněním ji můžete použít následující informace. 

### <a name="runbooks"></a>Runbooks

Vaše runbooks můžete exportovat do skriptů pomocí portálu pro správu Azure nebo rutinu [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) v prostředí Windows PowerShell.  Tyto soubory skriptu lze importovat do jiného účtu automatizaci, jak je uvedeno v [vytváření nebo do ní importují postupu Runbook](https://msdn.microsoft.com/library/dn643637.aspx).


### <a name="integration-modules"></a>Integrace moduly

Integrace moduly nelze exportovat z Azure automatizaci.  Musíte se ujistit, že jsou k dispozici mimo automatizaci účtu.

### <a name="assets"></a>Prostředky

Z Azure automatizaci nelze exportovat [prostředky](https://msdn.microsoft.com/library/dn939988.aspx) .  Pomocí portálu pro správu Azure, musíte zaznamenat podrobnosti o proměnné přihlašovací údaje, certifikáty, připojení a plány.  Je nutné pak vytvořit ručně prostředky, které využívají runbooks, který importujete do jiného automatizaci.

[Azure rutin](https://msdn.microsoft.com/library/dn690262.aspx) můžete použít k načtení podrobnosti o nešifrovaném prostředky a potom uložit je pro pozdější potřeby nebo vytvoření odpovídající prostředky v rámci jiného účtu automatizaci.

Nemůže získat hodnoty pro šifrované proměnné nebo pole pro heslo používání rutin přihlašovacích údajů.  Pokud si nejste jisti tyto hodnoty, můžete je načíst z postupu runbook pomocí aktivit [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) a [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) .

Certifikáty nelze exportovat z Azure automatizaci.  Musíte se ujistit, že všechny certifikáty jsou k dispozici mimo Azure.

### <a name="dsc-configurations"></a>DSC konfigurace

Konfigurace můžete exportovat do skriptů pomocí portálu pro správu Azure nebo rutinu [Export AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) v prostředí Windows PowerShell. Tyto konfigurace můžete importovat a použít v rámci jiného účtu automatizaci.


##<a name="geo-replication-in-azure-automation"></a>GEO replikace v Azure automatizaci

Replikace GEO, směrodatnou v Azure automatizaci účty zálohovat data účet pro jinou zeměpisnou oblast pro redundance. Při nastavení účtu si můžete vybrat primární oblasti a poté sekundární oblasti se přiřadí k němu automaticky. Sekundární daty zkopírovanými z oblasti primární průběžně aktualizují v případě ztráty dat.  

Následující tabulka zobrazuje párování dostupné primárních a sekundárních oblast.

|Primární            |Sekundární
| ---------------   |----------------
|Jižní centrální USA   |Severní centrální USA
|Východního USA 2          |Centrální USA
|Západní Evropě        |Severní Evropě
|Jihovýchodní Asie    |Východní Asie
|Japonsko východ         |Japonsko západní

V případě pravděpodobné, že dojde ke ztrátě primární oblast dat Microsoft pokusí se obnovit. Nejde obnovit primární data, pak se provádí geo převzetí a problémového zákazníci se oznámení o tomto prostřednictvím předplatné.

