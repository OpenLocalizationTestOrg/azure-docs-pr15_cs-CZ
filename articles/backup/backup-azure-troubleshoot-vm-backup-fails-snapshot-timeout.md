<properties
   pageTitle="Azure zálohování OM selže: Nelze komunikovat s agentem OM snímek stavu - snímek OM dílčí úkol vypršení časového limitu | Microsoft Azure"
   description="Příznaky příčiny a řešení pro Azure OM záložní selhání nelze komunikovat s agentem OM snímek stavu. Snímek OM dílčí úkol vypršení časového limitu chyby"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="cfreeman"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jimpark; markgal;genli"/>

# <a name="azure-vm-backup-fails-could-not-communicate-with-the-vm-agent-for-snapshot-status---snapshot-vm-sub-task-timed-out"></a>Azure zálohování OM selže: Nelze komunikovat s agentem OM snímek stavu - vypršel časový limit snímek OM dílčí úkol

## <a name="summary"></a>Souhrn

Po registraci a plánování Azure záložní virtuálního počítače (OM), službu Azure záložní spustí úlohy zálohování naplánovaný komunikaci s záložní doplněk OM na snímek v daném okamžiku. Jsou podmínky, které můžou zabránit snímku spouštěný, které vedou k selhání zálohování. Tento článek obsahuje kroky pro řešení potíží pro problémům souvisejícím s Azure OM záložní chyb souvisejících s snímek vypršení časového limitu.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Příznaku

Microsoft Azure zálohování pro infrastruktury služby (IaaS) OM selže a vrátí tato chybová zpráva chyby podrobnosti projektu v [Azure portálu](https://portal.azure.com/):

**Nelze komunikovat s agentem OM snímek stavu - vypršel časový limit snímek OM dílčí úkol.**

## <a name="cause"></a>Příčina
Existují čtyři běžné příčiny této chyby:

- OM nebude mít přístup k Internetu.
- Agent Microsoft Azure OM nainstalovaných v OM je aktuální (pro Linux VMs).
- Zálohování rozšíření se nepodařilo aktualizovat nebo načtení.
- Nelze získat stav snímky nebo nelze pořízené snímky.

## <a name="cause-1-the-vm-does-not-have-internet-access"></a>Příčina 1: OM neměl přístup k Internetu
Za požadovaného nasazení OM má žádné přístup k Internetu nebo má omezení na místě, která umožňují přístup na Azure infrastruktury.

Zálohování rozšíření vyžadují připojení k Azure veřejnou IP adresy budou fungovat správně. Je to proto odešle příkazy koncového Azure úložiště (HTTP adresy URL) pro správu snímky OM. Pokud koncovku nemá přístup k veřejnému Internetu, zálohování se nezdaří.

### <a name="solution"></a>Řešení
V tomto scénáři použijte jeden z následujících metod řešení tohoto problému:

- Rozsahy IP adres povolených Azure datacentru
- Vytvořením cesty pro přenos HTTP plovoucí dlaždice

### <a name="to-whitelist-the-azure-datacenter-ip-ranges"></a>Povolených Azure rozsahů IP datacentru

1. Získání [seznamu Azure datacentra IP adresy](https://www.microsoft.com/download/details.aspx?id=41653) povolené.
2. Odblokování adresy IP pomocí rutinu New-NetRoute. Spusťte tuto rutinu v Azure OM v okno zvýšenými oprávněními prostředí PowerShell (Spustit jako správce).
3. Přidání pravidel k síti zabezpečení skupiny (NSG) Pokud nemáte umožní přístup k IP adresy.

### <a name="to-create-a-path-for-http-traffic-to-flow"></a>K vytvoření cesty pro přenos HTTP plovoucí dlaždice

1. Případnými omezeními sítě místa (například NSG) nasazení serveru HTTP proxy serveru směrovat přenosy.
2. Pokud máte skupinu zabezpečení sítí (NSG) přidáte pravidla povolíte přístup k Internetu z proxy pro HTTP.

Zjistěte, jak [nastavit nastavit informace HTTP proxy záloh OM](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## <a name="cause-2-the-microsoft-azure-vm-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Příčina 2: Microsoft Azure OM agent nainstalovaných v OM zastaralý (pro Linux VMs)

### <a name="solution"></a>Řešení
Problémy, které ovlivňují staré agent OM příčinou nejčastěji související s agent nebo rozšíření selhání Linux VMs. Jako obecné vodítko jsou tyto první kroky při řešení tohoto problému:

1. [Instalace nejnovější verze služby Azure OM agent](https://github.com/Azure/WALinuxAgent).
2. Ujistěte se, jestli je spuštěný agenta Azure na OM. K tomuto účelu spusťte tento příkaz:```ps -e```

    Pokud tento proces nespustí, použijte následující příkazy restartujte ho.

    Pro systémem Ubuntu:```service walinuxagent start```

    Pro ostatní distribuce: "" služby počáteční waagent
```

3. [Configure the auto restart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).

4. Run a new test backup. If the failures persist, please collect logs from the following folders for further debugging.

    We require the following logs from the customer’s VM:

    - /var/lib/waagent/*.xml
    - /var/log/waagent.log
    - /var/log/azure/*

If we require verbose logging for waagent, follow these steps to enable this:

1. In the /etc/waagent.conf file, locate the following line:

    Enable verbose logging (y|n)

2. Change the **Logs.Verbose** value from n to y.
3. Save the change, and then restart waagent by following the previous steps in this section.

## Cause 3: The backup extension fails to update or load
If extensions cannot be loaded, then Backup fails because a snapshot cannot be taken.

### Solution
For Windows guests:

1. Verify that the iaasvmprovider service is enabled and has a startup type of automatic.
2. If this is not the configuration, enable the service to determine whether the next backup succeeds.

For Linux guests:

The latest version of VMSnapshot Linux (extension used by backup) is 1.0.91.0

If the backup extension still fails to update or load, you can force the VMSnapshot extension to be reloaded by uninstalling the extension. The next backup attempt will reload the extension.

### To uninstall the extension

1. Go to the [Azure portal](https://portal.azure.com/).
2. Locate the particular VM that has backup problems.
3. Click **Settings**.
4. Click **Extensions**.
5. Click **Vmsnapshot Extension**.
6. Click **uninstall**.

This will cause the extension to be reinstalled during the next backup.

## Cause 4: The snapshots status cannot be retrieved or the snapshots cannot be taken
VM backup relies on issuing snapshot command to underlying storage. The backup can fail because it has no access to storage or because of a delay in snapshot task execution.

### Solution
The following conditions can cause snapshot task failure:

| Cause | Solution |
| ----- | ----- |
| VMs that have Microsoft SQL Server Backup configured. By default, VM Backup runs a VSS Full backup on Windows VMs. On VMs that are running SQL Server-based servers and on which SQL Server Backup is configured, snapshot execution delays may occur. | Set following registry key if you are experiencing backup failures because of snapshot issues.<br><br>[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE" |
| VM status is reported incorrectly because the VM is shut down in RDP. If you shut down the virtual machine in RDP, check the portal to determine whether that VM status is reflected correctly. | If it’s not, shut down the VM in the portal by using the ”Shutdown” option in the VM dashboard. |
| Many VMs from the same cloud service are configured to back up at the same time. | It’s a best practice to spread out the VMs from the same cloud service to have different backup schedules. |
| The VM is running at high CPU or memory usage. | If the VM is running at high CPU usage (more than 90 percent) or high memory usage, the snapshot task is queued and delayed and eventually times out. In this situation, try on-demand backup. |
|The VM cannot get host/fabric address from DHCP.|DHCP must be enabled inside the guest for IaaS VM Backup to work.  If the VM cannot get host/fabric address from DHCP response 245, then it cannot download ir run any extensions. If you need a static private IP, you should configure it through the platform. The DHCP option inside the VM should be left enabled. View more information about [Setting a Static Internal Private IP](../virtual-network/virtual-networks-reserved-private-ip.md).|
