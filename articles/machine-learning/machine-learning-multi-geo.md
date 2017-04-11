<properties
   pageTitle="Pomoc s víc Geo si přečtěte následující dokumentaci | Microsoft Azure"
   description="Naučte se vytvořit pracovní prostor a publikovat webové služby Azure oblasti odlišný od jih centrální USA (SCUS) Azure oblast."
   services="machine-learning"
   documentationCenter=""
   authors="tedway"
   manager="jhubbard"
   editor="rmca14"
   tags=""/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="tedway; neerajkh"/>

# <a name="multi-geo-help-documentation"></a>Více Geo nápovědě

Tento článek popisuje, jak můžete vytvořit pracovní prostor a publikovat webové služby v ostatních oblastech Azure.

## <a name="create-a-workspace"></a>Vytvoření pracovního prostoru aplikace Groove

1. Přihlaste se k portálu Azure klasické.

2.  Klikněte na **+ Nový** > **datové služby** > **počítače VÝUKOVÉ** > **vytvořit**.  V části **umístění** vyberte jiné oblasti, například **jihovýchodní Asie**.
![Pomoc s víc Geo obrázek 1][1]
3. Vyberte příslušný pracovní prostor a klikněte na **Přihlásit se ML Studio**.
![Pomoc s víc Geo obrázek 2][2]

4. Nyní máte pracovního prostoru aplikace Groove v jiné oblasti, které bude možné používat stejně jako jakékoli jiné pracovního prostoru. Pokud chcete přepnout mezi pracovních prostorů, podívejte se na pravém horním rohu obrazovky. Klikněte na šipku rozevíracího seznamu, vyberte požadovanou oblast a vyberte pracovního prostoru. Všechno, co je místní k oblasti pracovního prostoru; všechny webové služby vytvořené z pracovního prostoru aplikace Groove například budou ve stejné oblasti, který je součástí pracovního prostoru.
![Pomoc s víc Geo obrázek 3][3]

## <a name="open-an-experiment-from-gallery"></a>Otevřete pokus z Galerie

Pokud jste otevřeli pokus z galerie, můžete také vybrat které oblasti chcete zkopírovat pokus.

![Pomoc s víc Geo obrázek 4][4a]

## <a name="web-service-management"></a>Správa webové služby

Programově spravovat webové služby, jako je třeba pro rekvalifikace, použijte adresu příslušnou oblast:
- https://asiasoutheast.Management.azureml.NET
- https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Co je potřeba pamatovat

1.  Můžete kopírovat pouze pokusy mezi pracovní prostory, které patří do stejné oblasti tímto způsobem. Pokud potřebujete zkopírovat experiment přes pracovní prostory v různých oblastech, můžete [prostředí](http://aka.ms/amlps) PowerShell [*AmlExperiment kopie*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) dělat. Jiné řešení je publikovat testu do Galerie v nezařazené režimu, otevřete si ho v pracovním prostoru z jiné oblasti.
2.  Výběr oblasti bude ukazovat jenom pracovní prostory pro jednu oblast najednou. V budoucnu budete moct zobrazit úplný seznam pracovních prostorů máte přístup k ve všech oblastech ve stejnou dobu.  
3.  Vytváření a použitý ve jih centrální USA bezplatné pracovního prostoru nebo hosty (anonymní) pracovního prostoru V budoucnu bude moct vytvářet pracovní prostory Free/hosty v oblasti, které zvolíte.  
4.  Webové služby nasazeny z pracovního prostoru v jihovýchodní Asie bude být umístěny ve jihovýchodní Asie. V budoucnu bude moct flexibilitu vytváření pokusů v jednom regionu a nasazení generovaného koncové body webové služby do jiné oblasti.  

## <a name="more-information"></a>Další informace

Zeptejte se na [Fórum komunity výukové počítače Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
