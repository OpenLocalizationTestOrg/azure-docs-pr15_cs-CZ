<properties
    pageTitle="Začínáme s test ve výrobním pro Web Apps"
    description="Informace o zkušební funkce výrobní (TiP) v Azure aplikace služby Web Apps."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="cephalin"/>

# <a name="get-started-with-test-in-production-for-web-apps"></a>Začínáme s test ve výrobním pro Web Apps

Testování ve výrobním nebo live testování webovou aplikaci pomocí provozu živou zákazníků, je strategie test, vývojáře aplikace stále integrovat do jejich [aktivní vývojové](https://en.wikipedia.org/wiki/Agile_software_development) metodologii služby. Umožňuje otestování kvality aplikace s živou provozu v provozním prostředí, protikladu k hledání syntetizují dat v testovacím prostředí. Vystavením novou aplikaci reálným uživatelům můžete informován o skutečné potíže, které aplikace může být vystaven po jejím nasazení. Můžete ověřit funkce, výkon a hodnota svoje aktualizace aplikací vůči objem, rychlosti a řadu skutečné provozu, které můžete nikdy aproximaci počtu v testovacím prostředí.

## <a name="traffic-routing-in-app-service-web-apps"></a>Vysílání směrování ve webových aplikacích pro aplikaci služby

S funkcí směrování přenosu v [Aplikaci služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)můžete odkázat část živou uživatele umožnění datových přenosů do jednoho nebo více [sloty nasazení](web-sites-staged-publishing.md)a analyzovat aplikace s [Azure aplikace přehledy](/services/application-insights/) [Azure Hdinsightu](/services/hdinsight/)nebo nástroji třetích stran stejně jako [Nové Relic](/marketplace/partners/newrelic/newrelic/) potvrďte změny. Můžete třeba implementovat tyto scénáře s aplikaci služby:

- Seznamte se s funkční chyby nebo zdůraznit kritické v aktualizace před nasazením webu
- Provedení "řízený test lety" změn měřením usibility metriky přihlášená beta
- Postupně množství práce postupně zvyšovat až nové aktualizace a řádně zpátky na aktuální verzi Pokud dojde k chybě 
- Optimalizace vaše aplikace obchodních výsledků spuštěním [A / B testuje](https://en.wikipedia.org/wiki/A/B_testing) nebo [multivariační testů](https://en.wikipedia.org/wiki/Multivariate_testing_in_marketing) v několika paticích nasazení

### <a name="requirements-for-using-traffic-routing-in-web-apps"></a>Požadavky pro použití směrování přenosu ve webových aplikacích

- Webovou aplikaci nutné spustit **Standard** nebo **Premium** osy, jako je nutný pro více sloty nasazení.
- Abyste mohli pracovat správně, vyžaduje směrování přenosu v prohlížeči uživatelů povolení souborů cookie. Směrování přenosu používá soubory cookie Připnutí prohlížeče klienta pro nasazení úsek po dobu relace klienta.
- Směrování přenosu podporuje pokročilé TiP scénáře pomocí rutin prostředí PowerShell Azure.

## <a name="route-traffic-segment-to-a-deployment-slot"></a>Směrování přenosu segment úsek nasazení

Na úrovni základní v každé TiP firem směrovat předdefinované procentuální část živou přenosů na testovacím nasazení úsek. K tomuto účelu postupujte následujícím způsobem:

>[AZURE.NOTE] Tímto postupem předpokládá, že už máte [testovacím nasazení úsek](web-sites-staged-publishing.md) a že požadovanou webovou aplikaci obsah je už pro něj [nasazené](web-sites-deploy.md) .

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. V zásuvné webovou aplikaci, klikněte na **Nastavení** > **Směrování přenosu**.
  ![](./media/app-service-web-test-in-production/01-traffic-routing.png)
3. Vyberte úsek, který chcete směrovat přenosy a procento celkové přenosy své oblíbené a potom klikněte na **Uložit**.

    ![](./media/app-service-web-test-in-production/02-select-slot.png)

4. Přejděte na zásuvné úsek nasazení. Teď byste měli vidět živou přenosy směrovány na něj.

    ![](./media/app-service-web-test-in-production/03-traffic-routed.png)

Po konfiguraci směrování přenosu dosažení zadaného procenta klientů náhodně směrovány na testovacím pozici. Je ale důležité mít na paměti, že po klienta je automaticky směrovat konkrétní úsek, ho bude třeba "připnuté" na tuto pozici po dobu trvání této relace klienta. To udělat pomocí soubor cookie Připnutí relaci uživatele. Pokud kontrola požadavků HTTP zjistíte `TipMix` souborů cookie v každé další žádosti.

![](./media/app-service-web-test-in-production/04-tip-cookie.png)

## <a name="force-client-requests-to-a-specific-slot"></a>Platnost požadavků klientů konkrétní úsek

Kromě automatické přenosy směrování, je možné směrování požadavků na konkrétní úsek aplikaci služby. To je užitečná, když chcete, aby měli uživatelé moct určovat, jestli se do nebo odhlášení z aplikace beta. K tomuto účelu použijte `x-ms-routing-name` dotazu parametr.

Přesměrovat uživatelům určité úsek pomocí `x-ms-routing-name`, ujistěte se, že úsek už přidali k seznamu směrování přenosu. Když chcete směrovat pozici explicitně, skutečné směrování procentuální nastavených nezáleží. Pokud chcete, můžete vytvořit "beta odkazu", pomocí kterého uživatelé můžou získat přístup k aplikaci beta.

![](./media/app-service-web-test-in-production/06-enable-x-ms-routing-name.png)

### <a name="opt-users-out-of-beta-app"></a>Určovat, jestli se uživatelé mimo aplikaci beta

Pokud chcete, aby uživatelé vyjádření výslovného nesouhlasu beta aplikace, například můžete dát tento odkaz na webovou stránku:

    <a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>

Řetězec `x-ms-routing-name=self` určuje úsek výroby. Jakmile prohlížeče klienta přístup k propojení, se přesměruje do úsek výrobní nejen, ale bude obsahovat všechny následující požadavky `x-ms-routing-name=self` cookie, který PinY relace úsek výroby.

![](./media/app-service-web-test-in-production/05-access-production-slot.png)

### <a name="opt-users-in-to-beta-app"></a>Určovat, jestli se uživatelé v aplikaci beta

Jestli chcete, aby uživatelé přihlášení do aplikace beta, nastavte stejný parametr dotazu na název testovacím úsek, například:

        <webappname>.azurewebsites.net/?x-ms-routing-name=staging

## <a name="more-resources"></a>Další materiály ##

-   [Nastavení pracovní prostředí pro web apps v aplikaci služby Azure](web-sites-staged-publishing.md)
-   [Nasazení aplikace složité předvídatelností v Azure](app-service-deploy-complex-application-predictably.md)
-   [Aktivní software development aplikace službou Azure](app-service-agile-software-development.md)
-   [Efektivní použití DevOps prostředí pro webové aplikace](app-service-web-staged-publishing-realworld-scenarios.md)