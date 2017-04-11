<properties
    pageTitle="Vytvoření modelu s uživatelským rozhraním Recommnendations | Microsoft Azure"
    description="Azure doporučení výukové počítače – vytvoření modelu s uživatelským rozhraním doporučení"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="luisca"/>

# <a name="building-a-model-with-the-recommendations-ui"></a>Vytvoření modelu s uživatelským rozhraním doporučení

Tento dokument je podrobného průvodce. Naším cílem je pro vás provede jednotlivými kroky potřebné k vlaku modelu pomocí [Doporučení uživatelského rozhraní](https://recommendations-portal.azurewebsites.net/).
Procházení výkon umožňuje porozumět procesu pro vytváření modelu dříve, než jste ho programově. Je taky vás seznámí s uživatelským rozhraním, což je užitečné při spuštění pomocí služby.

Tento cvičení trvá asi 30 minut.

<a name="Step1"></a>
## <a name="step-1---sign-in-to-the-recommendations-ui"></a>Krok 1: znak v doporučení uživatelského rozhraní ##

1. Pokud jste to ještě neudělali, budete muset [Registrace](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) pro nové předplatné [Rozhraní API doporučení](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) . Pro službu na **Azure** na [http://portal.azure.com/](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) a přihlaste se můžete přihlásit pomocí účtu Azure. Podrobné informace o procesu registrace jsou k dispozici v *úkolu 1* [Příručky Začínáme](cognitive-services-recommendations-quick-start.md) 

1. Už jste obdrželi **klíč** pro vaše předplatné rozhraní API doporučení, přejděte do [Uživatelského rozhraní doporučení](https://recommendations-portal.azurewebsites.net/). 

1. Přihlaste se k portálu tak, že zadáte kód do pole **Klíč účtu** a potom klikněte na tlačítko **přihlášení** .

    ![Doporučení uživatelského rozhraní: Přihlašovací dialogové okno.][reco_signin]


<a name="Step2"></a>
## <a name="step-2---lets-gather-some-training-data"></a>Krok 2 – Pojďme shromáždění některá data školení ##

Než budete moct vytvářet sestavení, modul potřebuje dvěma datovými informací: souboru katalogu a sadu použití souborů. 

Soubor katalogu obsahuje informace o položkách, že budete nabízet zákazníkovi. Použití soubor obsahuje informace o použití těchto položek nebo transakce z vašeho podniku.

Obvykle byste měli dotaz databáze úložiště k těchto informací. V současné době uvádíme ukázkových dat pro vás tak, aby informace o použití rozhraní API doporučení.

Stažení dat ze [http://aka.ms/RecoSampleData](http://aka.ms/RecoSampleData). Kopírování a rozbalit **Books.Zip** souboru do jiné složky na místním počítači. Například **c:\data**.

Podrobné informace o schématu katalogu a použití souborů najdete v článku [Operace shromažďování dat jak proškolit modelu](cognitive-services-recommendations-collecting-data.md) .
 
U tohoto cvičení budeme pro práci s malé soubory, abyste pokaždé nemuseli velmi čekat příliš dlouho školení. Pokud chcete zkusit běžného souboru, jsme také jste umístili **MsStoreData.zip** obsahující ukázkové transakce z Microsoft Store ve [stejném umístění](http://aka.ms/RecoSampleData).

<a name="Step3"></a>
## <a name="step-3---create-a-project-and-upload-catalog-and-usage-data"></a>Krok 3: vytvoření projektu a nahrajte katalogu a použití dat ##

Po přihlášení do [Uživatelského rozhraní doporučení](https://recommendations-portal.azurewebsites.net/), zobrazí se stránka projekty. Pokud jste dříve vytvořili projekty, měli byste vidět je tady.
Projekt (označovaná taky jako *modelu* odkazu rozhraní API) je kontejner pro vaše data katalogu a použití. Můžete vytvořit několik *vytvoří* do projektu. Nemůžeme vás provede procesem v dalším krokům.

1. Vytvoření nového projektu, že zadáte název na textové pole (něco jako vhodná "MyFirstModel") a klikněte na **Přidat projekt**.
 
    ![Doporučení uživatelského rozhraní: Stránka projekty.][reco_projects]

1. Po vytvoření projektu získá kliknutím na tlačítko **vyhledat soubor** v části **Přidat soubor katalogu** . Nahrajte katalog, který jste získali v kroku 2. Pokud jste ho uložili na *c:\data*, budete muset přejít do této složky.

    ![Doporučení uživatelského rozhraní: Přidání dat do projektu.][reco_firstmodel]

1. Po odeslání katalogu kliknutím na tlačítko **vyhledat soubor** v části **Přidat soubory použití** . Přidání souboru usage_large.txt.

> **Co když používám velké soubory použití zásad správy informací?**
>
> Soubory jenom využití menší než 200 MB je možné uložit. Však systému můžou obsahovat na 2 GB zboží v hodnotě data transakce, tak v případě potřeby můžete nahrát víc než jeden soubor.
> Nemusí být nutné to zpracovávaných dat ke generování správný model, není právě velikosti data, která otázek, ale kvalitu data. Je běžné najdete v článku použití dat, kde jsou většina transakce jenom na hrstku Nejoblíbenější položky a u jiných položek je "trochu signál".

<a name="Step4"></a>
## <a name="step-4---lets-do-some-training"></a>Krok 4 – Pojďme proveďte některé školení! ##

Teď jste nahráli katalogu a použití dat, jsme připravení k školení modul tak, aby se dozvíte vzorků z našich dat.

1.  Klikněte na tlačítko **Vytvořit nový** .

1.  **Doporučení** vyberte jako typ Tvůrce dotazů. Všimněte si, že podporujeme řazení vytvářet a FBT (často koupili společně) vytvořit také typy.

    ![Doporučení uživatelského rozhraní: Vytvoření dialogového okna.][reco_build_dialog.png]


    Sestavení FBT umožňuje určit vzorce pro produkty, které jsou obvykle koupili/spotřebované množství ve stejné transakci.
    Sestavení hodnocení se používá k identifikaci funkce zájmu. 
    Jsme nebude obsahují velmi tmavě FBT nebo hodnocení vytvoří v dílně, ale pokud vás zajímá by měl podívejte se na [Vytvoření typů a modelu kvality si přečtěte následující dokumentaci stránky](cognitive-services-recommendations-buildtypes.md).

1. Klikněte na tlačítko **vytvořit** . Proces vytváření trvá asi pět minut, pokud používáte knihy data. Trvá déle na větší datové sady.

<a name="Step5"></a>
## <a name="step-5---lets-find-out-what-the-machine-learned"></a>Krok 5 – Pojďme zjistit, co počítač naučili! ##

Po dokončení vaše sestavení zjistíte nové sestavit v seznamu sestavení. Toto sestavení jde zpracovávat doporučení zboží a uživatele.

1. Po dokončení vaše sestavení klikněte na **skóre**. Umožňuje přehrávat s modelem a zobrazit položky, které jsou vám doporučené.

    ![Doporučení uživatelského rozhraní: Tlačítko skóre][reco_score_button]

1. Vyberte položku, kterou chcete zobrazit položky, které jsou vráceny jako doporučení pro danou položku. Všimněte si, že pokud není dost transakce odhadnout doporučení pro konkrétní položku, systému nevrátí doporučení pro danou položku.  Pokud z nějakého důvodu položky, která vrací žádná doporučení, zkuste bodování ostatními položkami.

<a name="Step6"></a>
## <a name="step-6---next-steps"></a>Krok 6 – Další kroky ##
Blahopřejeme! Máte školení modelu a potom dostali doporučení od tento model.  Uživatelské rozhraní doporučení je užitečné nástroj, který vám umožní zobrazit stav vašich projektů a vytvoří. 

Teď, když máte modelu, je vhodné zjistěte, jak provádět všechny výše uvedené kroky programově. Seznamte se zavolat programové rozhraní API, pomozte zkontrolujte [Doporučení rozhraní API odkaz](http://go.microsoft.com/fwlink/?LinkId=759348) a [Doporučení ukázková aplikace](http://go.microsoft.com/fwlink/?LinkID=759344)stáhnout.


[reco_signin]:../media/cognitive-services/reco_signin.PNG
[reco_projects]:../media/cognitive-services/reco_projects.PNG
[reco_firstmodel]:../media/cognitive-services/reco_firstmodel.png
[reco_build_dialog.png]:../media/cognitive-services/reco_build_dialog.png
[reco_score_button]:../media/cognitive-services/reco_score_button.png
