<properties
    pageTitle="Web App klonováním portálu Azure"
    description="Zjistěte, jak vytvořit kopii Web Apps na nový Web Apps pomocí portálu Azure."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/08/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Aplikace služby Azure aplikace klonováním pomocí Azure portálu#

Funkce klonování ve [Webových aplikacích pro Azure aplikace služby](http://go.microsoft.com/fwlink/?LinkId=529714) umožňuje snadno klonovat existující web aplikace do nově vytvořený aplikace v jiné oblasti nebo ve stejné oblasti. To vám umožní zákazníkům nasadit celá řada aplikací mezi různými oblastmi snadno a rychle.

Kopírování aplikace je v současné době podporuje jen premium osy aplikace služby plány. Nové funkce využívá stejné omezení jako zálohování webových aplikací najdete v tématu [obecnějším údajům web app v aplikaci služby Azure](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 


## <a name="cloning-an-existing-app"></a>Kopírování existující aplikace ##

Web appu musí běžet v režimu **Premium** , aby je možné vytvořit klonovat pro web app.

1. Na [Portálu Azure](https://portal.azure.com/)otevřete webovou aplikaci zásuvné.
2. Klikněte na **Nástroje**. **Nástroje** zásuvné kliknutím na **Klonovat aplikace**.

    ![][1]

    > [AZURE.NOTE]
    > Pokud web appu dosud v režimu **Premium** , zobrazí se zpráva s oznámením podporované režimy pro klonováním aplikace. Nyní máte možnost vybrat **upgradovat**.
    
3. V **Aplikaci klonovat** zásuvné zadejte název nové webové aplikace, skupina zdroje a plán služeb aplikací. Také uživatel bude moct vyberte, jestli chcete vytvořit kopii počet nastavení zdrojový web app nebo ne.

    ![][2]

4. Po kliknutí na příkaz **vytvořit** platformu začnou pracovat týkající se vytváření klonovat zdrojový web appu.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Kopírování existující aplikace prostředí aplikace služby##

V **Aplikaci klonovat** zásuvné zákazníka bude mít možnost zvolit fondu aplikací v prostředí existující služby aplikace.

## <a name="current-restrictions"></a>Aktuální omezení ##

Tato funkce momentálně v náhledu, pracujeme na Přidat nové funkce v čase, v následujícím seznamu jsou známé omezení aktuální podpory aplikace klonováním Azure portálu:

- Nejsou klonovat Azure nastavení přenosy správce
- Nastavení automatického nejsou klonovat
- Nastavení zálohování plánu nejsou klonovat
- Nastavení VNET nejsou klonovat
- Aplikace přehledy nejsou nastavení automaticky na cílový web appu
- Nejsou klonovat snadno Auth nastavení
- Nejsou klonovat kudu rozšíření
- Tip: pravidla nejsou klonovat
- Nejsou klonovat obsah databáze


### <a name="references"></a>Odkazy ###
- [Web App klonováním pomocí prostředí PowerShell](app-service-web-app-cloning.md)
- [Obecnějším údajům web app v aplikaci služby Azure](web-sites-backup.md)
- [Postup vytvoření prostředí aplikace služby](app-service-web-how-to-create-an-app-service-environment.md)
- [Vytvoření do webových aplikací v prostředí aplikace služby](app-service-web-how-to-create-a-web-app-in-an-ase.md)
- [Úvod do prostředí aplikace služby](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png