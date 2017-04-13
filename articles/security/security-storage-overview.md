<properties
   pageTitle="Přehled zabezpečení Azure úložiště | Microsoft Azure"
   description=" Azure úložiště je řešení cloudové úložiště pro moderní aplikace, které jsou závislé na životnost, dostupnost a škálovatelnost podle potřeby svých zákazníků. Tento článek obsahuje přehled základních funkcí Azure zabezpečení, které lze použít s Azure úložiště. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/16/2016"
   ms.author="terrylan"/>

# <a name="azure-storage-security-overview"></a>Přehled zabezpečení Azure úložiště

Azure úložiště je řešení cloudové úložiště pro moderní aplikace, které jsou závislé na životnost, dostupnost a škálovatelnost podle potřeby svých zákazníků. Azure úložiště poskytuje komplexní sady možnosti zabezpečení:

- Účet úložiště lze zabezpečit pomocí řízení přístupu na základě rolí a Azure Active Directory.
- Data lze zabezpečit mezi aplikací a Azure pomocí šifrování na straně klienta, HTTPS nebo SMB 3.0.
- Data můžete nastavit automaticky zašifrovat zápisu k základnímu úložišti Azure pomocí úložiště služby šifrování.
- Šifrování pomocí šifrování disku Azure můžete nastavit OS a dat disků používaný virtuálních počítačích.
- Delegovaná přístup k objektům dat v úložišti Azure můžete k ní mít udělený používání sdílené podpisů přístup.
- Na metodě ověřování používané někdo při přístupu k úložiště můžete sledovat pomocí analýzy úložiště.

Podrobnější informace v zabezpečení v úložišti Azure najdete v článku [Průvodce zabezpečením Azure úložiště](../storage/storage-security-guide.md). Tato příručka obsahuje hloubkové postupy do funkce zabezpečení Azure úložiště například klávesy účtu úložiště, šifrování data při přenosu šifrovaná a na ostatních a technologie pro analýzu úložiště.

Tento článek obsahuje přehled funkcí Azure zabezpečení, které lze použít s úložištěm Azure. Za předpokladu, že na články, které poskytují podrobnosti každé funkce, které se můžou dozvědět víc jsou odkazy.

Tady jsou základní funkce vztahovat v tomto článku:

- Řízení přístupu na základě rolí
- Delegovaná přístup k úložišti objektů
- Šifrování při přenosu šifrovaná
- Šifrování na zbývající/úložiště služby šifrování
- Šifrování Azure disku
- Azure klíčové trezoru

## <a name="role-based-access-control-rbac"></a>Řízení přístupu na základě rolí (RBAC)

Můžete zabezpečení účtu úložiště pomocí řízení přístupu na základě rolí (RBAC). Omezení přístupu na základě [znát](https://en.wikipedia.org/wiki/Need_to_know) a [nejnižších možných oprávnění](https://en.wikipedia.org/wiki/Principle_of_least_privilege) zásad zabezpečení je v organizacích, které chcete musí vynucovat nějaké zásady zabezpečení pro přístup k datům. Tato přístupová práva uděluje přiřazení příslušné RBAC role do skupiny a aplikací na určitý obor. [Předdefinované RBAC role](../active-directory/role-based-access-built-in-roles.md), například Přispěvatel účtu úložiště, můžete použít k přiřazování oprávnění uživatelům.

Víc se uč:

- [Řízení přístupu na základě rolí Azure Active Directory](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-to-storage-objects"></a>Delegovaná přístup k úložišti objektů

Sdílený přístup k podpisu (přidružení zabezpečení) obsahuje delegovanou přístupu k prostředkům ve vašem účtu úložiště. Přidružení zabezpečení znamená, že můžete udělit oprávnění k objektům ve vašem účtu úložiště klienta omezené stanovený počet minut a zadaný sadu oprávnění. Tato omezená oprávnění můžete udělovat aniž by bylo nutné sdílet svůj účet přístupové klávesy. Přidružení zabezpečení je identifikátor URI, který zahrnuje v jeho parametry dotazu všechny informace potřebné pro ověřený přístup úložiště zdroji. Přístup k prostředkům úložiště přidružením, klienta pouze musí předat přidružení zabezpečení odpovídající konstruktor nebo metody.

Víc se uč:

- [Principy modelu přidružení zabezpečení](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Kam zmizely přidružení zabezpečení s úložiště objektů Blob](../storage/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Šifrování při přenosu šifrovaná
Šifrování při přenosu šifrovaná je mechanismus ochrany data při přenosu sítích. S úložištěm Azure zabezpečit lze dat pomocí:

- [Šifrování transportní](../storage/storage-security-guide.md#encryption-in-transit), například HTTPS při převodu dat nebo stmívání Azure úložiště.
- [Drátěný šifrování](../storage/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), například plány SMB 3.0 šifrování Azure sdílené složky.
- [Šifrování na straně klienta](../storage/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage)a šifrování data před převádí se na úložiště dešifrování data po přenosu z úložiště.

Další informace o šifrování na straně klienta:

- [Šifrování klienta pro úložišti Microsoft Azure](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
- [Cloud zabezpečení určuje řadu: šifrování Data při přenosu šifrovaná](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Šifrování na ostatní

Pro mnoho organizace je [šifrování dat na zbývající](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) povinné krokem k dat ochrany osobních údajů, dodržování předpisů a zůstala zachovaná svrchovanost data. Existují tři Azure funkce, které poskytují šifrování data, která jsou "at zbývající":

- [Úložiště služby šifrování](../storage/storage-security-guide.md#encryption-at-rest) umožňuje požadovat, aby služba úložiště automaticky zašifrovat dat při psaní k základnímu úložišti Azure.
- [Šifrování na straně klienta](../storage/storage-security-guide.md#client-side-encryption) také poskytuje funkce šifrování na ostatních.
- [Šifrování disku Azure](../storage/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) umožňuje šifrování s operačním systémem od disků dat používaných ve počítače virtuální IaaS.

Další informace o úložiště služby šifrování:

- [Šifrování služby Azure úložiště](https://azure.microsoft.com/services/storage/) je dostupný u [Úložišti objektů Blob Azure](https://azure.microsoft.com/services/storage/blobs/). Podrobnosti o jiných typech Azure úložiště najdete v tématu [soubor](https://azure.microsoft.com/services/storage/files/) [disku (Premium úložiště)](https://azure.microsoft.com/services/storage/premium-storage/), [tabulky](https://azure.microsoft.com/services/storage/tables/)a [fronty](https://azure.microsoft.com/services/storage/queues/).
- [Šifrování služby Azure úložiště dat u ostatních](../storage/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Šifrování Azure disku

Azure šifrování disku pro virtuálních počítačích (VMs) vám pomůže adresy organizace zabezpečení a dodržování předpisů požadavky zašifrováním disků OM (včetně spuštění a data disků) s klíče a zásad, které můžete řídit v [Azure klíč trezoru](https://azure.microsoft.com/services/key-vault/).

Šifrování disku pro VMs vyhovovat operační systémy Windows a Linux. Taky používá klíč trezoru týkající se zabezpečení, Správa a auditování disku šifrovacího klíče. Všechna data v disků OM zašifrován u ostatních pomocí technologie šifrování standardní v úložišti Azure účty. Šifrování disku řešení pro systém Windows je založeno na [Microsoft nástroje BitLocker šifrovacím](https://technet.microsoft.com/library/cc732774.aspx)a Linux řešení je založeno na [dm crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Víc se uč:

- [Šifrování Azure disku pro Windows a Linux IaaS virtuálních počítačích](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure klíčové trezoru

Azure šifrování disku používá [Azure klíč trezoru](https://azure.microsoft.com/services/key-vault/) vám pomůže určit a Správa šifrovacího klíče disku a tajemství předplatné klíčové trezoru zároveň zajistit, že jsou všechna data v disků virtuálního počítače šifrované v klidu v úložišti Azure. Měli byste použít klíč trezoru mají být auditovány klíče a použití zásad.

Víc se uč:

- [Co je Azure klíč trezoru?](../key-vault/key-vault-whatis.md)
- [Začínáme s trezoru klíč Azure](../key-vault/key-vault-get-started.md)
