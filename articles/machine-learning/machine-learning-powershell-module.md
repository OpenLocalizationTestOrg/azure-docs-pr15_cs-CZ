<properties
    pageTitle="Modul Powershellu pro počítač výukové | Microsoft Azure"
    description="Modul Powershellu pro Azure počítače výukové je k dispozici v režimu veřejné náhledu. Pomocí prostředí PowerShell můžete vytvořit a spravovat pracovní prostory, pokusy, webovým službám a další."
    keywords="Vyzkoušejte lineární regrese algoritmů výukové počítače, počítače výukového kurzu, prediktivní modelování technik experiment vědy dat"
    services="machine-learning"
    documentationCenter=""
    authors="hning86"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="garye;haining"/>

# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>Modul Powershellu pro Microsoft Azure počítače výuka

Modul Powershellu pro Azure počítače výukové je výkonný nástroj, který umožňuje používat Windows PowerShell ke správě pracovní prostory, pokusy, datové sady, web serivces a další.

Můžete zobrazit v dokumentaci a stažení modulu spolu s celou zdrojového kódu v [https://aka.ms/amlps](https://aka.ms/amlps). 

## <a name="what-is-the-machine-learning-powershell-module"></a>Co je modulu PowerShell výukové počítače?

Modul Powershellu výukové počítače. Na základě čistého DLL moduly, které umožňuje plně spravovat Azure počítače výukové pracovní prostory pokusy, datové sady, webovým službám a koncové body webové služby z prostředí Windows PowerShell. Spolu s modulu můžete si stáhnout celou zdrojového kódu, která zahrnuje předchozího oddělené [vrstvy C# API](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). To znamená odkazovat tuto knihovnu DLL z .NET projektu a spravovat Azure počítače učení pomocí kódu .NET. Kromě toho DLL závisí na podkladového ZBÝVAJÍCÍ rozhraní API můžete využít přímo z vašeho Oblíbené klienta.

## <a name="what-can-i-do-with-the-powershell-module"></a>Co můžete dělat s modulu PowerShell?

Tady jsou některé úkoly, které lze provádět s Tento modul Powershellu. Podívejte se na [úplný si přečtěte následující dokumentaci](https://aka.ms/amlps) pro tyto a mnoho dalších funkcí.

- Zřízení nového pracovního prostoru pomocí certifikátu management ([Nový AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
- Export a import souboru JSON představující experiment grafu ([Export AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) a [Import AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
- Spuštění pokus ([Start AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
- Vytvoření webové služby mimo prediktivní experiment ([Nový AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
- Vytvoření nového koncového bodu na publikované webové služby ([Přidat AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
- Vyvolání záznamy a/nebo BES webové služby koncového ([Vyvolat AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) a [Vyvolat AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Tady je rychlý příklad: použití Powershellu ke spuštění pokus existující:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Důkladněji případu použití, naleznete v článku o používání modulu PowerShell k automatizaci velmi požadované úkol: [vytvořit mnoho modely výukové počítače a webové služby koncové body z pokus pomocí Powershellu](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Jak mám začít?

Začínáme s prostředím PowerShell výukové počítače, stahování [uvolněte balíčku](https://github.com/hning86/azuremlps/releases) GitHub a postupujte podle [pokynů k instalaci](https://github.com/hning86/azuremlps/blob/master/README.md). Budete muset odblokování stáhli rozbaleny DLL a importujte ho na prostředí PowerShell. Většina rutin je nutné zadat ID pracovního prostoru, token se tak mohli ověřovat pracovní prostor a Azure oblast, která pracovního prostoru v. Nejjednodušší způsob, jak poskytnout tyto je prostřednictvím výchozí config.json soubor, který se vztahuje podrobně pokyny k instalaci. Samozřejmě můžete taky klonovat stromu libovolná a změnit/kompilace kódu místně pomocí aplikace Visual Studio.

## <a name="next-steps"></a>Další kroky

Modul Powershellu zůstanou lepší a rozbalené během tohoto období náhled. Sledujte na [Cortana měřítka a počítač výukové blogu](https://blogs.technet.microsoft.com/machinelearning/) Další informace a informace.
