<properties 
    pageTitle="Vytvoření aplikace použití logických operátorů ve Visual Studiu | Microsoft Azure" 
    description="Vytvoření projektu ve Visual Studiu vytvořte a nasaďte logiky aplikaci." 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/> 
    
# <a name="build-and-deploy-logic-apps-in-visual-studio"></a>Vytvoření a nasazení aplikace použití logických operátorů ve Visual Studiu

I když se na [Portál Azure](https://portal.azure.com/) dává skvělý způsob, jak návrh a spravovat své aplikace logiku, můžete také můžete navrhnout a nasaďte logiku aplikaci z aplikace Visual Studio místo.  Použití logických operátorů aplikace pochází s formátovaným nástrojů Visual Studia, který umožňuje vytvářet aplikace logiky pomocí návrháře, konfigurace nasazení a automatizace šablon a nasadit na každém prostředí.  

## <a name="installation-steps"></a>Postup instalace

Níže najdete pokyny k instalaci a nastavení Visual Studio nástroje pro použití logických operátorů aplikace.

### <a name="prerequisites"></a>Zjistit předpoklady pro

- [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
- [Nejnovější Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 nebo novější)
- [Azure Powershellu] (https://github.com/Azure/azure-powershell#installation)
- Přístup k webu při použití Tvůrce vložený

### <a name="install-visual-studio-tools-for-logic-apps"></a>Instalace nástroje aplikace Visual Studio logiky aplikací pro

Jakmile máte zjistit předpoklady pro instalaci 

1. Otevřete aplikaci Visual Studio 2015 nabídku **Nástroje** a vyberte **rozšíření a aktualizace**
1. Vybrat kategorii **Online** hledání online
1. Vyhledání **Aplikací logiku** zobrazte **Azure logiku aplikace Tools for Visual Studio**
1. Klikněte na tlačítko **Stáhnout** si stáhněte a nainstalujte rozšíření
1. Restartujte aplikaci Visual Studio po instalaci

> [AZURE.NOTE] Můžete také stáhnout rozšíření přímo z [tohoto odkazu](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9)

Po instalaci budete moct používat project Azure pole Skupina zdroje s aplikací Návrhář použití logických operátorů.

## <a name="create-a-project"></a>Vytvoření projektu

1. Přejděte na nabídku **soubor** a vyberte **Nový** >  **projektu** (nebo přejděte na **Přidat** a potom zvolte **Nový projekt** přidáte do existujícího řešení):  ![nabídka Soubor](./media/app-service-logic-deploy-from-vs/filemenu.png)

1. V dialogovém okně Najít **cloudu**a vyberte **Pole Skupina zdroje Azure**. Zadejte **název** a potom klikněte na **OK**.
    ![Přidání nového projektu](./media/app-service-logic-deploy-from-vs/addnewproject.png)

1. Vyberte šablonu **logiky aplikace** . Tím vytvoříte šablona nasazení aplikace prázdné logiky začínat.
    ![Vyberte šablonu Azure](./media/app-service-logic-deploy-from-vs/selectazuretemplate.png)

1. Jakmile jste vybrali **šablonu**, stiskněte tlačítko **OK**.

    Projekt aplikace logiky teď se přidá do vašeho řešení. Měli byste vidět nasazení soubor v Průzkumníku řešení:  

    ![Nasazení](./media/app-service-logic-deploy-from-vs/deployment.png)

## <a name="using-the-logic-app-designer"></a>Používání návrháře aplikace logiky

Až budete mít Azure pole Skupina zdroje projekt, který obsahuje aplikaci použití logických operátorů, můžete otevřít návrháře aplikace Visual Studio při vytváření pracovního postupu.  Návrhář vyžaduje připojení k Internetu za účelem dotazu spojnice k dispozici vlastnosti a data (například pokud pomocí konektoru Dynamics CRM Online, návrháři bude dotaz CRM instanci seznamu dostupné vlastní a výchozí vlastnosti).

1. Klikněte pravým tlačítkem myši na `<template>.json` souboru a vyberte **Otevřít v aplikace Návrhář logiky** (nebo `Ctrl+L`)
1. Vyberte předplatné, skupina zdroje a umístění pro nasazení šablony
    - Je důležité mít na paměti, že návrhu aplikace použití logických operátorů vytvoří **Rozhraní API připojení** zdroje k vytvoření dotazu pro vlastnosti průběhu návrhu.  Vybraná skupina zdroje bude použitá k vytvoření těchto připojení během návrhu skupina zdroje.  Můžete zobrazit nebo upravit všechna připojení rozhraní API tak, že přejdete na portál Azure a procházení pro **Rozhraní API připojení**.
    ![Výběr předplatného](./media/app-service-logic-deploy-from-vs/designer_picker.png)
1. Návrháři vykreslují na základě definice v `<template>.json` soubor.
1. Nyní můžete vytvořit a návrh aplikace logiku a změny se aktualizují v šabloně nasazení.
    ![Návrhář ve Visual Studiu](./media/app-service-logic-deploy-from-vs/designer_in_vs.png)

Zobrazí se také `Microsoft.Web/connections` prostředků do souboru prostředků pro každé připojení potřebné pro aplikaci logiku funkce.  Tyto vlastnosti připojení můžete nastavit při nasazení a spravovaných po nasadíte v **Rozhraní API připojení** na portálu Azure.

### <a name="switching-to-the-json-code-view"></a>Přepnutí do zobrazení kódu JSON

Můžete vybrat na kartě **Zobrazení Kód** v dolní části okna návrháře přejdete do JSON znázornění použití logických operátorů aplikace.  Pokud chcete přepnout zpátky na úplné zdroje JSON, klikněte pravým tlačítkem myši `<template>.json` souboru a vyberte **Otevřít**.

### <a name="saving-the-logic-app"></a>Uložení aplikace logiky

Uložíte aplikaci logiky na kdykoli tlačítkem **Uložit** nebo `Ctrl+S`.  Pokud v okamžiku uložení chybám aplikaci pro použití logických operátorů, se zobrazí v okně **výstupy** aplikace Visual Studio.

## <a name="deploying-your-logic-app"></a>Nasazení aplikace pro použití logických operátorů

Nakonec po konfiguraci aplikace nástroje můžete nasazovat přímo z aplikace Visual Studio v několika krocích. 

1. Klikněte pravým tlačítkem myši na projektu v Průzkumníku řešení a přejděte na **Deploy** > **Nové nasazení...** 
     ![Nové nasazení](./media/app-service-logic-deploy-from-vs/newdeployment.png)

2. Zobrazí se výzva k přihlášení k Azure předplatného. 

3. Teď budete muset zvolte podrobnosti, kterou chcete nasadit aplikaci použití logických operátorů k skupina zdroje. 
    ![Nasazení pole Skupina zdroje](./media/app-service-logic-deploy-from-vs/deploytoresourcegroup.png)

     > [AZURE.NOTE]    Je potřeba vybrat správné soubory šablony a parametry skupina zdroje (například Pokud nasazujete provozním prostředí, že je vhodné zvolit soubor parametry výrobní). 
4. Klikněte na tlačítko Instalovat
 
    
6. Stav nasazení se zobrazí v okně **výstupu** (budete muset zvolte **Azure zřizování**. 
    ![Výstup](./media/app-service-logic-deploy-from-vs/output.png)

V budoucnu můžete opravit aplikace logiky do ovládacího prvku zdroje a pomocí aplikace Visual Studio nasazení nové verze. 

> [AZURE.NOTE] Pokud upravíte definici na portálu Azure přímo, bude možné přepsat při příštím nasadíte z aplikace Visual Studio provedené změny.

## <a name="next-steps"></a>Další kroky

- Začínáme s aplikacemi jiných použití logických operátorů, postupujte podle kurz [Vytvoření logiky aplikace](app-service-logic-create-a-logic-app.md) .  
- [Zobrazení společných příklady a scénáře](app-service-logic-examples-and-scenarios.md)
- [Automatizace firemních procesů pomocí aplikace logiky](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Zjistěte, jak integrovat systémů s aplikacemi jiných logiky](http://channel9.msdn.com/Events/Build/2016/P462)