<properties
    pageTitle="Koncové body služby Azure"
    description="Popisuje nastavení koncový bod služby Azure v Azure sada nástrojů pro Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# <a name="azure-service-endpoints"></a>Koncové body služby Azure #

Koncové body služby Azure zjistit, že jestli vaše aplikace je používaný a spravuje globální Azure platformu, Azure provozovaný společností 21Vianet v Číně nebo osobní Azure platformu. Dialogové okno **Koncové body služby** vám umožní určit koncové body které služby, kterou chcete použít. Otevření dialogového okna **Koncové body služby** v rámci zatmění, klikněte na tlačítko **okno**, klikněte na **Předvolby**, rozbalte **Azure**a pak klikněte na **Koncové body služby**. Nastavení pole **Aktivní nastavení** Určuje, které koncové body služby Azure se použije pro Azure projekty v aktuálním pracovního prostoru.

Na následujícím obrázku je dialogové okno **Koncové body služby** .

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>Chcete-li nastavit koncové body služby ##

V dialogovém okně **Koncové body služby** proveďte jednu z následujících akcí:

* Pokud chcete použít globální Azure informacemi o platformě, z **Active nastavení** rozevíracího seznamu, vyberte **windowsazure.com** a klikněte na **OK**.
* Pokud chcete používat Azure provozovaný společností 21Vianet v Číně, z **Active nastavení** rozevíracího seznamu, vyberte **windowsazure.cn (Čína)** a klikněte na **OK**.
* Pokud chcete použít soukromé Azure platformu:
    1. Klikněte na **Upravit**.
    2. Otevře dialogové okno informací o tom, že bude dialogu **Koncové body služby** zavřeli, a otevře se soubor sady předvoleb. Klikněte na **OK**.
    3. V souboru preferencesets.xml, vytvořte novou `preferenceset` prvek. : Tento element nové vytvoření `name`, `blob`, `management`, `portalURL` a `publishsettings` atributy a zadat hodnoty pro ně, které odpovídají soukromé Azure platformu. Použijete hodnoty zadané pro stávající `preferenceset` prvky jako šablony. **Poznámka**: hodnota použitá pro `blob` atributu musí obsahovat text "blob" v adrese URL.
    4. Uložte a zavřete preferencesets.xml.
    5. Znovu otevřete dialogové okno **Koncové body služby** .
    6. Z rozevíracího seznamu **Aktivní nastavení** vyberte aktivní sadu, kterou jste vytvořili a klikněte na **OK**.
    7. Po vytvoření soukromého Azure platformu `preferenceset` prvek, můžete změnit hodnoty přiřazená po kliknutí na tlačítko **Upravit** v dialogovém okně **Koncového bodu služby** . Můžete taky vytvořit více soukromé Azure platformu `preferenceset` prvky, pokud.

## <a name="see-also"></a>Viz taky ##

[Azure sada nástrojů pro Eclipse][]

[Instalace Azure sada nástrojů pro Eclipse][] 

[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění][]

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure][].

<!-- URL List -->

[Středisko pro vývojáře Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalace Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png
