<properties
    pageTitle="Doplněk pro počítač výukové webové služby aplikace Excel | Microsoft Azure"
    description="Jak používat Azure počítače výukové webové služby přímo v aplikaci Excel bez psaní kódu."
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/05/2016"
    ms.author="tedway;garye" />

# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Doplněk aplikace Excel Azure počítače výukové webové služby

Excel snadno volání webové služby přímo bez nutnosti napsat jakýkoli kód.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Kroky pro použití stávající webové služby v sešitu

1. Otevřete [Ukázkový Excelový soubor](http://aka.ms/amlexcel-sample-2), který obsahuje doplněk aplikace Excel a data cestující na Titanic.
2. Vyberte webová služba tak, že na ni kliknete-"Titanic pozůstalým prognostických (ukázka doplňku aplikace Excel) [výsledek]" v tomto příkladu.

    ![Vyberte webové služby][01]

3. Tím přejdete do oddílu **Predict** .  Tento sešit už obsahuje ukázková data, ale pro prázdný sešit můžete vybrat buňku v aplikaci Excel a klikněte na **použít ukázková data**.
4. Vyberte data, která se záhlavími a klikněte na ikonu oblast vstupní data.  Zkontrolujte, že je zaškrtnuté políčko "Moje data obsahují záhlaví".
5. Ve skupinovém rámečku **výstup**zadejte počet buněk místo, kam chcete výstupu být, třeba "H1" tady.
6. Klikněte na tlačítko **předpovídání**.

    ![Předpovídání oddílu][02]

## <a name="steps-to-add-a-new-web-service"></a>Kroky potřebné k přidání nové webové služby

Nasazení webové služby nebo použít existující webové služby. Další informace o nasazení webové služby najdete v tématu [návodu krok 5: nasazení Azure počítače výukové webová služba](machine-learning-walkthrough-5-publish-web-service.md).

Pokud potřebujete klávesu rozhraní API pro webové služby. Kde to uděláte, závisí na tom, jestli jste publikovali klasické počítače výukové webové služby nový počítač výukové webové služby.

**Pomocí klasické webové služby** 

1. Ve počítače výukové studiu klikněte v části **Webové služby** v levém podokně a pak vyberte Webová služba.

    ![Vyberte Studio webové služby][04]

2. Zkopírujte klávesu rozhraní API pro webové služby.

    ![Klíč Studio rozhraní API][05]

3. Karta **řídicí panel** webové služby klikněte na odkaz **Žádostí a odpovědí** .
4. Hledejte části **Požádat o URI** .  Zkopírujte a uložte adresu URL.

>[AZURE.NOTE] Nyní je možné přihlásit na portál [Azure počítače výukové webové služby](https://services.azureml.net) a získat klávesu rozhraní API pro klasické počítače výukové webové služby.

**Pomocí nové webové služby**

1. Na portálu [Azure počítače výukové webové služby](https://services.azureml.net) klikněte na **Web služby**a potom vyberte Webová služba. 
2. Klikněte na **používat**.
3. Najděte v části **základní spotřeby informace** . Zkopírujte a uložte **Primární klíč** a **Odpovědi na žádosti o** adresu URL.


## <a name="steps-to-add-a-new-web-service"></a>Kroky potřebné k přidání nové webové služby

1. Nasazení webové služby nebo použít existující webové služby. Další informace o nasazení webové služby najdete v tématu [návodu krok 5: nasazení Azure počítače výukové webová služba](machine-learning-walkthrough-5-publish-web-service.md).
2. Klikněte na **používat**.
3. Najděte v části **základní spotřeby informace** . Zkopírujte a uložte **Primární klíč** a **Odpovědi na žádosti o** adresu URL.
2. V aplikaci Excel, přejděte k části **Webové služby** (Pokud jste v části **Predict** , klikněte na šipku zpět pro návrat na seznam webové služby).

    ![Přejděte na Web služby výběru][03]
    
3. Klikněte na **Přidat webové služby**.
4. Vložte adresu URL do Excelu doplněk text políčko **Adresa URL**.
5. Vložte do textového pole s názvem **rozhraní API klíč**rozhraní API/primární klíč.
6. Klikněte na **Přidat**.

    ![Adresa URL a rozhraní API klíč pro klasické webové služby.][06]

10. Použití webové služby, pokyny předchozí, "Kroky pro použití stávající webové služby."

## <a name="sharing-your-workbook"></a>Sdílení sešitu

Pokud svůj sešit uložíte, rozhraní API/primární klíč pro webové služby, které jste přidali rovněž uloženo. To znamená, že sešit by měl sdílet jenom s osob, kterým důvěřujete.

Zeptejte se všechny v následující části komentáře nebo na naše [Fórum komunity](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
