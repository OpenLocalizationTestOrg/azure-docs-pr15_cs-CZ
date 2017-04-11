<properties
    pageTitle="Krok 6: Přístup k webové službě výukové počítači | Microsoft Azure"
    description="Krok 6 vývoje prediktivní řešení návod: přístup k webové služby active výukové Azure počítače."
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
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Návod krok 6: Přístup k webové službě výukové počítače Azure

Jedná se o poslední krok návodu [vývoje řešení prediktivní analýzy Azure počítač přečíst](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Vytvoření pracovního prostoru aplikace Groove výukové počítače](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Nahrát existující data](machine-learning-walkthrough-2-upload-data.md)
3.  [Vytvoření nového testu](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Školení a vyhodnocení modely](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Nasazení webové služby](machine-learning-walkthrough-5-publish-web-service.md)
6.  **Přístup k webové službě**

----------

V předchozím kroku v tomto návodu jsme nasazené webové služby, který používá našeho platební rizika předpovědí modelu. Teď uživatelé musí mít možnost odeslat data a získat výsledky. 

Webová služba je Azure webové služby, které můžete zobrazit a vrátit data pomocí rozhraní REST API jedním ze dvou způsobů:  

-   **Žádostí a odpovědí** – uživatel odešle jeden nebo více řádků dat platební ke službě pomocí protokolu HTTP a službě odpoví sadami jeden nebo více výsledků.
-   **Spuštění dávky** – uživatel ukládá jeden nebo více řádků dat platební objektů blob Azure a potom odešle umístění objektů blob služby. Služba získává všechny řádky z data ve vstupní objektů blob, výsledek uloží do jiného objektů blob a vrátí adresu URL, které obsahují.  

Nejrychlejší a nejsnadnější způsob přístup k webové službě je prostřednictvím webové šablony aplikací dostupné v [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).
Tyto webové aplikace šablony můžete vytvořit vlastní webové aplikace, které zná vstupních dat a co se vrátí webové služby. Je třeba udělat stačí poskytnutí přístupu ke webové službě a daty a šablona bude zbývající.

Další informace o použití šablony web app najdete v článku [spotřebě webové služby Azure počítače učení pomocí šablony webové aplikace](machine-learning-consume-web-service-with-web-app-template.md).

Taky můžete vyvinout vlastní aplikace pro přístup k webové služby pomocí kódu starter podle můžete R, C# a Python jazyky.
Úplné podrobnosti najdete v tématu [jak používat webové služby Azure počítače výuka, která byla publikovaná z počítače výukové pokus](machine-learning-consume-web-services.md).
