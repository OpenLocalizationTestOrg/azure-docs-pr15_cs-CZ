<properties
    pageTitle="Nejčastější dotazy k vrstvě Azure | Microsoft Azure"
    description="Azure zásobníku nejčastější dotazy."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="helaw"/>

# <a name="frequently-asked-questions-for-azure-stack"></a>Nejčastější dotazy k vrstvě Azure

## <a name="deployment"></a>Nasazení

### <a name="do-i-need-to-format-my-data-disks-before-starting-or-restarting-an-installation"></a>Je nutné před spuštěním nebo restartování instalace naformátovat disků Moje data?

Disků by měl být v původním formátu. Pokud přeinstalaci operačního systému, budete muset zaškrtněte, pokud staré fondu úložiště stále existuje a odstranit pomocí následujících kroků:

1. Spusťte správce serveru.
2. Vyberte fondů úložiště.
3. Zkontrolujte, zda je uveden fondu úložiště.
4. Klikněte pravým tlačítkem **fondu úložiště** , pokud uvedené a povolení pro čtení a zápis.
5. Klikněte pravým tlačítkem myši **virtuální pevný Disk** (levém dolním rohu) a vyberte odstranit.
6. Klikněte pravým tlačítkem myši **Fondu úložiště** a klikněte na odstranit.
7. Znovu spusťte Azure zásobníku skriptu a ověřte, že ověření disku předá.

V případě potřeby lze tento skript:

```PowerShell
$pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
if ($pools -ne $null) {
  $pools| Set-StoragePool -IsReadOnly $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools = Get-StoragePool -IsPrimordial $False -ErrorVariable Err -ErrorAction SilentlyContinue
  $pools | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$False
  $pools | Remove-StoragePool -Confirm:$False
}
```

### <a name="can-i-use-all-ssd-disks-for-the-storage-pool-in-the-poc-installation"></a>Můžete použít všech discích SSD fondu úložiště v zařízení Koncepce?

Konfigurace není podporované v této verzi.  Další informace najdete v tématu [požadavky na příručku](azure-stack-deploy.md) pro další informace.

### <a name="can-i-use-nvme-data-disks-for-the-microsoft-azure-stack-poc"></a>Můžete použít NVMe dat disků pro Koncepce zásobníku Microsoft Azure?

Zatímco úložiště mezery přímé podporuje NVMe disků, Azure zásobníku pouze podporuje podmnožinu typy možné jednotek a možná kombinace úložiště mezery přímé. 

### <a name="how-can-i-reinstall-azure-stack"></a>Jak můžu nainstalovat znova zásobníku Azure?
Postupujte podle kroků uvedených v [příručce nového nasazení](azure-stack-redeploy.md).  

## <a name="tenant"></a>Klienta

### <a name="can-i-deploy-my-own-images-as-a-tenant"></a>Můžete nasadit vlastní obrázky jako klienta?

Ano, stejně jako v Azure, klienta, kterého můžete odeslat obrázky ve vrstvě Azure kromě použití obrázky od správce služby. Základní informace najdete v článku [Přidání OM obrázek](azure-stack-add-vm-image.md). 

## <a name="testing"></a>Testování

### <a name="can-i-use-nested-virtualization-to-test-the-microsoft-azure-stack-poc"></a>Můžete použít vnořené virtualizace k testování Koncepce zásobníku Microsoft Azure?

Vnořené virtualizace není podporována nebo testováno s Azure zásobníku Technical Preview 2.

## <a name="virtual-machines"></a>Virtuálních počítačích

### <a name="does-azure-stack-support-dynamic-disks-for-virtual-machines"></a>Podporuje Azure zásobníku dynamických disků pro virtuálních počítačích?

Azure zásobníku nepodporuje dynamických discích.

## <a name="next-steps"></a>Další kroky

[Řešení potíží](azure-stack-troubleshooting.md)
