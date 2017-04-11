<properties
   pageTitle="Ukázka upozornění webhook analýzy protokolování"
   description="Jednou z akce, které můžete použít v odpovědi na upozornění na protokolu analýzy je *webhook*, který umožňuje vyvolat externí proces až jednu žádost HTTP. Tento článek provede příklad vytvoření webhook akce v upozornění k protokolu analýzy pomocí časová rezerva."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="webhooks-in-log-analytics-alerts"></a>Webhooks v protokolu analýzy upozornění

Jednou z akce, které můžete použít v odpovědi na [upozornění protokolu analýzy](log-analytics-alerts.md) je *webhook*, který umožňuje vyvolat externí proces až jednu žádost HTTP.  Další informace o podrobnosti o oznámení a webhooks v [upozornění v protokolu analýzy](log-analytics-alerts.md)

V tomto článku se projdeme příklad vytvoření webhook akce v upozornění k protokolu analýzy pomocí časová rezerva, což je služba zasílání zpráv.

>[AZURE.NOTE] Musíte mít účet časová rezerva dokončení v tomto příkladu.  Můžete si jej vytvořit bezplatný účet u [slack.com](http://slack.com).

## <a name="step-1---enable-webhooks-in-slack"></a>Krok 1: povolení webhooks v časová rezerva
2.  Přihlaste se k časové rezervy v [slack.com](http://slack.com).
3.  V části **kanálů** v levém podokně vyberte kanál.  Toto je kanál, který může odeslané zprávy.  Můžete vybírat z výchozí kanály ATP **obecného** **náhodné**.  Ve scénáři výrobní by nejspíš vytvoříte zvláštní kanálu například **criticalservicealerts**. <br>

    ![Časová rezerva kanály](media/log-analytics-alerts-webhooks/oms-webhooks01.png)

3. Klikněte na **Přidat aplikaci nebo vlastní integrace** otevřete adresář aplikací.
3.  Do vyhledávacího pole zadejte *webhooks* a pak vyberte **Příchozí WebHooks**. <br>

    ![Časová rezerva kanály](media/log-analytics-alerts-webhooks/oms-webhooks02.png)

4.  Klikněte na **nainstalovat** vedle názvu týmu.
5.  Klikněte na **Přidat konfigurace**.
6.  Vyberte kanál, který chcete použít v tomto příkladu a potom klikněte na **Přidat příchozí WebHooks integrace**.  
6. Zkopírujte adresu **Webhook URL**.  Budete vkládat to do upozornění konfigurace. <br>

    ![Časová rezerva kanály](media/log-analytics-alerts-webhooks/oms-webhooks05.png)

## <a name="step-2---create-alert-rule-in-log-analytics"></a>Krok 2 – vytvořit pravidlo výstrahy v protokolu analýzy
1.  [Vytvoření pravidla výstrahy](log-analytics-alerts.md) se následující nastavení.
    - Dotaz:```    Type=Event EventLevelName=error ```
    - Vyhledat tuto výstrahu každé: 5 minut
    - Počet výsledků: větší než 10
    - Přes tento časového intervalu: 60 minut
    - Vyberte možnost **Ano** pro **Webhook** a **Ne** pro jiné akce.
7. Vložte adresu URL časové rezervy do pole **Adresa URL Webhook** .
8. Vyberte možnost **zahrnout vlastní datové JSON**.
9. Zvětralého očekává datové naformátovaný parametr s názvem *text*ve formátu JSON.  Toto je text, který se zobrazí v okně zprávy, který předtím vytvořil.  Můžete použít jednu nebo více upozornění parametrů pomocí *#* symbol tak jako v následujícím příkladu.

    ```
    {
    "text":"#alertrulename fired with #searchresultcount records which exceeds the over threshold of #thresholdvalue ."
    }
    ```

    ![Příklad JSON data](media/log-analytics-alerts-webhooks/oms-webhooks07.png)

9.  Klepněte na tlačítko **Uložit** uložte pravidlo výstrahy.

10. Počkejte, než dostatek času pro upozornění, vytvořit a potom zaškrtněte časová rezerva pro zprávy, která bude vypadat podobně jako tento.

    ![Příklad webhook v časová rezerva](media/log-analytics-alerts-webhooks/oms-webhooks08.png)


### <a name="advanced-webhook-payload-for-slack"></a>Rozšířené webhook data pro časovou rezervu

Můžete přizpůsobit rozsáhlé příchozích zpráv s časovou rezervou. Další informace najdete v tématu [Příchozí Webhooks](https://api.slack.com/incoming-webhooks) na časové rezervy webu. Následuje složitější datové bohaté zprávu vytvoříte pomocí formátování:

    {
        "attachments": [
            {
                "title":"OMS Alerts Custom Payload",
                "fields": [
                    {
                        "title": "Alert Rule Name",
                        "value": "#alertrulename"},
                    {
                        "title": "Link To SearchResults",
                        "value": "<#linktosearchresults|OMS Search Results>"},
                    {
                        "title": "Search Interval",
                        "value": "#searchinterval"},
                    {
                        "title": "Threshold Operator",
                        "value": "#thresholdoperator"},
                    {
                        "title": "Threshold Value",
                        "value": "#thresholdvalue"}
                ],
                "color": "#F35A00"
            }
        ]
    }


To v časová rezerva vygeneruje zprávu podobnou následující.

![Příklad zprávy v časová rezerva](media/log-analytics-alerts-webhooks/oms-webhooks09.png)

## <a name="summary"></a>Souhrn

Pomocí tohoto oznámení pravidla na místě má Komu časová rezerva pokaždé, když dojde ke splnění kritéria.  

Toto je pouze příkladem akci, která můžete vytvořit v odpovědi na upozornění.  Můžete vytvořit webhook akci, která volá jiné externí služby, postupu runbook akce v Azure automatizaci začít postupu runbook nebo e-mailu akci, kterou chcete odeslat e-mail sami sobě nebo jiným příjemcům.   

## <a name="next-steps"></a>Další kroky

- Získejte další informace o [upozornění v protokolu analýzy](log-analytics-alerts.md) včetně příkazu jiné akce.
- [Vytvoření runbooks v Azure automatizaci](../automation/automation-webhooks.md) , můžete volat z webhook.
