<properties 
    pageTitle="Fáze nasazení služby cloudu (Node.js) | Microsoft Azure" 
    description="Naučte se nasadit Azure aplikaci testovacím prostředí a potom nasazení provozním prostředí pomocí vyměňovat virtuální IP (VIP)." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="staging-an-application-in-azure"></a>Pracovní aplikace v Azure

Aplikace sbalený může být nasazené na testovacím prostředí v Azure otestuje předtím, než je nepřesunete do provozním prostředí, ve kterém aplikace je dostupný na Internetu. Pracovní prostředí vypadá přesně provozním prostředí, s tím rozdílem, že pouze se dostanete fázované aplikaci zastaralý adresu URL, která je generováno aplikací Azure. Po ověření, že aplikace funguje zařízení správně ji může být nasazené na provozním prostředí provedením vyměňovat virtuální IP (VIP).

> [AZURE.NOTE] Postup v tomto článku platí pouze aplikacím uzel hostované jako cloudové služby Azure.

## <a name="step-1-stage-an-application"></a>Krok 1: Příprava aplikace

Tento úkol popisuje, jak pomocí **Powershellu Microsoft Azure**Připravení aplikace.

1.  Při publikování do služby, jednoduše předejte **-úsek** parametr rutinu **AzureServiceProject publikovat** .

    ```powershell
    Publish-AzureServiceProject -Slot staging
    ```

2.  Přihlaste se k [Azure klasické portál] a vyberte **Cloudovým službám**. Po vytvoření cloudové služby a stav sloupec **pracovní** byl aktualizován **spuštění**, klikněte na název služby.

    ![portál zobrazení spuštěné služby][cloud-service]

3.  Vyberte **řídicího panelu**a pak vyberte **pracovní**.

    ![řídicí panel služeb cloudu][cloud-service-dashboard]

4. Poznamenejte si hodnotu ve sloupci **Adresu URL webu** vpravo. Název DNS je zastaralý vnitřní ID, které vygenerovalo Azure.

    ![Adresa url webu][cloud-service-staging-url]

Teď můžete ověřit, že aplikace funguje zařízení správně v testovacím prostředí pomocí pracovní adresu URL webu.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Krok 2: Aktualizace aplikace produktivních výměna virtuální

Po ověření upgradovanou verzi aplikace v testovacím prostředí můžete rychle zpřístupnit ho ve výrobním tak, že výměna virtuální adresy IP (adres VIP) prostředí pracovní a výroby.

> [AZURE.NOTE] Tento krok předpokládá, že jste již nasazení aplikace výroby a připravené upgradovanou verzi aplikace.

1.  Přihlaste se k [Azure klasické portál], klikněte na **Cloudovým službám** a vyberte název služby.

2.  Z **řídicího panelu**vyberte **pracovní**a potom klikněte na **Zaměnit** v dolní části stránky. Otevře se dialogové okno VIP vyměňovat.

    ![Dialogové okno vyměňovat VIP][vip-swap-dialog]

3.  Zkontrolujte informace a potom klikněte na **OK**. Dva nasazení začít, aktualizace a pracovní nasazení kombinace kláves vymění výroby provozní nasazení se přepne do pracovní.

Máte úspěšně připravené na nasazení a upgradovat provozní nasazení tak, že výměna virtuální s nasazením v pracovní.

## <a name="additional-resources"></a>Další zdroje informací

- [Implementaci upgradu služeb do výrobní tak, že výměna virtuální v Azure]

[Azure klasické portálu]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Implementaci upgradu služeb do výrobní tak, že výměna virtuální v Azure]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
