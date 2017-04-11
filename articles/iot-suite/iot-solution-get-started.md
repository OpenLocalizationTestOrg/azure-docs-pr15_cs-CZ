<properties
    pageTitle="Příklad MyDriving Azure IoT: úvodní | Microsoft Azure"
    description="Začínáme s aplikace, která je komplexní ukázku architektonické systému IoT pomocí Microsoft Azure včetně technologie pro analýzu toku, výukové počítače a rozbočovače události."
    services=""
    documentationCenter=".net"
    suite=""
    authors="harikmenon"
    manager="douge"/>

<tags
    ms.service="multiple"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="03/25/2016"
    ms.author="harikm"/>

# <a name="mydriving-iot-system-quick-start"></a>Systém MyDriving IoT: úvodní

MyDriving je systém, který ukazuje, návrh a implementace typické řešení [Internet věcí](iot-suite-overview.md) (IoT) shromažďuje telemetrie ze zařízení, zpracuje tato data v cloudu, který slouží k použití počítače se naučíte poskytují adaptivní odpověď. Ukázkové protokoly údaje o vaší autem pomocí dat z mobilního telefonu a adaptér shromažďuje informace ze systému správy auta. Používá tyto údaje k poskytnutí zpětné vazby z řidiče k jízdě stylu ve srovnání s ostatními uživateli.

MyDriving účel skutečné vám usnadní zahájení práce v tématu Vytvoření IoT řešení. Ale před tímto, Pojďme se Začínáme s aplikací MyDriving – jako člen náš tým zkušební uživatele. To vám dá prostředí systému aplikace a pozadí jako příjemce, než se pustíte do architektury. Je taky vás seznámí s HockeyApp zajímavých způsob správy alfa a beta rozdělení aplikace testování uživatelů.

## <a name="use-the-mobile-experience"></a>Použití mobilního prostředí

Pokud máte zařízení s Androidem, iOS nebo Windows 10 můžete používat aplikaci MyDriving.

### <a name="android-and-windows-10-mobile-installation"></a>Android a Windows 10 Mobile instalace

Na vašem zařízení:

1.  Povolte vývoj aplikací:

    -   Android: V **dialogovém okně Nastavení** > **zabezpečení**povolit aplikace od **neznámých zdrojů**.

    -   Windows 10: V **dialogovém okně Nastavení** > **aktualizace** > **Pro vývojáře**nastavení **režimu Vývojář**.

2.  Připojit se ke náš tým testování tak, že se přihlásíte nebo přihlášení k [HockeyApp](https://rink.hockeyapp.net). HockeyApp usnadňuje distribuce nejdříve možné verzích aplikaci a otestujte uživatelů.

    Pokud používáte Windows 10, použijte okraj prohlížeče.

    Pokud jste účastníkem sestavení 2016, přihlaste se pomocí stejné Microsoft účtu e-mailu registrované pro konferenci, pomocí jedné z tlačítka Microsoft. Jste už zaregistrovaní s HockeyApp.

    ![HockeyApp přihlašovací obrazovka](./media/iot-solution-get-started/image1.png)

3.  Stažení a instalace aplikace z tohoto umístění:

    -   [Android](http://rink.io/spMyDrivingAndroid)

    -   [Windows 10](http://rink.io/spMyDrivingUWP)

    Existují dvě položky. Nainstalujte certifikát v **Oblasti lidé zobrazuje důvěryhodné**. Nainstalujte aplikaci.

*Všech problémů, ke spuštění aplikace pro Windows 10 Mobile?* Telefonu může být aktualizaci jednu až dvě za. Zkontrolujte, jestli že máte nejnovější aktualizace nebo když si nainstalujete:

 - [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 

 - [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 

 - [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)


### <a name="ios-installation"></a>instalace iOS

Pokud jste se zúčastnili sestavení 2016, stáhněte si ji jako člen náš tým test na HockeyApp:

1.  Na zařízení s iOS Přihlaste se k [HockeyApp](https://rink.hockeyapp.net).
    Použijte jeden z tlačítka přihlášení společností Microsoft a přihlaste se pomocí stejného Microsoft účtu e-mailu, který registrovaný u konference. (Nepoužívejte pole e-mailu a hesla.)

    ![HockeyApp přihlašovací obrazovka](./media/iot-solution-get-started/image1.png)

2.  Na řídicím panelu HockeyApp vyberte MyDriving a stáhněte si ho.

3.  Ověření verze beta z HockeyApp:

    na. Přejděte na **Nastavení** > **Obecné** > **profily a správy zařízení pomocí.**

    b. Důvěřujete **Bit Stadium GmbH** certifikátu.

Pokud jste se vlastně účasti sestavení 2016, můžete vytvořit a nasazení aplikace:

1.   Stáhněte si tento kód [z GitHub].

2.   Vytvořte a nasaďte pomocí [Xamarin].

Další podrobné informace najdete v [MyDriving referenční příručka](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>Získání adaptér OBD (volitelné)

Sem zadejte tu část, které je to skutečné systém Internet věci! Můžete být díky aplikaci bez, ale je víc zábavy s reálným věc a nejsou drahé.

Integrovaný Diagnostika (OBD) je funkce vaše Auto používající garáž optimalizovat vaše Auto a Diagnostika liché zvuky a žárovky upozornění. Pokud vaše Auto je skvělý antiquity, najdete socket jinam, postupujte v kabině obvykle za klopa v řídicím panelu. Pomocí konektoru vpravo můžete získat metriky modulu výkonu a určité doladění. Konektor OBD se dá zakoupit levně z obvykle míst. Připojení pomocí aplikace na telefonu s Bluetooth nebo Wi-Fi.

V tomto případě chceme připojení vaše Auto do cloudu. Přímé připojení OBD je do telefonu, ale funguje naše aplikace tak, aby sloužil. Vaše Auto telemetrie jsou odeslány přímo k rozbočovači MyDriving IoT zpracovávající protokolování řízením cest a posoudit vaše řidiče k jízdě styl

Připojení zařízení OBD:

1.  Ověřte, zda vaše Auto OBD socket.

2.  Získejte adaptér OBD:

    -   Pokud používáte Android a Windows phone, budete potřebovat adaptér Bluetooth s podporou II OBD. Jsme použili [BAFX produkty 34t5 Bluetooth OBDII skenování nástroj].

    -   Pokud používáte telefonu s iOS, potřebovat adaptér OBD povolené Wi-Fi. Jsme použili [ScanTool OBDLink MX Wi-Fi: OBD adaptér/Diagnostic skener].

3.  Postupujte podle pokynů dodaných s adaptér OBD se připojit k telefonu. Mějte na paměti následující:

    -   Adaptér Bluetooth musí spárované s telefonem, na stránce **Nastavení** .

    -   Adaptér Wi-Fi může mít adresu v oblasti 192.168.xxx.xxx.

4.  Pokud ještě několika automobilů dostanete pro každou (maximálně tři) samostatný adaptér.

Pokud nemáte adaptér OBD, aplikace pořád odešle umístění a rychlosti dat z telefonu na GPS sluchátko back-end a zobrazí dotaz, jestli chcete, aby napodobily OBD.

Můžete zjistit další informace o použití dat z adaptér OBD aplikaci a o možnostech pro vytváření zařízení OBD v části 2.1 "IoT zařízení" [Referenční příručka MyDriving](http://aka.ms/mydrivingdocs).

## <a name="use-the-app"></a>Použití aplikace

Spusťte aplikaci. Existuje počáteční rychlý úvod vás provede jak to funguje.

### <a name="track-your-trips"></a>Sledování vaší cest

Klepněte na tlačítko záznam (velký červené zakroužkování v dolní části obrazovky) spusťte výletu a dalším klepnutím na tlačítko Ukončit.

![Obrázek tlačítka záznam pro sledování zpráv](./media/iot-solution-get-started/image2.png)

Při každém spuštění ze služební cesty, pokud existuje žádné OBD zařízení, budete požádáni Pokud chcete použít simulator.

Na konci výletu klepněte na tlačítko Zastavit a najděte souhrn.

![Příklad cesty souhrn](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Zkontrolujte svůj cest

![Příklad minulých ze služební cesty](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Zobrazit profil

![Příklad řízení styl profilu](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Pošlete nám svůj názor test

Protože jsme vytvořili MyDriving usnadnit jumpstart systém IoT, určitě chceme se od vás něco dozvědět o tom, jak správně funguje. Dejte nám vědět, pokud:

- Se dostanete do potíží nebo úkoly.

- Existuje rozšíření bod, který by byl vhodnější nefunguje.

- Najděte efektivnější způsob, jak provádět určitých potřeb.

- Máte další návrhy pro vylepšení MyDriving nebo tato si přečtěte následující dokumentaci.

V rámci samotnou aplikaci MyDriving můžete použít předdefinované mechanismus HockeyApp zpětné vazby: na iOS a Android, jenom poskytnout telefonu se nebo pomocí příkazu nabídky **názory** . Snímek obrazovky, to bude připojit automaticky, takže jsme budete vědět, co mluvíte o. A pokud jsou všechny unfortunate havaruje, HockeyApp shromažďují protokoly pád se s námi o nich znáte. Můžete taky odeslat názor prostřednictvím [portálu HockeyApp].

Můžete taky souboru [problém na GitHub]nebo komentář dole (cs-cz edition).

Podíváme vpřed sluchu od vás!

## <a name="next-steps"></a>Další kroky

-   Prozkoumejte [MyDriving referenční příručka](http://aka.ms/mydrivingdocs) chcete porozumět tomu, jak jste navržený jsme integrované celého systému MyDriving.

-   [Vytvoření a nasazení systému vlastní](iot-solution-build-system.md) pomocí našich správce prostředků Azure skriptů. [Referenční příručka MyDriving](http://aka.ms/mydrivingdocs) také vás provede oblastí, kde budete udělejte většina úpravy.

  [z GitHub]: https://github.com/Azure-Samples/MyDriving
  [použití Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
  [Nástroj naskenovaný obrázek BAFX Products 34t5 Bluetooth OBDII]: http://www.amazon.com/gp/product/B005NLQAHS
  [Skener adaptér/Diagnostic OBD ScanTool OBDLink MX Wi-Fi:]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
  [Portál HockeyApp]: https://rink.hockeyapp.org
  [problému na GitHub]: https://github.com/Azure-Samples/MyDriving/issues
