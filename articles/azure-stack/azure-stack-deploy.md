<properties
    pageTitle="Před nasazením Azure zásobníku Koncepce | Microsoft Azure"
    description="Zobrazení prostředí a hardwarové požadavky pro Azure zásobníku Koncepce (Správce služby)."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="erikje"/>

# <a name="azure-stack-deployment-prerequisites"></a>Zjistit předpoklady pro nasazení Azure zásobníku

Než nasadíte Azure zásobníku Koncepce ([Ověření koncepce](azure-stack-poc.md)), zkontrolujte, jestli že váš počítač splňuje následující požadavky.
Technical Preview 2 nasazení požadavky Koncepce jsou stejné jako potřebných pro Technical Preview 1. Proto můžete použít stejné hardware, který jste použili předchozím jednoho pole (verze Preview).

## <a name="hardware"></a>Hardwarová

| Součásti | Minimální  | Doporučené |
|---|---|---|
| Diskových jednotek: operační systém | 1 s operačním systémem disk s minimálně 200 GB umožňující system partition (SSD pevný disk) | 1 s operačním systémem disk s minimálně 200 GB umožňující system partition (SSD pevný disk) |
| Diskových jednotek: Obecné Azure zásobníku Koncepce dat | 4 discích. Každý disk obsahuje aspoň 140 GB kapacitu (SSD nebo pevného disku). Všechny dostupné disků se použijí. | 4 discích. Každý disk obsahuje aspoň 250 GB kapacitu (SSD nebo pevného disku). Všechny dostupné disků se použijí.|
| Výpočet: procesoru | Dvou Socket: 12 fyzické jádra (celkem)  | Dvou Socket: 16 fyzické jádra (celkem) |
| Výpočet: paměti | 96 GB PAMĚTI RAM  | 128 GB PAMĚTI RAM |
| Výpočet: BIOS | Pro Hyper-V povolené (s podporou SLAT)  | Pro Hyper-V povolené (s podporou SLAT) |
| Sítě: NIC | Windows serveru 2012 R2 osvědčení vyžadované pro NIC; žádné specializované funkce povinné | Windows serveru 2012 R2 osvědčení vyžadované pro NIC; žádné specializované funkce povinné |
| Certifikát Shoduje s logem | [Oprávnění pro Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Oprávnění pro Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

**Konfigurace diskové jednotce dat:** Všechny datové jednotky musí být stejného typu (všechna přidružení zabezpečení nebo všechny SATA) a kapacity. Použijete-li přidružení zabezpečení diskových jednotek, musí být diskových jednotek připojena pomocí jedné cesty (je k dispozici žádné MPIO podporu více cesta).

**Možnosti konfigurace HBA**
 
- (Upřednostňovaný) Jednoduchý HBA
- RAID HBA – adaptér musí být nakonfigurované v režimu "projít"
- RAID HBA – disků by měl být nakonfigurování jako jednoduché, RAID 0

**Podporované bus a multimédií zadejte kombinace**

-   PEVNÝ DISK

-   PŘIDRUŽENÍ ZABEZPEČENÍ PEVNÝ DISK

-   PEVNÝ DISK RAID

-   RAID SSD (Pokud je tento parametr/Neznámý typu médií\*)

-   SATA SSD + PEVNÝ DISK

-   PŘIDRUŽENÍ ZABEZPEČENÍ SSD + PEVNÝ DISK PŘIDRUŽENÍ ZABEZPEČENÍ

\*Řadiče RAID bez předávací možnosti nerozpoznal typů médií. Tyto řadiče označí pevného disku a SSD jako Neuvedeno. V takovém případě SSD použijí jako trvalý úložiště místo ukládání do mezipaměti zařízení. Proto nástroje můžete nasazovat Koncepce Microsoft Azure zásobníku na tyto disky SSD.

**Příklad HBA**: LSI 9207 8i, LSI 9300 8i nebo 9265 8i LSI v předávací režimu

Konfigurace OEM výběru jsou k dispozici.

## <a name="operating-system"></a>Operační systém

| | **Požadavky**  |
|---|---|
| **Operační systém verze** | Windows Server 2012 R2 nebo novější. Verze operačního systému pro vás nejsou důležité před spuštěním nasazení, jak budete spustíte hostitelském počítači do virtuálního pevného disku, která je součástí zip instalace Azure vrstvě. Operační systém a všechny požadované opravy jsou už integrovat do obrázku. Nepoužívejte stiskněte klávesy aktivovat všechny instance systému Windows Server použít v Koncepce.|

## <a name="deployment-requirements-check-tool"></a>Nástroj Kontrola požadavků nasazení

Po instalaci operačního systému, můžete [Nasazení kontroly 2 Technical Preview Azure zásobníku](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) k potvrzení, že hardwaru splňuje požadavky na.



## <a name="microsoft-azure-active-directory-accounts"></a>Účty Microsoft Azure Active Directory

Nasazení Microsoft Azure zásobníku Koncepce musí být připojená ke Azure. Proto třeba účet Microsoft Azure Active Directory připravit před spuštěním nasazení skript Powershellu. Tento účet změní globální správce klienta služby Azure Active Directory. Se použijí k zřizovat a delegování aplikací a služeb objekty služby Azure zásobníku, které spolupracují s Azure Active Directory a rozhraní API obrázku. Je také sloužit jako vlastník předplatného výchozího poskytovatele, (které později můžete změnit). Na portálu správce systému Azure zásobníku se můžete přihlásit pomocí tohoto účtu.

1. Vytvořte účet Azure AD, která je mohl správce adresáře alespoň jeden Azure Active Directory. Pokud ještě nemáte, můžete použít. Jinak můžete vytvořit postupně zdarma [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (v Číně, navštěvujte blog o <http://go.microsoft.com/fwlink/?LinkID=717821> místo toho.)

    Uložte tyto přihlašovací údaje pro použití v kroku 6 [Spustit nasazení skript Powershellu](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script). Tento účet *Správce služby* můžete konfigurovat a spravovat zdroje mračnech uživatelských účtů, plány klienta, kvót a ceny. Na portálu budou vytvořit web mračnech soukromé mračnech virtuálního počítače, vytvořit plán a správa předplatná na jméno uživatele.

2. [Vytvoření](azure-stack-add-new-user-aad.md) alespoň jeden účet tak, aby se můžete přihlásit Koncepce zásobníku Azure jako klienta.

  	| **Účet Azure Active Directory**  | **Podporované?** |
  	|---|---| 
  	| Pracovní nebo školní účet s platné veřejné předplatné Azure  | Ano |
  	| Account Microsoft platné veřejné předplatné Azure  | Ne |
  	| Pracovní nebo školní účet s předplatným Azure platné Číně  | Ano |
  	| Pracovní nebo školní účet s platné nám Government Azure předplatné  | Ano |


## <a name="network"></a>Sítě

### <a name="switch"></a>Přepínač

Jeden dostupné port na přepínač na počítači Koncepce.  

Koncepce zásobníku Azure počítač podporuje připojení k přepínač přístup nebo portu dálkovou linku. Žádné specializované funkce vyžadovaných na přepínač. Pokud používáte portu dálkovou linku nebo pokud nemáte nastavený VLAN ID, budete muset zadat ID VLAN jako parametr nasazení. Zobrazí se příklady v [seznamu nasazení parametrů](azure-stack-run-powershell-script.md).

### <a name="subnet"></a>Podsítě

Nepřipojovat Koncepce počítače k následující podsítí:
- 192.168.200.0/24
- 192.168.100.0/27
- 192.168.101.0/26
- 192.168.102.0/24
- 192.168.103.0/25
- 192.168.104.0/25

Tyto podsítě jsou vyhrazená pro interní sítě prostředí Microsoft Azure zásobníku Koncepce.

### <a name="ipv4ipv6"></a>IPv4 a IPv6

Je podporována pouze IPv4. Nelze vytvořit sítích IPv6.

### <a name="dhcp"></a>DHCP

Zkontrolujte, že NIC připojené k síti se systémem je k dispozici serveru DHCP. Pokud DHCP není k dispozici, je třeba připravit další statických síť IPv4 kromě používají hostitele. Je nutné zadat tuto IP adresu a branou jako parametr nasazení. Zobrazí se příklady v [seznamu nasazení parametrů](azure-stack-run-powershell-script.md).

### <a name="internet-access"></a>Přístup k Internetu

Azure zásobníku vyžaduje přístup k Internetu, přímo nebo prostřednictvím průhledné proxy serveru. Azure zásobníku nepodporuje konfigurace proxy serveru webové povolit přístup k Internetu. IP Host (hostitel) a nové IP přidělené MAS BGPNAT01 (DHCP nebo statické IP) musíte mít přístup k Internetu. Porty 80 a 443 se používají v části domény graph.windows.net a login.windows.net.

### <a name="telemetry"></a>Telemetrie

Podporuje toku dat telemetrie, musí být otevřený v síti port 443 (HTTPS). Koncový bod klienta je https://vortex-win.data.microsoft.com.


## <a name="next-steps"></a>Další kroky

[Stáhnout balíček pro nasazení Koncepce zásobníku Azure](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Nasazení Azure zásobníku Koncepce](azure-stack-run-powershell-script.md)
