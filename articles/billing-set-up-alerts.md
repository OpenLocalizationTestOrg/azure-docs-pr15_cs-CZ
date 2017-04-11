<properties
    pageTitle="Nastavení upozornění pro vaše předplatné Microsoft Azure fakturace | Microsoft Azure"
    description="Popisuje, jak můžete nastavit upozornění na faktuře Azure tak, aby se můžete vyhnout fakturační překvapení."
    services=""
    documentationCenter=""
    authors="vikdesai"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/18/2016"
    ms.author="vikdesai"/>

# <a name="set-up-billing-alerts-for-your-microsoft-azure-subscriptions"></a>Nastavit upozornění fakturace u předplatného Microsoft Azure

Jedná se o kolik jste výdaje každý měsíc předplatného Azure? Pokud jste správce účet Azure předplatného, můžete Azure fakturace služby oznámení vytvořit vlastní fakturační upozornění, které vám pomůžou sledovat a spravovat fakturaci aktivitu pro Azure účty.

Toto je služba náhled, tak, aby první věc, kterou je potřeba udělat Odhlásit se pro něj. Navštivte [stránku funkce náhledu](https://account.windowsazure.com/PreviewFeatures) v portálu pro správu účet Azure tuto funkci povolit.

## <a name="set-the-alert-threshold-and-email-recipients"></a>Nastavit upozornění mezní hodnota a e-mailu příjemce

Když dostanete e-mailové potvrzení, že je zapnutá fakturační služby pro vaše předplatné, navštivte [stránku předplatná](https://account.windowsazure.com/Subscriptions) v portálu účtu. Klikněte na předplatné, které chcete sledovat a potom klikněte na **upozornění**.

![][Image1]

Klepnutím na tlačítko **Přidat upozornění** , vytvořit svůj první z nich – můžete nastavit celkem pět fakturační upozornění na jedno předplatné s jinou mezní hodnota a až dva příjemců e-mailu pro každou upozornění.

![][Image2]

Když přidáte upozornění, zadejte jedinečný název, zvolte výdajů mezní hodnoty a zvolte místo, kam oznámení odešle e-mailové adresy. Při nastavování prahové hodnoty, můžete **Fakturace celkové** nebo **Peněžní platební** ze seznamu **Upozornění pro** . Fakturační celkem odeslaný upozornění při předplatné výdaje větší než mezní hodnota. Pro peněžní platební odeslaný upozornění při přímé peněžní přeplatky pod limit. Peněžní přeplatky obvykle vyrovnat předplatná spojené s účty MSDN a bezplatné zkušební verze.

![][Image3]

Azure podporuje všechny e-mailovou adresu, ale není ověřte, že funguje e-mailovou adresu, takže znova zkontrolujte, pro překlepy.

## <a name="check-on-your-alerts"></a>Informace o nastavení upozornění

Po nastavení upozornění v centru účet obsahuje je a ukazuje kolik více můžete nastavit tak. Pro každý upozornění uvidíte datum a čas odeslání ho, ať už jde upozornění pro fakturaci celkové nebo peněžní platební a omezení, které nastavíte. Formát data a času 24hodinovém koordinaci univerzálním časem (UTC) a datum rrrr mm-dd formát. Klepněte na znaménko plus pro upozornění v seznamu pro úpravy, nebo klikněte na odpadkový a vymažete ho.

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png
