<properties 
   pageTitle="Poznámky k verzi StorSimple 8000 řady aktualizace 1.2 | Microsoft Azure"
   description="Popisuje nové funkce, problémy a řešení pro StorSimple 8000 řady aktualizace 1.2."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-12-release-notes"></a>Poznámky k verzi StorSimple 8000 řady aktualizace 1.2  

## <a name="overview"></a>Základní informace

Následující poznámky k verzi popisy s novými funkcemi a určit kritické otevřené problémy pro StorSimple 8000 řady aktualizace 1.2. Obsahují také seznam software StorSimple, ovladač a tato verze zahrnuje aktualizaci firmwaru disku. 

Aktualizace 1.2 lze použít na libovolném zařízení StorSimple systém vydání (GA), aktualizace 0,1, aktualizace 0,2 nebo aktualizace 0,3. Aktualizace 1.2 není k dispozici, pokud vaše zařízení se systémem aktualizace 1 nebo aktualizace 1.1. Pokud vaše zařízení běží vydání (GA), ujistěte se, aby vám pomohou s instalací tuto aktualizaci [Kontaktujte podporu od Microsoftu](storsimple-contact-microsoft-support.md) .

Následující tabulka obsahuje odpovídající aktualizace 1, 1.1 a 1.2 spolehlivější zařízení.

| Pokud aktualizace... | Toto je verzi softwaru zařízení. |
|---------------------|---------------------------------------|
| Aktualizace 1.2          | 6.3.9600.17584                        |
| Aktualizace 1.1          | 6.3.9600.17521                        |
| Aktualizace 1.0          | 6.3.9600.17491                        |

Zkontrolujte informace obsažené v poznámky k verzi před nasazením aktualizace ve vašem StorSimple řešení. Další informace najdete v tématu Jak [nainstalovat aktualizace 1.2 na zařízení s StorSimple](storsimple-install-update-1.md). 

>[AZURE.IMPORTANT]
> 
- Trvá přibližně 5 až 10 hodin nainstalovat tuto aktualizaci (včetně aktualizací systému Windows). 
- Aktualizace 1.2 má software, LSI ovladač a aktualizace firmwaru disku. Pokud chcete nainstalovat, postupujte podle pokynů [nainstalujte aktualizaci 1.2 na zařízení s StorSimple](storsimple-install-update-1.md).
- Pro nové verze, nemusí zobrazit aktualizace okamžitě, protože jsme proveďte fázích aktualizace. Vyhledat aktualizace během několika dnů znovu se budou k dispozici brzy bude k dispozici.


## <a name="whats-new-in-update-12"></a>Co je nového v aktualizace 1.2

Tyto funkce vydané nejdřív číslem 1 aktualizace, které byl k dispozici omezené množině uživatelů. Verze 1.2 aktualizace většina uživatelů StorSimple uvidí následující nové funkce a vylepšení:

- **Migrace z řady 5000 7000 8000 zařízení řady** – tato verze uvádí nové funkce migrace, která uživatelům, kteří StorSimple 5000 7000 řady zařízení jejich data migrovat do StorSimple 8000 fyzické zařízení řady nebo virtuální zařízení. Funkce migrace obsahuje dva tvrzení hodnoty klíče:                                                                  

    - **Nepřerušený provoz**, povolením přenesení existujících dat na zařízení řady 5000 7000 na zařízení 8000 řady.
    - **Vylepšené funkce nabízené 8000 řady zařízení**, třeba efektivně Centrální správa více spotřebiče prostřednictvím StorSimple Správce služby lepší třídy hardware a aktualizaci firmwaru, virtuální spotřebiče, mobilitu dat a funkcí v budoucích Přehled.

    Podívejte se do [Průvodce migrací na](http://www.microsoft.com/download/details.aspx?id=47322) podrobné informace o tom, jak migrovat řady 5000 7000 StorSimple do zařízení s 8000 řady. 

- **Dostupnost na portálu pro státní správu Azure** – StorSimple je teď dostupná v portálu Azure Government. V tématu Jak [nasadit StorSimple zařízení na portálu pro státní správu Azure](storsimple-deployment-walkthrough-gov.md).

- **Podpora pro jiné poskytovatele služby cloudu** – jiných poskytovatelů služby cloudu podporované jsou Amazon S3 Amazon S3 se záznamy, HP a OpenStack (verze beta).

- **Aktualizace nejnovější rozhraní API úložiště** – s touto verzí StorSimple byl aktualizovaný nejnovější služby Azure úložiště rozhraní API. StorSimple 8000 řady zařízení s verzemi před aktualizace 1 software (vydání 0,1, 0,2 a 0,3) používají verze Azure úložiště služby rozhraní API starší než 17 červenec 2009. Jak je uvedeno v aktualizované [oznámení o odebrání úložiště služby verze](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx)1 srpen 2016, bude tato rozhraní API změněny. Je nezbytné použít StorSimple 8000 řady aktualizace 1 před 1 srpen 2016. Pokud neuděláte tak, StorSimple zařízení přestane fungovat správně.

- **Podpora pro zónu nadbytečné úložiště (ZRS)** – upgradu na nejnovější verzi rozhraní API úložiště řady StorSimple 8000 bude podporovat zóny nadbytečné úložiště (ZRS) kromě místně nadbytečné úložiště (LRS) a Geo nadbytečné úložiště (GRS). Podívejte se na tento [článek o úložišti Azure redundance možnosti](../storage/storage-redundancy.md) podrobnosti ZRS.

- **Vylepšené počáteční nasazení a aktualizace prostředí** – v této verzi, instalace a procesy aktualizací byla rozšířenou. Instalace Průvodce nastavením lepší k poskytnutí zpětné vazby uživateli, pokud jsou nesprávná konfigurace sítě a nastavení brány firewall. Další diagnostické rutiny jste obdrželi vám pomůže při řešení potíží s sítě zařízení. V tématu [Poradce při potížích s nasazení článku](storsimple-troubleshoot-deployment.md) Další informace o nové diagnostiky rutiny pro řešení potíží.

## <a name="issues-fixed-in-update-12"></a>Problémy s pevnou aktualizace 1.2

Následující tabulka obsahuje souhrn problémy, které byly pevně v aktualizace 1.2 1.1 a 1.    


| Ne. | Funkce | Problém | Pevné v aktualizace | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
|-----|---------|-------|-----------------|---------------------------------|--------------------------------|
| 1 | Prostředí Windows PowerShell pro StorSimple | Když uživatel vzdáleně používána v zařízení StorSimple pomocí Windows Powershellu pro StorSimple, potom spustit Průvodce nastavením zhroucení došlo k chybě co nejdříve, Data 0 IP bylo zadané. Tato chyba je teď pevné Update 1. | Aktualizace 1 | Ano | Ano |
| 2 | Obnovení Factory | V některých případech při provádění obnovení factory StorSimple zařízení se zarazí a zobrazí tato zpráva: **Obnovit factory probíhá (fáze 8)**. Se to stalo po stisknutí kombinace kláves CTRL + C době, kdy rutinu probíhá. Tato chyba je teď pevné.| Aktualizace 1 | Ano | Ne |
| 3 | Obnovení Factory | Po obnovení řadiče selhalo duální factory byly moct pokračovat registrace zařízení. Výsledkem nepodporované konfiguraci systému. Update 1 se zobrazí chybová zpráva a registraci je blokován na zařízení, aby byl selhání továrny obnovit. | Aktualizace 1 | Ano | Ne |
| 4 | Obnovení Factory | V některých případech byly mocninu falešně pozitivní neshodu upozornění. Nesprávné neshodu upozornění vygeneruje už na zařízení s aktualizace 1. | Aktualizace 1 | Ano | Ne |
| 5 | Obnovení Factory | Pokud byl před dokončením factory resetovat, zařízení zadali režimu obnovení a neumožňuje můžete přistupovat k prostředí Windows PowerShell pro StorSimple. Tato chyba je teď pevné. | Aktualizace 1 | Ano | Ne |
| 6 | Obnovení havárie | Chybu havárie obnovení (DR) byla pevnou, ve kterém by DR selhání při zjišťování zálohování na cílové zařízení. | Aktualizace 1 | Ano | Ano |
| 7 | Sledování LED | V určitých případech sledování LED zadní straně zařízení není indikaci stavu správné. Modrá LED je vypnutý. DATA 0 a 1 LED dat byly blýskavé, i když tyto rozhraní nebyla nastavena. Byly podařilo odstranit problém a sledování LED nyní označují správný stav.  | Aktualizace 1 | Ano | Ne |
| 8 | Sledování LED | V určitých případech po použití aktualizací 1 modrý indikátor aktivní řadiči vypnou a zůstanou vypnuté což těžko určit aktivní správce. Tento problém má pevně v této verzi opravy.| Aktualizace 1.2 | Ano | Ne |
| 9 | Rozhraní sítě | V předchozích verzích může odejít StorSimple zařízení nakonfigurována směrovat brány offline. V této verzi metriky pro Data 0 byl vytvořený nejnižší; Proto i v případě jiných síťová rozhraní povolena cloudu, všechny přenosy cloudu ze zařízení směrovaná přes Data 0. | Aktualizace 1 | Ano | Ano | 
| 10 | Zálohování | Oprava verze 1.1 aktualizace byl odstraněn chybu Update 1, která způsobila zálohy selhání po 24 dnech. | Aktualizace 1.1 | Ano | Ano |
| 11 | Zálohování | Chyba v předchozích verzích výsledkem nízký výkon k snímky cloudu s sazby nízký změnit. Tato chyba byl odstraněn v této verzi opravy.| Aktualizace 1.2 | Ano | Ano |
| 12 | Aktualizace | V této verzi opravy byl odstraněn chybu Update 1, vykázaného neúspěšného upgradu, která způsobila řadiče přejdete do režimu obnovení.| Aktualizace 1.2 | Ano | Ano |


## <a name="known-issues-in-update-12"></a>Známé problémy v aktualizace 1.2

Následující tabulka obsahuje souhrn známé problémy v této verzi.

| Ne. | Funkce | Problém | Komentáře a řešení | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Disk kvora | Výjimečně Pokud většinou disků v EBOD přílohu 8600 zařízení jsou to od ní odpojilo výsledkem bez disku kvora fondu úložiště bude offline. Zůstane v režimu offline, i když disků připojeni. | Je třeba Restartujte zařízení. Pokud potíže potrvají, obraťte se prosím Microsoft Support k dalším krokům. | Ano | Ne |
| 2 | ID nesprávné zařízení | Při provádění náhradní řadiče domény řadiče domény 0 může zobrazit jako řadiči 1. Během řadiče náhradní při načtení obrázek z uzel mezi dvěma účastníky ID řadiče zobrazit původně jako ID zařízení mezi dvěma účastníky. Výjimečně toto chování mohou také projeví po restartování systému. | Není nutná žádná akce uživatele. Tato situace vyřeší samotné po dokončení náhradní řadiče domény. | Ano | Ne |
| 3 | Účty úložiště | Odstranění účtu úložiště pomocí služba úložiště je nepodporované funkce. To vede k situaci, ve kterém nelze uživatelská data načíst. | Ano | Ano |
| 4 | Převzetí zařízení | Více předávání služeb při selhání kontejneru hlasitosti na stejné zařízení zdroje pro různé cílové zařízení není podporovaná. Přepnutí zařízení z jednoho nefunkční zařízení několika zařízeních pokusy hlasitost kontejnerů na první selhání zařízení dojít ke ztrátě dat vlastnictví. Po selhání tyto hlasitost kontejnery zobrazit nebo s odlišným chováním při zobrazení v portálu Azure klasické. | | Ano | Ne |
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

## <a name="physical-device-updates-in-update-12"></a>Aktualizace fyzické zařízení aktualizace 1.2

Pokud oprava aktualizovat 1.2 se použije pro fyzické zařízení (verzemi před Update 1), verzi softwaru se změní na 6.3.9600.17584.

## <a name="controller-and-firmware-updates-in-update-12"></a>Aktualizace řadiče domény a firmware aktualizace 1.2

Tato verze aktualizuje ovladač a firmware disk na vašem zařízení.
 
- Další informace o aktualizaci přidružení zabezpečení řadiče domény najdete v článku [aktualizace 1 pro řadiče přidružení LSI zabezpečení v aplikaci Microsoft Azure StorSimple zařízení](https://support.microsoft.com/kb/3043005). 

- Další informace o firmware disku aktualizovat najdete v tématu [disku firmware aktualizace 1 pro Microsoft Azure StorSimple zařízení](https://support.microsoft.com/kb/3063416).
 
## <a name="virtual-device-updates-in-update-12"></a>Aktualizace virtuální zařízení aktualizace 1.2

Tuto aktualizaci nelze použít pro virtuální zařízení. Nová virtuální zařízení muset vytvořit. 

## <a name="next-steps"></a>Další kroky

- [Nainstalujte aktualizaci 1.2 na vašem zařízení](storsimple-install-update-1.md).
 
