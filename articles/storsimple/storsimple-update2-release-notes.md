<properties 
   pageTitle="Poznámky k verzi StorSimple 8000 řady aktualizace 2 | Microsoft Azure"
   description="Popisuje nové funkce, problémy a řešení StorSimple 8000 řady aktualizace 2."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-2-release-notes"></a>Poznámky k verzi StorSimple 8000 řady aktualizace 2  

## <a name="overview"></a>Základní informace

Následující poznámky k verzi popisy s novými funkcemi a identifikovat kritické otevřené problémy StorSimple 8000 řady aktualizace 2. Obsahují také seznam software StorSimple, ovladač a tato verze zahrnuje aktualizaci firmwaru disku. 

Aktualizace 2 lze použít na libovolném zařízení StorSimple projdete aktualizace 1.2 vydání (GA) nebo aktualizace 0,1. Verze zařízení přidružený k aktualizaci 2 je 6.3.9600.17673.

Zkontrolujte informace obsažené v poznámky k verzi před nasazením aktualizace ve vašem StorSimple řešení.

>[AZURE.IMPORTANT]
> 
- Trvá asi 4 7 hodiny nainstalovat tuto aktualizaci (včetně aktualizací Windows). 
- Aktualizace 2 má software, LSI ovladač a aktualizace firmwaru SSD.
- Pro nové verze, nemusí zobrazit aktualizace okamžitě, protože jsme proveďte fázích aktualizace. Počkejte několik dnů a potom vyhledejte aktualizace znova se budou k dispozici brzy bude k dispozici.


## <a name="whats-new-in-update-2"></a>Co je nového v aktualizace 2

Aktualizace 2 zavádí následujícím novým funkcím.

- **Místně připnuté svazky** – v předchozích verzích řady StorSimple 8000 bloků dat byly vrstveny do cloudu na základě použití. Došlo zaručit, že by bloky zůstal na místní. V části aktualizace 2 při vytváření objem, můžete určit svazku jako místně připnuté a primární data z svazku nebude vrstveny do cloudu. Snímky místně připnuté svazky budou pořád zkopírovány do cloudu pro zálohování tak, aby cloudu se dá použít pro účely obnovení nastavení mobilních zařízení a havárie data. Kromě toho můžete změnit typ svazku (to znamená převést vrstveny svazky na místně připnuté svazky svazky převést místně připnuté ložisek a). 

- **Vylepšení virtuální zařízení StorSimple** – dříve řady StorSimple 8000 umístěný virtuální zařízení jako havárie využití nebo vývoj nebo zkoušení řešení. Došlo pouze jeden model virtuální zařízení (model 1100). Aktualizace 2 zavádí dva modely virtuální zařízení: 

     - 8010 (dříve označovaného jako 1100) – bez změny; má kapacitu 30 TB a používá Azure standardní úložiště.
     - 8020 – má kapacitu 64 TB a používá Azure Premium úložiště pro zvýšení výkonu.

    Je jediný virtuálního pevného disku pro oba virtuální zařízení modely (8010/8020). Při prvním spuštění virtuální zařízení, rozpozná parametry platformy a platí verze správné modelu.

- **Vylepšení sítě** – aktualizace 2 obsahuje tato sociální sítě vylepšení:

     - Více nic může být užitečné pro cloudu, aby převzetí může dojít, pokud NIC se nezdaří.
     - Směrování vylepšení s pevnou metriky pro cloudu povolený bloky.
     - Online opakovat nezdařeném uložení zdrojů před přepojení.
     - Nové upozornění pro chyby služby.

- **Aktualizace vylepšení** – aktualizace 1.2 a dřívějším řadu StorSimple 8000 aktualizovaném pomocí dvou kanálech: pomocí služby Windows Update clusterů, iSCSI a tak dál a Microsoft Update pro binární soubory a firmware.
    Aktualizace 2 používá Microsoft Update pro všechny aktualizace balíčků. To měli vést k méně času opravy nebo dělá převzetí služeb při selhání. 

- **Aktualizace firmwaru** – tyto aktualizace firmwaru jsou však započítávány:
    - LSI: verze produktu 2.00.72.10 lsi_sas2.sys
    - Pouze SSD (bez aktualizace pevný disk): XMGG XGEG, KZ50, F6C2 a VR08

- **Aktivní podpory** – aktualizace 2 umožňuje společnosti Microsoft pro získání dalších diagnostické informace ze zařízení. Když náš tým operace identifikuje zařízení, která jsou problémy, jsme mohou lépe shromáždění informací ze zařízení a Diagnostika problémů. **Po přijetí aktualizace 2 povolíte, abychom vám podporují aktivní**.    
 

## <a name="issues-fixed-in-update-2"></a>Problémy s pevnou v aktualizaci 2

Následující tabulka obsahuje souhrn problémy, které byly pevně v aktualizace 2.    

| Ne. | Funkce | Problém | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
|-----|---------|-------|--------------------------------|--------------------------------|
| 1 | Rozhraní sítě | Po upgradu na aktualizace 1 vykázaného službu StorSimple správce, aby se nezdařila porty Data2 a Data3 u jednoho řadiče. Tento problém byl odstraněn. | Ano | Ne |
| 2 | Aktualizace | Po upgradu na aktualizace 1 zvuková upozornění upozornění v došlo k portálu Azure klasické na několika zařízeních. Tento problém byl odstraněn. | Ano | Ne |
| 3 | Openstack ověřování | Pokud chcete použít Openstack jako poskytovatele služby cloudu, může dojít k chybě, že vaše cloudové ověřování řetězec je příliš dlouhý. To byl odstraněn. | Ano | Ne |


## <a name="known-issues-in-update-2"></a>Známé problémy v aktualizace 2

Následující tabulka obsahuje souhrn známé problémy v této verzi.

| Ne. | Funkce | Problém | Komentáře a řešení | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Disk kvora | Výjimečně Pokud většinou disků v EBOD přílohu 8600 zařízení jsou to od ní odpojilo výsledkem bez disku kvora fondu úložiště bude přejděte offline. Zůstane v režimu offline, i když disků připojeni. | Je třeba Restartujte zařízení. Pokud potíže potrvají, obraťte se prosím Microsoft Support k dalším krokům. | Ano | Ne |
| 2 | ID nesprávné zařízení | Při provádění náhradní řadiče domény řadiče domény 0 může zobrazit jako řadiči 1. Během řadiče náhradní při načtení obrázek z uzel mezi dvěma účastníky ID řadiče zobrazit původně jako ID zařízení mezi dvěma účastníky. Výjimečně toto chování mohou také projeví po restartování systému. | Není nutná žádná akce uživatele. Tato situace vyřeší samotné po dokončení náhradní řadiče domény. | Ano | Ne |
| 3 | Účty úložiště | Odstranění účtu úložiště pomocí služba úložiště je nepodporované funkce. To vede k situaci, ve kterém nelze uživatelská data načíst.|  | Ano | Ano |
| 4 | Převzetí zařízení | Více předávání služeb při selhání kontejneru hlasitosti na stejné zařízení zdroje pro různé cílové zařízení není podporovaná. Přepnutí z jednoho nefunkční zařízení několika zařízeních pokusy hlasitost kontejnerů na první selhání zařízení dojít ke ztrátě dat vlastnictví. Po selhání tyto hlasitost kontejnery zobrazit nebo s odlišným chováním při zobrazení v portálu Azure klasické. | | Ano | Ne |
| 5 | Instalace | Během StorSimple adaptér pro instalaci služby SharePoint budete muset zadat IP zařízení v pořadí k úspěšnému dokončení instalace.    | | Ano | Ne |
| 6 | Proxy serveru webové | Pokud konfigurace proxy serveru webové HTTPS jako zadaný protokol, bude to mít vliv komunikace služby zařízení a zařízení budou přesměrovány offline. Podpora balíčků vygeneruje také v procesu jinými významné zdroje na vašem zařízení. | Zajistěte adresu URL proxy HTTP jako zadaný protokol. Další informace přejděte na [proxy serveru webové konfigurovat pro svoje zařízení](storsimple-configure-web-proxy.md). | Ano | Ne |
| 7 | Proxy serveru webové | Pokud konfigurace a povolíte proxy serveru webové na registrovaných zařízení, je potřeba restartovat aktivní řadiče na vašem zařízení. | | Ano | Ne |
| 8 | Vysoký cloudové latence a vysoký pracovní zátěž vstupu a výstupu | Pokud vaše zařízení StorSimple zjistí kombinací čekacích velmi vysoké cloudu dob (pořadí sekund) a maximum pracovní zátěž vstupu a výstupu, svazky zařízení přejděte do jejich funkčnost bude omezena stavu a vstupně-výstupních se nemusí podařit, zobrazí se chyba "zařízení není připraveno". | Je třeba ručně restartujte zařízení řadiče a provedení přepojení zařízení k obnovení ze tato situace. | Ano | Ne |
| 9 | Azure Powershellu | Kdy použít rutinu StorSimple **Get-AzureStorSimpleStorageAccountCredential & #124; Vyberte objekt – nejdřív 1 - čekání** vyberte první objekt, takže můžete vytvořit nový objekt **VolumeContainer** , rutinu vrací všechny objekty. | Následujícím způsobem zalomení rutinu v závorkách: **(Get-Azure StorSimpleStorageAccountCredential) & #124; Vyberte objekt – nejdřív 1 - čekání** | Ano | Ano |
| 10| Migrace | Při předávání více kontejnery hlasitost migraci, ÉTA poslední zálohy je pouze pro prvním kontejneru hlasitost přesné. Paralelní migrace se spustí navíc po migraci první 4 zálohy v prvním kontejneru hlasitost. | Doporučujeme migrovat jeden kontejner hlasitost postupně. | Ano | Ne |
| 11| Migrace | Po obnovení svazky nepřičítají zásady zálohování nebo skupinu virtuální disk. | Musíte se přidá tyto svazky záložní zásady k vytvoření zálohy. | Ano | Ano |
| 12| Migrace | Po dokončení migrace zařízení 5000/7000 řady nesmí přístup kontejnery migrovaná data. | Doporučujeme po dokončení a potvrzené migrace odstranit kontejnery migrovaná data. | Ano | Ne |
| 13| Klonovat a DR | Zařízení StorSimple systém 1 aktualizace nelze klonovat nebo obnovení havárie zařízení 1 softwarem před aktualizace. | Budete muset aktualizovat cílové zařízení aktualizovat 1 umožňuje tyto operace | Ano | Ano |
| 14 | Migrace | Konfigurace zálohování migraci se nemusí podařit na zařízení řady 5000 7000 po hlasitost skupiny s žádný související svazky. | Odstranit všechny skupiny prázdné hlasitost s žádný související svazky a zopakujte zálohování konfigurace.| Ano | Ne |
| 15 | Azure rutiny prostředí PowerShell a místně připnuté svazky | Nelze vytvořit místně připnuté hlasitost pomocí rutin prostředí PowerShell Azure. (Svazky, který vytvoříte pomocí prostředí PowerShell Azure bude vrstveny.) |Vždy použijte službu StorSimple Správce konfigurace místně připnuté svazky.| Ano | Ne |
| 16 |Místa pro místně připnuté svazky | Pokud byste odstranili místně připnuté objem, nemusí být místa pro nové svazky aktualizované okamžitě. Služba StorSimple správce aktualizuje místa na disku místní přibližně každou hodinu.| Čekat na hodinu před pokusem o vytvoření nové hlasitost. | Ano | Ne |
| 17 | Místně připnuté svazky | Obnovení práce zpřístupňuje zálohy dočasné snímku v katalogu záložní, ale jenom po celou dobu úlohy obnovit. Dále poskytuje skupinu virtuální disk s předponu **tmpCollection** na stránce **Zálohování zásady** , ale jenom po celou dobu úlohy obnovit. | Toto chování dochází, jestliže obnovení práce obsahuje pouze místně připnuté svazky nebo kombinaci místně připnuté a vrstvené svazky. Pokud úlohy obnovení obsahuje pouze vrstvené svazky, toto chování nedojde. Požaduje bez zásahu uživatele. | Ano | Ne |
| 18 | Místně připnuté svazky | Pokud předplatné zrušíte obnovení úlohy a selhání řadiče probíhá ihned poté úlohy obnovení zobrazí **Vadný** místo **zrušeno**. Pokud se nezdaří úlohy obnovení a dojde k selhání řadiče ihned poté úlohy obnovení řádku se zobrazí **zrušeno** místo **se nezdařila**. | Toto chování dochází, jestliže obnovení práce obsahuje pouze místně připnuté svazky nebo kombinaci místně připnuté a vrstvené svazky. Pokud úlohy obnovení obsahuje pouze vrstvené svazky, toto chování nedojde. Požaduje bez zásahu uživatele. | Ano | Ne |
| 19 |Místně připnuté svazky | Pokud předplatné zrušíte obnovení projektu nebo obnovení nezdaří, a potom dojde k selhání řadiče domény, úlohy další obnovení se zobrazí na stránce **úlohy** . | Toto chování dochází, jestliže obnovení práce obsahuje pouze místně připnuté svazky nebo kombinaci místně připnuté a vrstvené svazky. Pokud úlohy obnovení obsahuje pouze vrstvené svazky, toto chování nedojde. Požaduje bez zásahu uživatele. | Ano | Ne |
| 20 |Místně připnuté svazky | Pokud se pokusíte převést vrstvené objem (vytvořené a klonovaný s aktualizace 1.2 nebo starší) místně připnuté hlasitost a vaše zařízení není dost místa nebo je výpadku cloudu, můžete clone(s) poškodil.| Tento problém pouze se svazky, které byly vytvořené a klonovaný s před 2 aktualizace. Je vhodné časté scénář.|
| 21 | Převod hlasitosti | Neaktualizují ACRs připojené k svazku probíhá převodu svazku (vrstveny k místně připnuté nebo naopak). Aktualizace ACRs může vést k poškození dat. | V případě potřeby aktualizujte ACRs před převodem hlasitost a provést další aktualizace ACR Probíhá převod. |

## <a name="controller-and-firmware-updates-in-update-2"></a>Aktualizace řadiče domény a firmware v aktualizaci 2

Tato verze aktualizuje ovladač a firmware disk na vašem zařízení.
 
- Další informace o LSI firmware aktualizace, najdete v článku znalostní báze Microsoft Knowledge base 3121900. 
- Další informace o firmware disku aktualizace, najdete v článku znalostní báze Microsoft Knowledge base 3121899.
 
## <a name="virtual-device-updates-in-update-2"></a>Aktualizace virtuální zařízení v aktualizaci 2

Tuto aktualizaci nelze použít pro virtuální zařízení. Nová virtuální zařízení muset vytvořit. 

## <a name="next-step"></a>Další krok

Zjistěte, jak [instalovat aktualizace 2](storsimple-install-update-2.md) na zařízení s StorSimple.
