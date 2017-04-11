 (záložní trezorů<properties
   pageTitle = "Azure Backup limits table"
   Popis = "popisuje limity pro systém Azure Backup."
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />
<tags
   ms.service="backup"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/05/2015"
   ms.author="trinadhk";"jimpark"; "aashishr" />


Pro zálohování Azure platí následující omezení.

| Identifikátor URI limit | Výchozí Limit |
|---|---|
|Počet serverů/počítačích registrovaných před každý trezoru|50 pro Windows serveru a klientských/SCDPM <br/> 200 IaaS VMs|
|Velikost zdroje dat pro data uložená v Azure trezoru úložiště|Maximální 54400 GB<sup>1</sup>|
|Počet záložní trezorů vytvořené v jednotlivých Azure předplatného|25 (zálohování trezorů) <br/> 25 služby recovery trezoru jednotlivých oblastech|
|Počet zálohování můžou být naplánované denně|3 denně pro Windows Server/klienta <br/> 2 denně SCDPM <br/> Jednou za den IaaS VMs|
|Disků dat připojených k Azure virtuálního počítače pro zálohování|16|

- <sup>1</sup> Limit 54400 GB netýká IaaS OM zálohování.

