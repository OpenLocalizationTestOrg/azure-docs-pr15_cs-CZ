<properties
   pageTitle="Použití šifrování disku v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu se dozvíte, jak implementovat Azure Centrum zabezpečení doporučení **šifrování disku použít**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="apply-disk-encryption-in-azure-security-center"></a>Použití šifrování disku v Centru zabezpečení Azure

Centrum zabezpečení Azure bude doporučujeme použít šifrování disku, pokud máte Windows nebo Linux OM disků, které nejsou šifrovaná pomocí šifrování disku Azure. Šifrování disku umožňuje šifrování disků Windows a OM IaaS Linux.  Šifrování doporučujeme OS a data svazky na vaše OM.


Šifrování disku využívá oborové standardní [nástroje BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkce systému Windows a [DM Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux poskytnout OS a šifrování dat k ochraně a ochranu dat a plnit organizační závazky zabezpečení a dodržování předpisů. Šifrování disku je integrována s [Azure klíč trezoru](https://azure.microsoft.com/documentation/services/key-vault/) vám pomůže určit a Správa šifrovacího klíče disku a tajemství předplatné klíč trezoru zároveň zajistit, že jsou všechna data v disků OM šifrované v klidu v [Úložišti Azure](https://azure.microsoft.com/documentation/services/storage/).

> [AZURE.NOTE] Azure šifrování disku je podporován následující serverové operační systémy Windows – Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2. Šifrování disku podporovány následující Linux serveru operačních systémech - systémem Ubuntu CentOS, SUSE a SUSE Linux Enterprise Server (SLES).

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. V zásuvné **doporučení** vyberte **použít šifrování disku**.
2. V zásuvné **šifrování disku použít** zobrazí se seznam VMs, u kterých se doporučuje šifrování disku.
3. Postupujte podle pokynů a použít šifrování těchto VMs.

![][1]

Když Pokud chcete šifrovat Azure virtuálních počítačích, které byly identifikovány Centrum zabezpečení jako nutností šifrování, doporučujeme podle těchto kroků:

- Instalace a konfigurace prostředí PowerShell Azure. To vám umožní provést příkazy Powershellu potřebné k nastavení požadavky potřebná k šifrování Azure virtuálních počítačích.
- Získání a spusťte skript Azure disku šifrovací požadavky Azure Powershellu.
- Šifrování virtuálních počítačích.

[Šifrování virtuálního počítače Azure](security-center-disk-encryption.md) vás provede tyto kroky.  Toto téma předpokládá, že používáte Windows 10 jako klientský počítač, ze které bude konfigurace šifrování disku.

Které lze nastavit požadavky a konfigurace šifrování pro Azure virtuálních počítačích mnoha způsoby. Pokud jste už dobře versed v prostředí PowerShell Azure nebo Azure rozhraní příkazového řádku, pak je možná budete chtít použít alternativní přístupů. Další informace o těchto dalších postupů najdete v článku [šifrování Azure disku](../security/azure-security-disk-encryption.md).



## <a name="see-also"></a>Viz taky

Tento dokument ukázal, jak implementovat Centrum zabezpečení doporučení "Použít disku šifrování." Další informace o šifrování disku, najdete v těchto článcích:

- [Šifrování a správy klíčů s trezoru klíč Azure](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video, 36 min 39 sec) – informace o použití Správa šifrování disků IaaS VMs a Azure klíč trezoru k ochraně a chránit vaše data.
- [Šifrování Azure virtuálního počítače](security-center-disk-encryption.md) (dokument) – zjistěte, jak k šifrování Azure virtuálních počítačích.
- [Šifrování Azure disku](../security/azure-security-disk-encryption.md) (dokument) – zjistěte, jak povolit šifrování disku pro Windows a Linux VMs.

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení.
- [Sledování stavu zabezpečení v Centru zabezpečení Azure](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Správa zabezpečení doporučení v Centru zabezpečení Azure](security-center-recommendations.md) – zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
- Příspěvky [azure zabezpečení blog](http://blogs.msdn.com/b/azuresecurity/) – najít blog týkající se Azure zabezpečení a dodržování předpisů.



<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
