<properties
    pageTitle="Úvod k zálohování Azure DPM | Microsoft Azure"
    description="Úvod k zálohování DPM serverů pomocí služby Azure zálohování"
    services="backup"
    documentationCenter=""
    authors="Nkolli1"
    manager="shreeshd"
    editor=""
    keywords="Centrum dat ochranu správce, správce ochranu dat, dpm zálohování"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="trinadhk;giridham;jimpark;markgal"/>

# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Příprava k obecnějším údajům pracovního vytížení Azure s DPM

> [AZURE.SELECTOR]
- [Server Azure zálohování](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Server Azure záložní (klasický)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (klasický)](backup-azure-dpm-introduction-classic.md)


Tento článek obsahuje úvod k používání Microsoft Azure zálohování můžete chránit servery systému Centrum Data Protection Manager (DPM) a úloh. Čtením ho budete porozumět:

- Jak funguje zálohování serveru Azure DPM
- Požadavky pro dosažení hladké záložní prostředí
- Typické došlo k chybám a jak zacházet s nimi
- Podporované scénáře

System Center DPM zálohovat data souborů a aplikace. Datové připojení do DPM můžete uložené na páskou na disku nebo zálohovat do Azure pomocí Microsoft Azure zálohování. DPM bude spolupracovat s Azure zálohování následujícím způsobem:

- **DPM nasazený jako fyzickou serveru nebo místní virtuálního počítače** – Pokud DPM nasazení serveru fyzickou nebo jako místní pro Hyper-V virtuálního počítače můžete data zálohujete do Azure záložní trezoru kromě disku a páskou zálohování.
- **DPM nasazený jako Azure virtuálního počítače** – z System Center 2012 R2 s aktualizace 3, může být nasazené DPM jako Azure virtuálního počítače. Při nasazení DPM jako Azure virtuální počítač, který můžete data zálohujete Azure disků připojených k virtuálního počítače DPM Azure nebo ukládání dat může převzít tak, že zálohování až Azure zálohování trezoru.

## <a name="why-backup-your-dpm-servers"></a>Proč zálohovat serverech DPM?

Obchodní výhody používání Azure zálohování pro zálohování DPM serverů, patří:

- V místním nasazení DPM můžete použít Azure zálohování jako alternativu k dlouhodobé nasazení páskou.
- Nasazení DPM v Azure Azure zálohování umožňuje převzít úložiště z Azure disku umožňuje škálování uložením starší data v Azure zálohování a nová data na disku.

## <a name="how-does-dpm-server-backup-work"></a>Jak funguje zálohování DPM serveru?
K obecnějším údajům virtuálního počítače, nejdřív snímku v okamžiku dat není potřeba. Služba Azure záložní zahájí úlohy zálohování naplánovaný a aktivace záložní rozšíření udělejte si snímek. Zálohování rozšíření souřadnice se službou VSS v guest dosáhnout konzistence a vyvolá rozhraní API snímek objektů blob Azure úložiště služby po dosáhl konzistence. Důvodem je získat konzistentní snímek disků virtuálního počítače, aniž by bylo nutné ho vypnout.

Jakmile se dostali na snímek, data předán službou Azure záložní záložní trezoru. Služba má na starosti identifikační a přenos pouze bloky, které byly změněny od posledního zálohování zvýšení efektivity zálohy úložiště a sítě. Po dokončení převodu dat zmizí snímek a obnovení bod je vytvořen. Tento bod obnovení uvidí Azure klasické portálu.

>[AZURE.NOTE] U Linux virtuálních počítačích je možné jenom soubor konzistentní zálohování.

## <a name="prerequisites"></a>Zjistit předpoklady pro
Příprava Azure zálohování dat DPM následujícím způsobem:

1. **Vytvoření zálohy trezoru** – vytvoření trezoru v konzole Azure zálohování.
2. **Stažení trezoru pověření** – v Azure zálohování odeslat certifikát správy jste vytvořili trezoru.
3. **Zálohování Agent Azure instalovat a registrovat serveru** – z Azure zálohování, instalovat agent na všech serverech DPM a zaregistrovat serveru DPM záložní trezoru.

[AZURE.INCLUDE [backup-create-vault](../../includes/backup-create-vault.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]


## <a name="requirements-and-limitations"></a>Požadavky na (a omezení)

- DPM můžete používat jako Hyper-V virtuálního počítače nainstalovali System Center 2012 SP1 nebo System Center 2012 R2 nebo pole fyzicky server. Můžete taky běžet jako Azure virtuální počítač se systémem System Center 2012 R2 s nejméně kumulativní aktualizace 3 pro DPM 2012 R2 nebo Windows virtuálního počítače v VMWare spuštěných pro System Center 2012 R2 s nejméně kumulativní aktualizaci 5.
- Pokud používáte DPM s System Center 2012 SP1, nainstalujte 2 kumulativní aktualizace pro systém Centrum správce ochranu dat SP1. To je nutné před instalací Azure Backup Agent.
- DPM server by měl mít prostředí Windows PowerShell a .net Framework 4.5 nainstalovaný.
- DPM můžete obecnějším údajům většina úloh zálohování Azure. Úplný seznam co je podporované, najdete v článku zálohy Azure podpory níže uvedených položek.
- Data uložená v Azure zálohování není možné obnovit s parametrem "Kopírovat do páskou".
- Musíte mít účet Azure s povolenou funkcí Azure zálohování. Pokud nemáte účet, můžete vytvořit bezplatný účet zkušební v jenom pár minut. Přečtěte si o [ceny Azure zálohování](https://azure.microsoft.com/pricing/details/backup/).
- Použití zálohování Azure vyžaduje Backup Agent Azure k instalaci na serverech, které chcete zálohovat. Každý server musí mít aspoň 10 % velikost data, která je právě zálohovala, jsou k dispozici jako místní bezplatného úložiště. Například zálohování 100 GB dat vyžaduje aspoň 10 GB volného místa v pomocné umístění. Když minimální je 10 %, je vhodné 15 % bezplatné místní úložiště pro umístění mezipaměti.
- Data budou uložena v úložišti Azure trezoru. Existuje neomezené množství dat, které je možné zpátky do zálohu Azure vault ale velikost zdroje dat (třeba virtuálního počítače nebo databáze) neměli překročit 54,400 GB.

Pro zpátky do Azure jsou podporovány tyto typy souborů:

- Šifrované (úplné zálohování pouze)
- Komprimovat (podporované přírůstková zálohování)
- Sparse (podporované přírůstková zálohování)
- Komprimovaná a sparse (považovaná za Sparse)

A to nejsou podporované:

- Servery v systému souborů malá a velká písmena nejsou podporované.
- Pevné odkazy (přeskočené)
- Body (přeskočené)
- Zašifrovaných a komprimovány (přeskočené)
- Zašifrované a sparse (přeskočilo)
- Komprimovaná toku
- Sparse toku

>[AZURE.NOTE] Z v System Center 2012 DPM s aktualizací SP1, můžete zálohovat nahoru pracovního vytížení chráněny DPM Azure pomocí Microsoft Azure zálohování.
