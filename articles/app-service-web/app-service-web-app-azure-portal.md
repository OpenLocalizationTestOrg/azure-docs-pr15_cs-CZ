<properties
    pageTitle="Odkazy pro navigaci portálu Azure"
    description="Další různých uživatelského prostředí pro webovou aplikaci služby mezi portálu pro správu a portálu Azure"
    services="app-service"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/26/2016"
    ms.author="jaime-espinosa"/>

# <a name="reference-for-navigating-the-azure-portal"></a>Odkazy pro navigaci portálu Azure

Azure weby jsou teď se nazývá [Aplikace služby Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Zdokonalujeme všechny naše si přečtěte následující dokumentaci k této změně název a k poskytnutí pokynů pro portálu Azure. Až do dokončení tohoto procesu můžete jako vodítko tento dokument pro práci s aplikací Web Apps na portálu Azure.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 
 
## <a name="the-future-of-the-azure-classic-portal"></a>Budoucí portálu klasický Azure

Když zjistíte brandingu změny na portálu klasický Azure, tento portál probíhá je někdo jiný nahradil portálem Azure. Jak portálu klasický je postupně, fokusu pro vývoj posunutí na portál Azure. Na portálu Azure chodily všechny nadcházejících nových funkcí pro Web Apps. Začínáme používat portál Azure využít nejnovější a největší, máte Web Apps nabízí.

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>Rozložení rozdíly mezi Azure klasické portálem a portál Azure

Na portálu klasické Azure služby najdete na levé straně. Navigace v portálu klasické sleduje stromové struktuře, kde začít od služby a přejděte do jednotlivé elementy. Tuto strukturu funguje dobře při správě nezávislé součásti. Aplikací jsou založeny na Azure se však sada propojené služeb, a tento stromové struktuře není ideální pro práci s kolekcí služeb. 

Portál Azure snadno vytvářet aplikace začátku do konce s součásti z více služeb. Na portálu jsou uspořádána jako *cest*. *Cesty* je řadu *listy*, které jsou kontejnery pro různé prvky. Příklad nastavení automatické měřítko pro web app je *cesty* , který vás několik listy jak je vidět v následujícím příkladu: zásuvné **webu** (aby zásuvné název nebyla aktualizována použít nové termíny), zásuvné **Nastavení** a **měřítko,** zásuvné. V příkladu automatické měřítko je nastaven závisí na využití procesoru, takže je také zásuvné **Procento využití procesoru** . Součásti uvnitř se *listy* někdy říká *částí*, které vypadají dlaždice. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Příklad navigace: Vytvořte webovou aplikaci

Vytvoření nové webové aplikace je stále stejně snadná jako 1-2-3. Následující obrázek znázorňuje klasické portálem a portálu vedle sebe prokázat, že není hodně změnil číslo kroky potřebné k získání do webových aplikací a systémem. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

Na portálu můžete vybírat z nejběžnější typů webových aplikací web apps, včetně aplikací Oblíbené Galerie jako WordPress. Úplný seznam dostupných aplikací Navštěvujte blog o [Azure Marketplace].

Když vytvoříte do webových aplikací, zadejte adresu URL, plán služeb aplikací a umístění na portálu jenom jako v portálu klasické. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Kromě toho na portálu můžete nastavit ostatní nastavení. Příklad [skupiny zdrojů](../azure-resource-manager/resource-group-overview.md) usnadňují zobrazit a spravovat související Azure zdroje. 

## <a name="navigation-example-settings-and-features"></a>Příklad navigace: funkcí a nastavení

Nastavení a funkcí jsou teď logicky seskupené do jednoho zásuvné, odkud můžete přejít.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Můžete například vytvořit vlastní domény kliknutím zásuvné **Nastavení** **vlastní domény a SSL** .

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

Nastavit upozornění, klikněte na **žádosti o a chyby** a potom na **Upozornění na Přidat**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Diagnostika, klepněte na možnost **protokolování diagnostiky** v zásuvné **Nastavení** .

![](./media/app-service-web-app-azure-portal/Diagnostics.png)
 
Konfigurace nastavení aplikace, klikněte na **Nastavení aplikace** v zásuvné **Nastavení** . 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Kromě názvu značku byly co je potřeba v portálu přejmenovat nebo seskupené jinak snadněji je najít. Tady je snímek obrazovky s odpovídající stránku aplikací například nastavení (**Konfigurovat**) na portálu klasické.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>Další materiály

[Azure Portal]: https://portal.azure.com
[Azure Marketplace]: /marketplace/

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="whats-changed"></a>Co se změnilo
* Průvodce na změnu z webů pro aplikaci služby v tématu: [aplikaci služby Azure a jeho dopad na existující služby Azure](http://go.microsoft.com/fwlink/?LinkId=529714)
 
