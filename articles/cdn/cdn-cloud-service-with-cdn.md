<properties
    pageTitle="Integrovat do cloudové služby Azure CDN | Microsoft Azure"
    description="Kurz, který se naučíte nasadit do cloudové služby, které bude sloužit obsahu z koncového integrované Azure CDN"
    services="cdn, cloud-services"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor="tysonn"/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="intro"></a>Integrovat do cloudové služby Azure CDN

Cloudová služba lze integrovat s Azure CDN podávání veškerý obsah z cloudové služby umístění. Tento přístup poskytuje následující výhody:

- Snadno nasazení a aktualizovat obrázky, skripty a šablony stylů v adresářích projektu cloudové služby
- Snadno upgrade balíčků NuGet do cloudové služby, jako je jQuery nebo zavádění verze
- Správa webové aplikace a vaše podávané CDN obsahu: všechny z stejného uživatelského rozhraní aplikace Visual Studio
- Jednotné nasazení pracovního postupu pro webové aplikace a podávané CDN obsahu
- Integrace ASP.NET navazují a minification s Azure CDN

## <a name="what-you-will-learn"></a>Co se dozvíte ##

V tomto kurzu se dozvíte, jak:

-   [Integrovat cloudové služby Azure CDN koncového bodu a vytisknout statický obsah na webových stránkách z Azure CDN](#deploy)
-   [Konfigurace nastavení mezipaměti na statický obsah do cloudové služby](#caching)
-   [Poskytovat obsah z akcím řadiče prostřednictvím Azure CDN](#controller)
-   [Podávání kombinovaný a zmenšenou obsahu prostřednictvím Azure CDN při zachování možnosti ve Visual Studiu ladění skriptu](#bundling)
-   [Nakonfigurujte nouzového skripty a CSS po offline Azure CDN](#fallback)

## <a name="what-you-will-build"></a>Co vytvoříte ##

Budete nasazovat role webové služby cloudu pomocí výchozí šablonu ASP.NET MVC, přidejte kód a bude předávat obsah z integrované CDN Azure, například obrázek, výsledky akce řadiče domény a výchozí soubory šablon stylů CSS a JavaScript a také vytvořit kód pro nastavení náhradní mechanismus při balíky podávané množství v případě, že CDN je offline.

## <a name="what-you-will-need"></a>Co budete potřebovat ##

Tento kurz obsahuje následující požadavky:

-   Aktivní [účet Microsoft Azure](/account/)
-   Visual Studio 2015 s [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)

> [AZURE.NOTE] Musíte mít účet Azure tento kurz:
> + Můžete [Otevřít účet Azure zdarma](/pricing/free-trial/) – načtení přeplatky můžete vyzkoušet placené služby Azure a i po byli zvyklí až můžete ponechat účet a použití uvolnit Azure služby, jako je weby.
> + Je možné [Aktivovat výhod odběratele MSDN](/pricing/member-offers/msdn-benefits-details/) : web MSDN pro vaše předplatné vám přeplatky každý měsíc, využívající služby placené Azure.

<a name="deploy"></a>
## <a name="deploy-a-cloud-service"></a>Nasazení do cloudové služby ##

V této části nasazení výchozí šablonu aplikace ASP.NET MVC ve Visual Studiu 2015 k roli webové služby cloudu a potom integrace s nový koncový bod CDN. Postupujte podle těchto pokynů:

1. Ve Visual Studiu 2015 nové Azure cloudové služby z vytvořit řádek nabídek tak, že přejdete do **Soubor > Nový > Projekt > cloudu > cloudové služby Azure**. Pojmenujte ho a klikněte na **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)

2. Vyberte **Roli Web ASP.NET** a klikněte **>** tlačítko. Klikněte na OK.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)

3. Vyberte **MVC** a klikněte na **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)

4. Teď publikujte tato role Web Azure cloudové služby. Klikněte pravým tlačítkem myši projekt služby cloudu a vyberte **Publikovat**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)

5. Pokud zatím ještě nemáte do Microsoft Azure, klikněte na šipku rozevíracího seznamu **Přidat účet …** a klikněte na položku **Přidat účet** .

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)

6. Na přihlašovací stránce Přihlaste se pomocí účtu Microsoft, který jste použili k aktivaci účtu Azure.
7. Jakmile se přihlásíte, klikněte na tlačítko **Další**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)

8. Za předpokladu, že jste ještě nevytvořili účet služby nebo úložiště cloudu, Visual Studio vám pomůže vytvořit obojí. V dialogovém okně **vytvořit Cloudová služba a účet** zadejte požadovaný název a vyberte požadovanou oblast. Potom klikněte na **vytvořit**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)

9. Na stránce nastavení publikovat ověřte konfiguraci a klikněte na **Publikovat**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)

    >[AZURE.NOTE] Procesu publikování pro cloudovým službám trvá příliš dlouho. Povolení nasazení webu pro možnost Vše role může být ladění vaše cloudové služby mnohem rychlejší zadáním rychlé (ale dočasné) aktualizace k rolím Web. Další informace na tuto možnost najdete v tématu [publikování do cloudové služby používat nástroje Azure](http://msdn.microsoft.com/library/ff683672.aspx).

    Když **Protokolu činnosti Microsoft Azure** ukazuje, že stav publikování **Dokončeno**, vytvoříte CDN koncový bod, který je integrována s cloudové služby.

    >[AZURE.WARNING] Pokud po publikování nasazeném cloudovou službu zobrazí při potížích, je pravděpodobné, protože cloudové služby, které jste nasazený používá [hosta OS, které neobsahuje .NET 4.5.2](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  Můžete alternativním řešením tohoto problému nasazením [.NET 4.5.2 jako úkol po spuštění](../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="create-a-new-cdn-profile"></a>Vytvoření nového profilu CDN

Profil CDN je kolekce CDN koncové body.  Každý profil obsahuje jeden nebo více CDN koncové body.  Můžete chtít používat více profily uspořádat vaše koncové body CDN domény Internetu, webové aplikace nebo jiných kritérií.

> [AZURE.TIP] Pokud už máte CDN profil, který chcete použít pro účely tohoto návodu, pokračujte [vytvořit nový koncový bod CDN](#create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Vytvoření nového koncového bodu CDN

**Vytvoření nového koncového bodu CDN pro váš účet úložiště**

1. Na [Portálu Správa Azure](https://portal.azure.com)přejděte do vlastního profilu CDN.  Se může mít připnuté ho na řídicí panel v předchozím kroku.  Pokud můžete, najdete ji kliknutím na **Procházet**a pak **CDN profily**a kliknete na profil chcete přidat koncový bod k.

    Zobrazí se zásuvné CDN profilu.

    ![CDN profilu][cdn-profile-settings]

2. Klikněte na tlačítko **Přidat koncový bod** .

    ![Přidání tlačítka koncový bod][cdn-new-endpoint-button]

    Zobrazí se zásuvné **Přidat koncový bod** .

    ![Přidání zásuvné koncový bod][cdn-add-endpoint]

3. Zadejte **název** pro tento CDN koncový bod.  Tento název se použije pro přístup k režim cached zdrojů v doméně `<EndpointName>.azureedge.net`.

4. V rozevíracím seznamu **Typ Origin** vyberte *cloudové služby*.  

5. V rozevíracím seznamu **Origin hostname** vyberte cloudové služby.

6. Ponechejte výchozí hodnoty pro **Origin cestu**, **záhlaví hostitele Origin**a **port protokol/Origin**.  Je třeba určit minimálně jeden protokol (HTTP nebo HTTPS).

7. Klikněte na tlačítko **Přidat** k vytvoření nového koncového bodu.

8. Po vytvoření koncového bodu se zobrazí v seznamu koncové body profilu. Zobrazení seznamu zobrazuje URL použít pro přístup k obsahu mezipaměti, jakož i původní doméně.

    ![Koncový bod CDN][cdn-endpoint-success]

    > [AZURE.NOTE] Koncový bod není možné okamžitě používat.  Může trvat až 90 minut registrace se rozšíří prostřednictvím sítě CDN. Uživatelé, kteří pokusíte použít název domény CDN okamžitě může zobrazit stavový kód 404, dokud obsah je k dispozici prostřednictvím CDN.

## <a name="test-the-cdn-endpoint"></a>Testování koncový bod CDN

Po publikování stav **Dokončeno**, otevřete okno prohlížeče a přejděte na * *http://<cdnName>*.azureedge.net/Content/bootstrap.css**. Tato adresa URL je v mé nastavení:

    http://camservice.azureedge.net/Content/bootstrap.css

Které odpovídá následující původní URL na CDN koncového bodu:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Když přejdete na * *http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, v závislosti na prohlížeče, zobrazí se výzva ke stažení nebo otevřete bootstrap.css, které jste dostali z vaší publikované webové aplikace.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

Podobně se dostanete veřejně přístupný URL na * *http://*&lt;název_služby >*.cloudapp.net/** přímo z koncového bodu CDN. Příklad:

-   Soubor JS z cesty/Script
-   Obsah souboru z značku/Content cesta
-   Všechny řadiče/akce
-   Pokud má řetězec dotazu aktivní na koncový bod CDN, všechny adresy URL řetězce dotazu

Ve skutečnosti s nastavením výše budete moct hostovat celý cloudovou službu z * *http://*&lt;cdnName >*.azureedge.net/**. Pokud můžu přejít **http://camservice.azureedge.net/ ** dostanu výsledku akci z domovské/indexu.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

Neznamená, ale, že je vždy dobré (nebo obecně dobré) vytisknout celý cloudové služby prostřednictvím Azure CDN. Upozornění jsou:

-   Tento postup vyžaduje celého webu byl veřejný, protože Azure CDN nemůže v současné době sloužit soukromý obsah.
-   Pokud z nějakého důvodu přejde do režimu offline koncový bod CDN, zda plánovanou údržbu nebo chyba uživatele celý cloudové služby přejde do režimu offline pokud zákazníků můžete přesměrovaní na původní URL * *http://*&lt;název_služby >*.cloudapp.net/**.
-   I s vlastními nastaveními Cache Control (viz [Konfigurovat možností mezipaměti pro statické soubory do cloudové služby](#caching)) koncový bod CDN nezlepší výkon vysoce dynamický obsah. Pokud jste se pokusili načtení domovské stránky z koncového bodu CDN jako výše uvedeným, Všimněte si, že trvala alespoň na úrovni 5 sekund načíst výchozí domovské stránky poprvé, což je velmi jednoduché stránky. Představte si, co by stane klientského rozhraní, pokud tato stránka obsahuje dynamický obsah, který musíte aktualizovat každou minutu. Použití dynamického obsahu z koncového bodu CDN vyžaduje vypršení platnosti krátké mezipaměti, které se převádí na Neúspěšné přístupy do častých mezipaměti na koncový bod CDN. Škodí jak výkonu Cloudová služba a účinně chrání před účel CDN.

Alternativní se rozhodnout, jaký obsah a bude předávat z Azure CDN na případy do cloudové služby. K tomuto účelu jste již viděli, jak získat přístup jednotlivé soubory obsahu z koncového bodu CDN. Můžu vám ukáže, jak do určité řadiče akce prostřednictvím koncový bod CDN [poskytovat obsah z akcím řadiče prostřednictvím Azure CDN](#controller).

<a name="caching"></a>
## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>Konfigurace možnosti ukládání statický soubory do cloudové služby ##

Integrací Azure CDN do cloudové služby můžete určit, jak chcete statický obsah do mezipaměti v CDN koncového bodu. K tomuto účelu otevřete *Web.config* z webového role projektu (například WebRole1) a přidejte `<staticContent>` prvek `<system.webServer>`. Dole XML nakonfiguruje mezipaměti vyprší za tři dny.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Až to uděláte, budou všechny statické soubory do cloudové služby sledovat stejné pravidlo mezipaměť CDN. Pro přesnější kontrolu nastavení mezipaměti přidejte *Web.config* souboru do jiné složky a přidejte svoje nastavení. Například přidat soubor *Web.config* ke složce *\Content* a nahraďte následujícím XML obsahu:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Toto nastavení způsobí, že všechny statický soubory ve složce *\Content* do mezipaměti 15 dní.

Další informace o tom, jak nakonfigurovat `<clientCache>` prvek, najdete v článku [mezipaměti klienta &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

V [poskytovat obsah z akcím řadiče prostřednictvím Azure CDN](#controller)můžu taky zjistíte konfiguraci nastavení mezipaměti pro řadiče akce výsledky v mezipaměti CDN.

<a name="controller"></a>
## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Poskytovat obsah z akcím řadiče prostřednictvím Azure CDN ##

Při integraci roli cloudové služby Web s Azure CDN je poměrně snadná poskytovat obsah z akcím řadiče prostřednictvím Azure CDN. Než podávání cloudové služby přímým Azure CDN (uvedeno výše), [Maarten Balliauw](https://twitter.com/maartenballiauw) ukazuje, jak to udělat pomocí zábavy MemeGenerator řadiči [zkrácení latence z webu Azure CDN](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN). Můžu se jednoduše ho reprodukovat tady.

Předpokládejme vaše cloudové služby, ale chcete zobrazovat memes založené na malých částí boku Norris obrázku (fotografie [Miroslav](http://www.flickr.com/photos/alan-light/218493788/)světla) takto:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

Máte jednoduchou `Index` akci, která umožňuje zákazníků, kteří mají zadat jejich na obrázku vygeneruje meme po jejich zveřejňují příspěvky k akci. Protože je Norris částí boku, očekáváte tuto stránku osvobozením globálně wildly Oblíbené. Toto je dobrým příkladem podávání částečně dynamického obsahu s Azure CDN.

Pomocí výše uvedených kroků nastavit tuto akci řadiče domény:

1. Ve složce *\Controllers* nejradši cs s názvem *MemeGeneratorController.cs* a nahraďte následující kód obsah. Ujistěte se, pokud chcete nahradit zvýrazněná část s názvem vaší CDN.  

        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;

        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();

                public ActionResult Index()
                {
                    return View();
                }

                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }

                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }

                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }

                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }

                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);

                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }

                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }

                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);

                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }

                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }

2. Klikněte pravým tlačítkem myši výchozí `Index()` akce a vyberte **Přidat zobrazení**.

    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)

3.  Přijměte následující nastavení a klikněte na **Přidat**.

    ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)

4. Otevřete nový *Views\MemeGenerator\Index.cshtml* a nahraďte následující jednoduchý kód HTML odeslání jejich obsah:

        <h2>Meme Generator</h2>

        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>

5. Znovu publikovat cloudovou službu a přejděte na * *http://*&lt;název_služby >*.cloudapp.net/MemeGenerator/Index** v prohlížeči.

Po odeslání formuláře hodnoty, které mají `/MemeGenerator/Index`, `Index_Post` metoda akce se vrátí odkaz `Show` akce metodu zadávání identifikátorem odpovídajících. Když kliknete na odkaz, se dostanete tento kód:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Pokud je připojen váš místní ladění, zobrazí se vám běžná ladění zkušenosti s místní přesměrování. Je-li spuštěna do cloudové služby, vás přesměruje na:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Které odpovídá následující původní URL na koncový bod CDN:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


Pak můžete použít `OutputCacheAttribute` atribut na `Generate` způsob, jak určit, jak výsledku akci by měl mezipaměti, které respektovat Azure CDN. Následující kód zadat vypršení platnosti mezipaměti 1 hodina (3600).

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Podobně platí, můžete si můžete vytisknout obsah ze všech akcí řadiče do cloudové služby prostřednictvím svého Azure CDN s požadovanou možnost.

V následující části se vám ukáže, jak k obsluze skupinové a zmenšenou skripty a CSS prostřednictvím Azure CDN.

<a name="bundling"></a>
## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>Integrace ASP.NET navazují a minification s Azure CDN ##

Šablony stylů CSS a skriptů změnám a jsou přednostní kandidáty pro Azure CDN mezipaměti. Nejjednodušší způsob, jak integrovat navazují minification s Azure CDN podávání roli celý prostřednictvím Azure CDN je. Jak chcete nemusí, můžete to udělat, můžu vám ukáže, jak na to při zachování prostředí požadované vývojářský ASP.NET navazují a minification, jako například:

-   Skvělé ladění režimu prostředí
-   Zjednodušené nasazení
-   Okamžité aktualizace klientů pro upgradu na novou verzi skript/šablony stylů CSS
-   Náhradní mechanismus selhání koncový bod CDN
-   Minimalizace úpravy kódu

V **WebRole1** projektu, který jste vytvořili v [Integrace koncového Azure CDN Azure obsahem webu a podávání statické na webových stránkách z Azure CDN](#deploy)otevřete *App_Start\BundleConfig.cs* a podívejte se `bundles.Add()` volání metody.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

První `bundles.Add()` údajů Přidá skript příruček virtuální adresář `~/bundles/jquery`. Potom otevřete *Views\Shared\_Layout.cshtml* zobrazíte vykreslení značku příruček skriptu. Je třeba moct najít následující řádek Razor kódu:

    @Scripts.Render("~/bundles/jquery")

Po spuštění tento kód Razor v roli Azure Web vykreslí `<script>` značku skript příruček podobná této:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Ale pokud ji běží ve Visual Studiu zadáním `F5`, se budou vykreslovat každý soubor skriptu v do skupiny jednotlivě (v případě výše pouze jeden soubor skriptu nachází v do skupiny):

    <script src="/Scripts/jquery-1.10.2.js"></script>

Umožňuje ladění kód JavaScript v vývojové prostředí během omezení připojení souběžné klienta (navazují) a vylepšení soubor stáhnout výkon (minification) do výroby. Je skvělé funkce zachovat integrací Azure CDN. Kromě toho, protože vykreslená příruček již obsahuje řetězec automaticky generované verze, chcete-li replikovat tuto funkci tak je při každé aktualizaci jQuery verze prostřednictvím NuGet jej lze aktualizovat na straně klienta co nejdříve.

Postupujte podle kroků pod navazují ASP.NET integrace a minification s koncovým CDN.

1. Po návratu do *App_Start\BundleConfig.cs*změnit `bundles.Add()` způsobů použití různých [příruček konstruktor](http://msdn.microsoft.com/library/jj646464.aspx), který určuje CDN adresu. K tomuto účelu nahradit `RegisterBundles` definice metody s kódem následující:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Nezapomeňte místo `<yourCDNName>` s názvem Azure CDN.

    Uveďte stručný nastavujete `bundles.UseCdn = true` a přidá pečlivě zpracovanou URL CDN do každého svazku. Například první konstruktoru kód:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))

    je stejná jako:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))

    Tento konstruktor uvedeno ASP.NET navazují a minification vykreslení soubory jednotlivé skriptů při ladění místně, ale použít zadanou adresu CDN pro přístup k předmětné skriptu. Však dvě důležité vlastnosti s touto pečlivě zpracovanou adresou URL CDN:

    -   Pro tuto adresu URL CDN pochází `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, což je skutečně virtuální adresář příruček skriptu do cloudové služby.
    -   Vzhledem k tomu, že používáte konstruktor CDN, značky script CDN pro svazku již obsahuje řetězec automaticky generované verze vykreslená adresy URL. Řetězec jedinečné verze nutné ručně generovat každé změně příruček skript vynutit mezipaměti paní v Azure CDN. Ve stejnou dobu musí zůstat tento řetězec jedinečné verze konstantní prostřednictvím život nasazení maximalizovat přístupy do mezipaměti v Azure CDN po nasazení do skupiny.
    -   V řetězci dotazu = < W.X.Y.Z > si z *Properties\AssemblyInfo.cs* ve webovém role projektu. Můžete mít nasazení pracovního postupu, který obsahuje postupně verzi sestavení pokaždé, když publikujete ve službě Azure. Nebo můžete upravit *Properties\AssemblyInfo.cs* jenom v projektu, abyste pokaždé, když jste sestavili pomocí zástupných znaků zvýšit automaticky řetězec verze "*". Příklad:

            [assembly: AssemblyVersion("1.0.0.*")]

        Další strategie optimalizovat generování jedinečný řetězec po dobu trvání nasazení fungovalo tady.

3. Publikovat cloudové služby a získat přístup na domovskou stránku.

4. Zobrazení kódu HTML na stránce. Je třeba neuvidíte adresu URL CDN vykreslovat řetězcem jedinečné verze pokaždé, když znovu publikovat změny cloudové služby. Příklad:  

        ...

        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>

        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>

        ...

        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>

        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>

        ...

5. Ve Visual Studiu ladění cloudovou službu ve Visual Studiu zadáním `F5`.,

6. Zobrazení kódu HTML na stránce. Zobrazí se pořád každý soubor skriptu jednotlivě vykreslovat tak, že máte konzistentní ladění dojít ve Visual Studiu.  

        ...

            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>

            <script src="/Scripts/modernizr-2.6.2.js"></script>

        ...

            <script src="/Scripts/jquery-1.10.2.js"></script>

            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>

        ...   

<a name="fallback"></a>
## <a name="fallback-mechanism-for-cdn-urls"></a>Náhradní mechanismus při CDN adresy URL ##

Selhání koncový bod Azure CDN z nějakého důvodu chcete webovou stránku je natolik inteligentní, přístup k webu origin jako možnost náhradní načítání JavaScriptu nebo zavádění. Není dost vážně ztratíte obrázků na vašem webu z důvodu nedostupnost CDN, ale mnohem přísnější ztratíte zásadní stránek funkce poskytované skripty a šablony stylů.

Třídy [Sada](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) obsahuje vlastnost s názvem [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) , který umožňuje konfigurovat náhradní mechanismus CDN chyby. Pokud chcete použít tuto vlastnost, postupujte následujícím způsobem:

1. Ve webovém role projektu otevřete *App_Start\BundleConfig.cs*, které jste přidali adresy URL CDN v jednotlivých [příruček konstruktor](http://msdn.microsoft.com/library/jj646464.aspx)a změňte následující zvýrazněný přidání nouzový mechanismus do sady výchozí:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                "~/Scripts/bootstrap.js",
                                "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Když `CdnFallbackExpression` není null skript je vložené do formátu HTML a zkontrolovat, zda je úspěšně načíst do skupiny, pokud ne, přístup do skupiny přímo z webového serveru origin. Tato vlastnost je potřeba nastavit, aby JavaScript výraz, který testuje, zda odpovídajících příruček CDN vložen správně. Výraz potřebné k testování každého svazku odlišně podle toho, obsah. Pro výchozí balíky nahoře:

    -   `window.jquery`je definován v jquery-{verze} js
    -   `$.validator`je definován v jquery.validate.js
    -   `window.Modernizr`je definován v modernizer-{verze} js
    -   `$.fn.modal`je definován v bootstrap.js

    Možná jste si všimli, že mám nenastavil CdnFallbackExpression pro `~/Cointent/css` příruček. Důvodem je, že tam je v současné době [chyb v System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) vloží `<script>` značku náhradní CSS místo očekávaného `<link>` značku.

    Existuje, ale dobré [Nouzového Sada stylů](https://github.com/EmberConsultingGroup/StyleBundleFallback) nabízených [Členskými konzultační skupiny](https://github.com/EmberConsultingGroup).

2. Použít toto alternativní řešení pro šablony stylů CSS, vytvoříte nový soubor cs ve složce *App_Start* projektu role webu s názvem *StyleBundleExtensions.cs*a nahraďte [kód z GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs)její obsah.

4. V *App_Start\StyleFundleExtensions.cs*přejmenujte oboru role webového jméno (například **WebRole1**).

3. Přejděte zpátky na `App_Start\BundleConfig.cs` a upravte poslední `bundles.Add` údajů následující zvýrazněným kódem:  

        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));

    Toto nové přípony používá stejné myšlenky k vloží skript ve formátu HTML ke kontrole DOM pro odpovídající název třídy, název pravidla a pravidla hodnotu podle sada šablon stylů CSS a spadá zpátky na webový server origin selže vyhledala hodnotu.

4. Znovu publikovat cloudové služby a získat přístup na domovskou stránku.

5. Zobrazení kódu HTML na stránce. Vložené skripty měli najít podobně jako tento:    

        ...

        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>

        ...

            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>

        ...


    Stále obsahuje vyvolání chybové zbývajících z vloženého skriptu pro sada šablon stylů CSS `CdnFallbackExpression` vlastnost v řádku:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    Ale od první část || výraz vždy vrátí hodnotu true (v řádku nad tím), funkce document.write() bude nespouštět.

## <a name="more-information"></a>Další informace ##
- [Základní informace o síť pro doručování Azure obsahu (CDN)](http://msdn.microsoft.com/library/azure/ff919703.aspx)
- [Použití Azure CDN](cdn-create-new-endpoint.md)
- [ASP.NET navazují a Minification](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)



[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
