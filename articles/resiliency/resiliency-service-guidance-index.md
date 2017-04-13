<properties
   pageTitle="Služby odolnost proti chybám pokyny | Microsoft Azure"
   description="Odkazy na havárie obnovení a aktivní odolnost proti chybám a dostupnosti pokyny pro služby Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

# <a name="microsoft-azure-service-resiliency-guidance"></a>Pokyny pro odolnost proti chybám služby Microsoft Azure
Microsoft Azure slouží k poskytování prostředky, které potřebujete, když je potřebujete. Jako součást dobrý návrh a provozní postupy byste měli vědět o architektonické použití Azure služeb dosáhnout dostupnost jak co dělat, když aplikace ovlivněny přerušení služby. Vám pomohou při tomto procesu tento dokument obsahuje odkazy na pokyny obnovení havárie, jakož i pokyny pro návrh pro různé služby Azure.

##<a name="disaster-recovery-guidance"></a>Pokyny pro obnovení havárie
Pokyny pro obnovení havárie odkazů jsou vám může poskytnout informací potřebných pro vám pomohou aplikace zpět do stavu online rychle, pokud jsou ovlivněny přerušení služby Azure. Tyto odkazy byly vytvořeny k odpovídání otázek, "Můžu mi jsou ovlivněny přerušení služby Azure, co můžete dělat?"

##<a name="design-guidance"></a>Pokyny pro návrh
Těchto odkazů návrh pokyny jsou návrh a architektonických pokyny vytvořenou k vám pomůže pochopit, jak nejlépe používat každé služby Azure tak, aby maximalizuje provozu vaše aplikace. Tyto odkazy byly vytvořeny k odpovídání otázek "Jak ověřím, že chybu, selhání hardwaru, přerušení služby nebo jiné chyby nebudou mít vliv na celkové dostupnosti aplikace?" Pokud žádné zvláštní návod pro službu, kterou jste právě hledali se pravděpodobně v článku [dostupnost pro aplikace založený na Microsoft Azure](./resiliency-high-availability-azure-applications.md) Další informace, které vám můžou pomoct při návrhu. 

##<a name="service-guidance"></a>Pokyny pro služby
| Služba  | Pokyny pro obnovení havárie | Pokyny pro návrh |
|:---------|:--------------------------:|:------------------:|
| [Cloud Services] (https://azure.microsoft.com/services/cloud-services/ "Azure cloudových služeb")       | [odkaz] (../cloud-services/cloud-services-disaster-recovery-guidance.md "Pokyny pro obnovení havárie Azure Cloud Services")   | Není k dispozici |
| [Klíčové trezoru] (https://azure.microsoft.com/services/key-vault/ "Azure klíčové trezoru")                      | [odkaz] (../key-vault/key-vault-disaster-recovery-guidance.md "Pokyny pro obnovení havárie trezoru klíč Azure")        | Není k dispozici |
| [Úložiště] (https://azure.microsoft.com/services/storage/ "Azure úložiště")                            | [odkaz] (../storage/storage-disaster-recovery-guidance.md "Pokyny pro obnovení havárie úložišti Azure")          | Není k dispozici |
| [Databáze SQL] (https://azure.microsoft.com/services/sql-database/ "Databáze Azure SQL")           | [odkaz] (../sql-database/sql-database-disaster-recovery.md  "Pokyny pro obnovení havárie databáze SQL Azure")    | [odkaz] (../sql-database/sql-database-business-continuity.md "Základní informace o nepřerušený s databáze SQL Azure") |
| [Virtuálních počítačích] (https://azure.microsoft.com/services/virtual-machines/ "Azure virtuálních počítačích") | [odkaz] (../virtual-machines/virtual-machines-disaster-recovery-guidance.md "Pokyny pro obnovení havárie virtuálních počítačích Azure") | Není k dispozici |
| [Virtuální sítě] (https://azure.microsoft.com/services/virtual-network/ "Azure virtuální sítě")    | [odkaz] (../virtual-network/virtual-network-disaster-recovery-guidance.md "Pokyny pro obnovení havárie virtuální sítě Azure")  | Není k dispozici |

##<a name="next-steps"></a>Další kroky
Pokud hledáte pokyny, které se nerozhodnete zaměřuje na systémy a řešení, přečtěte si prosím [havárie obnovení a dostupnost pro aplikace založený na Microsoft Azure](https://aka.ms/drtechguide).
