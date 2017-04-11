<properties
    pageTitle="Poradce při potížích s Retraining Azure počítače výukové klasické webové služby | Microsoft Azure"
    description="Identifikovat a vyřešit běžné problémy s encounted při modelu jsou přeškolení webové služby Azure počítače učení."
    services="machine-learning"
    documentationCenter=""
    authors="VDonGlover"
   manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="v-donglo"/>

# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Poradce při potížích s dalším vzdělávání Azure počítače výukové klasické webové služby

## <a name="retraining-overview"></a>Přeškolení základní informace

Při nasazení prediktivní experiment jako bodování webová služba je statický model. Nová data k dispozici nebo pokud spotř rozhraní API má svá data, musí být retrained modelu. 

Kompletní informace o rekvalifikaci proces klasické webové služby najdete v článku [Retrain počítače výukové modely programově](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Přeškolení obrázku

Pokud potřebujete retrain webové služby, musíte přidat některé další částí:

* Webová služba nasazeny z školení testu. Testu musí mít modulu **Webové služby výstup** připojené k výstup modulu **Modelu vlaku** .  

    ![Připojení k modelu vlaku výstupu webové služby.][image1]

* Nový koncový bod přidali hodnocení webové služby.  Můžete přidat koncový bod programově pomocí ukázkový kód programově odkazovat v modelech Retrain výukové počítače téma nebo prostřednictvím portálu Azure klasické.

Pak můžete ukázkového C# kódu z webové služby školení rozhraní API stránku nápovědy se přeškolit modelu. Jakmile vyhodnocení výsledky a spokojeni, aktualizujte školení model hodnocení webové služby pomocí nového koncový bod, který jste přidali.

Všechny části na místě hlavních kroků, kterými musí retrain modelu jsou následujícím způsobem:

1.  Volání webové služby školení: volání je do dávku spuštění služby (BES), ne požádat o odpověď služby (záznamy). Ukázka C# kód můžete na stránce Nápověda k rozhraní API Pokud chcete volat. 
2.  Vyhledejte hodnoty pro *BaseLocation*, *RelativeLocation*a *SasBlobToken*: tyto hodnoty z volání webové službě školení vracejí do výstupu. 
      ![zobrazení výstupu rekvalifikaci vzorku a BaseLocation, RelativeLocation a SasBlobToken hodnoty.][image6]
3.  Aktualizovat přidané koncový bod z bodování webové služby pomocí nový model školení: pomocí kódu vzorku podle modely Retrain výukové počítače programově aktualizovat nový koncový bod přidaný do bodování modelu nově školení modelu z webové služby školení.

## <a name="common-obstacles"></a>Běžné překážky

### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Zkontrolujte, jestli máte správná adresa URL oprava

Adresu URL oprava používaném musí být přidružené nový bodování koncový bod, které jste přidali hodnocení webové služby. Existuje několik způsobů, jak získat adresu URL oprava:

**Možnost 1: programově**

Chcete-li získat správnou adresu URL oprava:

1.  Spusťte [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) ukázkový kód.
2.  Z výstupu AddEndpoint najděte hodnotu *HelpLocation* a zkopírujte adresu URL.

    ![HelpLocation do výstupu addEndpoint vzorku.][image2]

3.  Vložte adresu URL v prohlížeči přejděte na stránku, která obsahuje odkazy na nápovědu k webové služby.
4.  Klikněte na odkaz **Zdroj aktualizace** otevřete stránku nápovědy opravy.

**Možnost 2: Použití portálu Azure klasické**

1.  Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).
2.  Otevřete kartu výukové počítače. 
     ![Karta opřené počítače.][image4]
3.  Klikněte na název pracovního prostoru, pak **Webové služby**.
4.  Klikněte na bodování webové služby, kterou pracujete. (Pokud není změnit výchozí název webové služby, ho bude končí řetězcem [bodování Exp.].)
5.  Klikněte na **Přidat koncový bod**.
6.  Po přidání koncový bod, klikněte na název koncového bodu. Potom klikněte na **Aktualizace zdroje** otevřete stránku oprav nápovědy.

>[AZURE.NOTE] Pokud jste přidali koncový bod k webové službě školení místo prediktivní webové služby, zobrazí se tato chyba při kliknutí na odkaz **Zdroj aktualizace** : je nám líto, ale tato funkce není podporovaná nebo dostupná v tomto kontextu. Tato webová služba nemá žádné aktualizovatelný zdroje. Omlouváme se za nepříjemnosti a pracujete vylepšení tento pracovní postup.

![Nový řídicí panel koncového bodu.][image3]

Na stránce nápovědy oprava obsahuje oprava adresa URL je třeba použít a poskytuje ukázkový kód, který slouží k zavolejte ho.

![Adresa URL opravy.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Zkontrolujte, že aktualizujete správné bodování koncový bod

* Oprava není webová služba školení: musí být provést operaci oprava bodování webové služby.
* Oprava nejsou výchozí koncový bod na webová služba: operaci opravy musí provést na nové bodování Web koncový bod služby, které jste přidali.

Můžete zkontrolovat, které webové služby na koncový bod zapnuté pomocí portálu pro Azure klasické. 

>[AZURE.NOTE] Ujistěte se, že přidáváte koncový bod prediktivní webové služby, není školení webové služby. Pokud jste správně nasazena školení a prediktivní webové služby, měli byste zkontrolovat dvě samostatná webové služby. Prognostické webová služba by měl končit "[prognostické funkce exp.]".

1.  Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).
2.  Otevřete kartu výukové počítače. 
     ![Pracovní prostor výukové počítače uživatelského rozhraní.][image4]
3.  Vyberte pracovního prostoru.
4.  Klikněte na **Web služby**.
5.  Vyberte prediktivní webové služby.
6.  Přidejte nový koncový bod byl webové služby.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>Kontrola pracovního prostoru, který webová služba je v a zkontrolujte, že ho správné oblasti:

1.  Přihlaste se k [Azure klasické portálu](https://manage.windowsazure.com).
2.  V nabídce vyberte výukové počítače.
      ![Počítač výukové oblast uživatelského rozhraní.][image4]
3.  Zkontrolujte umístění pracovního prostoru.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png