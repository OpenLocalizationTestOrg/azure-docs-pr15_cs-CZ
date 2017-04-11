<properties
   pageTitle="Poradce při potížích s Cloudovým službám pomocí aplikace přehledy | Microsoft Azure"
   description="Zjistěte, jak řešit problémy se službou cloudu pomocí aplikace přehledy proces dat z Azure diagnostiky."
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />


# <a name="troubleshoot-cloud-services-using-application-insights"></a>Poradce při potížích s Cloudovým službám pomocí aplikace přehledy

Diagnostika příponu [Azure SDK 2,8](https://azure.microsoft.com/downloads/) a Azure 1,5 můžete nyní poslat dat Azure diagnostiky cloudové službě přímo interpretace aplikace. Různé typy protokoly shromážděná diagnostiky Azure včetně protokoly aplikací, protokoly událostí windows, trasování událostí pro Windows a protokolu výkonnosti teď můžete posílat interpretace aplikace a vizualizovat v portálu aplikace přehledy uživatelského rozhraní. Při použití společně s přehledy SDK aplikaci teď získáte, podstatu metriky a protokoly pocházejících z aplikace, jakož i systém a infrastruktury úrovně dat pocházejících z Azure diagnostiky.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Konfigurace Azure diagnostiky odeslání dat do aplikace přehledy

Tento postup nastavení projektu služby cloudu odeslání Azure diagnostiky dat pro přehledy aplikace.

1) V okně Průzkumník řešení Visual Studio klikněte pravým tlačítkem myši na roli a vyberte příkaz **Vlastnosti** otevřete návrháře rolí

![Vlastnosti rolí Průzkumníka řešení][1]

2) V nástroji Návrhář Role v části diagnostics zaškrtněte políčko Odeslat **data diagnostiky interpretace aplikace**

![Návrhář roli odeslat data diagnostiky interpretace aplikace][2]

3) V dialogovém okně překryvné okno Vyberte zdroj přehledy aplikace, který chcete odeslat data Azure diagnostiky do. Dialogové okno umožňuje vybrat existující přehledy aplikace zdroj ze svého předplatného nebo ručně zadat kód Product key pro přístrojového vybavení aplikace přehledy zdroje. Pokud nemáte existující přehledy aplikace zdroj pak můžete vytvoříte na kliknutím na **vytvořit nový zdroj** odkaz, který se otevře okno prohlížeče na portál Azure klasické kterých je možné vytvořit přehledy prostředek aplikace. Další informace o vytváření zdrojů aplikace přehledy najdete v článku [Vytvoření nové aplikace přehledy zdroje](../application-insights/app-insights-create-new-resource.md)

![Vyberte zdroj přehledy aplikace][3]

4) Jakmile přidáte zdroje aplikace přehledy přístrojového vybavení klíč pro daný zdroj uložených jako nastavení konfigurace služby s názvem **APPINSIGHTS_INSTRUMENTATIONKEY**. Toto nastavení konfigurace u jednotlivých konfigurace služeb a prostředí můžete změnit tak, že vyberete jinou konfiguraci ze seznamu konfigurace služby dolů a zadáte nový klíč přístrojového vybavení pro tuto konfiguraci.

![Vyberte konfiguraci služby][4]

Konfigurace nastavení **APPINSIGHTS_INSTRUMENTATIONKEY** používá Visual Studio koncovku diagnostiky nakonfigurovat příslušné informace o zdroji aplikace přehledy při publikování. Konfigurace nastavení je pohodlný způsob definování různých přístrojového vybavení klávesové zkratky pro jiné službě konfigurace. Visual Studio přeložit toto nastavení a vložit ho do veřejné konfigurace rozšíření diagnostiky při publikování. Zjednodušit proces konfigurace koncovku diagnostiky pomocí prostředí PowerShell balíček výstup z aplikace Visual Studio taky obsahuje veřejné konfiguraci XML s příslušnou přístrojového vybavení aplikace přehledy klíč zahrnout. Veřejné konfigurace soubory vytvořené ve složce rozšíření a postupujte podle vzorku PaaSDiagnostics. <RoleName>. PubConfig.xml. Všechny prostředí PowerShell založeny nasazení lze použít tento každou konfiguraci přiřadit roli.

5) Povolení **odeslání diagnostiky dat pro přehledy aplikace** automaticky nastaví její konfiguraci Azure diagnostiky odeslat všechny výkonnosti a úroveň protokolů chyb, které se shromažďují agentem Azure diagnostiky interpretace aplikace. Pokud chcete dále konfigurovat jaká data se pošle přehledy aplikace a pak budete muset ručně upravit soubor *diagnostics.wadcfgx* pro každou roli. Najdete v článku [Konfigurace Azure diagnostiky odeslání dat pro přehledy aplikace](../azure-diagnostics-configure-applicationinsights.md) zobrazíte další informace o ruční aktualizaci konfigurace.

Jak zajistit koncovku Azure Diagnostika je povolený po odeslání dat Azure diagnostiky interpretace aplikace, že kterou můžete ho nasadit do Azure obvyklým způsobem, jako je konfiguraci cloudové služby. V tématu [publikování do cloudové služby pomocí Visual Studia](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Zobrazení dat Azure Diagnostika v přehledy aplikace
Azure diagnostiky telemetrie se objeví v aplikaci přehledy zdroje nakonfigurován pro vaše cloudové služby.

Následující obrázek je jak protokolu různých Azure diagnostiky typy mapy na koncepty přehledy aplikace:  

-  Výkonnosti jsou zobrazeny jako metriky vlastní v přehledy aplikace
-  Protokoly událostí Windows se zobrazí jako trasování a vlastní události v aplikaci přehledy
-  Aplikace protokoly, protokoly trasování událostí pro Windows a všechny infrastruktury diagnostických nástrojů se zobrazí jako trasování v přehledy aplikace.

Zobrazení dat Azure Diagnostika v aplikaci přehledy:

- Použití [Průzkumníka metriky](../application-insights/app-insights-metrics-explorer.md) vizualizovat vlastní výkonnosti nebo počty různé typy událostí v protokolu událostí systému windows.

![Vlastní metriky v Průzkumníku metriky][5]

- Použití [vyhledávání](../application-insights/app-insights-diagnostic-search.md) k prohledat různých protokoly trasování odeslaný Azure diagnostiky. Pro příklad Pokud jste měli neošetřené výjimce v roli, která způsobila roli selhat a odpadkového těchto informací by objeví v *aplikaci* kanálu *protokolu událostí systému Windows*. Pomocí funkce hledání můžete prohlédnout Chyba v protokolu událostí systému Windows a získat úplné zásobníku výjimky umožňuje najít hlavní příčinu problému.

![Trasování hledání][6]

## <a name="next-steps"></a>Další kroky

- [Přidání aplikace SDK přehledy vaše cloudové služby,](../application-insights/app-insights-cloudservices.md) odešlete data o požadavky, výjimek, závislosti a vlastní telemetrie z aplikace. Sloučí s diagnostiky Azure data můžete získat dokončení zobrazení systému a aplikace všechny stejný zdroj přehled aplikace.  


<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
