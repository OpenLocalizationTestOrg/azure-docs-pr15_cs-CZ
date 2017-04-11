<properties
    pageTitle="Retrain počítače výukové modely programově | Microsoft Azure"
    description="Zjistěte, jak programově retrain modelu a aktualizovat webové služby na používání nově školení modelu Azure počítač přečíst."
    services="machine-learning"
    documentationCenter=""
    authors="raymondlaghaeian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="raymondl;garye;v-donglo"/>


# <a name="retrain-machine-learning-models-programmatically"></a>Programově Retrain modely výukové počítače  

V tomto návodu se dozvíte, jak se programově přeškolit Azure počítače výukové webové služby pomocí C# a služby strojového výukové dávku spuštění.

Jakmile máte retrained modelu, v následujících seznamech předvedení k aktualizaci modelu v prediktivní webové služby:

- Pokud jste nainstalovali klasické webové služby na portálu počítače výukové webové služby, najdete v článku [Retrain klasické webové služby](machine-learning-retrain-a-classic-web-service.md). 
- Pokud jste nainstalovali nové webové služby, najdete v článku [Retrain nové webové služby pomocí rutiny pro správu výukové počítače](machine-learning-retrain-new-web-service-using-powershell.md).

Přehled procesu rekvalifikaci najdete v článku [Retrain modelu výukové počítače](machine-learning-retrain-machine-learning-model.md).

Pokud chcete začít s existující webové služby nové správce prostředků Azure na základě, najdete v článku [Retrain existující prediktivní webové služby](machine-learning-retrain-existing-resource-manager-based-web-service.md).

## <a name="create-a-training-experiment"></a>Vytvoření experiment školení
 
V tomto příkladu, ale použijete "vzorku 5: vlaku, testu vyhodnotit pro binární klasifikace: dospělého datovou sadu" z webu Microsoft Azure počítače Learning vzorků. 
    
Vytvoření testu:

1.  Přihlaste se do na Microsoft Azure počítače výukové Studio. 
2.  V pravém dolním rohu na řídicím panelu klikněte na **Nový**.
3.  Z Microsoft Samples vyberte ukázkový 5.
4.  Přejmenování testu v horní části plátno experiment vyberte název experiment "vzorku 5: vlaku, testu vyhodnotit pro binární klasifikace: dospělého datovou sadu".
5.  Typ sčítání modelu.
6.  V dolní části testu plátna, klikněte na příkaz **Spustit**.
7.  Klikněte na **nastavení webové služby** a vyberte **Retraining webové služby**. 

    ![Počáteční testu.][2]

Diagram 2: Počáteční testu.

## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a>Vytvoření prediktivní testu a publikovat jako webové služby  

Další vytvoříte Predicative testu.

1.  V dolní části plátno testu klikněte na **Nastavení webové služby** a vyberte **Prediktivní webové služby**. Uloží do modelu jako modelu školení a přidá webové služby vstupní a výstupní moduly. 
2.  Klikněte na **Spustit**. 
3.  Po testu dokončení, klikněte na **Nasazení webová služba [klasické]** nebo **Nasadit [New] webové služby**.

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a>Nasazení experiment školení jako školení webové služby

Se přeškolit školení modelu, je nutné nasadit testu školení, který jste vytvořili jako Retraining webové služby. Tato webová služba, musí modulu *Webové služby výstup* připojené k * [Vlaku modelu] [ train-model] * modul, abyste mohli vytvářet nové školení modely.

1. Vraťte se do testu školení, klikněte na ikonu pokusů v levém podokně a potom klikněte na experiment s názvem sčítání modelu.  
2. Do pole Hledat položky Experiment hledání zadejte webové služby. 
3. Přetáhněte modulu *Vstupní webové služby* na plátno testu a připojte jeho výstup modulu *Vyčistit chybějící Data* .  Zajistíte tím, že rekvalifikaci dat zpracovávají stejným způsobem jako původní data školení.
4. Přetáhněte dva moduly *Webová služba výstup* na plátno testu. Výstup modulu *Vlaku modelu* připojení k jedné a výstup modulu *Vyhodnocení modelu* k druhému. Výstup webové služby **Vlaku modelu** dostaneme nový školení model. Připojené k **Vyhodnocení modelu** vrátí tento modul výstup, což je výsledky.
5. Klikněte na **Spustit**. 

Další zaveďte experiment školení jako webová služba vypočítávající modelu školení a model hodnocení výsledků. K tomu vaše další akce jsou závisí na tom, jestli pracujete s webové služby Classic nebo nové webové služby.  
  
**Klasický webové služby**

V dolní části plátno testu klikněte na **Nastavení webové služby** a vyberte **Nasazení webová služba [klasické]**. Webová služba **řídicích panelů** se zobrazí se klávesu rozhraní API a na stránce nápovědy rozhraní API pro spuštění dávky. Pouze spuštění dávky metoda slouží k vytváření školení modely.

**Nové webové služby**

V dolní části plátno testu klikněte na **Nastavení webové služby** a vyberte **Nasazení [New] webové služby**. Portál webové služby Azure počítače výukové webové služby se otevře stránka nasazení webové služby. Zadejte název pro webové služby a zvolte plán plateb a pak klikněte na **nasazení**. Pouze spuštění dávky metoda slouží k vytváření školení modely

V obou případech po dokončení testu spuštěné výsledné pracovního postupu by měl vypadat takto:

![Výsledný pracovní postup pro po spuštění.][4]

Diagram 3: Výsledné po spuštění pracovního postupu.

## <a name="retrain-the-model-with-new-data-using-bes"></a>Retrain modelu pomocí nových dat pomocí BES

V tomto příkladu používáte C# vytvořit rekvalifikaci aplikaci. Můžete také ukázkové kód Python nebo R k provedení tohoto úkolu.

Chcete-li zavolat rozhraní API přeškolení:

1. Vytvoření aplikace konzoly C# ve Visual Studiu (nové -> projektu -> Windows Desktop -> aplikace konzoly).
2.  Přihlaste se k portálu počítače výukové webové služby.
3.  Pokud pracujete s klasické webové služby, klikněte na **Klasické webové služby**.
    1.  Klikněte na webové služby, kterou pracujete.
    2.  Klikněte na výchozí koncový bod.
    3.  Klikněte na **používat**.
    4.  V dolní části stránky **spotřebě** , v části **Ukázka kód** klikněte na **dávku**.
    5.  Pokračujte krokem 5 tohoto postupu.
4.  Pokud pracujete s novou webové služby, klikněte na **Webové služby**.
    1.  Klikněte na webové služby, kterou pracujete.
    2.  Klikněte na **používat**.
    3.  V dolní části stránky spotřebě, v části **Ukázka kód** klikněte na **dávku**.
5.  Zkopírujte ukázková C# kód pro spuštění dávky a vložit ho do souboru Program.cs, jak zajistit, že oboru zůstane nedotčený.

Přidáte do Nuget Microsoft.AspNet.WebApi.Client určené v komentářích. Pokud chcete přidat odkaz na Microsoft.WindowsAzure.Storage.dll, může musíte nejdřív nainstalovat knihovnu klienta služby Microsoft Azure úložiště. Další informace najdete v tématu [Úložiště služby](https://www.nuget.org/packages/WindowsAzure.Storage).

### <a name="update-the-apikey-declaration"></a>Aktualizace apikey deklarace

Vyhledejte **apikey** deklarace.

    const string apiKey = "abc123"; // Replace this with the API key for the web service

V části **základní spotřeby informace** stránce **spotřebě** vyhledejte primární klíč a jeho zkopírování do deklaraci **apikey** .

### <a name="update-the-azure-storage-information"></a>Aktualizovat informace o úložišti Azure

Ukázkový kód BES odeslání souboru z místní disk (například "C:\temp\CensusIpnput.csv") k základnímu úložišti Azure, zpracuje ji a data zapisuje výsledků zpět k základnímu úložišti Azure.  

K provedení této úlohy musíte získat úložiště název, klíče a kontejneru informací o účtu pro váš účet úložiště z portálu klasické Azure a aktualizace odpovídajících hodnot v kódu. 

1. Přihlaste se k portálu klasické Azure.
1. Ve sloupci levém navigačním panelu klikněte na **úložiště**.
1. Ze seznamu úložiště účtů vyberte skupinu pro ukládání retrained modelu.
1. V dolní části stránky klikněte na **Správa přístupových kláves**.
1. Zkopírujte a **Primární klíč přístup** uložit a zavřete dialogové okno. 
1. V horní části stránky klikněte na **kontejnery**.
1. Vyberte kontejneru stávající nebo vytvořte nový účet a uložte název.

Vyhledejte deklarace *StorageAccountName* *StorageAccountKey*a *StorageContainerName* a aktualizovat hodnoty, které jste uložili z portálu Microsoft Azure.

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name
            
Taky musíte se ujistit, že vstupní soubor je k dispozici v umístění, do kterého můžete zadat kód. 

### <a name="specify-the-output-location"></a>Zadejte umístění výstupu

Při určování umístění výstupu v datové požádat o koncovku souboru zadaného v *RelativeLocation* musí být zadán jako ilearner. 

Naleznete v následujícím příkladu:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

>[AZURE.NOTE] Názvy umístění výstupu se může lišit od těch, které jsou v tomto návodu založená na pořadí, ve které jste přidali moduly výstup webové služby. Protože nastavení tento kurz experimentovat s dvěma výstupy výsledky informacemi úložiště umístění pro oba.  

![Přeškolení výstup][6]

Diagram 4: Přeškolení výstupu.

## <a name="evaluate-the-retraining-results"></a>Vyhodnocení rekvalifikaci výsledků
 
Při spuštění aplikace zahrnuje adresy URL a přidružení zabezpečení tokenu potřebné pro přístup k výsledků hodnocení.

Uvidíte výsledky retrained modelu součtem *BaseLocation*, *RelativeLocation*a *SasBlobToken* ve výsledcích výstup pro *output2* (jak ukazuje předchozí obrázek rekvalifikaci výstup) a vložte úplnou adresu URL v adresním řádku prohlížeče.  

Zkontrolujte výsledky a zjistit, zda nově školení modelu provádí dostatečném nahradit stávající.

Zkopírujte *BaseLocation*, *RelativeLocation*a *SasBlobToken* ve výsledcích výstup, ale použijete během procesu rekvalifikaci.

## <a name="next-steps"></a>Další kroky

Pokud jste nainstalovali prediktivní webová služba po kliknutí na **Nasazení webová služba [klasické]**, najdete v článku [Retrain klasické webové služby](machine-learning-retrain-a-classic-web-service.md).

Pokud jste nainstalovali prediktivní webová služba po kliknutí na **Nasazení [New] webové služby**, najdete v článku [Retrain nové webové služby pomocí rutiny pro správu výukové počítače](machine-learning-retrain-new-web-service-using-powershell.md).

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/