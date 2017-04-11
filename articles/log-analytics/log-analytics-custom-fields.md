<properties
   pageTitle="Vlastní pole v protokolu analýzy | Microsoft Azure"
   description="Vlastní pole funkce analýzy Log umožňuje vytvořit vyhledávání polím z dat OMS přidat do vlastnosti shromážděných záznamu.  Tento článek popisuje postup vytvoření vlastní pole a poskytuje podrobné informace událost nastavit jako ukázka."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-fields-in-log-analytics"></a>Vlastní pole v protokolu analýzy

**Vlastní pole** funkce analýzy Log umožňuje rozšíření stávajících záznamů v úložišti OMS přidáním vyhledávání polím.  Vlastní pole jsou automaticky vyplněné z data extrahovaných z jiné vlastnosti ve stejném záznamu.

![Vlastní pole přehled](media/log-analytics-custom-fields/overview.png)

Například následující ukázkové záznam obsahuje užitečné data skryty v popis události.  Extrahování tato data do samostatných vlastností díky kterému je dostupný pro činnosti, jako je řazení a filtrování.

![Tlačítko Hledat protokolu](media/log-analytics-custom-fields/sample-extract.png)

>[AZURE.NOTE] V náhledu se omezuje 100 vlastních polí v pracovním prostoru.  Tímto limitem budou rozbalená tuto funkci dosažení všeobecně dostupná.

## <a name="creating-a-custom-field"></a>Vytvoření vlastního pole

Při vytváření vlastního pole protokolu analýzy musí porozumět tomu, jaký dat sloužící k naplnění jeho hodnotu.  Používá technologii služby Microsoft Research s názvem FlashExtract rychlá identifikace tato data.  Místo nutnosti explicitního pokyny, analýzy protokolu dozvídá o data, která chcete extrahovat ze příklady, které zadáte.

Postup pro vytvoření vlastního pole v následujících oddílech.  Na konci tohoto článku je návod odebráním vzorku.

> [!NOTE] Při přidávání záznamů odpovídající zadaným kritériím úložišti dat OMS tak, aby se zobrazí pouze u záznamů shromažďované po vytvoření vlastního pole je zadá vlastní pole.  Vlastní pole se nepřidá záznamy, které již máte v úložišti při vytvoření.

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Krok 1 – identifikovat záznamy, které bude mít vlastní pole
Cílem prvního kroku je identifikujícího záznamy, které se zobrazí vlastního pole.  Spusťte [Standardní protokolu vyhledávání](log-analytics-log-searches.md) a pak vybrat záznam, který má fungovat jako model, který protokolu analýzy dozvíte z.  Když nastavíte, že se chystáte extrahovat data do vlastního pole **Pole extrahování Průvodce** otevřené kde ověření a zpřesnit kritéria.

2. Přejděte do **Protokolu hledání** a použití [dotazu pro načítat záznamy](log-analytics-log-searches.md) , která bude mít vlastní pole.
2. Vyberte záznam využívající technologie pro analýzu protokolu má fungovat jako modelu pro získání dat k naplnění vlastní pole.  Uvidíte data, která chcete extrahovat z tohoto záznamu a technologie pro analýzu protokolu použije tyto informace k určení logickou k naplnění vlastní pole pro všechny podobně jako záznamy.
3. Klikněte na tlačítko nalevo od všech vlastnosti text záznamu a vyberte **extrahovat pole z**.
4. **Otevřít průvodce extrahování pole**a záznam, který jste vybrali se zobrazí ve sloupci **Hlavní příklad** .  Vlastní pole bude definované pro tyto záznamy se stejnými hodnotami ve vlastnostech, které jsou vybrány.  
5. Pokud je výběr přesně co budete potřebovat, vyberte další pole, která kritéria zúžit.  Chcete-li změnit hodnoty polí pro kritéria, musíte zrušit a vyberte jiný záznam splňující kritéria, které chcete.

### <a name="step-2---perform-initial-extract"></a>Krok 2: provedení počáteční extrahovat.
Jakmile jste určili záznamy, které bude mít vlastní pole, identifikujete data, která chcete extrahovat.  Protokol analýzy budou tyto informace použít k identifikaci vzorů podobně jako v podobné záznamy.  V kroku po uplynutí tohoto budou moct ověřte výsledky a poskytují další podrobnosti o protokolu technologie pro analýzu můžete v jeho analýza.

1. Zvýraznění textu v ukázkové záznamu, který chcete k naplnění vlastní pole.  Potom zobrazí dialogové okno zadat název pole a provést počáteční extrahovat.  Znaky ** \_CF** budou automaticky přidána.
2. Klikněte na **extrahovat** analýza shromážděných záznamů.  
3. Oddíly **Souhrn** a **Výsledky hledání** zobrazení výsledků extrahovat, můžete zkontrolovat jeho přesnost.  **Souhrn** zobrazí kritéria slouží k identifikaci záznamů a počet u každé hodnoty dat identifikovat.  **Výsledky vyhledávání** obsahuje podrobný seznam záznamů splňujících kritéria.


### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Krok 3 – Zkontrolujte správnost výpisu a vytvořit vlastní pole

Po provedení počáteční extrahovat protokolu analýzy zobrazit jeho výsledky podle data, která již byla vybrána.  Pokud výsledky najděte přesné můžete vytvořit vlastní pole s žádné další řešení.  V opačném případě pak můžete upřesnit výsledky tak, aby protokolu analýzy můžete vylepšit jeho použití logických operátorů.

2.  Pokud všechny hodnoty v počáteční extrahovat nejsou správná, klikněte na ikonu **Upravit** vedle nepřesných záznamu a zvolte **změnit toto zvýraznění** upravit výběru.
3.  Položka zkopírována do oddílu **Další příklady** pod **Hlavní příklad**.  Zvýraznění Zde můžete upravit pomůže protokolu analýzy pochopit výběru, které by měl mít udělali.
4.  Klikněte na **extrahovat** do použijte tyto nové informace k posouzení všech stávajících záznamů.  Výsledky může změnit záznamy než ten, který jste právě změnili založené na této nový měřítka.
5.  Pokračujte v přidávání oprav, dokud se všechny záznamy ve výpisu správně identifikovat dat pro vyplnění nové vlastní pole.
6. Až budete spokojeni s výsledky, klikněte na **Uložit extrahovat** .  Teď definované vlastního pole, ale ještě nebude přidána do všech záznamů.
7.  Počkejte, než pro nové záznamy odpovídající zadaným kritériím shromažďovat a pak znova spusťte hledání protokolu. Nové záznamy měli vlastního pole.
8.  Použití vlastního pole jako jiné záznamu vlastnictví.  Můžete použít k agregační a seskupení dat a používat i k vytvoření nové přehledy.


## <a name="viewing-custom-fields"></a>Zobrazení vlastní pole
Ve skupině Správa z **Nastavení** dlaždice řídicího panelu OMS můžete zobrazit seznam všech vlastních polí.  Vyberte **Data** a potom **vlastní** seznam všech vlastních polí v pracovním prostoru.  

![Vlastní pole](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Odebrání vlastního pole
Existují dva způsoby, jak odebrat vlastní pole.  První je možnost **Odebrat** pro každé pole při prohlížení kompletní seznam ve výše uvedeném.  Jiné metody je načtení záznamu a klikněte na tlačítko nalevo od pole.  V nabídce bude mít možnost odebrání vlastního pole.

## <a name="sample-walkthrough"></a>Ukázka návod

V následující části provede kompletní příklad vytvoření vlastního pole.  V tomto příkladu extrahuje název služby v systému Windows události, které označují službu změna stavu.  To je založena na události vytvořené pomocí Správce služeb v protokolu systému Windows počítačů.  Pokud chcete sledovat v tomto příkladu, musíte být [shromažďování informací událostí pro protokolu systému](log-analytics-data-sources-windows-events.md).

Jsme zadejte následující dotaz vrátí všechny události z Správce služeb, máte ID události 7036 což je události, která označuje služby spuštění a ukončení.

![Dotaz](media/log-analytics-custom-fields/query.png)

Jsme klikněte na libovolný záznam s událostí 7036 ID.

![Zdroj záznamů](media/log-analytics-custom-fields/source-record.png)

Chceme služby název, který se zobrazí ve vlastnosti **RenderedDescription** a klikněte na tlačítko vedle této vlastnosti.

![Extrahování pole](media/log-analytics-custom-fields/extract-fields.png)

Otevřít **Průvodce extrahování pole** a že nemáte vybrané pole **událostí** a **EventID** ve sloupci **Hlavní příklad** .  Tento údaj označuje, že bude je to možné definovat vlastní pole pro události z protokolu systému s ID události 7036..  Toto je dostatečně jsme nemuseli vyberte požadovaná pole.

![Hlavní příklad](media/log-analytics-custom-fields/main-example.png)

Vyberte název služby na vlastnost **RenderedDescription** jsme **službu k určení názvu služby** .  Vlastní pole se nazývá **Service_CF**.

![Pole název](media/log-analytics-custom-fields/field-title.png)

Vidíme, že název služby identifikovány správně pro některé záznamy, ale nikoli pro ostatní uživatele.   Zobrazit **Výsledky hledání** , že není zaškrtnuté část názvu **WMI výkonu adaptér** .  **Souhrn** ukazuje, že čtyři záznamy služby **DPRMA** nesprávně zahrnuty navíc aplikace word a dva záznamy identifikovat **Moduly instalační** místo **Instalační služby systému Windows moduly**.  

![Výsledky hledání](media/log-analytics-custom-fields/search-results-01.png)

Začneme tím se záznam **WMI výkonu adaptér** .  Jsme klikněte na její ikonu Upravit a potom **změnit toto zvýraznění**.  

![Úprava zvýraznění](media/log-analytics-custom-fields/modify-highlight.png)

Jsme rozšiřte zvýraznění připsat slovo **WMI** a znovu spusťte extrahovat.  

![Další příklad](media/log-analytics-custom-fields/additional-example-01.png)

Jsme zjistí, že opravili položky pro **Adaptér WMI výkonu** a technologie pro analýzu protokolu také tyto informace opravte záznamy **Modul Instalační služby**systému Windows.  V části **Souhrn** se zobrazují v případě, že **DPMRA** není pořád identifikují správně.

![Výsledky hledání](media/log-analytics-custom-fields/search-results-02.png)

Posuňte se záznamem se službou DPMRA jsme budete postupovat stejně problém můžete vyřešit tak tento záznam.

![Další příklad](media/log-analytics-custom-fields/additional-example-02.png)

 Když jsme spustíte extrakci se zobrazují všechny naše výsledky správnost nyní.

![Výsledky hledání](media/log-analytics-custom-fields/search-results-03.png)

Vidíme, že se vytvoří **Service_CF** , ale není zatím nepřidali na všechny záznamy.

![Počáteční počet](media/log-analytics-custom-fields/initial-count.png)

Po určitou dobu tak nové události odebírají, vidíme, že pole **Service_CF** teď se přidá do záznamů odpovídající naše kritéria.

![Konečné výsledky](media/log-analytics-custom-fields/final-results.png)

Teď můžete vlastního pole jako ostatní vlastnost záznamu.  Pro znázornění je vytvoříme dotaz, který seskupí podle **Service_CF** pole nové kontrolovat služby, které jsou nejčastěji aktivní.

![Seskupit podle dotazu](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Další kroky

- Informace o [hledání protokolu](log-analytics-log-searches.md) k vytvoření dotazů pomocí vlastní pole kritérií.
- Sledování [protokoly vlastní](log-analytics-data-sources-custom-logs.md) , můžete analyzovat pomocí vlastních polí.