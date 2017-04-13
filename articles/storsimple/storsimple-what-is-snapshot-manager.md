<properties 
   pageTitle="Co je StorSimple snímek správce? | Microsoft Azure"
   description="Popisuje správce snímek StorSimple, jeho architektura a jeho funkcí."
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

# <a name="what-is-storsimple-snapshot-manager"></a>Co je StorSimple snímek správce?

## <a name="overview"></a>Základní informace

Správce snímek StorSimple je snap-in konzoly MMC (Microsoft Management), který zjednodušuje ochrana dat a správu zálohování v prostředí Microsoft Azure StorSimple. Pomocí Správce StorSimple snímku můžete spravovat Microsoft Azure StorSimple dat v datovém centru i schránkami v cloudu jako jeden integrované úložiště řešení, tedy zjednodušení procesů zálohování a snížit náklady.

Přehled představuje správce snímek StorSimple, popisuje jeho funkcí a vysvětluje jeho roli v aplikaci Microsoft Azure StorSimple. 

Přehled o celém systému Microsoft Azure StorSimple, včetně StorSimple zařízení, přihlašovacích údajů StorSimple správce, správce snímek StorSimple a StorSimple adaptér pro SharePoint, najdete v článku [řady StorSimple 8000: řešení hybridní cloudové úložiště](storsimple-overview.md). 
 
>[AZURE.NOTE] 
>
>- Správce snímek StorSimple nelze použít ke správě Microsoft Azure StorSimple virtuální pole (označovaná taky jako StorSimple místní virtuální zařízení).
>
>- Pokud chcete nainstalovat StorSimple aktualizace 2 na zařízení s StorSimple, nezapomeňte si stáhnout nejnovější verzi aplikace StorSimple snímek Manager a nainstalujte ji **před instalací StorSimple aktualizace 2**. Nejnovější verzi StorSimple snímek Manager je zpětně kompatibilní a spolupracuje všech verzích Microsoft Azure StorSimple. Pokud používáte dřívější verzi StorSimple snímek Manager, musíte aktualizovat (nepotřebujete před nainstalovat novou verzi odinstalovat v předchozí verzi).

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>Správce snímek StorSimple účel a architektura

Správce snímek StorSimple poskytuje Konzola Centrální správy, která slouží k vytvoření konzistentního, v okamžiku záložní kopie místní a cloudu data. Můžete třeba použít konzole:

- Konfigurace, obecnějším údajům a svazky odstranit.
- Konfigurace skupin hlasitost zajistit zálohovat data se konzistenci aplikací.
- Správa zásad záložní tak, aby zálohování předem plánu.
- Vytvoření místních i cloudových snímky, které můžete uložený v cloudu a použít k obnovení havárie.

Správce snímek StorSimple načte seznamu aplikací registrovaný u poskytovatele VSS na hostiteli. Vytvoření zálohy konzistenci aplikací, poté zkontroluje svazky používané aplikace a navrhne hlasitost skupin pro nastavení. Správce snímek StorSimple používá tyto skupiny hlasitost pro vytvoření záložní kopie, které jsou konzistenci aplikací. (Aplikace konzistence dochází při všech souvisejících souborů a databáze se synchronizují a představují true stavu aplikace v určitém bodě v čase.) 

Zálohování StorSimple snímek správce podobu přírůstková snímky, které zachytit pouze změny od posledního zálohování. V důsledku toho zálohy vyžadují méně úložiště a lze vytvořit a rychle obnovit. Správce snímek StorSimple používá Windows hlasitost stín kopie služby (VSS) zajistit, že zachytit snímky aplikace konzistentních dat. (Další informace, přejděte na integraci s částí služby Windows hlasitost stín kopie.) Pomocí Správce StorSimple snímku můžete vytvořit záložní plány nebo v případě potřeby převzít okamžité zálohy. V případě potřeby obnovit data ze záložní, umožňuje StorSimple snímek správce vyberete z katalogu místní nebo cloudu snímky. Azure StorSimple obnoví jen tu část dat, které je potřeba, jako je to potřeba, který brání zpoždění v dostupnosti dat během operace obnovení).

![Architektura StorSimple snímek správce](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**Architektura StorSimple snímek správce** 

## <a name="support-for-multiple-volume-types"></a>Podpora více typů hlasitosti

Správce snímek StorSimple slouží ke konfiguraci a obecnějším údajům těmihle svazky: 

- **Základní svazky** – základní hlasitost je jeden oddíl na základní disk. 

- **Jednoduché svazky** – jednoduché hlasitost je dynamické obsahující místa na disku z jednoho dynamický. Jednoduchý hlasitost se skládá z jedné oblasti na disku nebo více oblastí, které jsou propojeny na stejném disku. (Pouze na dynamických discích můžete vytvořit jednoduché svazky.) Jednoduché svazky nejsou odolnost proti chybám.

- **Dynamické svazky** – dynamické hlasitost je vytvořil dynamický. Dynamické disků pomocí databáze můžete sledovat informace o svazky obsažené na dynamických discích v počítači. 

- **Dynamické svazky s odrážející strukturu** – dynamické svazky s odrážející strukturu jsou založeny na architektura RAID 1. Číslem RAID 1 stejná data zapisuje na dvě nebo více disku, vytváření zrcadlený sadu. Žádost o čtení můžete pak uskutečněných jednotlivými disku, která obsahuje požadovaná data.

- **Sdílené clusteru svazky** – s sdílených clusteru svazky (CSVs) více uzlů v překlopení obrázku můžete současně načtení nebo zápisu dat na stejný disk. Přepnutí z jednoho uzlu jiný uzel může dojít rychle, aniž by bylo změny ve jednotka vlastnictví nebo místní připojení, odpojení a odebrání svazku. 

>[AZURE.IMPORTANT] Není kombinovat CSVs a jiných CSVs ve stejném snímku. Kombinování CSVs a jiných CSVs snímek není podporovaná. 
 
Správce snímek StorSimple slouží k obnovení celého svazku skupiny nebo klonovat jednotlivé svazky a obnovit jednotlivé soubory.

- [Svazky a hlasitost skupiny](#volumes-and-volume-groups) 
- [Typy zálohování a zálohování zásady](#backup-types-and-backup-policies) 

Další informace o funkcích StorSimple snímek správce a jak je používat najdete v článku [Správce snímek StorSimple uživatelského rozhraní](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Svazky a hlasitost skupiny

Pomocí Správce snímek StorSimple vytvoření svazky a konfiguraci je do skupin hlasitost. 

Správce snímek StorSimple používá k vytvoření záložní kopie, které jsou konzistenci aplikací hlasitost skupiny. Aplikace konzistence dochází při všech souvisejících souborů a databáze se synchronizují a představují skutečném stavu aplikace v určitém bodě v čase. Hlasitost skupin, (které jsou označovaná taky jako *konzistence skupiny*) základ zálohy nebo obnovení projektu.

Hlasitost skupiny nejsou stejné jako kontejnery hlasitost. Kontejner hlasitost obsahuje jeden nebo více svazky majících účet cloudové úložiště a další atributy, jako jsou například spotřebu šifrování a šířky pásma. Kontejner jednoho hlasitost může obsahovat až 256 tence zřizování svazky StorSimple. Další informace o multilicenčních kontejnery přejděte na [Správa kontejnery hlasitost](storsimple-manage-volume-containers.md). Hlasitost skupiny jsou kolekcí svazky, které můžete konfigurovat pro usnadnění zálohování. Pokud vyberete dva svazky, které patří do různých hlasitost kontejnery, že je umístíte ve skupině jednoho hlasitost a vytvořte záložní zásad pro tuto skupinu objem, každý hlasitost zálohovala v kontejneru odpovídající hlasitost pomocí účtu odpovídající úložiště.

>[AZURE.NOTE] Všechny svazky ve skupině hlasitost musí pocházet z jednoho cloudu poskytovatele služeb.

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Integrace se službou Windows hlasitost stín kopie

Správce snímek StorSimple používá Windows hlasitost stín kopie služby (VSS) k zaznamenání aplikace konzistentní data. VSS usnadňuje konzistenci aplikace tak, že komunikaci s VSS aplikací ke koordinaci vytváření přírůstková snímků. VSS zajišťuje aplikace dočasně neaktivní nebo klidný, když jsou pořízené snímky. 

Správce snímek StorSimple provádění VSS spolupracuje SQL serveru a svazcích obecný. Procesu je následující: 

1. O synchronizaci adresářů, které se obvykle správy dat a řešení ochrany (například správce snímek StorSimple) nebo záložní aplikace vyvolá VSS s dotazem a shromáždění informací ze software Redaktor v cílové aplikaci.

2. VSS kontaktů komponentu Redaktor k získání popisu data. Pisatele vrací popis data, která chcete zálohovat. 

3. VSS signalizuje pisatele Příprava žádost o zálohování. Pisatele připraví data pro zálohování provedením otevřené transakce aktualizací transakce protokoly a tak dál a upozorní aplikace VSS.

4. VSS pokyn pisatele dočasně zastavit úložiště dat aplikace a ujistěte se, že žádná data je došlo k zápisu hlasitost během vytvořena kopie stín. Tento krok zajistí konzistence dat a zabírá víc než 60 sekund.

5. VSS pokyn poskytovatele k vytvoření kopie stín. Poskytovatelé, které mohou být nebo hardwarová a softwarová, spravovat svazky, které jsou aktuálně spuštěných a vytvořte stín kopií je jako služba. Zprostředkovatel vytvoří kopii stín a oznámí, že VSS dokončení.

6. VSS kontaktů pisatele sdělit aplikaci můžete pokračovat v/v operací a potvrďte, že vstupu a výstupu pozastaveno úspěšně během vytváření stínové kopie. 

7. Pokud výtisku byl úspěšný, vrátí VSS umístění kopírovat žadateli předán. 

8. Pokud data napsané při vytvoření kopie stín, zálohování budou konzistentní. VSS odstraní výtisku stín a upozorní o synchronizaci adresářů. O synchronizaci adresářů můžete samé záložní automaticky nebo upozornit správce pokus později.

Viz následující obrázek.

![Proces VSS](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Proces kopírování služby systému Windows hlasitost stín** 

## <a name="backup-types-and-backup-policies"></a>Typy zálohování a zálohování zásady

Pomocí Správce snímek StorSimple můžete zálohovat data a uložte ji místně a v cloudu. Správce snímek StorSimple slouží k obecnějším údajům data okamžitě nebo zásady zálohování můžete použít k vytvoření plánu pro vytváření záloh automaticky. Zálohování zásady umožňují určit, kolik snímky se zachovají. 

### <a name="backup-types"></a>Typy zálohování

Správce snímek StorSimple slouží k vytvoření těmihle zálohování:

- **Místní snímky** – místní snímky jsou v okamžiku kopií hlasitost data, která jsou uložená na StorSimple zařízení. Tento typ zálohování obvykle lze vytvořit a rychle obnovit. Místní snímku můžete použít jako místní záložní kopie.

- **Snímky cloudu** – cloudové snímky jsou v okamžiku kopií hlasitost dat uložených v cloudu. Snímek cloudu odpovídá snímek replikovat na jiné, mimo úložiště systému. Snímky cloudu jsou užitečné zejména v případech obnovení havárie.

### <a name="on-demand-and-scheduled-backups"></a>Na vyžádání a plánované zálohování

Pomocí Správce StorSimple snímku můžete zahájit jednorázovou zálohy okamžitě vytvořit nebo zásady zálohování můžete použít k naplánování opakované zálohování.

Zásady zálohování je sada automatické pravidla, které můžete použít k plánování pravidelného zálohování. Zásady zálohování umožňuje definovat počet_plateb a parametrech pořizování snímků konkrétní hlasitost skupiny. Zásady umožňuje zadat datum zahájení a vypršení platnosti, časy, frekvencí a požadavkům na uchovávání dat, pro obě místních i cloudových snímky. Ihned po můžete definovat je zásada použita. 

Správce StorSimple snímek konfiguraci nebo překonfigurovat záložní zásady kdykoli je to nutné. 

Konfigurace pro každý zásady zálohování, kterou vytvoříte následující informace:

- **Název** – jedinečný název vybrané zásady zálohování.

- **Typ** – typ zásady zálohování; místní snímku nebo v cloudu snímku.

- **Hlasitost skupiny** – skupině hlasitost s vybranou zásady zálohování přiřazenou.

- **Uchovávání informací** – počet záložní kopie, pokud chcete zachovat. Pokud zaškrtnete políčko **vše** , se zachovají všechny záložní kopie do dosažení maximální počet záložní kopií svazku, v tomto okamžiku bude zásada selhat a generovat chybové zprávy. Můžete taky můžete zadat počet zálohy uchovávání (1 až 64).

- **Datum** – datum vytvoření zásady zálohování.

Informace o konfiguraci záložní zásad přejděte na [Správce snímek StorSimple pomocí můžete vytvořit a spravovat záložní zásady](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Úlohy zálohování monitorování a správu

Správce snímek StorSimple slouží ke sledování a správa nadcházející plánované a dokončené úlohy zálohování. Kromě toho StorSimple snímek správce poskytuje katalogu až 64 dokončené zálohy. Pomocí katalogu můžete najít a obnovit svazky nebo jednotlivé soubory. 

Informace o sledování úlohy zálohování přejděte na [Správce snímek StorSimple použít k zobrazení a Správa úloh zálohování](storsimple-snapshot-manager-manage-backup-jobs.md).


## <a name="next-steps"></a>Další kroky

- Další informace o [použití Správce snímek StorSimple ke správě StorSimple řešení](storsimple-snapshot-manager-admin.md).

- Stáhněte [snímek StorSimple správce](https://www.microsoft.com/download/details.aspx?id=44220).
