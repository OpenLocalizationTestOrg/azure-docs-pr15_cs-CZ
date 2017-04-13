<properties 
   pageTitle="Poznámky k verzi virtuální pole StorSimple | Microsoft Azure"
   description="Popisuje kritické otevřít problémů a řešení pro pole virtuální StorSimple."
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
   ms.date="05/13/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-release-notes"></a>Poznámky k verzi StorSimple virtuální matice

## <a name="overview"></a>Základní informace

Následující poznámky k verzi popsána kritické otevřené problémy březen 2016 všeobecně dostupná (GA) verzi aplikace Microsoft Azure StorSimple virtuální pole (označovaná taky jako virtuální zařízení místní StorSimple nebo StorSimple virtuální zařízení). Tato verze odpovídá verzi softwaru 10.0.10271.0.

Poznámky k verzi jsou průběžně aktualizují a jak kritické otázek, které vyžadují řešení se objeví, přidají se. Před nasazením virtuální zařízení StorSimple pečlivě zkontrolujte informace obsažené v poznámky. 

Následující tabulka obsahuje souhrn známé problémy v této verzi.


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
