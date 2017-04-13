<properties
   pageTitle="Centrum zabezpečení Azure Průvodce pro řešení | Microsoft Azure"
   description="Tento dokument pomáhá Poradce při potížích v Centru zabezpečení Azure."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-troubleshooting-guide"></a>Poradce při potížích s příručka Centra zabezpečení Azure
Tato příručka je určené pro profesionály informačních technologií informace, analytiky zabezpečení informací a cloudu správci jehož organizace pomocí Centra zabezpečení Azure muset Poradce při potížích s že problémy související s Centrem zabezpečení.

## <a name="troubleshooting-guide"></a>Příručka pro řešení potíží
Tato příručka vysvětluje, jak řešit problémy související s Centrem zabezpečení. Většina Poradce při potížích udělat i v Centru zabezpečení se provede nejprve kontrolou záznamů [Protokolu auditování](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) pro komponentu selhalo. Až protokolů auditování můžete určit:

- Operací, které byly provedeny
- Kdo operaci zahájil
- Kdy došlo k operace
- Stav operace
- Textové pole hodnoty další vlastnosti, které vám mohou pomoci operace

Protokol auditování obsahuje všechny zápisu operací (umístění, příspěvku DELETE) na svých prostředcích, ale nezahrnuje operace čtení (GET).

## <a name="troubleshooting-monitoring-agent-installation-in-windows"></a>Řešení potíží s instalací sledování zástupce v systému Windows

Centrum zabezpečení sledování agent slouží k provádění shromažďování dat. Když je povolený shromažďování dat a agent správně nainstalovaných v počítači cílového, tyto procesy by měl být na spuštění:

- ASMAgentLauncher.exe - Azure sledování agenta 
- ASMMonitoringAgent.exe - Azure sledování zabezpečení rozšíření
- ASMSoftwareScanner.exe – Azure prohledávání správce

Rozšíření sledování zabezpečení Azure vyhledává různých relevantní nastavení zabezpečení a umožňuje shromáždit protokoly zabezpečení z virtuální počítač. Správce naskenovaný obrázek se použije jako skener opravy.

Pokud instalace proběhne úspěšně byste měli vidět podobná té pod v protokolech auditování pro cílové OM položku:

![Protokolů auditování](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

Můžete taky získat další informace o instalaci načtením protokoly agenta c:\ *%systemdrive%\windowsazure\logs* (Příklad: C:\WindowsAzure\Logs).

> [AZURE.NOTE] Pokud Agent Centrum zabezpečení Azure chovají, bude potřeba restartovat cílové OM od není příkaz zastavení a spuštění agent.

## <a name="troubleshooting-monitoring-agent-installation-in-linux"></a>Řešení potíží s instalací sledování agent v Linux
Při odstraňování potíží s instalací OM zástupce v systému Linux je třeba zajistit, aby byl rozšíření stahuje do/var/knihovny/waagent /. Spuštěním příkazu dole můžete ověřit, zda byl nainstalován:

`cat /var/log/waagent.log` 

Jiných souborů, které můžete zkontrolovat pro účely odstraňování potíží se: 

- /var/log/mdsd.err
- / var/protokol/azure /

V systému pracovní byste měli vidět připojení k procesu mdsd na TCP 29130. Toto je syslog komunikaci s procesu mdsd. Toto chování můžete ověřit spuštěním příkazu níže:

`netstat -plantu | grep 29130`

## <a name="contacting-microsoft-support"></a>Kontaktování podpory společnosti Microsoft

Některé problémy lze zjistit pomocí pokynů za předpokladu, že v tomto článku se ostatním, které můžete zjistit taky uvedených na veřejné Centrum zabezpečení [Fórum komunity](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter). Ale pokud potřebujete další vyřešit potíže, můžete otevřít novou žádost o podporu pomocí portálu Azure, jak je ukázáno v následujícím příkladu: 

![Podpora společnosti Microsoft](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Viz taky

V tomto dokumentu se naučíte konfigurace zásad zabezpečení v Centru zabezpečení Azure. Další informace o Centru zabezpečení Azure, najdete v těchto článcích:

- [Plánování Centrum zabezpečení Azure a příručka](security-center-planning-and-operations-guide.md) – informace o plánování a návrh Principy přijmout Azure Centrum zabezpečení.
- [Sledování v Centru zabezpečení Azure stavu zabezpečení](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení Azure](security-center-faq.md) – najít časté otázky k používání služby
- [Azure zabezpečení Blog](http://blogs.msdn.com/b/azuresecurity/) – najít příspěvky Azure zabezpečení a dodržování předpisů
