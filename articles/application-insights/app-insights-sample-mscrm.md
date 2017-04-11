<properties 
    pageTitle="Návod: Sledování Microsoft Dynamics CRM s přehledy aplikace" 
    description="Pokud potřebujete telemetrie z Microsoft Dynamics CRM Online s použitím aplikace přehledy. Návod instalace získávání dat, vizualizace a export." 
    services="application-insights" 
    documentationCenter=""
    authors="mazharmicrosoft" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>
 
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Návod: Povolení Telemetrie Microsoft Dynamics CRM Online s použitím aplikace přehledy

V tomto článku se dozvíte, jak získat telemetrickými daty z [Webu Microsoft Dynamics CRM Online](https://www.dynamics.com/) pomocí [Přehledy aplikace Visual Studio](https://azure.microsoft.com/services/application-insights/). Budete projdeme úplný postup přidání skript aplikace přehledy aplikaci, zaznamenání dat a vizualizaci dat.

>[AZURE.NOTE] [Procházet řešení vzorku](https://dynamicsandappinsights.codeplex.com/).

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Přidání aplikace přehledy nové nebo existující CRM Online instanci 

Sledování aplikace, přidejte SDK přehledy aplikace do aplikace. V SDK pošle telemetrie [přehledy aplikace portál](https://portal.azure.com), kde můžete použít naše výkonné analýzy a diagnostických nástrojů, nebo exportovat data k základnímu úložišti.

### <a name="create-an-application-insights-resource-in-azure"></a>Vytvoření zdrojů aplikace přehledy v Azure

1. Zřízení [účtu v Microsoft Azure](http://azure.com/pricing). 
2. Přihlaste se k [portálu Azure](https://portal.azure.com) a přidání nového prostředku přehledy aplikace. Toto je místo, kam zpracování a zobrazí data.

    ![Klikněte na +, Developer Services, aplikaci přehledy.](./media/app-insights-sample-mscrm/01.png)

    Zvolte ASP.NET jako typ aplikace.

3. Otevřete kartu Rychlý Start a otevřete skript kód.

    ![](./media/app-insights-sample-mscrm/03.png)

**Nechte stránku kód otevřenou** i v případě se na další krok v jiném okně prohlížeče. Musíte mít kód brzy bude k dispozici. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Vytvoření webového prostředku JavaScript v aplikaci Microsoft Dynamics CRM

1. Otevřete CRM Online instance a přihlaste se s oprávněními správce.
2. Otevřete Microsoft Dynamics CRM nastavení vlastních úprav přizpůsobení systému

    ![](./media/app-insights-sample-mscrm/04.png)
    
    ![](./media/app-insights-sample-mscrm/05.png)


    ![](./media/app-insights-sample-mscrm/06.png)

3. Vytvořte zdroj JavaScript.

    ![](./media/app-insights-sample-mscrm/07.png)

    Zadejte název, vyberte **skript (JScript)** a otevřete textový editor.

    ![](./media/app-insights-sample-mscrm/08.png)
    
4. Zkopírujte kód z aplikace přehledy. Při kopírování zkontrolujte přeskočit skript značky. Označovat pod snímek:

    ![](./media/app-insights-sample-mscrm/09.png)

    Tento kód obsahuje klíč přístrojového vybavení, který identifikuje prostředku přehledy aplikace.

5. Uložte a publikujte.

    ![](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Nástroje formuláře

1. V aplikaci Microsoft CRM Online otevření formuláře Klient

    ![](./media/app-insights-sample-mscrm/11.png)

2. Otevřete formulář vlastnosti

    ![](./media/app-insights-sample-mscrm/12.png)

3. Přidání zdroje JavaScript web, který jste vytvořili

    ![](./media/app-insights-sample-mscrm/13.png)

    ![](./media/app-insights-sample-mscrm/14.png)

4. Uložte a publikujte formulář.


## <a name="metrics-captured"></a>Metriky nezaznamenávají

Teď jste nastavili telemetrie zachycení pro formulář. Pokaždé, když se používá, data odeslána zdroji přehledy aplikace.

Tady jsou příklady data, která se zobrazí.

#### <a name="application-health"></a>Ochrana aplikací

![](./media/app-insights-sample-mscrm/15.png)

![](./media/app-insights-sample-mscrm/16.png)

Výjimky prohlížeče:

![](./media/app-insights-sample-mscrm/17.png)

Klikněte na graf a získejte další informace:

![](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Použití

![](./media/app-insights-sample-mscrm/19.png)

![](./media/app-insights-sample-mscrm/20.png)

![](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Prohlížeče

![](./media/app-insights-sample-mscrm/22.png)

![](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Zeměpisná poloha

![](./media/app-insights-sample-mscrm/24.png)

![](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Vnitřní stránky zobrazit požadavek

![](./media/app-insights-sample-mscrm/26.png)

![](./media/app-insights-sample-mscrm/27.png)

![](./media/app-insights-sample-mscrm/28.png)

![](./media/app-insights-sample-mscrm/29.png)

![](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Ukázkový kód

[Procházet ukázkový kód](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI

I hlubší analýze můžete dělat, když [Exportovat data k Microsoft Power BI](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Ukázka řešení Microsoft Dynamics CRM

[Tady je ukázka řešení implementovaná v aplikaci Microsoft Dynamics CRM] (https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Víc se uč

* [Co je aplikace přehledy?](app-insights-overview.md)
* [Přehledy aplikace na webových stránkách](app-insights-javascript.md)
* [Další příklady a návody](app-insights-code-samples.md)

 
