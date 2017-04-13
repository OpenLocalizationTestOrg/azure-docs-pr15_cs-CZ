<properties 
   pageTitle="Uvolněte 8000 StorSimple poznámky k verzi verze | Microsoft Azure"
   description="Popisuje nových funkcí, otevřené problémy a k dispozici řešení pro červenec 2014 Microsoft Azure StorSimple vydání."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>Poznámky k verzi StorSimple 8000 řady verzi – červenec 2014 

## <a name="overview"></a>Základní informace

Poznámky k verzi sledovat identifikovat kritické otevřené problémy pro řadu 8000 StorSimple červenec 2014 všeobecně dostupná (GA) verzi Microsoft Azure StorSimple. Tato verze odpovídá verzi softwaru 6.3.9600.17215.  

Pokud není zadáno jinak, tyto poznámky k verzi platí pro všechny modely StorSimple zařízení. Poznámky k verzi jsou průběžně aktualizují; jak jsou zjištěny kritické otázek, které vyžadují řešení, se přidají. Před nasazením řešení Microsoft Azure StorSimple, zvažte následující informace.  

## <a name="known-issues-in-this-release"></a>Známé problémy v této verzi
Následující tabulka obsahuje souhrn známé problémy v této verzi.  
 
| Ne. | Funkce | Problém | Komentáře a řešení | Platí pro fyzické zařízení | Platí pro virtuální zařízení |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Obnovení Factory | V některých případech při provádění factory resetovat zařízení StorSimple může být pomalé a zobrazit tato zpráva: **Obnovit factory probíhá (fáze 8)**. Tím se stane, když probíhá rutinu stiskněte kombinaci kláves CTRL + C. | Není stisknutím klávesy CTRL + C po zahájení factory obnovit. Pokud už jste tento stav, obraťte se prosím Microsoft Support k dalším krokům. | Ano | Ne |
| 2 | Disk kvora | Výjimečně Pokud většinou disků v EBOD přílohu 8600 zařízení jsou to od ní odpojilo výsledkem bez disku kvora fondu úložiště bude offline. Zůstane v režimu offline, i když disků připojeni. | Je třeba Restartujte zařízení. Pokud potíže potrvají, obraťte se prosím Microsoft Support k dalším krokům. | Ano | Ne |
| 3 | Selhání snímek cloudu | Výjimečně může selhat cloudu snímek s chybou **maximální limit záložní dosažení**. K tomu dojde, pokud překročit 255 online klony na stejné zařízení ze stejného objemem původní, který byl odstraněn. | | Ano | Ano |
| 4 | ID nesprávné zařízení | Při provádění náhradní řadiče domény řadiče domény 0 může zobrazit jako řadiči 1. Během řadiče náhradní při načtení obrázek z uzel mezi dvěma účastníky ID řadiče zobrazit původně jako ID zařízení mezi dvěma účastníky. Výjimečně toto chování mohou také projeví po restartování systému. | Není nutná žádná akce uživatele. Tato situace vyřeší samotné po dokončení náhradní řadiče domény. | Ano | Ne |
| 5 | Zařízení sledování grafy | Ve službě StorSimple správce sledování grafy zařízení nefunguje Basic nebo ověřování NTLM je povolen v konfiguraci proxy serveru pro zařízení. | Změna konfigurace proxy web pro zařízení registrovaný u StorSimple Správce služby tak, že ověření nastavíte na žádný. K tomuto účelu spustit prostředí Windows PowerShell pro rutinu StorSimple Set-HcsWebProxy. | Ano | Ano |
| 6 | Účty úložiště | Odstranění účtu úložiště pomocí služba úložiště je nepodporované funkce. To vede k situaci, ve kterém nelze uživatelská data načíst. | | Ano | Ano |
| 7 | Překlopení zpět | Navrácení 24 hodin havárie obnovení (DR) není podporované. | | Ano | Ne |
| 8 | Převzetí zařízení | Více předávání služeb při selhání kontejneru hlasitosti na stejné zařízení zdroje pro různé cílové zařízení není podporovaná. Přepnutí z jednoho nefunkční zařízení několika zařízeních pokusy hlasitost kontejnerů na první selhání zařízení dojít ke ztrátě dat vlastnictví. Po selhání tyto hlasitost kontejnery zobrazit nebo s odlišným chováním při zobrazení v portálu Azure klasické. | | Ano | Ne |
| 9 | Instalace | Během StorSimple adaptér pro instalaci služby SharePoint budete muset zadat IP zařízení k úspěšnému dokončení instalace. | | Ano | Ne |
| 10 | Rozhraní sítě | Síťová rozhraní DATA 2 a 3 dat byly vyměnit softwaru. | Pokud potřebujete ke konfiguraci těchto rozhraní, obraťte se na Microsoft Support. | Ano | Ne |


 
