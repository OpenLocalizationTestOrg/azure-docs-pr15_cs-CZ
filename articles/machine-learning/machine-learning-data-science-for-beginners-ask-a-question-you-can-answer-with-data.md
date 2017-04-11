<properties
   pageTitle="Položení otázky můžete odpovědět s daty – formulace otázky | Microsoft Azure"
   description="Zjistěte, jak formulace otázku vědy dat v jiné vědecké dat pro začátečníky video 3. Obsahuje porovnání klasifikaci a regresní otázky."
   keywords="data pro výzkum otázky formulace otázky, regresní otázek, klasifikace otázky, ostřejším otázka"
   services="machine-learning"
   documentationCenter="na"
   authors="cjgronlund"
   manager="jhubbard"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="cgronlun;garye"/>

# <a name="ask-a-question-you-can-answer-with-data"></a>Položení otázky, které můžete odpovědět s daty

## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Výzkum Data pro začátečníky řady

Zjistěte, jak formulace otázku vědy dat v jiné vědecké dat pro začátečníky video 3. Toto video obsahuje porovnání otázek klasifikaci a regresní algoritmů.

Využívejte řadě, můžete sledujte všechny. [Přejděte do seznamu videí](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-ask-a-question-you-can-answer-with-data]

## <a name="other-videos-in-this-series"></a>Další videa v této řadě

*Výzkum dat pro začátečníky* je rychlý úvod a jiné vědecké dat v pěti krátká videa.

  * Video 1: [Jiné vědecké dat 5 otázky a odpovědi](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 minut 14 sekund)*
  * Video 2: [hotovou dat pro výzkum dat?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 minut 56 sekund)*
  * Video 3: Položení otázky, které můžete odpovědět s daty
  * Video 4: [Předpověď odpověď s jednoduchého modelu](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 42 s min)*
  * Video 5: [Kopírování ostatních lidí práce pro výzkum dat](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 18 s min)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Přepis: Položení otázky, které můžete odpovědět s daty

Vítá vás třetí video v řadě "Dat jiné vědecké pro začátečníky."  

Během tuto položku uslyšíte několik tipů pro zpracovávající otázku, kterou může odpovědět s daty.

Může se zobrazit další mimo toto video, pokud nejprve přehrát dvě starší videa v této řadě: "vědy dat 5 otázky můžete odpovědět" a "Je datům umístěným dat pro výzkum?"

## <a name="ask-a-sharp-question"></a>Zeptejte se ostrý

Jsme jste kontakt o tom, jak vědy dat je proces se jmény (nazývané také kategorií nebo popisky) a čísla odhadnout odpověď na dotaz. Ale nemůže být jenom jakékoli otázky; musí být *ostrý otázku.*

Otázce nepřesných nemá k zodpovězení s názvem nebo číslo. Musí být ostrý otázku.

Představte si, že najít magic svítilnu s genie, kdo pravdivě odpoví všechny otázku. Ale je mischievous genie a mu se pokusíte dělat svou odpověď jako nepřesných a přehlednější jako mu můžete procházelo. Chcete-li připnout ho tak vzduchotěsným mu nelze pomáhají ale zjistit, co budete chtít zjistit dotazu.

Pokud by bylo potřeba zeptejte se nepřesných, třeba "Co bude stát se můj burzovního?", může odpovědět genie, "cenu se změní". To je pravdivé odpověď, ale není velmi užitečné.

Ale pokud by bylo potřeba zeptejte se ostrý, třeba "Co Moje burzovního prodejní cenu budou příští týden?", genie nelze Nápověda, ale můžete udělit konkrétní odpovíte a předpovídání prodejní cenu.

## <a name="examples-of-your-answer-target-data"></a>Příklady odpověď: cílová data

Jakmile formulace svou otázku, zkontrolujte, jestli máte příklady odpověď ve vašich datech.

Pokud naše otázku je "Co Moje burzovní prodejní cenu bude příští týden?" potom máme Ujistěte se, že naše data obsahují historii burzovní cena.

Pokud je naše otázku "které auta v mém loďstva bude selhání nejdřív?" potom máme Ujistěte se, že naše data obsahují informace o chybách předchozí.

![Cílová data: Příklady odpověď. Formulace otázce vědy data.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-target-data.png)

Tyto příklady odpovědi se označují jako cíle. Cílový dokument je co jsme pokoušíte předpovídání o budoucí datových bodů, ať už jde kategorii nebo číslo.

Pokud nemáte žádná data cílové, musíte získat některé. Nebude moct zodpovědět bez něj.

## <a name="reformulate-your-question"></a>Byla znovu formulována svou otázku

V některých případech můžete Změna znění svou otázku získat zvýšíte jeho přínos odpovědí.

Na otázku "Je tento datový bod A a B?" předpovídá kategorie (nebo název nebo popisek) jevu. Přijmout ji používáme *klasifikace algoritmus*.

Na otázku "Kolik?" nebo "Kolik?" předpovídá částky. Přijmout ji používáme *regresní algoritmus*.

Jak to můžete transformovat jsme zobrazíte Pojďme se podívat na otázku "které zpravodajský článek jsou nejzajímavější toto druhé osobě?" Požádá předpověď jednu volbu z mnoha možností – tedy "Je tento A a B nebo C a D?" - a byste měli použít algoritmu klasifikace.

Ale tuto otázku lze zjednodušit s dotazem, zda Změna znění ho ve formátu "jak zajímavými je každý text je v tomto seznamu a prohlížeč?" Teď dejte každý článek číselných skóre a potom je snadno identifikovat článek nejvyšší bodování. Toto je bublině klasifikace otázky do otázce regresní nebo kolik?

![Byla znovu formulována svou otázku. Otázka klasifikace porovnání regresní otázku.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-classification-question-vs-regression-question.png)

Jak pokládat otázky je potvrzením které algoritmu vám může odpověď.

Zjistíte, aby určité skupiny algoritmů -, jako jsou ty v našem příkladu postupu příspěvky - úzce související. Byla znovu formulována svou otázku, která vám nejvíc vyhovovat odpovědí algoritmus.

Ale, nejdůležitější zeptejte se tam, které ostrý - otázku, na kterou odpovíte s daty. A ujistěte se, jestli že máte správná data můžete přijmout ji.

Jsme jste kontakt o některé základní principy položit otázku, že můžete odpovědět s daty.

Ujistěte se, podívejte se na další videa v "Data pro výzkum pro začátečníky" z webu Microsoft Azure počítače Learning.


## <a name="next-steps"></a>Další kroky

  * [Opakujte první pokusy vědy dat s počítače výukové Studio](machine-learning-create-experiment.md)
  * [Získat Úvod do počítače výukové na Microsoft Azure](machine-learning-what-is-machine-learning.md)
