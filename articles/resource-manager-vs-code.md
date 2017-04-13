<properties
   pageTitle="Použití kódu a šablonami správce prostředků | Microsoft Azure"
   description="Ukazuje, jak nastavit Visual Studio kódu pro vytváření šablon správce prostředků Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="cmatskas"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="chmatsk;tomfitz"/>

# <a name="working-with-azure-resource-manager-templates-in-visual-studio-code"></a>Práce s Azure správce prostředků šablon ve Visual Studiu kódu

Azure šablony správce prostředků jsou JSON soubory, které popisují zdroje a související závislosti. Tyto soubory někdy může být velkých a komplikovaně tak, aby nástroje podpory důležité. Visual Studio kód je editor nové lightweight, otevřít zdroj, různé platformy kódu. Podporuje vytvoření a úprava šablon správce prostředků prostřednictvím [nového rozšíření](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools). Kód a spustí všude a nevyžaduje přístup k Internetu, pokud chcete také nasazení šablon správce prostředků.

Pokud už nemáte a kód, můžete ji nainstalovat na [https://code.visualstudio.com/](https://code.visualstudio.com/).

## <a name="install-the-resource-manager-extension"></a>Instalace rozšíření Správce zdrojů

Pracovat se šablonami JSON v kódu a, budete potřebovat k instalaci rozšíření. Podle těchto kroků stažení a instalace podpora jazyků pro správce prostředků JSON šablony:

1. Spuštění a kód 
2. Otevření rychlé otevřít (Ctrl + P) 
3. Spusťte tento příkaz: 

        ext install azurerm-vscode-tools

4. Restartujte kód a po zobrazení výzvy k povolení rozšíření. 

 Dokončení úkolu.

## <a name="set-up-resource-manager-snippets"></a>Nastavení správce prostředků výstřižků

Předchozích kroků nedá nainstalována podpora nástrojů, ale teď potřebujeme ke konfiguraci a kód použít fragmenty šablony JSON.

1. Zkopírujte obsah souboru v úložišti [azure xplat arm nástrojů](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) do schránky.
2. Spuštění a kód 
3. V kódu a můžete otevřít soubor fragmenty JSON tak, že přejdete na **soubor** -> **Předvolby** -> **Uživatele fragmenty** -> **JSON**, nebo tak, že vyberete **F1** na a napíšou **Předvolby** , dokud nebude vybrána položka **Předvolby: fragmenty**.

    ![fragmenty předvoleb](./media/resource-manager-vs-code/preferences-snippets.png)

    V možnosti vyberte **JSON**.

    ![Vyberte json](./media/resource-manager-vs-code/select-json.png)

4. Vložení obsahu ze souboru v kroku 1 do souboru fragmenty uživatele před poslední "}" 
5. Ujistěte se, vypadá ve formátu JSON OK a nejsou žádné pravopis kdekoli. 
6. Uložte a zavřete soubor fragmenty uživatele.

To je všechno potřebné začít používat fragmenty správce prostředků. Toto nastavení jsme pak budete umístění testu.

## <a name="work-with-template-in-vs-code"></a>Práce se šablonou v kódu a

Nejjednodušší způsob, jak začít pracovat se šablonou je uchopte některé z Snadné spuštění dostupných šablon na [Github](https://github.com/Azure/azure-quickstart-templates) nebo použijte jeden z vlastní. Je možné snadno [export šablony](resource-manager-export-template.md) z některého ze svých skupin zdrojů prostřednictvím portálu. 

1. Pokud jste exportovali šablony ze skupiny zdrojů, otevřete extrahované soubory v kódu a.

    ![zobrazení souborů](./media/resource-manager-vs-code/show-files.png)

2. Otevřete soubor template.json tak, aby se daly upravovat a přidání některé další zdroje informací. Po **"zdroje": [** stisknutím klávesy enter začít nový řádek. Pokud zadáte **arm**, zobrazí se uvedené s seznam možností. Tyto možnosti jsou fragmentů šablony, které jste si nainstalovali. By měl vypadat takto: 

    ![Zobrazit výstřižků](./media/resource-manager-vs-code/type-snippets.png)

3. Zvolte fragment kódu, kterou chcete. V tomto článku výběru **arm ip** k vytvoření nové veřejnou IP adresu. Zadejte čárku za pravou hranatou závorku "}" nově vytvořený zdroje, aby zkontrolovala šablony syntaxe je platný.

     ![Přidání čárkou](./media/resource-manager-vs-code/add-comma.png)

4. Kód a má vestavěné technologie IntelliSense. Při úpravě šablon a kód navrhne dostupné hodnoty. Například do šablony přidat oddíl proměnných, přidat **""** (dvě uvozovky) a vyberte **Kombinace kláves Ctrl + mezerník** mezi těmito nabídek. Zobrazí se možnosti včetně **proměnných**.

    ![Přidáním proměnných](./media/resource-manager-vs-code/add-variables.png)

5. Technologie IntelliSense můžete také navrhnout dostupné hodnoty nebo funkce. Nastavovaná vlastnost na hodnotu parametru Tvorba výrazu s **"[]"** a **Ctrl + mezerník**. Můžete začít psát název funkce. Vyberte **kartu** , když jste našli funkci, kterou chcete.

    ![Přidání parametru](./media/resource-manager-vs-code/select-parameters.png)

6. Znovu vyberte **Kombinace kláves Ctrl + mezerník** ve funkci zobrazíte seznam dostupných parametrů v rámci vaší šablony.

    ![Přidání parametru](./media/resource-manager-vs-code/select-avail-parameters.png)

7. Pokud máte problémy ověření schématu v šabloně, zobrazí se seznámit pravopis v editoru. Zadáním **Kombinaci kláves Ctrl + Shift + M** nebo výběrem piktogramy v dolním levém stavovém řádku můžete zobrazit seznam chyb a upozornění.

    ![chyby](./media/resource-manager-vs-code/errors.png)

    Ověření šablony se dozvíte, zjišťování syntaxe problémy. ale může taky zobrazit chyby, které můžete ignorovat. V některých případech je editor porovnání šablony proti schéma, které není aktuální a proto hlásí chybu, i když víte, že je správné. Předpokládejme například, funkce naposledy přidala Správci zdroje, ale schématu aktualizován nebyl. Editor hlásí chybu bez ohledu na to, které funkce funguje správně během nasazení.

    ![chybová zpráva](./media/resource-manager-vs-code/unrecognized-function.png)

## <a name="deploy-your-new-resources"></a>Nasazení nového zdroje

Pokud vaši šablonu hotovou, můžete nasadit nové zdroje postupujte podle pokynů níže: 

### <a name="windows"></a>Windows

1. Otevřete okno příkazového řádku prostředí PowerShell 
2. Na přihlašovací typ: 

        Login-AzureRmAccount 

3. Pokud máte víc předplatných, zobrazte seznam předplatných pomocí:

        Get-AzureRmSubscription

    A vyberte předplatné používat.
   
        Select-AzureRmSubscription -SubscriptionId <Subscription Id>

4. Aktualizovat parametry v souboru parameters.json
5. Spuštění Deploy.ps1 nasazení šablony na Azure

### <a name="osxlinux"></a>OSX/Linux

1. Otevřete okno terminálu 
2. Na přihlašovací typ:

        azure login 

3. Pokud máte víc předplatných, vyberte správné předplatné se:

        azure account set <subscriptionNameOrId> 

4. Aktualizujte parametrů v souboru parameters.json.
5. Abyste mohli nasadit šabloně, spusťte:

        azure group deployment create -f <PathToTemplate> 

## <a name="next-steps"></a>Další kroky

- Další informace o šablonách najdete v tématu [vytváření správce prostředků Azure šablony](resource-group-authoring-templates.md).
- Další informace o funkcích šablony najdete v tématu [funkce šablony správce prostředků Azure](resource-group-template-functions.md).
- Další příklady práce s kódem Visual Studio najdete v článku [sestavení cloudu aplikace Visual Studio kódem](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) z [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 připojit [ukázku](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Rychlé další prohlídky z ukázku HealthClinic.biz najdete v článku [Rychlé Azure vývojář nástroje prohlídky](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
