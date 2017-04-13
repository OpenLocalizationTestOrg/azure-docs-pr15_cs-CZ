<properties 
    pageTitle="Import a Export dat do mezipaměti Redis Azure | Microsoft Azure" 
    description="Zjistěte, jak import a export dat do a z úložiště objektů blob s vaší instancemi Azure Redis mezipaměti premium" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="import-and-export-data-in-azure-redis-cache"></a>Import a Export dat do mezipaměti Redis Azure

Import nebo Export je Azure Redis mezipaměti dat správu operaci umožňující import dat do mezipaměti Redis Azure či exportovat data z mezipaměti Redis Azure import a Export snímku Redis mezipaměti databáze (RDB) z mezipaměti premium objektů blob stránky v účet Azure úložiště. Import nebo Export umožňuje migrace mezi jiné instance Azure Redis mezipaměti nebo vyplnění mezipaměti se data před použitím.

Tento článek obsahuje příručku pro import a export dat s Azure Redis mezipamětí a odpovědi na nejčastější dotazy.

>[AZURE.IMPORTANT] Import nebo Export v náhledu a je dostupný jenom u [vrstvy premium](cache-premium-tier-intro.md) mezipaměti.

## <a name="import"></a>Import

Import mohou sloužit k přenesení Redis kompatibilní RDB soubory z Redis serveru v cloudu nebo prostředí, včetně Redis na Linux, systému kteréhokoliv poskytovatele cloudu například Amazon webovým službám a další. Import dat je snadný způsob, jak vytvořit mezipaměť přednastavený daty. Během procesu importu Azure Redis mezipaměť načte RDB soubory z Azure úložiště do paměti a vloží klávesy do mezipaměti.

>[AZURE.NOTE] Před zahájením importu, ujistěte se, že Redis databáze (RDB) souboru nebo souborům jsou odeslány do stránky objektů BLOB Azure úložiště, oblasti stejných předplatné jako Azure Redis mezipaměti instance. Další informace najdete v tématu [Začínáme s úložiště objektů Blob Azure](../storage/storage-dotnet-how-to-use-blobs.md). Při exportu souboru RDB pomocí funkce [Azure Redis mezipaměti Export](#export) souboru RDB uložené v objektů blob stránky a je připraven k importu.

1. Import jeden nebo více exportovaného mezipaměti objektů BLOB, [Procházet a mezipaměť](cache-configure.md#configure-redis-cache-settings) Azure portálu a klikněte na **Import dat** z **Nastavení** zásuvné instance mezipaměti.

    ![Import dat][cache-import-data]

2. Klikněte na **Zvolit Blob(s)** a vyberte účet úložiště, který obsahuje data, která chcete importovat.

    ![Vyberte účet úložiště][cache-import-choose-storage-account]

3. Klikněte na kontejner obsahující data, která chcete importovat.

    ![Zvolte kontejneru][cache-import-choose-container]

4. Vyberte jeden nebo více objektů BLOB k importu kliknutím na oblast a vlevo od jména objektů blob a potom klikněte na **Výběr**.

    ![Zvolte objekty BLOB][cache-import-choose-blobs]

5. Klikněte na **Import** zahájíte importu.

    >[AZURE.IMPORTANT] Mezipaměť nebývá klienty mezipaměti během importu a odstranit stávající data v mezipaměti.

    ![Import][cache-import-blobs]

    Sledovat oznámení z portálu Microsoft Azure nebo zobrazení událostí v [protokolu auditování](cache-configure.md#support-amp-troubleshooting-settings)můžete sledovat průběh operace importu.

    ![Průběh importu][cache-import-data-import-complete] 


## <a name="export"></a>Export

Export umožňuje exportovat data uložená v mezipaměti Redis Azure k Redis kompatibilní soubory RDB. Tato funkce slouží k přesunutí dat z Azure Redis mezipaměti instancemi do jiného nebo jiné Redis server. Při exportu vytvoříte dočasný soubor je vytvořen na OM hostujícího instanci server Azure Redis mezipaměti a soubor odeslat k tomuto účtu určené úložiště. Po dokončení operace exportu se stavem úspěch nebo selhání dočasný soubor se odstraní.

1. Chcete exportovat aktuální obsah mezipaměti k základnímu úložišti, [přejděte do mezipaměti](cache-configure.md#configure-redis-cache-settings) Azure portálu a klikněte na **Exportovat data** z **Nastavení** zásuvné instance mezipaměti.

    ![Zvolte kontejner úložiště][cache-export-data-choose-storage-container]

2. Klikněte na **Zvolit kontejner úložiště** a vyberte požadované úložiště účet. Účet úložiště musí být ve stejném předplatné a oblasti jako mezipaměť.

    >[AZURE.IMPORTANT] Import nebo Export funguje s objekty BLOB stránky, které jsou podporovány klasické a účty ARM úložiště, ale nepodporuje [účty úložiště objektů Blob](../storage/storage-blob-storage-tiers.md#blob-storage-accounts) v současné době.

    ![Úložiště účtu][cache-export-data-choose-account]

3. Vyberte požadovanou objektů blob kontejner a klikněte na **Výběr**. Použít nový kontejneru, klikněte na **Přidat kontejneru** nejdřív přidat a potom vyberte ze seznamu.

    ![Zvolte kontejner úložiště][cache-export-data-container]

4. Zadejte **název předponu objektů Blob** a klikněte na **Exportovat** spuštění operace exportu. Předpona jména objektů blob slouží k předpona názvy souborů generovaného při operaci exportu.

    ![Export][cache-export-data]

    Můžete sledovat průběh operace exportu, sledování oznámení z portálu Microsoft Azure nebo zobrazení událostí v [protokolu auditování](cache-configure.md#support-amp-troubleshooting-settings).

    ![][cache-export-data-export-complete]

    Mezipaměti zůstávají k dispozici pro použití při exportu vytvoříte.


## <a name="importexport-faq"></a>Import nebo Export časté otázky

Tato část obsahuje nejčastější dotazy týkající se funkcí Import nebo Export.

-   [K čemu ceny úrovně můžete použít Import nebo Export?](#what-pricing-tiers-can-use-importexport)
-   [Můžete importovat data z libovolného Redis serveru?](#can-i-import-data-from-any-redis-server)
-   [Moje mezipaměti budou k dispozici během operace importu nebo exportu?](#will-my-cache-be-available-during-an-importexport-operation)
-   [Mám používat Import nebo Export se Redis obrázku?](#can-i-use-importexport-with-redis-cluster)
-   [Jak funguje Import nebo Export s vlastní nastavení databáze?](#how-does-importexport-work-with-a-custom-databases-setting)
-   [Čím se liší od Redis trvalé Import nebo Export](#how-is-importexport-different-from-redis-persistence)
-   [Můžu automatizovat Import nebo Export pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiných správy klientů?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
-   [Dostali chybě vypršení časového limitu během operace Moje Import nebo Export. Co znamená?](#i-received-a-timeout-error-during-my-importexport-operation.-what-does-it-mean)
-   [Chyba získali při exportu dat k úložišti objektů Blob Azure. Co se přihodilo?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage.-what-happened)


### <a name="what-pricing-tiers-can-use-importexport"></a>K čemu ceny úrovně můžete použít Import nebo Export?

Import nebo Export je k dispozici pouze v premium ceny osy.

### <a name="can-i-import-data-from-any-redis-server"></a>Můžete importovat data z libovolného Redis serveru?

Ano, můžete kromě importu dat vyexportovaný z mezipaměti Redis Azure instance import RDB souborů z jakéhokoli Redis serveru s Windows, v cloudu nebo prostředí, například Linux, nebo cloudového poskytovatelů například Amazon webové služby. K tomuto účelu nahrát soubor RDB z požadovaný Redis server do objektů blob stránky na Azure úložiště účtu a pak je naimportujte do mezipaměti Redis Azure instanci aplikace premium. Můžete třeba exportovat data z mezipaměti výrobní a importujte ho do mezipaměti používá jako součást testovacím prostředí pro testování nebo migrace. 

### <a name="will-my-cache-be-available-during-an-importexport-operation"></a>Moje mezipaměti budou k dispozici během operace importu nebo exportu?

-   **Export** – mezipamětí ještě zbývají k dispozici a můžete dál používat mezipaměť během operace exportu.
-   **Import** - mezipaměti nedostupná při spuštění operace importu a je k dispozici pro použití po dokončení operace importu.


### <a name="can-i-use-importexport-with-redis-cluster"></a>Mám používat Import nebo Export se Redis obrázku?

Ano, a je možné import nebo export mezi skupinový mezipaměti a neseskupené mezipaměti. Od Redis clusteru [podporuje pouze databáze 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), nebudou importovány všechna data v databázích než 0. Po importu skupinový mezipaměti dat jsou rozdělí mezi klíči mezi shards clusteru. 

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Jak funguje Import nebo Export s vlastní nastavení databáze?

Některé ceny úrovní mít jiné [databáze omezení](cache-configure.md#databases), aby při importu, pokud jste nakonfigurovali vlastní hodnoty pro některé aspekty, které jsou `databases` nastavení při vytváření mezipaměti.

-   Při importu ceny osy s dolní `databases` omezení než osy, ze kterého jste exportovali:
    -   Pokud používáte výchozí počet `databases` tedy 16 pro všechny úrovně ceny, bez data budou ztracena.
    -   Pokud používáte vlastní počet `databases` této spadá do limity pro osy, do kterého chcete importovat, bez data budou ztracena.
    -   Pokud data exportovaná data v databázi, která překračuje limity nové osy, data z těchto vyšších se neimportují.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Čím se liší od Redis trvalé Import nebo Export

Azure trvalé Redis mezipaměti umožňuje uchovávat data uložená v Redis k základnímu úložišti Azure. Při konfiguraci trvalé zůstává v mezipaměti Redis Azure snímek mezipaměti Redis v binárním formátu Redis podle frekvenci konfigurovatelné záložní disk. Pokud dojde k kritické události, která zakáže primární a otevřené mezipaměti, se obnoví data v mezipaměti automaticky pomocí poslední snímek. Další informace najdete v tématu [jak nakonfigurovat trvání dat pro mezipaměť Redis Azure Premium](cache-how-to-premium-persistence.md).

Import a Export umožňuje data do importovat nebo exportovat z mezipaměti Redis Azure. Není konfigurace zálohování a obnovení pomocí Redis trvalé.


### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Můžu automatizovat Import nebo Export pomocí prostředí PowerShell, rozhraní příkazového řádku nebo jiných správy klientů?

Ano, pro PowerShell pokyny najdete v článku [Import Redis mezipaměti](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) a [Export Redis mezipaměť](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).



### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Můžu doručeny chybě vypršení časového limitu během operace Moje Import nebo Export. Co znamená?

Pokud zůstat na zásuvné **data importovat** nebo **Exportovat data** po dobu delší než 15 minut před zahájením operace můžete dojde k chybě podobně jako tento.

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Tento problém vyřešíte, zahajte import nebo export operaci dříve, než 15 minut uplynul.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Chyba získali při exportu dat k úložišti objektů Blob Azure. Co se přihodilo?

Import nebo Export spolupracuje pouze RDB souborů uložených jako objekty BLOB stránky. Další typy objektů blob nejsou podporovány v současné době, včetně účtů úložiště objektů blob s žádanou a zajímavých úrovní.


## <a name="next-steps"></a>Další kroky
Naučte se práce s dalšími funkcemi mezipaměť premium.

-   [Úvod do osy Azure Redis mezipaměť Premium](cache-premium-tier-intro.md)    

  
<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png








