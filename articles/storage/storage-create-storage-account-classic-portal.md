<properties
    pageTitle="Jak vytvářet, spravovat nebo odstraněním účtu úložiště na portálu klasické Azure | Microsoft Azure"
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

Účet Azure úložiště vám umožňuje přístup ke službám objektů Blob Azure, fronty, tabulky a souboru v úložišti Azure. Účtu úložiště poskytuje jedinečných názvů pro datové objekty Azure úložiště. Ve výchozím nastavení je dostupná jenom pro vás vlastník účtu data ve vašem účtu.

Existují dva typy úložiště účtů:

- Standardní úložiště účet obsahuje úložiště objektů Blob, tabulky, fronty a soubor.
- Účet premium úložiště v současné době podporuje pouze u disků Azure virtuálního počítače. V tématu [Premium úložiště: výkonné úložiště pro Azure virtuálního počítače úloh](storage-premium-storage.md) podrobné informace o Premium úložiště.

## <a name="storage-account-billing"></a>Fakturace účtu úložiště

Je faktura pro využití úložiště Azure podle účtu úložiště. Úložiště náklady jsou založeny na čtyři skutečností: kapacitu úložiště, replikace schéma, úložiště transakce a výstupní data.

- Kapacitu úložiště odkazuje na kolik plnění účtu úložiště jste používat k ukládání dat. Náklady jednoduše ukládání dat, je určený podle množství zpracovávaných dat ukládáte a jak je replikovat.
- Replikace Určuje, kolik kopií dat jsou zachovány v celém dokumentu a v umístění, ve kterých.
- Transakce v nápovědě k všechny čtení a zápis k základnímu úložišti Azure.
- Výstupní data odkazuje přenesených z Azure oblast dat. Když data ve vašem účtu úložiště přistupuje aplikací, který není spuštěný ve stejné oblasti, jestli tato aplikace je Cloudová služba nebo jiný typ aplikace, pak vám bude účtovaná za výstupní data. (Služby Azure, může trvat postup seskupit data a služby ve stejném datacentrech ke snížení nebo odstranění poplatky za data výstupní.)  

Stránka [Azure úložiště ceny](https://azure.microsoft.com/pricing/details/storage) obsahuje podrobné informace o kapacitu úložiště, replikace a transakce cenách. Na stránce [Podrobnosti ceny přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/) obsahuje podrobné informace o výstupní data cenách.

Podrobnosti o výkonu a kapacity cílů účtu úložiště najdete v tématu [Azure úložiště škálovatelnost a výkonu cílů](storage-scalability-targets.md).

> [AZURE.NOTE] Při vytváření Azure virtuálního počítače k účtu úložiště se vytvoří automaticky v umístění nasazení Pokud už nemáte účet úložiště v uvedeném umístění. Takže není nutné zadávat postupujte podle pokynů a vytvořte účet úložiště pro disků virtuálního počítače. Název účtu úložiště bude podle názvu virtuálního počítače. Najdete v [dokumentaci Azure virtuálních počítačích](https://azure.microsoft.com/documentation/services/virtual-machines/) další podrobnosti.

## <a name="create-a-storage-account"></a>Vytvoření účtu úložiště

1. Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).

2. Na hlavním panelu v dolní části stránky klikněte na **Nový** . Klikněte na **Data služby** | **úložiště**a potom klikněte na **Vytvořit**.

    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. **Adresa URL**vyplňte název účtu úložiště.

    > [AZURE.NOTE] Názvy účtů úložiště musí být mezi 3 a 24 znaků a může obsahovat čísla pouze malými písmeny.
    >  
    > Název účtu úložiště musí být jedinečný v rámci Azure. Klasický portálu Azure výskyt znamená, pokud se již považuje název účtu úložiště, kterou vyberete.

    Další informace o použití název účtu úložiště při řešení objekty v úložišti Azure najdete v článku [koncové body účtu úložiště](#storage-account-endpoints) níže.

4. **Umístění/spřažení skupiny**vyberte umístění pro váš účet úložiště, která je zavřít můžete nebo zákazníkům. Pokud data ve vašem účtu úložiště se k nim získat přístup z jiné služby Azure, například dlouhé Azure virtuálního počítače nebo cloudové služby, můžete vybrat skupinu spřažení ze seznamu do skupiny účtu úložiště v centru dat s jinými Azure službami, které používáte pro zvýšení výkonu a nižší náklady.

    Všimněte si, že musíte vybrat skupinu spřažení při vytvoření účtu úložiště. Existující účet nelze přesunout do skupiny spřažení. Další informace o skupinách spřažení najdete v článku [umístění služby spolu se skupinou spřažení](#service-co-location-with-an-affinity-group) dole.

    >[AZURE.IMPORTANT] Určit, která umístění jsou k dispozici pro vaše předplatné, můžete volat operaci [seznam všech poskytovatelů zdroje](https://msdn.microsoft.com/library/azure/dn790524.aspx) . Seznam poskytovatele z prostředí PowerShell, zavolejte na [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx). Z .NET použijte metodu [seznamu](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) třídy ProviderOperationsExtensions.
    >
    >Další informace o služby, jsou k dispozici v které oblasti navíc uvidíte [Azure oblasti](https://azure.microsoft.com/regions/#services) .


5. Pokud máte víc předplatných Azure, se zobrazí pole **předplatného** . **Předplatné**vyplňte Azure, který chcete použít účet úložiště u předplatného.

6. **Replikace**vyberte požadovanou úroveň replikace účtu úložiště. Možnost doporučené replikace je geo nadbytečné replikace, který obsahuje maximální životnosti pro vaše data. Podrobné informace o možnosti replikace v úložišti Azure najdete v článku [replikace Azure úložiště](storage-redundancy.md).

6. Klikněte na **vytvořit účet úložiště**.

    Může trvat několik minut k vytvoření účtu úložiště. Pokud chcete zkontrolovat stav, můžete sledovat oznámení v dolní části klasické portálu Azure. Po vytvoření účtu úložiště vašemu novému účtu úložiště má **Online** stav a je připraven k použití.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### <a name="storage-account-endpoints"></a>Koncové body účtu úložiště

Každý objekt, který máte uložené v úložišti Azure obsahuje jedinečné adresy URL. Název účtu úložiště tvoří subdoménu, které budou řešit. Kombinace subdoménu a název domény, která je specifická pro každou službu, tvoří *koncový bod* pro váš účet úložiště.

Pokud váš účet úložiště jmenuje *mystorageaccount*, pak výchozí koncové body účtu úložiště jsou například:

- Kulatý služby: http://*mystorageaccount*. blob.core.windows.net

- Tabulku služba: http://*mystorageaccount*. table.core.windows.net

- Fronta služby: http://*mystorageaccount*. queue.core.windows.net

- Služba souborů: http://*mystorageaccount*. file.core.windows.net

Zobrazí se koncové body účtu úložiště na řídicím panelu úložiště na [Klasické portál Azure](https://manage.windowsazure.com) po vytvoření účtu.

Adresa URL pro přístup k objektu v účtu úložiště je integrována přidáním jeho umístění v účtu úložiště na koncový bod. Například adresu objektů blob může mít tento formát: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Můžete taky nakonfigurovat vlastní název domény použít s účtem úložiště. Další informace v tématu [Konfigurace vlastního názvu domény pro váš koncový bod úložiště objektů blob](storage-custom-domain-name.md) .

### <a name="service-co-location-with-an-affinity-group"></a>Umístění služby spolu se skupinou spřažení

*Skupina spřažení* je zeměpisné seskupení Azure služeb a VMs s účtem Azure úložiště. Skupinu spřažení můžete zlepšit výkonu služby vyhledáním pracovního vytížení počítače ve stejném datovém centru obrázek nebo poblíž cílovou skupinu uživatelů. Navíc žádné fakturační náklady jsou vzniklé v souvislosti s výstupní při dat v účtu úložiště k nim získat přístup z jiné služby, která je součástí stejné spřažení skupiny.

> [AZURE.NOTE]  Vytvořit skupinu spřažení, otevřete oblasti <b>Nastavení</b> [Portálu klasické Azure](https://manage.windowsazure.com), klikněte na tlačítko <b>spřažení skupiny</b>a klikněte na možnost <b>Přidat skupinu spřažení</b> nebo na tlačítko <b>Přidat</b> . Můžete taky vytváření a Správa skupin spřažení pomocí rozhraní API správy služby Azure. Další informace najdete v tématu <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">operace spřažení skupiny</a> .

## <a name="view-copy-and-regenerate-storage-access-keys"></a>Zobrazení, kopírovat a obnovit úložiště přístupových kláves

Při vytváření účtu úložiště Azure vygeneruje dva 512-bit úložiště přístupové klávesy, které se používají pro ověřování při přístupu k účtu úložiště. Zadáním dvou úložiště přístupových kláves Azure vám umožní obnovit klávesy s přístup ke službě nebo nijak nepřeruší služba úložiště.

> [AZURE.NOTE] Doporučujeme vyhnout se přístupových kláves úložiště nasdílet nikoho jiného. K povolení přístupu k prostředkům úložiště aniž by se přístupové klávesy, můžete *podpis sdílený přístup*. Podpis sdílený přístup poskytuje přístup k prostředku ve vašem účtu pro interval, který určíte a oprávněními, které zadáte. Další informace najdete v tématu [Používání sdílených přístup podpisy (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md) .

Na [Portálu klasické Azure](https://manage.windowsazure.com)klávesami **Spravovat** na řídicím panelu nebo stránku **úložiště** k zobrazení, kopírovat a obnovit přístupových kláves úložiště, které se používají pro přístup ke službám objektů Blob, tabulky a fronty.

### <a name="copy-a-storage-access-key"></a>Zkopírujte přístupová klávesa úložiště  

**Správa klíčů** můžete zkopírovat úložiště přístupová klávesa používat v připojovacím řetězci. Připojovací řetězec vyžaduje název účtu úložiště a klíč, který používá ověřování. Informace o konfiguraci řetězce připojení pro přístup ke službám Azure úložiště najdete v tématu [Konfigurace Azure úložiště připojovací řetězec](storage-configure-connection-string.md).

1. [Azure klasické portál](https://manage.windowsazure.com)kliknout na **úložiště**a potom klikněte na název účtu úložiště otevřete řídicího panelu.

2. Klikněte na **spravovat klíče**.

    **Správa přístupových kláves** otevře.

    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. Zkopírovat přístupová klávesa úložiště, vyberte text klíče. Po klepnutí pravým tlačítkem myši a klikněte na **Kopírovat**.

### <a name="regenerate-storage-access-keys"></a>Obnovit úložiště přístupových kláves
Doporučujeme Změna přístupových kláves ke svému účtu úložiště pravidelně na zabezpečit připojení k úložišti. Dva přístupové klávesy, mají přiřazenou tak, aby k zachování připojení k účtu úložiště pomocí jednoho přístupové klávesy, zatímco obnovit přístupové klávesy.

> [AZURE.WARNING] Obnovení přístupových kláves můžou mít vliv na služby Azure, jakož i vlastní aplikace, které jsou závislé na účtu úložiště. Všechny klienty, které používají přístupová klávesa pro přístup k účtu úložiště musí být aktualizovány pomocí nového klíče.

**Media services** – Pokud máte médií služby, které jsou závislé na vašem účtu úložiště, třeba znovu synchronizujete přístupových kláves s vašimi službami médií po obnovit klíče.

**Aplikace** – Pokud máte webových aplikací nebo cloudové služby, které používají účet úložiště, můžete dojde ke ztrátě připojení obnovit klávesy, pokud vrátit klíče. 

**Průzkumník úložiště** – Pokud používáte všechny [aplikace explorer úložiště](storage-explorers.md), bude nejspíš potřebujete aktualizovat klávesu úložiště používá v těchto aplikacích.

Tady je proces pro otočení úložiště přístupových kláves:

1. Aktualizujte připojení řetězce v kódu aplikace neodkazuje sekundární přístupová klávesa účtu úložiště.

2. Obnovit primární přístupová klávesa účtu úložiště. V [Klasickém portál Azure](https://manage.windowsazure.com), z řídicího panelu nebo na stránce **Konfigurovat** klikněte na **Správa klíčů**. Klepněte na **Obnovit** primární přístupové klávesy a potom klikněte na **Ano** potvrďte, že chcete generovat nový klíč.

3. Aktualizujte připojení řetězce v kódu k odkazu na nový přístup primární klíč.

4. Obnovit sekundární přístupové klávesy.

## <a name="delete-a-storage-account"></a>Odstranění účtu úložiště

Odebrání účtu úložiště, kterou už používáte, můžete **Odstranit** na řídicím panelu nebo na stránce **Konfigurovat** . **Odstranění** odstraní celý úložiště účtu, včetně všech objektů BLOB, tabulky a fronty do účtu.

> [AZURE.WARNING] Není možné obnovit odstraněné úložiště účet nebo načíst těch typů obsahu, který obsahuje před odstraněním. Ujistěte se, že obecnějším údajům všechno, co, který chcete uložit před odstraněním účtu. Toto také platí pro všechny zdroje v okně účet – po odstranění objektů blob, tabulky, fronty nebo soubor, se trvale odstraní.
>
> Obsahuje-li účtu úložiště souborů virtuální pevný disk Azure virtuálního počítače, pak je nutné odstranit všechny obrázky a disků, kteří používají tyto soubory virtuálního pevného disku, před odstraněním účtu úložiště. Nejdřív zastavit virtuální počítač, pokud je spuštěná a potom ho odstraňte. Chcete-li odstranit disků, přejděte na kartu **disků** a všechny discích. Chcete-li odstranit obrázky, přejděte na kartu **obrázky** a obrázky, které jsou uložené na účtu.

1. Na [Portálu klasické Azure](https://manage.windowsazure.com)klikněte na **úložiště**.

2. Klikněte na libovolné místo v okně položka účtu úložiště kromě název a potom klikněte na **Odstranit**.

     – Nebo –

    Klikněte na název účtu úložiště otevřete řídicího panelu a potom klikněte na **Odstranit**.

3. Klikněte na tlačítko **Ano** potvrďte, že chcete odstranění účtu úložiště.

## <a name="next-steps"></a>Další kroky

- Další informace o úložišti Azure, najdete v článku [si přečtěte následující dokumentaci Azure úložiště](https://azure.microsoft.com/documentation/services/storage/).
- Navštivte [Blog týmu Azure úložiště](http://blogs.msdn.com/b/windowsazurestorage/).
- [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
