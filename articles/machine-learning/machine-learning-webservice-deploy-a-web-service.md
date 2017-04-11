<properties
   pageTitle="Nasazení nové webové služby"
   description="Pracovní postup nasazení ARM na základě webové služby"
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
    ms.date="10/04/2016"
    ms.author="v-donglo"/>

# <a name="deploy-a-new-web-service"></a>Nasazení nové webové služby

Microsoft Azure Machine výukové nyní obsahuje webové služby, které jsou založené na [Azure správce prostředků](../azure-resource-manager/resource-group-overview.md) umožňující nové fakturace možnosti plánování a nasazení webové služby na více oblastí.

Obecné pracovní postup pro nasazení webové služby Microsoft Azure počítače výukové webových službách je:

* Vytvoření prediktivní testu
* nasazení
* Konfigurace názvu
* fakturační plán
* Vyzkoušejte
* ho zpracovat.

Následující obrázek znázorňuje pracovního postupu.

![Pracovní postup nasazení webové služby][1]
 
## <a name="deploy-web-service-from-studio"></a>Nasazení webové služby z Studio 

Chcete-li nasadit pokus jako nové webové služby. Přihlaste se k Studio výukové počítače a vytvořit nové prediktivní webové služby. 

**Poznámka**: Pokud jste již nainstalovali pokus jako klasická webová služba nelze nasadíte ho jako nové webové služby.
 
Klikněte na **Spustit** v dolní části plátno testu a klikněte na **Nasazení webové služby** a **Nasazení [New] webové služby**. Otevře se nasazení stránce správce počítače výukové webové služby.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Správce služeb strojového výukové Web nasazení Experiment stránky
Na stránce nasazení Experiment zadejte název pro webové služby.
Vyberte ceny plán. Pokud máte existující plán ceny, že které lze vybrat v opačném případě musí vytvořit nový plán ceny pro službu. 

1.  V **Plánu cena** rozevírací seznam, vyberte existující plán nebo **Nový plán vyberte** možnost.
2.  Do pole **Název plánu**zadejte název, který bude identifikovat plán na faktuře.
3.  Vyberte jednu ze **Měsíční plánování úrovní**. Poznámka: nasazení výchozí úrovně plán plány výchozí oblasti a webové služby na dané oblasti.

Klikněte na **nasazení** a stránce Rychlý úvod otevře vaší webové služby.

## <a name="quickstart-page"></a>Rychlý úvod stránky
Webové služby rychlý úvod stránce vám nabídne přístup a podle pokynů na nejčastější úkoly, které budete provádět po vytvoření nové webové služby. Tady snadno dostanete **vytištěná testovací** stránka a **spotřebě** stránky.

## <a name="testing-your-web-service"></a>Testování webové služby

Na stránce Rychlý úvod klikněte na testovat webové služby v rámci běžné úkoly.   

Chcete-li otestovat webová služba jako odpovědi na žádosti o službu (RR):

* Na řádku nabídek klikněte na tlačítko **Testovat** .
* Klikněte na **Žádosti o odpověď**.
* Zadejte příslušné hodnoty pro zadávání sloupce vaší testu.
* Klikněte na Test **Žádost o odpověď**.

Výsledky se zobrazí na pravé straně stránky.

Chcete-li otestovat webové služby dávku spuštění služby (BES), bude používat souboru CSV:

* Na řádku nabídek klikněte na tlačítko **Testovat** .
* Klikněte na **dávkové**.
* Ve skupinovém rámečku vstup klikněte na tlačítko Procházet a přejděte na Ukázka datového souboru.
* Klikněte na **Testovat**.

Stav test se zobrazí v **Testování dávkové úlohy**.

## <a name="consuming-your-web-service"></a>Jinými webové služby

Nasazené webové služby, zadejte Azure počítače výukové pokusy rozhraní REST API, který může být spotřebované množství široká škála zařízení a platformách. Důvodem je jednoduché rozhraní REST API přijme a zpráv ve formátu odpoví JSON. Portál Azure počítače výukové obsahuje kód, který lze použít k volání webové služby v R, C# a Python.
 
Na stránce spotřeba najdete:

* Rozhraní API klíč a identifikátory URI pro využívání webové služby v aplikacích.
* Excel a webové šablony aplikací a spusťte spustit proces spotřebu.
* Ukázkový kód v jazyce C# python a R vám usnadní zahájení práce.

Další informace o jinými webové služby najdete v článku [jak používat webové služby Azure výukové stroje, které byly nasazeny z počítače výukové testu](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Další kroky

Další informace o jinými webové služby najdete v článku:

[Jak používat webové služby Azure počítače výukové, které byly nasazeny z počítače výukové testu](machine-learning-consume-web-services.md)

[Azure počítače výukové webové služby: Nasazení a spotřeby](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
