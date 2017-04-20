<properties
    pageTitle="Krok 5: Zavést Machine Learning webové služby | Microsoft Azure"
    description="Krok 5 z vývoje řešení prediktivní návod: nasazení prediktivní experimentu v Machine Learning Studio jako webová služba."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a>Návod krok 5: Zavést Azure Machine Learning webové služby

Jedná se o pátý krok návod, [vývoj řešení prediktivní analytiky v Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Vytvořit pracovní prostor Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Odeslat existující data.](machine-learning-walkthrough-2-upload-data.md)
3.  [Vytvořit nový pokus](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Vlaku a hodnotit modely](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  **Nasazení webových služeb**
6.  [Přístup k webové službě](machine-learning-walkthrough-6-access-web-service.md)

----------

Ostatním uživatelům možnost používat prediktivní model, který jsme vyvinuli v tomto návodu, doporučujeme jej nasadit jako webovou službu na Azure.

V tomto okamžiku jsme experimentovali s školení našeho modelu. Ale nasazené služby již bude provádět školení - generuje předpovědi podle bodování vstupu uživatele na základě našeho modelu. Tak chceme provést některé přípravné ***školení*** pokus převést tento pokus ***prediktivní*** experimentovat. 

To je dvoustupňový proces:  

1. Převést *školení experimentovat* , vytvořili jsme *prediktivní experimentu*
2. Prediktivní experiment nasadit jako webovou službu

Ale nejprve musíme oříznout tento experiment trochu dolů. V současné době máme dva různé modely v testu, ale chceme pouze jeden model, když jsme to nasadit jako webovou službu.  

Řekněme, že můžeme říct, že byl zesílen strom modelu lepší model použít. Takže prvním krokem je odebrat [Počítač Vektor dvě třídy podpory] [ two-class-support-vector-machine] modulu a modulů, které byly použity pro školení je. Chcete vytvořit kopii testu nejprve klepnutím na tlačítko **Uložit jako** v dolní části experimentu plátno.

Je nutné odstranit následující moduly:  

- [Podpora dvě třídy Vector stroje][two-class-support-vector-machine]
- [Model vlaku] [ train-model] a [Skóre Model] [ score-model] moduly, které byly připojeny k němu
- [Normalizovat Data] [ normalize-data] (obě)
- [Vyhodnotit Model s][evaluate-model]

Vyberte modul a stiskněte klávesu Delete nebo klepněte pravým tlačítkem myši modul a klepněte na příkaz **Odstranit**.

Nyní jsme připraveni k zavedení tohoto modelu pomocí [Třídy dvě zesílen rozhodovacího stromu][two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Převést na prediktivní experiment pokus školení

Převod na prediktivní experiment zahrnuje tři kroky:

1. Uložit model jsme jste vyškoleni a potom nahradit našich vzdělávacích modulů
2. Trim pokus odebrat moduly, které byly třeba pouze pro vzdělávání
3. Definovat, pokud webová služba přijme vstup a kde generuje výstup

Naštěstí všechny tři kroky lze provést klepnutím na tlačítko **nastavit webové služby** v dolní části experimentu plátno (vyberte možnost **prediktivní webové služby** ).

Po klepnutí na tlačítko **nastavit webové služby**, dojde k několika změnám:

- Vyškolených modelu uložena jako jeden modul **Vyškolených modelu** v paletě modulu k levému okraji plátna experimentu (najdete ho v části **Vyškolených modely**).
- Moduly, které byly použity pro školení budou odebrány. Konkrétně:
  - [Dvě třídy zesílen rozhodovací strom][two-class-boosted-decision-tree]
  - [Model vlaku][train-model]
  - [Rozdělená Data][split]
  - druhý [Skript spouštět R] [ execute-r-script] modulu, který byl použit pro testovací data
- Uložený vyškolených modelu je přidána zpět do experimentu.
- **Vstupní webové služby** a **webové služby výstupní** moduly jsou přidány.

> [AZURE.NOTE] Pokus byl uložen ve dvou částech podle karet, které byly přidány v horní části plátna experimentu: původní experiment školení na kartě **školení experimentovat**, a nově vytvořený prediktivní experimentu je pod **Predictive experimentovat**.

Musíme provést další krok s tohoto určitého experimentu.
Jsme přidali dvě [Provedení skriptu R] [ execute-r-script] moduly váhové funkce dat poskytovat školení a testování. Není třeba provést v konečné modelu.

Machine Learning Studio odebrán jeden [Skript spouštět R] [ execute-r-script] modul odebrán [rozdělení] [ split] modulu. Nyní jsme nyní odebrat druhé a [Metadata Editor] připojení[ metadata-editor] přímo do [Modelu skóre][score-model].    

Náš experiment by měl nyní vypadat takto:  

![Bodování vyškolených modelu.][4]  

> [AZURE.NOTE] Možná se divíte, proč jsme v experimentu prediktivní vlevo UCI německé kreditní karty Data dataset. Služba bude používat data uživatele, nikoli původní dataset, tak proč nechat původní dataset v modelu?
>
>Je pravdou, že služba nemusí původní údaje platební karty. Ale potřebuje schéma pro data, které zahrnují například kolik sloupců jsou a sloupce, které jsou číselné. Informace o schématu je nutné interpretovat data uživatele. Můžeme nechat tyto součásti připojení tak, aby hodnocení modulu Schéma dataset Pokud je služba spuštěna. Data se nepoužívá, pouze schématu.  

Spuštění testu jednou (klepněte na příkaz **Spustit**.) Pokud chcete ověřit, zda je stále funkční model, klepněte na tlačítko výstup [Modelu skóre] [ score-model] modul a vyberte **Zobrazit výsledky**. Uvidíte, že původní data zobrazená hodnota úvěrové riziko ("popisky skóre") a bodování hodnota pravděpodobnosti ("skóre pravděpodobnosti".) 

## <a name="deploy-the-web-service"></a>Nasazení webových služeb

Je možné nasadit experiment jako klasické webové služby nebo nové webové služby založené na Azure Resource Manager.

### <a name="deploy-as-a-classic-web-service"></a>Nasadit jako klasické webové služby ###

Nasazení klasické webové služby odvozené z našeho testu, klepněte na tlačítko **Nasazení webové služby** pod plátno a vyberte **Nasazení webové služby [Classic]**. Machine Learning Studio nasadí experimentu jako webovou službu a přejdete na panel Digital dashboard pro tuto webovou službu. Zde můžete vrátit k experimentu (**zobrazení snímku** nebo **zobrazení nejnovějších**) a spustit jednoduchý test webové služby (viz **Test webové služby** níže). Je také informace pro vytváření aplikací, které přístup k webové službě (podrobnosti o této v dalším kroku tohoto návodu).

![Řídicí panel webové služby][6]

Službu můžete konfigurovat klepnutím na kartu **Konfigurace** . Zde můžete upravit název služby (poskytla název experimentu ve výchozím nastavení) a zadejte jeho popis. Můžete také předat další popisné štítky pro vstupní a výstupní sloupce.  

![Konfiguraci webové služby][5]  

### <a name="deploy-as-a-new-web-service"></a>Nasadit jako nové webové služby

Nasadit nové webové služby odvozené z našeho testu, klepněte na tlačítko **Nasazení webové služby** pod plátno a **Webová služba nasazení [New]**. Machine Learning Studio převede stránku experimentu nasazení Azure Machine Learning webové služby.

Zadejte název pro webovou službu a vybrat plán ocenění. Pokud máte existující plán ocenění, že které lze vybrat, jinak musíte vytvořit nový plán ceny služby. 

1.  V rozevíracím seznamu **Plán cena** vyberte existující plán nebo vyberte možnost **Vybrat nový plán** .
2.  V poli **Název plánu**zadejte název, který identifikuje plán ve vašem vyúčtování.
3.  Vyberte jednu z **Vrstev měsíční plán**. Do této oblasti je nasazena výchozí úrovně plán plány výchozí oblasti a webové služby.

Klepněte na tlačítko **instalovat** a stránce **Quickstart** služby otevře váš Web.

Klepnutím na možnost **Konfigurovat** nabídky lze konfigurovat službu. Zde můžete upravit název služby (poskytla název experimentu ve výchozím nastavení) a zadejte jeho popis. 

Vyberte webové služby klepnutím na možnost **Testovat** nabídky (viz níže uvedené **Testování webové služby** ). Informace o vytváření aplikací, které lze získat přístup k webové služby klepněte na možnost nabídky **spotřebě** (podrobnosti o této v dalším kroku tohoto návodu).

> [AZURE.TIP] Poté, co jste nasazen, můžete aktualizovat webové služby. Například pokud chcete změnit váš model, upravit experimentu školení, nástroje tweak parametry modelu a klepněte na tlačítko **Nasazení webové služby**. Vyberte **Nasazení webové služby [Classic]** nebo **Webová služba nasazení [New]**. Při nasazení experimentu, nahradí webové služby pomocí aktualizovaný model.  

## <a name="test-the-web-service"></a>Testování webové služby

### <a name="test-a-classic-web-service"></a>Vyzkoušejte klasické webové služby

Službu můžete otestovat v Machine Learning Studio nebo na portálu Azure Machine Learning webové služby. Testování v portálu Azure Machine Learning webové služby má tu výhodu, umožňuje získat povolení 

**Testování v Machine Learning Studio**

Na stránce **řídicího PANELU** klepněte na tlačítko **Test** **Výchozí koncový bod**. Dialogové okno bude vyskakující a výzvu k zadání vstupních dat pro službu. Jedná se o stejné sloupce, které se zobrazily v původní dataset německé úvěrové riziko.  

Zadejte sadu dat a potom klepněte na tlačítko **OK**. 

**Test v portálu Azure Machine Learning webové služby**

Na stránce **řídicího PANELU** klepněte na odkaz Náhled **Test** **Výchozí koncový bod**. Zkušební stránku na portálu Azure Machine Learning webové služby pro koncový bod služby Web otevře a výzvou k zadání vstupních dat pro službu. Jedná se o stejné sloupce, které se zobrazily v původní dataset německé úvěrové riziko.

Klepněte na tlačítko **Test požadavek odpověď**. 

Ve webové službě data zadá prostřednictvím modulu **vstupní webové služby** pomocí [Editoru metadat] [ metadata-editor] modul a [Skóre Model] [ score-model] modulu, kde je skóre. Výsledky jsou ve výstupu z webové služby prostřednictvím **webové služby výstupní**.

**Vyzkoušejte nové webové služby**

V portálu Azure Machine Learning webové služby klepněte na tlačítko **Test** v horní části stránky. **Zkušební** stránka se otevře a lze vstupní data pro službu. Zobrazí vstupní pole odpovídají sloupců, které se objevily v původní dataset německé úvěrové riziko. 

Sady dat a klepněte na tlačítko **Test požadavek-odpověď**.

Výsledky testu se zobrazí na pravé straně stránky ve sloupci výstup. 

Při testování v portálu Azure Machine Learning webové služby, můžete povolit ukázková data, která slouží k testování služby požadavek odpověď. Pokud jste vytvořili webovou službu Machine Learning Studio, ukázkových dat je převzat z dat váš použitý model vlaku.

> [AZURE.TIP] Tak máme prediktivní experimentu nakonfigurován, celý výsledkem [Skóre Model] [ score-model] modulu jsou vráceny. Jedná se o vstupní data plus hodnota úvěrové riziko a hodnocení pravděpodobnosti. Pokud byste se chtěli vrátit něco jiného - například pouze kreditní rizika hodnota - pak jste mohli vložit [Sloupce projektu] [ project-columns] modul mezi [Skóre Model] [ score-model] a odstranit sloupce, které nechcete, aby webové služby vrátit **výstupní webové služby** . 

## <a name="manage-the-web-service"></a>Spravovat webové služby

**Správa klasické webové služby**

Jakmile jste nasadili klasické webové služby, můžete jej spravovat z [portálu Azure klasické](https://manage.windowsazure.com).

1. Přihlášení k [portálu Azure klasické](https://manage.windowsazure.com).
2. V panelu služby Microsoft Azure klikněte na **STROJOVÉM učení**.
3. Klepněte na tlačítko Nastavení pracovního prostoru.
4. Klepněte na kartu **webové služby** .
5. Klepněte na webovou službu, kterou jsme vytvořili.
6. Klepněte na koncový bod "Výchozí".

Zde můžete provést akce, jako je sledovat, jak se dělá webové služby a zpřístupnit vylepšení výkonu změnou počtu souběžných volání služby dokáže zpracovat.
Webovou službu můžete publikovat i v Azure Marketplace.

Další informace naleznete v následujících tématech:

- [Vytváření koncových bodů](machine-learning-create-endpoint.md)
- [Škálování webových služeb](machine-learning-scaling-webservice.md)
- [Azure Machine Learning webové služby publikování na webu Azure Marketplace](machine-learning-publish-web-service-to-azure-marketplace.md)

**Spravovat webové služby v portálu Azure Machine Learning webové služby**

Jakmile jste nasazení webové služby, Classic nebo nové, můžete jej spravovat z [portálu Azure Machine Learning webové služby](https://servics.azureml.net).

Sledování výkonu webové služby:

1. Přihlášení k [portálu Azure Machine Learning webové služby](https://servics.azureml.net).
2. Klepněte na tlačítko **webové služby**.
3. Klepněte na tlačítko webové služby.
4. Klepněte na tlačítko **řídicí panel**.

----------

**Dále: [přístup k webové službě](machine-learning-walkthrough-6-access-web-service.md)**

[1]: ./media/machine-learning-walkthrough-5-publish-web-service/publish1.png
[2]: ./media/machine-learning-walkthrough-5-publish-web-service/publish2.png
[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/