<properties 
    pageTitle="Publikování počítače výukové webové služby Azure Marketplace | Microsoft Azure" 
    description="Jak publikovat webové služby Azure počítače výukové Azure Marketplace" 
    services="machine-learning" 
    documentationCenter="" 
    authors="BharathS" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="bharaths"/>

# <a name="publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a>Publikovat Azure počítače výukové webové služby Azure Marketplace 

Azure Marketplace umožňuje publikovat webové služby Azure počítače výukové jako zaplacené nebo bezplatné služby pro spotřebu externí zákazníky. Tento článek obsahuje přehled o tomto procesu s odkazy na Rady, které vám usnadní zahájení práce. Pomocí tohoto procesu můžete zpřístupnit webové služby pro ostatní vývojářům používat ve svých aplikací.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a>Přehled procesu publikování 

Následují kroky pro publikování webové služby Azure počítače výuka k webu Azure Marketplace:

1. Vytváření a publikování služby strojového výukové žádost o odpovědi na (záznamy)
2. Nasadit služby výrobní a získat informace o klíči rozhraní API a OData koncového bodu.
3. Použití adresy URL publikované webové služby publikujete [Azure Marketplace (Data Market)](https://publish.windowsazure.com/workspace/) 
4. Po odeslání vaši nabídku: bude posouzena a vyžaduje schválení před vaši zákazníci můžete začít zakoupení ho. Publikování může trvat několik pracovních dní. 

## <a name="walk-through"></a>Projděte si postup
###<a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Krok 1: Vytvoření a publikování služby strojového výukové žádost o odpovědi na (záznamy)###
 Pokud jste to ještě neudělali, zkontrolujte podívejte se na tento [projděte si postup](machine-learning-walkthrough-5-publish-web-service.md).

###<a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a>Krok 2: Výrobní nasadit služby a získat klíč rozhraní API a informace koncového bodu OData###
1. Z [Portálu klasické Azure](http://manage.windowsazure.com)vyberte možnost **VÝUKOVÉ počítače** v levém navigačním panelu a vyberte pracovního prostoru. 

2. Klikněte na kartě **Webovým službám** a vyberte webové služby, kterou chcete publikovat na Tržiště.

    ![Azure Marketplace][workspace]

3. Vyberte koncový bod se mají mít marketplace používat. Pokud jste nevytvořili jakékoli další koncové body, můžete vybrat **výchozí** koncový bod.

4. Po klepnutí na koncový bod, budete moct **Rozhraní API klíče**. Budete potřebovat tato hardwarová informací později v kroku 3, proto Přesvědčte jeho kopii.

    ![Azure Marketplace][apikey]

5. Klikněte na metodu **Žádostí a odpovědí** této fázi, že nepodporujeme publikování služby spuštění dávky na Tržiště. Tím přejdete na stránku nápovědy rozhraní API pro metodu žádostí a odpovědí.

6. Zkopírujte **Adresu koncový bod OData**, musíte tyto informace později v kroku 3.

    ![Azure Marketplace][odata]




nasazení služby do výroby.



###<a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a>Krok 3: Použití adresy URL publikované webové služby publikujete Azure Marketplace (DataMarket)###

1.  Přejděte na [Azure Marketplace (Data Market)](http://datamarket.azure.com/home) 
2.  Klikněte na odkaz **Publikovat** v horní části stránky. Tím přejdete na [Portál publikování Microsoft Azure](https://publish.windowsazure.com)
3.  Klikněte na oddíl **Vydavatelé** zaregistrovat jako vydavatel.
4.  Při vytváření nové nabídky, vyberte **Datové služby**a potom klikněte na **vytvořit nové datové služby**. 
 
    ![Azure Marketplace][image1]

    <br />


5.  V části **plány jednotného zasílání zpráv** poskytuje informace o nabídky, včetně cen plánu. Rozhodněte, pokud bude nabízet placené nebo bezplatné služby. Chcete-li platby, zadejte platební údaje například banky a daně informace o.

6.  V části **marketingové** zadejte informace o vaší nabídky, jako je název a popis pro vaši nabídku.

7.  V části **ceny** můžete nastavit cenu pro plány pro určité země nebo systému "autoprice", aby vaši nabídku.

8. Na kartě **Datové služby** klikněte na **Webová služba** jako **Zdroj dat**.

    ![Azure Marketplace][image2]

9.  Potřebujete klávesu adresy URL a rozhraní API webových služeb z portálu klasické Azure způsobem popsaným v kroku 2 nahoře.

10. V dialogovém okně Nastavení Marketplace datové služby vložte adresu koncový bod OData do textového pole **Adresy URL služby** .

11. **Ověření**klikněte na **záhlaví** jako **Schéma ověřování**.

    - Zadejte "Se tak mohli ověřovat" **názvy záhlaví**.
    - **Záhlaví hodnota**zadejte "Nosný" (bez uvozovek), klikněte na pruh **místa** a vložte klávesu rozhraní API.
    - **Tato služba je OData** zaškrtněte políčko.
    - Klikněte na **Testovat připojení** na test připojení.

12. Ve skupinovém rámečku **kategorie**zkontrolujte, zda **Že počítač výukové** zaškrtnuté.

13. Po dokončení zadávání všechna metadata o vaší nabídky klikněte na **Publikovat**a potom **pracovní**. V tomto okamžiku budete upozorněni zbývající problémů, které potřebujete k řešení.

14. Po zajistí dokončení všech zbývajících problémů klikněte na **požádat o schválení posunout výroby**. Publikování může trvat několik pracovních dní. 


[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png
 
