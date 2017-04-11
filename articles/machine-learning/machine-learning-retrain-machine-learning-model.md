<properties
    pageTitle="Retrain počítače výukové modelu | Microsoft Azure"
    description="Zjistěte, jak retrain modelu a aktualizovat webové služby na používání nově školení modelu Azure počítač přečíst."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="v-donglo"/>

# <a name="retrain-a-machine-learning-model"></a>Retrain počítače výukové modelu

Jako součást procesu operationalization počítače výukové modelů Azure počítače dozvědět modelu školení a uložili. Potom ji použijete k vytvoření predicative webové služby. Webová služba může být potom používán v weby, řídicích panelů a mobilní aplikace. 

Obvykle nejsou statické modely, vytvořené pomocí počítače výukové. Nová data k dispozici nebo pokud spotř rozhraní API má svá data do modelu, musí být retrained. 

Přeškolení může dojít, často. Pomocí funkce programové rozhraní API přeškolení můžete programově retrain modelu pomocí rozhraní API rekvalifikace a aktualizovat webové služby pomocí nově školení modelu. 

Tento dokument popisuje rekvalifikaci obrázku a se dozvíte, jak používat rozhraní API přeškolení.

## <a name="why-retrain-defining-the-problem"></a>Proč retrain: definování problému  

V rámci počítače výukové proces školení je modelu školení pomocí množiny dat. Obvykle nejsou statické modely, vytvořené pomocí počítače výukové. Nová data k dispozici nebo pokud spotř rozhraní API má svá data do modelu, musí být retrained.

V těchto případech programové rozhraní API poskytuje pohodlný způsob, jak povolit můžete nebo spotř vaše rozhraní API vytvořit klienta, můžete na základě jednorázové nebo běžná retrain modelu pomocí svá data. Potom mohou vyhodnocení výsledků rekvalifikace a aktualizovat rozhraní API webových služeb použití nově školení modelu.

>[AZURE.NOTE] Pokud máte existující Experiment školení a nové webové služby, je vhodné najdete v článku obsloužených existující prediktivní webové služby místo následujících kroků uvedených v následující části.

## <a name="end-to-end-workflow"></a>Pracovní postup začátku do konce 

Proces zahrnuje následující součásti: Pokus A školení a prediktivní Experiment publikován jako webové služby. Povolit dalším vzdělávání modelu školení, musí být Experiment školení publikován jako webové služby pomocí výstupu školení modelu. Díky rozhraní API přístup do modelu přeškolení. 

Následující postup platí pro nové a klasické webové služby:

Vytvoření počáteční prediktivní webové služby:

* Vytvoření experiment školení
* Vytvoření experiment prediktivní web
* Nasazení prediktivní webové služby

Retrain webové služby:

* Aktualizace školení experiment umožňující přeškolení
* Nasazení rekvalifikaci webové služby
* Použití kód služby spuštění dávky se přeškolit modelu

Návod předchozích kroků najdete v článku [Retrain výukové počítače modely programově](machine-learning-retrain-models-programmatically.md).

Pokud jste nainstalovali klasické webové služby:

* Vytvoření nového koncového bodu na prediktivní webové služby
* Oprava adresy URL a kód
* Použití adresy URL oprava tak, aby ukazovaly na nové koncový bod na retrained modelu 

Návod předchozích kroků najdete v článku [Retrain klasické webové služby](machine-learning-retrain-a-classic-web-service.md).

Pokud narazíte na problémy přeškolení klasické webové služby, přečtěte si článek [Poradce při potížích s dalším vzdělávání Azure počítače výukové klasické webové služby](machine-learning-troubleshooting-retraining-models.md).

Pokud jste nainstalovali nové webové služby:

* Přihlaste se ke svému účtu správce prostředků Azure
* Získání definice webové služby
* Exportovat jako JSON definice webové služby
* Odkaz na aktualizaci `ilearner` objektů blob v ve formátu JSON
* Naimportujte ve formátu JSON definici webové služby
* Aktualizovat webové služby s novou definici webové služby

Návod předchozích kroků najdete v článku [Retrain nové webové služby pomocí rutin prostředí Management výukové počítače](machine-learning-retrain-new-web-service-using-powershell.md).

Postup pro nastavení přeškolení klasické webové služby zahrnuje tyto kroky:

![Přeškolení přehled procesu][1]

Postup pro nastavení rekvalifikace pro nové webové služby zahrnuje tyto kroky:

![Přeškolení přehled procesu][7]

## <a name="other-resources"></a>Další zdroje

- [Modely rekvalifikace a aktualizace výuka Azure počítače s Azure Data Factory](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
- [Vytvoření mnoho modely výukové počítače a web koncové body služby z pokus pomocí prostředí PowerShell](machine-learning-create-models-and-endpoints-with-powershell.md)
- [AML přeškolení modely pomocí rozhraní API](https://www.youtube.com/watch?v=wwjglA8xllg) video ukazuje, jak retrain počítače výukové modelů vytvořených v Azure počítače výukové pomocí rozhraní API rekvalifikace a Powershellu.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

