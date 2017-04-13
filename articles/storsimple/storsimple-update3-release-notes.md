<properties 
   pageTitle="Poznámky k verzi StorSimple 8000 řady aktualizace 3 | Microsoft Azure"
   description="Popisuje nové funkce, problémy a řešení pro StorSimple 8000 řady aktualizace 3."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/14/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-3-release-notes"></a>Poznámky k verzi StorSimple 8000 řady aktualizace 3  

## <a name="overview"></a>Základní informace

Následující poznámky k verzi popisy s novými funkcemi a určit kritické otevřené problémy pro StorSimple 8000 řady aktualizace 3. Obsahují také seznam aktualizace softwaru StorSimple tato verze zahrnuje. 

Aktualizace 3 lze použít na libovolném zařízení StorSimple projdete aktualizace 2.2 vydání (GA) nebo aktualizace 0,1. Verze zařízení přidružený k aktualizaci 3 je 6.3.9600.17759.

Zkontrolujte informace obsažené v poznámky k verzi před nasazením aktualizace ve vašem StorSimple řešení.

>[AZURE.IMPORTANT]
> 
> - Aktualizace 3 má software zařízení, LSI ovladač a firmware a Storport a Spaceport aktualizuje. Trvá asi 1,5-2 hodin nainstalovat tuto aktualizaci. 

> - Pro nové verze, nemusí zobrazit aktualizace okamžitě, protože jsme proveďte fázích aktualizace. Počkejte několik dnů a potom vyhledejte aktualizace znova se budou k dispozici brzy bude k dispozici.


## <a name="whats-new-in-update-3"></a>Co je nového v aktualizace 3

Tato vylepšení klíče a opravy chyb byly vloženy aktualizace 3.

 
- **Automatické mezery obnovit změny** – spuštění aktualizace 3, algoritmů obnovit místo spustit v úsporném správce systému výsledkem rychlejší. Další informace o ty, které jsou potřebné pro práci s obnovit místo v příručce [StorSimple síťové požadavky](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

- **Zvýšení výkonu** – aktualizace 3 vylepšili pro čtení i zápis výkonu do cloudu.

- **Migrace související vylepšení** – v této verzi, několik opravy chyb a vylepšení byly Hotovo pro funkci migrace z 5000/7000 řady zařízení a 8000 řady zařízení. Další informace o tom, jak používat funkci migrace, přejděte na [migraci z řady zařízení 5000/7000 8000 řady zařízení](https://www.microsoft.com/en-us/download/details.aspx?id=47322). 

- **Sledování související opravy** – v této verzi chyby týkající se sledování grafy, řídicí panel služeb a řídicích zařízení byly odstraněny.


## <a name="issues-fixed-in-update-3"></a>Chyby v aktualizace 3

Následující tabulka obsahuje souhrn problémy, které byly pevně v aktualizace 3.    

| Ne | Funkce                                    | Problém                                                                                                                                                                                                                                                                                        | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Migrace dat na straně Host (hostitel)                      | Ve starší verzi zařízení cloudu StorSimple byl přechodu do offline režimu v průběhu migrace dat na straně hostitele. Tento problém bude pevně v této verzi.                                                | Ne                        | Ano                        |
| 2  | Místně připnuté svazky                     | V předchozí verzi došlo k problémům souvisejícím s vstupu a výstupu selhání selhání převodu hlasitost a datapath selhání místně připnuté svazky. Tyto problémy byly způsobil kořenové a pevně v této verzi.                | Ano                        | Ne                       |
| 3  | Sledování                                    | Došlo k více otázky týkající se vytváření sestav jednotky a sledování a místo, kam bylo zobrazí nesprávné informace o místně připnuté svazky grafy řídicího panelu zařízení. Tyto opravy v této verzi. | Ano                         | Ne                       |
| 4  | Hodně zápisy vstupu a výstupu                          | Při použití StorSimple pro pracovního vytížení týkající se nestahují zápisy, uživatel by nastat časté chyba, kdy byla právě pracovní sada vrstveny do cloudu. Tato chyba je pevné v této verzi. | Ano                        | Ano                       |
| 5  | Zálohování                                     | Výjimečně určitých v předchozích verzích aplikace softwaru kdy uživatel záložní kopii vzdálené klonovat, by nastat chyby cloudu a operace by chyby. V této verzi podařilo odstranit problém a úspěšném dokončení operace.             | Ano                        | Ano                       |
| 6  | Zásady zálohování                                     | Výjimečně určitých v dřívějších verzích software, jste udělali chybu týkající se odstranění zásady zálohování. Tento problém bude pevně v této verzi.       | Ano                        | Ano                       |



## <a name="known-issues-in-update-3"></a>Známé problémy v aktualizace 3

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
| 20 |Místně připnuté svazky | Pokud se pokusíte převést vrstvené objem (vytvořené a klonovaný s aktualizace 1.2 nebo starší) místně připnuté hlasitost a vaše zařízení není dost místa nebo je výpadku cloudu, můžete clone(s) poškodil.| Tento problém pouze se svazky, které byly vytvořené a klonovaný s před 2.1 aktualizace. Je vhodné časté scénář.|
| 21 | Převod hlasitosti | Neaktualizují ACRs připojené k svazku probíhá převodu svazku (vrstveny k místně připnuté nebo naopak). Aktualizace ACRs může vést k poškození dat. | V případě potřeby aktualizujte ACRs před převodem hlasitost a provést další aktualizace ACR Probíhá převod. |
| 22 | Aktualizace | Při použití aktualizace 3, stránky **údržby** Azure klasické portálu zobrazí následující zpráva související s aktualizace 2: "řady 8000 StorSimple aktualizace 2 umožňuje společnosti Microsoft včasným shromažďování informací protokolu z vašeho zařízení případě jsme vyhledat potenciální problémy". To je zavádějící jak značí, že je zařízení aktualizovány aktualizace 2. Po zařízení succeesfully aktualizuje tak, aby aktualizace 3 se tato zpráva zmizí. | Toto chování se opraví v budoucí verzi. | Ano | Ne |


## <a name="controller-and-firmware-updates-in-update-3"></a>Aktualizace řadiče domény a firmware v aktualizace 3

Tato verze nemá LSI firmwaru a aktualizace. Další informace o tom, jak nainstalovat LSI ovladač a aktualizaci firmwaru najdete v článku [instalace aktualizací 3](storsimple-install-update-3.md) na zařízení s StorSimple.

 
## <a name="virtual-device-updates-in-update-3"></a>Aktualizace virtuální zařízení v aktualizace 3

Tuto aktualizaci nelze použít pro zařízení cloudu StorSimple (označovaná taky jako virtuální zařízení). Nová virtuální zařízení muset vytvořit. 


## <a name="next-step"></a>Další krok

Zjistěte, jak [instalovat aktualizace 3](storsimple-install-update-3.md) na zařízení s StorSimple.
