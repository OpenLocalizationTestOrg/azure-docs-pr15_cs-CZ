<properties
   pageTitle="Obnovení služby trezoru nejčastější dotazy týkající se | Microsoft Azure"
   description="Tato verze nejčastější dotazy týkající se podporuje verzi Preview veřejné služby Azure zálohování. Odpovědi na nejčastější dotazy týkající se záložní agent, zálohování a uchovávání informací, využití, zabezpečení a další časté otázky týkající se řešení Azure zálohování."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="řešení zálohování; zálohování služby"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# <a name="recovery-services-vault---faq"></a>Obnovení služby trezoru – nejčastější dotazy


Tento článek obsahuje informace specifické pro služby Recovery trezoru a doplňuje [Nejčastější dotazy týkající se Azure zálohování](backup-azure-backup-faq.md). ČLÁNKU zálohy Azure najdete úplné sady otázky a odpovědi týkající se služby Azure zálohování.  

Zálohování Azure v části Disqus v tomto článku nebo související článku můžete klást otázky. Taky můžete pokládat dotazy k služby Azure zálohování [diskusní fóra](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Obnovení služby trezorů správcem zdroje na základě. Zálohování trezorů (klasický režim) i nadále podporuje? <br/>
Ano, zálohování trezorů nadále podporuje. Vytvořte zálohu trezorů [klasický portál](https://manage.windowsazure.com). Vytvoření trezorů obnovení služby [Azure portálu](https://portal.azure.com). Však můžete vytvářet obnovení služby trezoru jako všechny budoucí vylepšení důrazně doporučujeme nebude k dispozici pouze v služby Recovery trezoru.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Můžete migrovat trezoru zálohování do služby Recovery trezoru? <br/>
Bohužel Ne, v současné době nejde nejdřív migrujete obsah trezoru zálohování do služby Recovery trezoru. Pracujeme na přidání tuto funkci, ale není k dispozici v rámci veřejné náhled.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Obnovení služby trezorů podporují klasické VMs nebo správce prostředků na základě VMs? <br/>
Obnovení služby trezorů domovské stránce podpory oba modely.  Můžete obecnějším údajům OM vytvořené v portálu klasické, (které jsou VMs klasický režim) nebo OM vytvořené v portálu Azure, (které jsou na základě správce prostředků) do služby Recovery trezoru.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Můžu mít zálohovala Moje klasické VMs záložní trezoru. Teď chcete migrovat Moje VMs z klasický režim do režimu správce prostředků.  Jak lze zálohovat jejich obnovení služby trezoru?
Zálohování klasický VMs záložní trezoru se nemigrují automaticky do obnovení služby trezoru při migraci VMs z klasický režim správce prostředků. Migrace OM zálohy prosím tyto kroky:

1. Zálohování trezoru přejděte na kartu **Chráněné položky** a vyberte OM. Klikněte na [Odemknout](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Nechte *Odstranit spojenými daty záložní* možnost **zrušené zaškrtnutí políčka**.
2. Migrace virtuálního počítače z klasický režim do režimu správce prostředků. Ujistěte se, že úložiště a síťové odpovídající virtuální počítač také migrují do režimu správce prostředků.
3. Vytvoření trezoru služby obnovení a konfigurace zálohování virtuální počítače migrované pomocí **záložní** akce horní části řídicího panelu trezoru. Další informace o tom, jak [Povolit zálohování trezoru služby obnovení](backup-azure-vms-first-look-arm.md)
