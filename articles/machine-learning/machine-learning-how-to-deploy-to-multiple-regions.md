<properties
    pageTitle="Nasazení webové služby do více oblastí | Microsoft Azure"
    description="Kroky pro nasazení (Kopírovat) nové webové služby do jiných oblastí."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a>Nasazení webové služby do více oblastí

Nové webové služby Azure umožňují snadno nasadit webové služby na více oblastí aniž by musel víc předplatných nebo pracovní prostory. 

Ceny je oblast konkrétní, že proto je třeba definovat fakturační plán pro každou oblast, ve kterém nasadíte webové služby.

## <a name="to-create-a-plan-in-another-region"></a>Chcete-li vytvořit plán v jiné oblasti

1. Přihlaste se do [počítače Microsoft Azure výukové webové služby](https://services.azureml.net/).
2. Klikněte na možnost nabídky **plány jednotného zasílání zpráv** .
3. Na plány myši zobrazit stránky klikněte na **Nový**.
4. Z rozevírací nabídky **předplatné** vyberte předplatné, ve kterém bude nacházet nový plán.
5. V rozevíracím seznamu **oblast** vyberte oblast na nový plán. V části **Možnosti plánu** na stránce se zobrazí u položky možnosti plánu pro vybranou oblast.
6. V rozevíracím seznamu **Pole Skupina zdroje** vyberte skupina zdroje pro plán. Další informace o skupinách zdroje tématech [Správa Azure prostřednictvím portálu](../azure-portal/resource-group-portal.md).
7. Do **Plánu název** zadejte název plánu.
8. V části **Možnosti plánu**klikněte na fakturace úroveň na nový plán.
9. Klikněte na **vytvořit**.


## <a name="deploying-the-web-service-to-another-region"></a>Nasazení webové služby do jiné oblasti

1. Klikněte na možnost nabídky **Webové služby** .
2. Vyberte webová služba nasazujete novou oblast.
3. Klikněte na **Kopírovat**.
4. Do pole **Název webové služby**zadejte nový název pro webové služby.
5. Do pole **Popis webové služby**zadejte popis webové služby.
6. Z rozevírací nabídky **předplatné** vyberte předplatné, ve kterém bude nacházet nové webové služby.
7. V rozevíracím seznamu **Pole Skupina zdroje** vyberte požadovanou skupinu zdroje webové služby. Další informace o skupinách zdroje tématech [Správa Azure prostřednictvím portálu](../azure-portal/resource-group-portal.md).
8. V rozevíracím seznamu **oblast** vyberte požadovanou oblast na kterou chcete nasadit webové služby.
9. Z rozevírací nabídky **úložiště účtu** vyberte účet úložiště pro uložení webové služby.
10. V rozevíracím seznamu **Cena plán** vyberte požadovaný plán v oblasti, kterou jste vybrali v kroku 8.
11. Klikněte na **Kopírovat**.

