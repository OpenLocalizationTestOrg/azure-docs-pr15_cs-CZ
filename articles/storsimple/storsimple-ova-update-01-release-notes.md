<properties 
   pageTitle="Poznámky k verzi StorSimple virtuální pole aktualizace | Microsoft Azure"
   description="Popisuje kritické otevřít problémů a řešení pro pole virtuální StorSimple aktualizaci 0,2 a 0,1."
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
   ms.date="06/16/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>Poznámky k verzi virtuální pole aktualizace 0,2 a 0,1 StorSimple

## <a name="overview"></a>Základní informace

Následující poznámky k verzi popsána kritické problémy, otevřít a vyřešit problémy s aktualizací Microsoft Azure StorSimple virtuální pole. (Microsoft Azure StorSimple virtuální je argument matice nazývaný také StorSimple místní virtuální zařízení nebo StorSimple virtuální.) 

Poznámky k verzi jsou průběžně aktualizují a jak kritické otázek, které vyžadují řešení se objeví, přidají se. Před nasazením virtuální zařízení StorSimple pečlivě zkontrolujte informace obsažené v poznámky.

Aktualizace 0,2 odpovídá verzi softwaru **10.0.10280.0**; Aktualizace 0,1 je verze **10.0.10279.0**. Následující části obsahují seznam změny pro každou aktualizaci. 

> [AZURE.NOTE] Aktualizace jsou vyloučit a bude Restartujte zařízení. Pokud probíhají vstupu a výstupu, je možné zařízení za výpadek služeb.

## <a name="issues-fixed-in-the-update-02"></a>Problémy s pevnou aktualizovat 0,2
Aktualizace 0,2 obsahuje všechny změny Update 0,1 kromě opravy popsané v následující tabulce:

Funkce                              | Problém                                                                                                                                                                                                                                                                                                                           |
--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
Aktualizace                                 | V poslední verzi nebyly zjištěny aktualizace automaticky Azure klasické portálu tak, aby bylo nutné použít místní uživatelské rozhraní webu instalovat aktualizace. Tento problém bude pevně v této verzi. Po instalaci aktualizace 0,2, můžete nainstalovat budoucích aktualizacích na portálu Azure klasické.                       

## <a name="whats-new-in-the-update-01"></a>Co je nového ve službě Update 0,1

Aktualizace 0,1 obsahuje následující vylepšení a opravy chyb. 

- **Vylepšené pružnost pro výpadků cloudu**: Tato verze obsahuje několik opravy chyb kolem havárie obnovení zálohování, obnovení a vrstvení v případě přerušení připojení cloudu. 

- **Vylepšené obnovení výkonu**: Tato verze nemá opravy chyb, které mají značně snížit čas dokončení úloh obnovení.

- **Automatické mezery obnovit optimalizace**: Při odstranění dat tence zřizování objemů bloky nepoužitý úložiště muset znovu použít. Tato verze vylepšili procesu místo obnovit z cloudu výsledkem nepoužívané místo budou dostupné rychleji ve srovnání s předchozími verzemi.

- **Nové obrázky virtuálního disku**: nové virtuálního pevného disku, VHDX a VMDK jsou teď dostupné prostřednictvím portálu Azure klasické. Stáhněte si tyto obrázky zřízení nového zařízení aktualizace 0,1.

- **Zlepšení přesnosti stav úlohy na portálu**: V dřívější verzi softwaru nebyl podrobného stavu úloh sestav na portálu. Tento problém vyřešit v této verzi.

- **Připojení k doméně dojít**: opravy chyb souvisejících s připojením k domény a přejmenování zařízení.


## <a name="issues-fixed-in-the-update-01"></a>Problémy s pevnou aktualizovat 0,1

Následující tabulka obsahuje souhrn chyby v této verzi.

| Ne.  | Funkce                              | Problém                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | VMDK                                 | V některých verzích VMware byl disku OS považovat za sparse příčinou upozornění a narušuje normálně. Tento problém byl odstraněn v této verzi.                                                                                                                                                                                    |
| 2    | iSCSI serveru                         | V poslední verzi byl uživatel muset zadat bránu pro každé povolené síťové rozhraní StorSimple virtuální zařízení. Toto chování se změní v této verzi tak, že uživatel nakonfigurujte alespoň jednu bránu pro všechny rozhraní povolena sítě.                                                                              |
| 3    | Podpora balíčku                      | V dřívější verzi softwaru domovské stránce podpory kolekce balíčku se nepovedlo při velikosti balíčku větším než 1 GB. Tento problém bude pevně v této verzi.                                                                                                                                                                               |
| 4    | Přístup ke cloudu                         |  V poslední verzi Pokud pole virtuální StorSimple neobsahoval připojení k síti a restartování, místní uživatelského rozhraní mít problémy s připojením. Tento problém bude pevně v této verzi.                                                                                                                            |
| 5    | Sledování grafy                    | V předchozí verzi, sledovat přepojení zařízení grafy cloudu kapacita využití zobrazit nesprávnými hodnotami v portálu Azure klasické. To je pevné v aktuální verzi.                                                                                                                          |



## <a name="known-issues-in-the-update-01"></a>Známé problémy v aktualizace 0,1

Následující tabulka obsahuje souhrn známých problémů pro pole virtuální StorSimple a problémy vydání poznamenat z předchozích verzí. **Uvolnění problémy uvedené v této verzi označené hvězdičkou. Skoro všech problémů v tomto seznamu mít přenese z verze GA StorSimple virtuální pole.**


| Ne. | Funkce | Problém | Řešení: / komentáře |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Aktualizace | Virtuální zařízení vytvořené ve verzi preview nelze aktualizovat podporovanou verzi všeobecně dostupná. | Tato virtuální zařízení musí selhání verzi všeobecně dostupná pomocí pracovního postupu havárie obnovení (DR). |
| **2.** | Zřizování datový disk | Jakmile mít zřízení disk dat určité zadaný velikosti a vytvořili odpovídající StorSimple virtuální zařízení, nesmí zvětšení nebo zmenšení disku data. Pokoušející k tomu dojde ke ztrátě všechna data v místní úrovně zařízení. |   |
| **3.** | Zásady skupiny | Po zařízení se doméně použití zásad skupiny mohou nepříznivě ovlivnit operaci zařízení. | Zkontrolujte, zda je virtuální argument matice v své vlastní organizační jednotce (OU) služby Active Directory a bez objekty zásad skupiny (objekt zásad skupiny), použijí se k němu.|
| **4.** | Místní web uživatelského rozhraní | Pokud v aplikaci Internet Explorer (IE ESC) jsou povolené funkce zvýšené zabezpečení, některé místní uživatelské rozhraní webu Poradce při potížích ATP údržbu nemusí fungovat správně. Tlačítka na těchto stránkách nemusí fungovat. | Vypnutí funkcí Rozšířené zabezpečení v aplikaci Internet Explorer.|
| **5.** | Místní web uživatelského rozhraní | V virtuálního počítače Hyper-V síťových rozhraní na webu uživatelské rozhraní se zobrazí jako 10 GB/s rozhraní. | Toto chování se odraz Hyper-V. Pro Hyper-V vždy zobrazuje 10 GB/s pro virtuální síťové adaptéry. |
| **6.** | Vrstvené svazky nebo sdílené složky | Rozsah bajtů uzamčení pro aplikace, které spolupracují s StorSimple vrstvené svazky nepodporuje. Pokud je povoleno uzamčení bajt oblast, vrstvení StorSimple nebude fungovat. | Doporučená opatření patří: <br></br>Vypněte rozsah bajtů uzamčení logiky aplikace.<br></br>Zvolte Vložit data pro tuto aplikaci do místně připnuté svazky namísto vrstvené svazky.<br></br>*Výstrahou*: Pokud pomocí místně připnuté svazky a uzamčení oblast bajt aktivované, mějte na paměti, může být místně připnuté hlasitost online ještě před dokončením obnovení. V těchto případech Pokud obnovení probíhá, pak je nutné počkat obnovení dokončete. |
| **7.** | Vrstvené sdílené složky | Práce s velkých souborů může vést k pomalé osy se. | Při práci s velkých souborů, doporučujeme, aby největší soubor je menší než 3 % velikosti sdílet. |
| **8.** | Použít kapacita pro sdílené složky | Může se zobrazit sdílet spotřebu neexistuje žádná data ve sdílené složce. Důvodem je použité kapacita sdílených zahrnuje metadata. |   |
| **9.** | Obnovení havárie | Lze provést pouze obnovení havárie serveru soubor má tu samou doménu jako zdroj zařízení. Obnovení havárie do cílové zařízení v jiné doméně není podporované v této verzi. | To bude v pozdější verzi implementovaná. |
| **10.** | Azure Powershellu | Virtuální zařízení StorSimple nelze spravovat pomocí prostředí PowerShell Azure v této verzi. | Všechny Správa virtuální zařízení se provádí prostřednictvím portálu klasické Azure a místních webu uživatelského rozhraní. |
| **11.** | Změna hesla | Konzole zařízení virtuální pole pouze zadávaná do formátu klávesnice en US. |   |
| **12.** | CHAP | Po vytvoření přihlašovací údaje CHAP nejde odebrat. Kromě toho změnit přihlašovací údaje CHAP bude potřebnosti svazky jako offline a pak je pak online aby se změna projevila. | Tyto bude určeno v pozdější verzi. |
| **13.** | iSCSI serveru  | "Používá úložiště" pro jednotce iSCSI zobrazeny může lišit v službu StorSimple správce a hostiteli iSCSI. | Hostitel iSCSI má zobrazení systému souborů.<br></br>Zařízení uvidí bloky přidělený při objem byl maximální velikost.|
| **14.** | Soubor serveru *  | Pokud má soubor ve složce alternativní dat toku (REKLAM) přidružená, REKLAM není zálohovala obnovit prostřednictvím havárie obnovení klonovat a obnovení úroveň položky| |


## <a name="next-step"></a>Další krok

[Instalace aktualizací](storsimple-ova-install-update-01.md) na své StorSimple virtuální pole.
