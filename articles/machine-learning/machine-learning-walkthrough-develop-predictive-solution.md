<properties
    pageTitle="Prognostické vyřešíte platební rizika pomocí počítače výukové | Microsoft Azure"
    description="Podrobné informace s ukázkou vytvoření prediktivní analýzy vyřešíte platební rizik v Azure počítače výukové Studio."
    keywords="kreditní rizika, prediktivní analýzy řešení, rizik"
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
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Návod: Vyvíjet řešení prediktivní analýzy platební rizik Azure počítač přečíst

Předpokládejme, že budete muset předpovídání rizika platební jednotlivého uživatele na základě informací, které poskytují na platební aplikace.  

Kreditní rizik k potížím s složité, samozřejmě, ale Pojďme trochu zjednodušit parametry otázku. Potom jsme ho použít jako příklad, jak používat Microsoft Azure počítače Learning se Studio výukové počítače a webové služby strojového výukové vytvořit prediktivní analýzy řešení.  

V tomto návodu podrobné jsme budete pokračovat další vývoje modelu prediktivní analýzy ve počítače výukové studiu a nasazení ho jako webové služby Azure počítače výukové. Jsme budete začít s daty veřejně dostupný úvěr rizika, vývoj školení prediktivní model založený na tato data a nasazení modelu jako webové služby, které může používat ostatní pro platební rizik.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- -->

>[AZURE.TIP] Stáhnout a vytisknout diagram, který obsahuje přehled funkcí aplikace počítače výukové Studio, najdete v článku [Přehled některých dalších funkcí Azure počítače výukové Studio možnosti](machine-learning-studio-overview-diagram.md).

Řešení hodnocení rizik platební, jsme budete vytvoříte takto:  

1.  [Vytvoření pracovního prostoru aplikace Groove výukové počítače](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Nahrát existující data](machine-learning-walkthrough-2-upload-data.md)
3.  [Vytvoření nového testu](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Školení a vyhodnocení modely](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Přístup k webové službě](machine-learning-walkthrough-6-access-web-service.md)

Tohoto návodu je založená na zjednodušenou verzí [binární zařazení: kreditní rizika předpovědí](http://go.microsoft.com/fwlink/?LinkID=525270) experiment vzorku v [Galerii Intelligence Cortana](http://gallery.cortanaintelligence.com/).
