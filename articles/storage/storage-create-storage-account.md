<properties
    pageTitle="Jak vytvářet, spravovat nebo odstraněním účtu úložiště na portálu Azure | Microsoft Azure"
    description="Vytvoření nového účtu úložiště, spravovat svůj účet přístupových kláves nebo odstraněním účtu úložiště na portálu Azure. Další informace o účtech standardních a premium úložiště."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# <a name="about-azure-storage-accounts"></a>Informace o účtech Azure úložiště

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Základní informace

Účet Azure úložiště poskytuje jedinečných názvů moct ukládat a používat datové objekty Azure úložiště. Všechny objekty v účtu úložiště je faktura společně jako skupinu. Ve výchozím nastavení je dostupná jenom pro vás vlastník účtu data ve vašem účtu.

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Úložiště účtu fakturace

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [AZURE.NOTE] Při vytváření Azure virtuálního počítače k účtu úložiště se vytvoří automaticky v umístění nasazení Pokud už nemáte účet úložiště v uvedeném umístění. Takže není nutné zadávat postupujte podle pokynů a vytvořte účet úložiště pro disků virtuálního počítače. Název účtu úložiště bude podle názvu virtuálního počítače. Najdete v [dokumentaci Azure virtuálních počítačích](https://azure.microsoft.com/documentation/services/virtual-machines/) další podrobnosti.

## <a name="storage-account-endpoints"></a>Koncové body účtu úložiště

Každý objekt, který máte uložené v úložišti Azure obsahuje jedinečné adresy URL. Název účtu úložiště tvoří subdoménu, které budou řešit. Kombinace subdoménu a název domény, která je specifická pro každou službu, tvoří *koncový bod* pro váš účet úložiště.

Pokud váš účet úložiště jmenuje *mystorageaccount*, pak výchozí koncové body účtu úložiště jsou například:

- Kulatý služby: http://*mystorageaccount*. blob.core.windows.net

- Tabulku služba: http://*mystorageaccount*. table.core.windows.net

- Fronta služby: http://*mystorageaccount*. queue.core.windows.net

- Služba souborů: http://*mystorageaccount*. file.core.windows.net

> [AZURE.NOTE] Účet úložiště objektů Blob pouze zpřístupňuje koncový bod služby objektů Blob.

Adresa URL pro přístup k objektu v účtu úložiště je integrována přidáním jeho umístění v účtu úložiště na koncový bod. Například adresu objektů blob může mít tento formát: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Můžete taky nakonfigurovat vlastní název domény použít s účtem úložiště. Účty klasická úložiště najdete v článku [Konfigurace vlastní doménu název koncový bod úložiště objektů Blob](storage-custom-domain-name.md) podrobnosti. Správce prostředků úložiště účet a tuto funkci do přidáno [Azure portál](https://portal.azure.com) dosud, ale můžete nakonfigurovat pomocí prostředí PowerShell. Další informace najdete v tématu rutinu [Set-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) .  

## <a name="create-a-storage-account"></a>Vytvoření účtu úložiště

1. Přihlaste se k [portálu Azure](https://portal.azure.com).

2. V nabídce centrální vyberte **Nový** -> **dat + úložiště** -> **účtu úložiště**.

3. Zadejte název účtu úložiště. Další informace o použití název účtu úložiště při řešení objekty v úložišti Azure najdete v článku [koncové body účtu úložiště](#storage-account-endpoints) .

    > [AZURE.NOTE] Názvy účtů úložiště musí být mezi 3 a 24 znaků a může obsahovat čísla pouze malými písmeny.
    >  
    > Název účtu úložiště musí být jedinečný v rámci Azure. Portálu Azure výskyt znamená, pokud se je už název účtu úložiště, kterou vyberete.

4. Určení nasazení modelu pro použití: **Správce prostředků** nebo **Klasická**. **Správce prostředků** je doporučené nasazení modelu. Další informace najdete v tématu [Principy správce prostředků nasazení a klasické nasazení](../resource-manager-deployment-model.md).

    > [AZURE.NOTE] Účty úložiště objektů BLOB lze vytvořit jenom pomocí modelu nasazení Správce prostředků.

5. Vyberte typ účtu úložiště: **univerzální** nebo **úložiště objektů Blob**. **Univerzální** je výchozí hodnota.

    Pokud byla vybrána **univerzální** , pak zadejte osy výkonu: **Standard** nebo **Premium**. Výchozí hodnota je **Standardní**. Podrobné informace o standardních a premium úložiště účty, najdete v článku [Úvod k úložišti tabulek Microsoft Azure](storage-introduction.md) a [Premium úložiště: výkonné úložiště pro Azure virtuálního počítače úloh](storage-premium-storage.md).

    Pokud byla vybrána **Úložiště objektů Blob** , pak zadejte úrovně přístupu: **aktivní** nebo **zajímavé**. Výchozí hodnota je **aktivní**. V tématu [úložišti objektů Blob Azure: ochladí a teplé úrovní](storage-blob-storage-tiers.md) další podrobnosti.

6. Výběrem možnosti replikace účtu úložiště: **LRS**, **GRS**, **Vzdálená pomoc GRS**nebo **ZRS**. Výchozí hodnota je **Vzdálená pomoc GRS**. Podrobné informace o možnosti replikace v úložišti Azure najdete v článku [replikace Azure úložiště](storage-redundancy.md).

7. Vyberte předplatné, ve kterém chcete vytvořit nový účet úložiště.

8. Zadat nové skupiny prostředků nebo vyberte existující skupiny zdrojů. Další informace o skupinách zdroje najdete v článku [Přehled Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md).

9. Vyberte pro svůj účet úložiště zeměpisnou polohu. Další informace o tom, jaké služby k dispozici v které oblasti naleznete v tématu [Azure oblastí](https://azure.microsoft.com/regions/#services) .

10. Klikněte na **vytvořit** vytvořte účet úložiště.

## <a name="manage-your-storage-account"></a>Správa účtu úložiště

### <a name="change-your-account-configuration"></a>Změna konfigurace účtu

Po vytvoření účtu úložiště, můžete změnit jeho konfiguraci, třeba změňte možnosti replikace účtu aplikace nebo změnou úroveň přístupu účtu úložiště objektů Blob. [Azure portál](https://portal.azure.com)přejděte ke svému účtu úložiště, klikněte na **všechna nastavení** a klikněte na **konfigurační** k zobrazení nebo změna konfigurace účtu.

> [AZURE.NOTE] V závislosti na výkon osy, kterou jste zvolili při vytváření účtu úložiště nemusí být některé možnosti replikace dostupná.

Změna možnosti replikace změní vaše ceny. Další informace najdete v článku [Azure úložiště ceny](https://azure.microsoft.com/pricing/details/storage/) stránky.

U účtů úložiště objektů Blob změnit úroveň přístupu mohou být účtovány poplatky za změnu kromě změny vaší ceny. Najdete [účty úložiště objektů Blob – ceny a fakturace](storage-blob-storage-tiers.md#pricing-and-billing) další podrobnosti.

### <a name="manage-your-storage-access-keys"></a>Správa úložiště přístupových kláves

Při vytváření účtu úložiště Azure vygeneruje dva 512-bit úložiště přístupové klávesy, které se používají pro ověřování při přístupu k účtu úložiště. Zadáním dvou úložiště přístupových kláves Azure vám umožní obnovit klávesy s přístup ke službě nebo nijak nepřeruší služba úložiště.

> [AZURE.NOTE] Doporučujeme vyhnout se přístupových kláves úložiště nasdílet nikoho jiného. K povolení přístupu k prostředkům úložiště aniž by se přístupové klávesy, můžete *podpis sdílený přístup*. Podpis s sdílených přístup poskytuje přístup k prostředku ve vašem účtu pro interval, který určíte a oprávněními, které zadáte. Další informace najdete v tématu [Používání sdílených přístup podpisy (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md) .

#### <a name="view-and-copy-storage-access-keys"></a>Zobrazení a kopírování úložiště přístupových kláves

[Azure portál](https://portal.azure.com)přejděte ke svému účtu úložiště, klikněte na **všechna nastavení** a klikněte na **přístupových kláves z verze** zobrazit, kopírovat a obnovit přístupových kláves účtu. Zásuvné **Přístupových kláves z verze** obsahuje taky řetězce předem nakonfigurovaná připojení pomocí kódu primárních a sekundárních Key, které můžete zkopírovat můžete v aplikacích.

#### <a name="regenerate-storage-access-keys"></a>Obnovit úložiště přístupových kláves

Doporučujeme Změna přístupových kláves ke svému účtu úložiště pravidelně na zabezpečit připojení k úložišti. Dva přístupové klávesy, mají přiřazenou tak, aby k zachování připojení k účtu úložiště pomocí jednoho přístupové klávesy, zatímco obnovit přístupové klávesy.

> [AZURE.WARNING] Obnovení přístupových kláves můžou mít vliv na služby Azure, jakož i vlastní aplikace, které jsou závislé na účtu úložiště. Všechny klienty, které používají přístupová klávesa pro přístup k účtu úložiště musí být aktualizovány pomocí nového klíče.

**Media services** – Pokud jste média služby, které jsou závislé na vašem účtu úložiště, třeba znovu synchronizujete přístupových kláves s vašimi službami média po obnovit klíče.

**Aplikace** – Pokud máte webových aplikací nebo cloudové služby, které používají účet úložiště, můžete dojde ke ztrátě připojení obnovovat klíče, pokud vrátit klíče.

**Průzkumník úložiště** – Pokud používáte všechny [aplikace explorer úložiště](storage-explorers.md), bude nejspíš potřebujete aktualizovat klávesu úložiště používá v těchto aplikacích.

Tady je proces pro otočení úložiště přístupových kláves:

1. Aktualizujte připojení řetězce v kódu aplikace neodkazuje sekundární přístupová klávesa účtu úložiště.

2. Obnovit primární přístupová klávesa účtu úložiště. Na zásuvné **Přístupových kláves z verze** klikněte na **Obnovit klíč1**a potom klikněte na **Ano** potvrďte, že chcete generovat nový klíč.

3. Aktualizujte připojení řetězce v kódu k odkazu na nový přístup primární klíč.

4. Obnovit sekundární přístupová klávesa stejným způsobem.

## <a name="delete-a-storage-account"></a>Odstranění účtu úložiště

Odebrání účtu úložiště, kterou už používáte, přejděte na úložiště účet [Azure portál](https://portal.azure.com)a klikněte na **Odstranit**. Odstranění účtu úložiště odstraní celý účtu, včetně všech dat do účtu.

> [AZURE.WARNING] Není možné obnovit odstraněné úložiště účet nebo načíst těch typů obsahu, který obsahuje před odstraněním. Ujistěte se, že obecnějším údajům všeho, který chcete uložit před odstraněním účtu. Toto také platí pro všechny zdroje v okně účet – po odstranění objektů blob, tabulky, fronty nebo soubor, se trvale odstraní.

Odstranění účtu úložiště, který je spojený s Azure virtuálního počítače, je třeba nejprve zajistit, aby byl odstraněn všechny disků virtuálního počítače. Pokud není nejdřív odstraníte disků virtuálního počítače, pak při pokusu o odstranění účtu úložiště, zobrazí se podobná chybová zpráva:

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Pokud účet úložiště používá nasazení modelu Klasický, můžete odebrat disku virtuálního počítače pomocí následujících kroků [Azure portálu](https://manage.windowsazure.com):

1. Přejděte na [klasické Azure portálu](https://manage.windowsazure.com).
2. Přejděte na kartu virtuálních počítačích.
3. Klikněte na kartu discích.
4. Vyberte datový disk a klikněte na Odstranit disku.
5. Chcete-li odstranit obrázky disku, přejděte na kartu obrázky a obrázky, které jsou uložené na účtu.

Další informace najdete v článku [si přečtěte následující dokumentaci Azure virtuálního počítače](http://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="next-steps"></a>Další kroky

- [Úložiště objektů Blob Azure: Skvělé a aktivní vrstvy](storage-blob-storage-tiers.md)
- [Azure replikace úložiště](storage-redundancy.md)
- [Konfigurace Azure úložiště připojovací řetězec](storage-configure-connection-string.md)
- [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
- Navštivte [Blog týmu Azure úložiště](http://blogs.msdn.com/b/windowsazurestorage/).
