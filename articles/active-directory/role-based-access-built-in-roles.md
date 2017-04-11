<properties
    pageTitle="RBAC: Předdefinované role | Microsoft Azure"
    description="Toto téma popisuje vytvořená v jednotlivých rolích pro řízení přístupu na základě rolí (RBAC)."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/25/2016"
    ms.author="kgremban"/>

#<a name="rbac-built-in-roles"></a>RBAC: Předdefinované role

Azure na základě rolí přístup ovládacího prvku (RBAC) obsahuje následující předdefinované role přiřazené uživatelům, skupiny a služeb. Definice předdefinované role, nelze změnit. Však můžete vytvořit [vlastní role v Azure RBAC](role-based-access-control-custom-roles.md) konkrétní vyhověli vaší organizace.

## <a name="roles-in-azure"></a>Role v Azure

Následující tabulka obsahuje stručný popis předdefinované role. Klikněte na název role zobrazíte podrobný seznam **akcí** a **notactions** rolí. Vlastnost **Akce** určuje povolené akce na Azure zdroje. Akce řetězce můžete použít zástupné znaky. Vlastnost **notactions** určuje akce, které se nevynechávají povolené akce.

>[AZURE.NOTE] Definice Azure rolí neustále vyvíjí. Tento článek je aktualizován co nejvíce podporovat, ale máte vždycky nejnovější definice role v prostředí PowerShell Azure. Pomocí rutin `(get-azurermroledefinition "<role name>").actions` nebo `(get-azurermroledefinition "<role name>").notactions` podle potřeby.

| Název role | Popis |
| --------- | ----------- |
| [Rozhraní API Management Service přispěvatelů](#api-management-service-contributor) | Můžete spravovat rozhraní API správu přístupových |
| [Aplikace přehledy součásti přispěvatelů](#application-insights-component-contributor) | Můžete spravovat součásti aplikace přehledy |
| [Automatizace operátor](#automation-operator) | Možnost spustit, zastavit, pozastavení a obnovení úloh |
| [BizTalk přispěvatelů](#biztalk-contributor) | Můžete spravovat BizTalk služby |
| [Databáze ClearDB MySQL přispěvatelů](#cleardb-mysql-db-contributor) | Můžete spravovat databáze ClearDB MySQL |
| [Skupiny přispěvatelů](#contributor) | Spravovat všechno kromě přístup. |
| [Data Factory přispěvatelů](#data-factory-contributor) | Můžete vytvořit a spravovat zdroje dat a podřízené zdroje v nich. |
| [DevTest Labs uživatele](#devtest-labs-user) | Můžete zobrazit vše, co a připojit, a termínů zahájení, restart a vypnutí virtuálních počítačích |
| [Zóny DNS přispěvatelů](#dns-zone-contributor) | Můžete spravovat zóny DNS a záznamy |
| [Účet DocumentDB přispěvatelů](#documentdb-account-contributor) | Můžete spravovat DocumentDB účty |
| [Inteligentní systémy účtu přispěvatelů](#intelligent-systems-account-contributor) | Můžete spravovat účty inteligentní systémy |
| [Síť přispěvatelů](#network-contributor) | Zvládne všechny zdroje sítě |
| [Nový Relic vylepšeného řízení spotřeby účtu Přispěvatel](#new-relic-apm-account-contributor) | Můžete spravovat nové řízení výkonu aplikace Relic účty a aplikací |
| [Vlastník](#owner) | Zvládne všechny položky, včetně aplikace access |
| [Čtečky](#reader) | Můžete zobrazit vše, ale nejde měnit |
| [Redis mezipaměti přispěvatelů](#redis-cache-contributor) | Můžete spravovat Redis mezipaměti |
| [Plánovač úloh kolekce přispěvatelů](#scheduler-job-collections-contributor) | Můžete spravovat kolekce Plánovač projektu |
| [Vyhledávací služba přispěvatelů](#search-service-contributor) | Můžete spravovat vyhledávací služby |
| [Správce zabezpečení](#security-manager) | Můžete spravovat součásti zabezpečení, zásady zabezpečení a virtuálních počítačích |
| [Přispěvatel DB SQL](#sql-db-contributor) | Můžete spravovat databáze SQL, ale ne jejich zásady zabezpečení |
| [Správce zabezpečení SQL](#sql-security-manager) | Můžete spravovat zásad související se zabezpečením servery SQL a databází |
| [SQL Server přispěvatelů](#sql-server-contributor) | Můžete spravovat servery SQL a databází, ale ne jejich zásady zabezpečení |
| [Klasický úložiště účtu přispěvatelů](#classic-storage-account-contributor) | Můžete spravovat účty klasické úložiště |
| [Úložiště účtu přispěvatelů](#storage-account-contributor) | Můžete spravovat úložiště účty |
| [Správce přístup uživatelů](#user-access-administrator) | Můžete spravovat přístup uživatelů k Azure zdroje |
| [Klasický virtuálního počítače přispěvatelů](#classic-virtual-machine-contributor) | Můžete spravovat klasické virtuálních počítačích, ale ne virtuální sítě nebo úložiště účet ke kterému jsou připojené |
| [Virtuální počítač přispěvatelů](#virtual-machine-contributor) | Můžete spravovat virtuálních počítačích, ale ne virtuální sítě nebo úložiště účet ke kterému jsou připojené |
| [Klasický sítě přispěvatelů](#classic-network-contributor) | Můžete spravovat klasické virtuálních sítí a rezervovaná IP adresy |
| [Plán přispěvatelů webu](#web-plan-contributor) | Můžete spravovat web plány |
| [Web přispěvatelů](#website-contributor) | Můžete spravovat weby, ale ne plány web ke kterým jsou připojeny |

## <a name="role-permissions"></a>Oprávnění role
Následující tabulka popisuje oprávnění vzhledem k jednotlivým rolím. Může se jednat **Akce**, které udělit oprávnění a **NotActions**, které je omezit.

### <a name="api-management-service-contributor"></a>Rozhraní API Management Service přispěvatelů
Můžete spravovat rozhraní API správu přístupových

| **Akce** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | Vytváření a správa rozhraní API správu přístupových |
| Microsoft.Authorization/*/read | Povolení pro čtení |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel upozornění |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Přečtěte si rolí a přiřazování rolí |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="application-insights-component-contributor"></a>Aplikace přehledy Component přispěvatelů
Můžete spravovat součásti aplikace přehledy

| **Akce** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a přiřazování rolí |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel upozornění |
| Microsoft.Insights/components/* | Vytváření a správa přehledy součásti |
| Microsoft.Insights/webtests/* | Vytváření a Správa webových testů |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="automation-operator"></a>Automatizace operátor
Možnost spustit, zastavit, pozastavení a obnovení úloh

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a přiřazování rolí |
| Microsoft.Automation/automationAccounts/jobs/read | Přečtěte si automatizace úloh účtu |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Obnovení úlohy účtu automatizaci |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Ukončit úlohu účtu automatizaci |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Přečtěte si automatizaci proudy úlohy účtu |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Pozastavení úloha účtu automation |
| Microsoft.Automation/automationAccounts/jobs/write | Psaní automatizace úloh účtu |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Přečtěte si automatizaci účetní úlohy schéma |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Přečtěte si automatizaci účetní úlohy schéma |
| Microsoft.Automation/automationAccounts/read | Automatizace účty pro čtení |
| Microsoft.Automation/automationAccounts/runbooks/read | Automatizace runbooks pro čtení |
| Microsoft.Automation/automationAccounts/schedules/read | Přečtěte si automatizaci plány účtu |
| Microsoft.Automation/automationAccounts/schedules/write | Psaní automatizaci plány účtu |
| Microsoft.Insights/components/* | Vytváření a správa přehledy součásti |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="biztalk-contributor"></a>BizTalk přispěvatelů
Můžete spravovat BizTalk služby

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a přiřazování rolí |
| Microsoft.BizTalkServices/BizTalk/* | Vytváření a správa BizTalk služby |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="cleardb-mysql-db-contributor"></a>Databáze ClearDB MySQL přispěvatelů
Můžete spravovat databáze ClearDB MySQL

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a přiřazování rolí |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel upozornění |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |
| successbricks.cleardb/Databases/* | Vytvářet a spravovat databáze ClearDB MySQL |

### <a name="contributor"></a>Skupiny přispěvatelů
Vše kromě aplikace access zvládne

| **Akce** ||
| ------- | ------ |
| * | Vytváření a Správa zdrojů všechny typy |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Delete | Nemůžete odstranit rolí a přiřazování rolí |  
| Microsoft.Authorization/*/Write | Nemůžete vytvořit rolí a přiřazování rolí |

### <a name="data-factory-contributor"></a>Data Factory přispěvatelů
Vytváření a správa dat továrny a podřízené zdroje v nich.

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a rolí přiřazení |
| Microsoft.DataFactory/dataFactories/* | Vytváření a správa dat továrny a podřízené zdroje obsahují. |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel upozornění |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="devtest-labs-user"></a>DevTest Labs uživatele
Můžete zobrazit vše, co a připojit, a termínů zahájení, restart a vypnutí virtuálních počítačích

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a rolí přiřazení |
| Microsoft.Compute/availabilitySets/read | Číst vlastnosti sady dostupnosti |
| Microsoft.Compute/virtualMachines/*/read | Číst vlastnosti virtuálního počítače (OM velikosti stav runtime, rozšíření OM, atd.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Zrušit virtuálních počítačích |
| Microsoft.Compute/virtualMachines/read | Číst vlastnosti virtuálního počítače |
| Microsoft.Compute/virtualMachines/restart/action | Restartujte virtuálních počítačích |
| Microsoft.Compute/virtualMachines/start/action | Zahájení virtuálních počítačích |
| Microsoft.DevTestLab/*/read | Číst vlastnosti laboratoři |
| Microsoft.DevTestLab/labs/createEnvironment/action | Vytvořit laboratorní prostředí |
| Microsoft.DevTestLab/labs/formulas/delete | Odstranění vzorce |
| Microsoft.DevTestLab/labs/formulas/read | Vzorce pro čtení |
| Microsoft.DevTestLab/labs/formulas/write | Přidat nebo upravit vzorce |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Vyhodnocení laboratorní zásad |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Připojit se ke fond adresu back-end Vyrovnávání zatížení |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Připojit se ke Vyrovnávání zatížení příchozí pravidla překladu síťových adres |
| Microsoft.Network/networkInterfaces/*/read | Číst vlastnosti síťové rozhraní (například všechny Vyrovnávání zatížení, které rozhraní sítě je součástí) |
| Microsoft.Network/networkInterfaces/join/action | Spojení virtuálního počítače a rozhraní sítě |
| Microsoft.Network/networkInterfaces/read | Rozhraní síť pro čtení |
| Microsoft.Network/networkInterfaces/write | Psaní síťová rozhraní |
| Microsoft.Network/publicIPAddresses/*/read | Číst vlastnosti veřejnou IP adresu |
| Microsoft.Network/publicIPAddresses/join/action | Připojit se ke veřejnou IP adresu |
| Microsoft.Network/publicIPAddresses/read | Přečtěte si IP adresy veřejné sítě |
| Microsoft.Network/virtualNetworks/subnets/join/action | Připojit se ke virtuální sítě |
| Microsoft.Resources/deployments/operations/read | Další operace nasazení |
| Microsoft.Resources/deployments/read | Přečtěte si nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Storage/storageAccounts/listKeys/action | Seznam úložiště účtu klíče |

### <a name="dns-zone-contributor"></a>Zóny DNS přispěvatelů
Můžete spravovat zóny DNS a záznamy.

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/\*/čtení | Přečtěte si rolí a přiřazování rolí |
| Microsoft.Insights/alertRules/\* | Vytváření a Správa pravidel upozornění |
| Microsoft.Network/dnsZones/\* | Vytváření a správa zóny DNS a záznamy |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/\* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Support/\* | Vytvářet a spravovat požadavky podpory |


### <a name="documentdb-account-contributor"></a>Účet DocumentDB přispěvatelů
Můžete spravovat DocumentDB účty

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a rolí přiřazení |
| Microsoft.DocumentDb/databaseAccounts/* | Vytváření a správě účtů DocumentDB |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel upozornění |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="intelligent-systems-account-contributor"></a>Inteligentní systémy účtu přispěvatelů
Můžete spravovat účty inteligentní systémy

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a rolí přiřazení |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah |
| Microsoft.IntelligentSystems/accounts/* | Vytváření a správě účtů inteligentní systémy |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="network-contributor"></a>Síť přispěvatelů
Můžete spravovat všechny zdroje sítě

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a rolí přiřazení |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah |
| Microsoft.Network/* | Vytváření a Správa sítě |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="new-relic-apm-account-contributor"></a>Nový Relic vylepšeného řízení spotřeby účtu Přispěvatel
Můžete spravovat nové řízení výkonu aplikace Relic účty a aplikací

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a rolí přiřazení |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |
| NewRelic.APM/accounts/* | Vytváření a správa nové Relic aplikace výkonu Správa účtů |

### <a name="owner"></a>Vlastník
Všechno, co, včetně aplikace access zvládne

| **Akce** ||
| ------- | ------ |
| * | Vytváření a Správa zdrojů všechny typy |

### <a name="reader"></a>Čtečky
Můžete zobrazit vše, ale nejde měnit

| **Akce** ||
| ------- | ------ |
| * / čtení | Další materiály všech typů, s výjimkou tajemství. |

### <a name="redis-cache-contributor"></a>Redis mezipaměti přispěvatelů
Můžete spravovat Redis mezipaměti

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a rolí přiřazení |
| Microsoft.Cache/redis/* | Vytváření a Správa mezipaměti Redis |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="scheduler-job-collections-contributor"></a>Plánovač úloh kolekce přispěvatelů
Můžete spravovat kolekce Plánovač projektu

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a rolí přiřazení |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |  
| Microsoft.Scheduler/jobcollections/* | Vytváření a správy kolekcí projektu |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory  |

### <a name="search-service-contributor"></a>Vyhledávací služba přispěvatelů
Můžete spravovat vyhledávací služby

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a rolí přiřazení |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |  
| Microsoft.Search/searchServices/* | Vytváření a správa služeb pro vyhledávání |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory  |

### <a name="security-manager"></a>Správce zabezpečení
Můžete spravovat součásti zabezpečení, zásady zabezpečení a virtuálních počítačích

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a rolí přiřazení |
| Microsoft.ClassicCompute/*/read | Další informace o konfiguraci klasické výpočet virtuálních počítačích |
| Microsoft.ClassicCompute/virtualMachines/*/write | Zapsat konfiguraci virtuálních počítačích |
| Microsoft.ClassicNetwork/*/read | Další informace o konfiguraci klasický síť  |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |  
| Microsoft.Security/* | Vytváření a Správa součásti zabezpečení a zásad |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory  |

### <a name="sql-db-contributor"></a>Přispěvatel databáze SQL
Můžete spravovat databáze SQL, ale ne jejich zásady zabezpečení

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si rolí a rolí přiřazení |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Sql/servers/databases/* | Vytváření a Správa SQL databáze |
| Microsoft.Sql/servers/read | Přečtěte si servery SQL |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Nelze upravit zásad auditování |
| Microsoft.Sql/servers/databases/auditingSettings/* | Nelze upravit nastavení auditování |
| Microsoft.Sql/servers/databases/auditRecords/read  | Nemůže přečíst záznamy auditu |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Nelze upravit zásady připojení |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Nelze upravit data maskování zásady |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Nelze upravit upozornění zásady zabezpečení |
| Microsoft.Sql/servers/databases/securityMetrics/* | Nemůžete upravovat metriky zabezpečení |

### <a name="sql-security-manager"></a>Správce zabezpečení SQL
Můžete spravovat zásad související se zabezpečením servery SQL a databází

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Povolení Microsoft pro čtení |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah přehledy |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Sql/servers/auditingPolicies/* | Vytváření a Správa zásad auditování SQL serveru |
| Microsoft.Sql/servers/auditingSettings/* | Vytváření a Správa nastavení auditování SQL serveru |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Vytváření a Správa zásad auditování databáze SQL serveru |
| Microsoft.Sql/servers/databases/auditingSettings/* | Vytváření a Správa nastavení auditování databáze SQL serveru |
| Microsoft.Sql/servers/databases/auditRecords/read | Číst auditování záznamy |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Vytváření a Správa zásad připojení databáze SQL serveru |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Vytváření a správa maskování zásady data v databázi SQL serveru |
| Microsoft.Sql/servers/databases/read | Databáze SQL pro čtení |
| Microsoft.Sql/servers/databases/schemas/read | Schémata databázi serveru SQL pro čtení |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Sloupce tabulky databáze serveru SQL pro čtení |
| Microsoft.Sql/servers/databases/schemas/tables/read | Tabulky databáze serveru SQL pro čtení |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Vytváření a Správa oznámení zásady zabezpečení serveru SQL server databáze |
| Microsoft.Sql/servers/databases/securityMetrics/* | Vytváření a správa metriky zabezpečení databáze SQL serveru |
| Microsoft.Sql/servers/read | Přečtěte si servery SQL |
| Microsoft.Sql/servers/securityAlertPolicies/* | Vytváření a Správa zásad upozornění zabezpečení SQL serveru |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="sql-server-contributor"></a>SQL Server přispěvatelů
Můžete spravovat servery SQL a databází, ale ne jejich zásady zabezpečení

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Povolení pro čtení|
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah přehledy |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Sql/servers/* | Vytváření a správa servery SQL |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | Nelze upravit zásad auditování SQL serveru |
| Microsoft.Sql/servers/auditingSettings/* | Nelze upravit nastavení auditování SQL serveru |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Nelze upravit zásad auditování databáze SQL serveru |
| Microsoft.Sql/servers/databases/auditingSettings/* | Nelze upravit nastavení auditování databáze SQL serveru |
| Microsoft.Sql/servers/databases/auditRecords/read  | Nemůže přečíst záznamy auditu |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Nelze upravit zásady připojení databáze SQL serveru |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Nelze upravit maskování zásady data v databázi SQL serveru |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Nelze upravit upozornění zásady zabezpečení serveru SQL server databáze |
| Microsoft.Sql/servers/databases/securityMetrics/* | Nemůžete upravovat metriky zabezpečení databáze SQL serveru |
| Microsoft.Sql/servers/securityAlertPolicies/* | Nelze upravit upozornění zásady zabezpečení SQL serveru |

### <a name="classic-storage-account-contributor"></a>Klasický úložiště účtu přispěvatelů
Můžete spravovat účty klasické úložiště

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Povolení pro čtení |
| Microsoft.ClassicStorage/storageAccounts/* | Vytváření a správě účtů úložiště |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah přehledy |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |  
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="storage-account-contributor"></a>Úložiště účtu přispěvatelů
Můžete spravovat účty úložiště, ale ne přístup k nim.

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Přečtěte si všechny povolení |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah přehledy |
| Microsoft.Insights/diagnosticSettings/* | Správa diagnostické nastavení |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |  
| Microsoft.Storage/storageAccounts/* | Vytváření a správě účtů úložiště |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="user-access-administrator"></a>Správce přístup uživatelů
Můžete spravovat přístup uživatelů k Azure zdroje

| **Akce** ||
| ------- | ------ |
| * / čtení | Další materiály všech typů, s výjimkou tajemství. |
| Microsoft.Authorization/* | Správa oprávnění |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="classic-virtual-machine-contributor"></a>Klasický virtuálního počítače přispěvatelů
Můžete spravovat klasický virtuálních počítačích, ale ne virtuální sítě nebo úložiště účet ke kterému jsou připojené

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Povolení pro čtení |
| Microsoft.ClassicCompute/domainNames/* | Vytváření a správa názvů domén klasické výpočetním |
| Microsoft.ClassicCompute/virtualMachines/* | Vytváření a správa virtuálních počítačích |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Připojení sítě skupiny zabezpečení |
| Microsoft.ClassicNetwork/reservedIps/link/action | Propojení vyhrazené IP adresy |
| Microsoft.ClassicNetwork/reservedIps/read | IP adresy vyhrazená pro čtení |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Připojit se ke virtuálních sítí |
| Microsoft.ClassicNetwork/virtualNetworks/read | Virtuální sítě pro čtení |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Přečtěte si disků účtu úložiště |
| Microsoft.ClassicStorage/storageAccounts/images/read | Další úložiště účtu obrázky |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Seznam úložiště účtu klíče |
| Microsoft.ClassicStorage/storageAccounts/read | Účty klasické úložiště pro čtení |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah přehledy |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="virtual-machine-contributor"></a>Virtuální počítač přispěvatelů
Můžete spravovat virtuálních počítačích, ale ne virtuální sítě nebo úložiště účet ke kterému jsou připojené

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Povolení pro čtení |
| Microsoft.Compute/availabilitySets/* | Vytváření a Správa sad výpočetních dostupnosti |
| Microsoft.Compute/locations/* | Vytvoření a správa výpočetním umístění |
| Microsoft.Compute/virtualMachines/* | Vytváření a správa virtuálních počítačích |
| Microsoft.Compute/virtualMachineScaleSets/* | Vytváření a Správa sad měřítko virtuálního počítače |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah přehledy |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Připojení sítě aplikace brány back-end adresa skupiny |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Připojit se ke fondy adres back-end Vyrovnávání zatížení |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Připojit se ke Vyrovnávání zatížení příchozí fondů překladu síťových adres |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Připojit se ke Vyrovnávání zatížení příchozí pravidla překladu síťových adres |
| Microsoft.Network/loadBalancers/read | Vyrovnávání zatížení pro čtení |
| Microsoft.Network/locations/* | Vytváření a správa síťová umístění |
| Microsoft.Network/networkInterfaces/* | Vytváření a Správa sítě rozhraní |
| Microsoft.Network/networkSecurityGroups/join/action | Připojení sítě skupiny zabezpečení |
| Microsoft.Network/networkSecurityGroups/read | Přečtěte si skupiny zabezpečení sítě |
| Microsoft.Network/publicIPAddresses/join/action | Připojení sítě veřejných IP adres |
| Microsoft.Network/publicIPAddresses/read | Přečtěte si IP adresy veřejné sítě |
| Microsoft.Network/virtualNetworks/read | Virtuální sítě pro čtení |
| Microsoft.Network/virtualNetworks/subnets/join/action | Připojit se ke virtuální podsítí |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |  
| Microsoft.Storage/storageAccounts/listKeys/action | Seznam úložiště účtu klíče |
| Microsoft.Storage/storageAccounts/read | Účty úložiště pro čtení |
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="classic-network-contributor"></a>Klasický sítě přispěvatelů
Můžete spravovat klasické virtuálních sítí a vyhrazená IP adresy

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Povolení pro čtení |
| Microsoft.ClassicNetwork/* | Vytváření a správa klasické sítí |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah přehledy |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |  
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |

### <a name="web-plan-contributor"></a>Plán přispěvatelů webu
Můžete spravovat web plány

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Povolení pro čtení |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah přehledy |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |  
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |
| Microsoft.Web/serverFarms/* | Vytváření a správa serverové farmy |

### <a name="website-contributor"></a>Web přispěvatelů
Můžete spravovat webům, ale ne plány web ke kterým jsou připojeny

| **Akce** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Povolení pro čtení |
| Microsoft.Insights/alertRules/* | Vytváření a Správa pravidel výstrah přehledy |
| Microsoft.Insights/components/* | Vytváření a správa přehledy součásti |
| Microsoft.ResourceHealth/availabilityStatuses/read | Přečtěte si stavu zdrojů |
| Microsoft.Resources/deployments/* | Vytváření a Správa zdrojů skupina nasazení |
| Microsoft.Resources/subscriptions/resourceGroups/read | Skupiny prostředků pro čtení |  
| Microsoft.Support/* | Vytvářet a spravovat požadavky podpory |
| Microsoft.Web/certificates/* | Vytváření a správa certifikátů webu |
| Microsoft.Web/listSitesAssignedToHostName/read | Číst weby přiřazená název hostitele |
| Microsoft.Web/serverFarms/join/action | Připojit se ke serverové farmy |
| Microsoft.Web/serverFarms/read | Přečtěte si serverové farmy |
| Microsoft.Web/sites/* | Vytvářet a spravovat weby |

## <a name="see-also"></a>Viz taky
- [Řízení přístupu na základě rolí](role-based-access-control-configure.md): Začínáme s RBAC Azure portálu.
- [Vlastní role v Azure RBAC](role-based-access-control-custom-roles.md): Naučte se vytvářet vlastní role podle vašich potřeb přístup.
- [Vytvoření aplikace access změnit sestava historie](role-based-access-control-access-change-history-report.md): Udržujte si přehled o změně přiřazování rolí v RBAC.
- [Řízení přístupu na základě rolí Poradce při potížích](role-based-access-control-troubleshooting.md): získání návrhů na opravy běžných problémů.
