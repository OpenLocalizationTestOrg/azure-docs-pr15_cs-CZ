<properties 
   pageTitle="Poznámky k verzi StorSimple virtuální pole aktualizace | Microsoft Azure"
   description="Popisuje kritické otevřít problémů a řešení pro pole virtuální StorSimple aktualizaci 0,3."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/15/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-03-release-notes"></a>Poznámky k verzi 0,3 StorSimple virtuální pole aktualizace

## <a name="overview"></a>Základní informace

Následující poznámky k verzi popsána kritické problémy, otevřít a vyřešit problémy s aktualizací Microsoft Azure StorSimple virtuální pole.

Poznámky k verzi jsou průběžně aktualizují a jak kritické otázek, které vyžadují řešení se objeví, přidají se. Před nasazením své StorSimple virtuální pole pečlivě zkontrolujte informace obsažené v poznámky.

Aktualizace 0,3 odpovídá verzi softwaru **10.0.10288.0**.

> [AZURE.NOTE] Aktualizace jsou vyloučit a restartujte zařízení. Pokud probíhají vstupu a výstupu, zařízení poněkud výpadek služeb.


## <a name="whats-new-in-the-update-03"></a>Co je nového ve službě Update 0,3

Aktualizace 0,3 je primárně oprava chyb Tvůrce dotazů. V této verzi několik chyby výsledkem záložní k chybám v předchozí verzi vyřešili.

## <a name="issues-fixed-in-the-update-03"></a>Problémy s pevnou aktualizovat 0,3

Následující tabulka obsahuje souhrn chyby v této verzi.

| Ne.  | Funkce                              | Problém                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Zálohování                                |Problém bylo vidět v dřívějších verzích kde zálohy selže dokončete ve sdílené složce. Pokud došlo k tomuto problému, selže úlohy zálohování a upozornění na kritické aktivované StorSimple Správce služby pro uživatele. Tento problém neovlivní data na sdílené složky nebo přístupu k datům. Příčinou identifikovat a pevně v této verzi. <br></br> Oprava netýká zpětně sdílené složky, které už vidí tento problém. Zákazníci, kteří vidí tento problém by měl nejdřív nainstalovat aktualizaci 0,3, a pak kontaktovat Microsoft Support k provedení úplné zálohy systému vyřešit problém. Místo kontaktovat Microsoft Support Zákazníci můžete taky obnovit nové sdílené složky ze zálohy správný pro problémového sdílené složky.                                                                                                                                                                                 |
| 2    | iSCSI                         | Problém byl jinak v dřívějších verzích kde svazky zmizí při kopírování dat do hlasitost StorSimple virtuální matici. Tento problém se pevně v této verzi. <br></br> Opravy se projeví jenom na nově vytvořený svazky. Opravy se nevztahují zpětně svazky, které už vidí tento problém. Zákazníci, by měli zobrazíte problémového svazky online pomocí portálu Azure klasické, zálohování pro tyto svazky a obnovovat tyto svazky na nové svazky.                                                               |


## <a name="known-issues-in-the-update-03"></a>Známé problémy v aktualizace 0,3

Následující tabulka obsahuje souhrn známých problémů pro pole virtuální StorSimple a problémy vydání poznamenat z předchozích verzí. 


| Ne. | Funkce | Problém | Řešení: / komentáře |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Aktualizace | Virtuální zařízení vytvořené ve verzi preview nelze aktualizovat podporovanou verzi všeobecně dostupná. | Tato virtuální zařízení musí selhání verzi všeobecně dostupná pomocí pracovního postupu havárie obnovení (DR). |
| **2.** | Zřizování datový disk | Jakmile mít zřízení disk dat určité zadaný velikosti a vytvořili odpovídající StorSimple virtuální zařízení, nesmí zvětšení nebo zmenšení disku data. Pokus o proveďte výsledky ztrátu všechna data v místním úrovní zařízení. |   |
| **3.** | Zásady skupiny | Po zařízení se doméně použití zásad skupiny mohou nepříznivě ovlivnit operaci zařízení. | Zkontrolujte, zda je virtuální argument matice v své vlastní organizační jednotce (OU) služby Active Directory a bez objekty zásad skupiny (objekt zásad skupiny), použijí se k němu.|
| **4.** | Místní web uživatelského rozhraní | Pokud v aplikaci Internet Explorer (IE ESC) jsou povolené funkce zvýšené zabezpečení, některé místní uživatelské rozhraní webu Poradce při potížích ATP údržbu nemusí fungovat správně. Tlačítka na těchto stránkách nemusí fungovat. | Vypnutí funkcí Rozšířené zabezpečení v aplikaci Internet Explorer.|
| **5.** | Místní web uživatelského rozhraní | V virtuálního počítače Hyper-V síťových rozhraní na webu uživatelské rozhraní se zobrazí jako 10 GB/s rozhraní. | Toto chování se odraz Hyper-V. Pro Hyper-V vždy zobrazuje 10 GB/s pro virtuální síťové adaptéry. |
| **6.** | Vrstvené svazky nebo sdílené složky | Rozsah bajtů uzamčení pro aplikace, které spolupracují s StorSimple vrstvené svazky nepodporuje. Pokud je povoleno uzamčení bajt oblast, vrstvení StorSimple nelze použít. | Doporučená opatření patří: <br></br>Vypněte rozsah bajtů uzamčení logiky aplikace.<br></br>Zvolte Vložit data pro tuto aplikaci do místně připnuté svazky namísto vrstvené svazky.<br></br>*Výstrahou*: při použití místně připnuté svazky a uzamčení oblast bajt je povoleno, může být místně připnuté hlasitost online ještě před dokončením na obnovit. V těchto případech Pokud obnovení probíhá, pak je nutné počkat obnovení dokončete. |
| **7.** | Vrstvené sdílené složky | Práce s velkých souborů může vést k pomalé osy se. | Při práci s velkých souborů, doporučujeme, aby největší soubor je menší než 3 % velikosti sdílet. |
| **8.** | Použít kapacita pro sdílené složky | Může se zobrazit spotřebu sdílejte, když nejsou žádná data ve sdílené složce. Důvodem je použité kapacita sdílených zahrnuje metadata. |   |
| **9.** | Obnovení havárie | Lze provést pouze obnovení havárie serveru soubor má tu samou doménu jako zdroj zařízení. Obnovení havárie do cílové zařízení v jiné doméně není podporované v této verzi. | To je v pozdější verzi implementovaná. |
| **10.** | Azure Powershellu | Virtuální zařízení StorSimple nelze spravovat pomocí prostředí PowerShell Azure v této verzi. | Všechny Správa virtuální zařízení se provádí prostřednictvím portálu klasické Azure a místních webu uživatelského rozhraní. |
| **11.** | Změna hesla | Konzole zařízení virtuální pole pouze zadávaná do formátu klávesnice en US. |   |
| **12.** | CHAP | Po vytvoření přihlašovací údaje CHAP nejde odebrat. Kromě toho změnit přihlašovací údaje CHAP potřebnosti svazky jako offline a pak je pak online aby se změna projevila. | Tento problém se řeší nové verze. |
| **13.** | iSCSI serveru  | "Používá úložiště" pro jednotce iSCSI zobrazeny může lišit v službu StorSimple správce a hostiteli iSCSI. | Hostitel iSCSI má zobrazení systému souborů.<br></br>Zařízení uvidí bloky přidělený při objem byl maximální velikost.|
| **14.** | Souborovém serveru  | Pokud má soubor ve složce alternativní dat toku (REKLAM) přidružená, REKLAM není zálohovala obnovit prostřednictvím havárie obnovení klonovat a obnovení úroveň položky| |


## <a name="next-step"></a>Další krok

[Nainstalujte aktualizaci 0,3](storsimple-ova-install-update-01.md) na své StorSimple virtuální pole.

## <a name="references"></a>Odkazy

Hledáte starší verze poznámky? Přejít na: 

- [StorSimple virtuální pole aktualizace 0,1 a 0,2 poznámky k verzi](storsimple-ova-update-01-release-notes.md)

- [StorSimple virtuální pole Obecné dostupnost poznámky](storsimple-ova-pp-release-notes.md)
 

