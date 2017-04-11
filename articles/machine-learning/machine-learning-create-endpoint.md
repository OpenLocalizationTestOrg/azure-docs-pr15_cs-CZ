<properties
    pageTitle="Vytváření koncové body webové služby ve počítače výukové | Microsoft Azure"
    description="Vytváření koncové body webové služby Azure počítač přečíst"
    services="machine-learning"
    documentationCenter=""
    authors="hiteshmadan"
    manager="padou"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="10/04/2016"
    ms.author="himad"/>


# <a name="creating-endpoints"></a>Vytváření koncové body

>[AZURE.NOTE] Toto téma popisuje technik pro klasické webové služby.

Když vytvoříte webové služby, které prodáváte vpřed zákazníkům, budete muset poskytují školení modely pro jednotlivé zákazníky, pořád propojené s experiment, ze kterého jste vytvořili webové služby. Kromě toho všechny aktualizace testu by měl použije selektivně koncový bod aniž byste přepsali vlastní nastavení.

K tomu Azure počítače výukové umožňuje vytvářet několika koncové body nasazeném webové služby. Každý koncový bod služby Webový je nezávisle na sobě adresovaná omezena a spravovat. Každý koncový bod je jedinečnou adresu URL a povolení klíč, který můžete rozeslat svým zákazníkům.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-to-a-web-service"></a>Přidání koncové body do webové služby

Existují tři způsoby, jak přidat koncový bod do webové služby.

* Programově
* Pomocí portálu Azure počítače výukové webové služby
* Když Azure klasické portálu

Po vytvoření koncového bodu můžete používat až synchronní rozhraní API, dávku rozhraní API a listy aplikace excel. Kromě přidání koncové body prostřednictvím tohoto uživatelského rozhraní, můžete také API správy koncový bod programově přidat koncové body.

 >[AZURE.NOTE] Pokud přidáte další koncové body k webové službě, nemůžete odstranit výchozí koncový bod.

## <a name="adding-an-endpoint-programmatically"></a>Programové přidávání koncový bod

Koncový bod můžete přidat k vaší webové službě programově [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) ukázkový kód.

## <a name="adding-an-endpoint-using-the-azure-machine-learning-web-services-portal"></a>Přidání koncový bod pomocí portálu Azure počítače výukové webové služby

1. Ve počítače výukové studiu ve sloupci levém navigačním panelu klikněte na Web služby.
2. V dolní části na řídicím panelu webové služby klikněte na **Spravovat koncové body**. Na portálu Azure počítače výukové webovým službám se otevře stránka koncové body webové služby.
3. Klikněte na **Nový**.
4. Zadejte název a popis pro nový koncový bod. Koncový bod názvy musí být 24 znak nebo menší délce a musí být tvořen malá písmena abecedy nebo čísla. Vyberte požadovanou úroveň protokolování a zda je povoleno ukázková data. Další informace o protokolování v tématu [Povolení protokolování pro počítač výukové webové služby](machine-learning-web-services-logging.md).

## <a name="adding-an-endpoint-using-the-azure-classic-portal"></a>Přidání koncový bod na portálu Azure klasické


1. Přihlaste se k [Azure klasické portál](http://manage.windowsazure.com), klikněte na **Počítač výukové** v levém sloupci. Klikněte na pracovní prostor, který obsahuje webové služby, které vás zajímají.

    ![Přejděte do pracovního prostoru](./media/machine-learning-create-endpoint/figure-1.png)

2. Klikněte na **Web služby**.

    ![Přejděte k webovým službám](./media/machine-learning-create-endpoint/figure-2.png)

3. Klikněte na webové služby, které vás zajímá zobrazíte seznam dostupných koncové body.

    ![Přejděte na koncový bod](./media/machine-learning-create-endpoint/figure-3.png)

4. V dolní části stránky klikněte na **Přidat koncový bod**. Zadejte název a popis, ujistěte se, že nejsou žádné koncové body se stejným názvem v této webové služby. Pokud nemáte zvláštní požadavky, nechte úroveň omezení s použita výchozí hodnota. Další informace o omezení, najdete v článku [Změna měřítka koncové body rozhraní API](machine-learning-scaling-webservice.md).

    ![Vytvoření koncového bodu](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Další kroky

[Jak používat publikované Azure počítače výukové webové služby](machine-learning-consume-web-services.md).
