<properties
    pageTitle="Nasazení virtuálního počítače měřítko sadu pomocí aplikace Visual Studio | Microsoft Azure"
    description="Nasazení sady měřítko virtuálního počítače pomocí aplikace Visual Studio a šablony správce prostředků"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="guybo"/>

# <a name="deploy-virtual-machine-scale-set-using-visual-studio"></a>Nasazení virtuálního počítače měřítko sadu pomocí aplikace Visual Studio

Tento článek ukazuje, jak nasazení Azure virtuálního počítače měřítko sadu pomocí Visual Studio zdrojů skupina nasazení.


[Azure virtuálního počítače měřítko sady](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) jsou zdroje Azure výpočet nasazovat a spravovat kolekce podobné virtuálních počítačích s snadno integrované možnosti pro automatické měřítko a vyrovnávání zatížení. Můžete zřízení a nasazení sady měřítko OM pomocí [šablon Azure zdroje správce (ARM)](https://github.com/Azure/azure-quickstart-templates). ARM šablony můžete nasazených pomocí ZBÝVAJÍCÍ Azure rozhraní příkazového řádku, PowerShell a taky přímo z aplikace Visual Studio. Visual Studio k dispozici sadu příklad šablony, které může být nasazené v rámci projektu Azure zdrojů skupina nasazení.

Pole Skupina zdroje Azure nasazení jsou umožňuje seskupit a publikovat sadu související Azure prostředků v operaci jednoho nasazení. Další informace o nich znáte tady: [Vytvoření a nasazení Azure zdroje skupiny pomocí aplikace Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Předpoklady

Jak začít nasazení OM měřítko sady ve Visual Studiu budete potřebovat:

- Visual Studio 2013 nebo 2015
- Azure SDK 2.7, 2,8 nebo 2.9

Poznámka: Tyto pokyny se předpokládá, že Visual Studio 2015 se službou [Azure SDK 2,8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Vytvoření projektu

1. Vytvořte nový projekt ve Visual Studiu 2015 výběrem **soubor | Nové | Projekt**.

    ![Soubor nový][file_new]

2. V části **Visual Basic | Cloud**, vyberte **Správce prostředků Azure** vytvořit projekt pro nasazení šablony ARM.

    ![Vytvoření projektu][create_project]

3.  V seznamu šablony vyberte Linux nebo Windows virtuálního počítače měřítko nastavení šablony.

    ![Vyberte šablonu][select_Template]

4. Po vytvoření projektu uvidíte nasazení skriptů Powershellu, šablonu správce prostředků Azure a parametr souboru pro sadu měřítko virtuálního počítače.

    ![Průzkumník řešení][solution_explorer]

## <a name="customize-your-project"></a>Přizpůsobení projektu

Teď můžete upravit šablonu, kterou chcete přizpůsobit potřebám vaší aplikace, třeba přidávat vlastnosti rozšíření OM nebo úpravy pravidla pro vyrovnávání zatížení. Ve výchozím nastavení nakonfigurované OM měřítko nastavení šablony pro nasazení rozšíření AzureDiagnostics dala snadno sčítat automatické měřítko pravidla. Také nasadí Vyrovnávání zatížení veřejnou IP adresy nakonfigurována příchozí pravidla překladu síťových adres, které umožňují připojení k OM instance SSH (Linux) nebo RDP (Windows) – rozsah portů front-end začíná 50000 což znamená v případě Linux, pokud jste SSH pro portů 50000 veřejnou IP adresu (nebo název domény) se směrovány s portem 22 první OM v sadě měřítko. Připojení k port 50001 budou směrovány na port 22 druhý OM a tak dál.

 Spolehlivých způsobů, jak úprava šablon aplikace Visual Studio je použití osnovy JSON k uspořádání parametry, proměnné a prostředky. Principy schématu Visual Studio můžete ukažte bez chyb v šabloně před jeho zavedením.

![Průzkumník JSON][json_explorer]

## <a name="deploy-the-project"></a>Nasazení projektu

6. Nasazení šablony ARM Azure zdroje OM měřítko vybrána položka vytvořit. Klikněte pravým tlačítkem myši na uzel projektu, zvolte **nasazení | Nové nasazení**.

    ![Nasazení šablony][5deploy_Template]

7. Vyberte předplatné v dialogovém okně "Nasadit do pole Skupina zdroje".

    ![Nasazení šablony][6deploy_Template]

8. Tady můžete taky vytvořit nové skupiny prostředků Azure nasazení šablony.

    ![Nová skupina zdrojů][new_resource]

9. Pak klikněte na tlačítko **Upravit parametry** k zadání parametrů, které bude předán šablony určitých hodnot, například uživatelské jméno a heslo pro operační systém jsou potřebných k vytvoření nasazení. Pokud nemáte prostředí PowerShell nástroje for Visual Studio nainstalovaný, doporučujeme zkontrolovat "Uložit hesla" vyhnout skryté příkazového řádku příkazovém řádku prostředí PowerShell nebo použít [keyvault podpory](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).

    ![Úprava parametrů][edit_parameters]

10. Nyní klikněte na **nasazení**. V okně **výstupu** zobrazí průběh nasazení. Všimněte si, že na akce provádí skript **AzureResourceGroup.ps1 nasazení** .

    ![Okno výstup][output_window]

## <a name="exploring-your-vm-scale-set"></a>Prozkoumání sadě měřítko OM

Po dokončení nasazení zobrazíte novou sadu měřítko OM v Visual Studio **Cloudu Explorer** (obnovení seznamu). Průzkumník cloudu umožňuje spravovat Azure zdrojů ve Visual Studiu při vývoj aplikací. Nastavte měřítko OM můžete zobrazit také v [Portálu Azure](https://portal.azure.com) a [Průzkumník Windows Azure zdroje](https://resources.azure.com/).

![Průzkumník cloudu][cloud_explorer]

 Na portálu obsahuje nejlepší způsob, jak vizuálně spravovat infrastrukturu Azure pomocí webového prohlížeče, zatímco Azure zdroje Explorer poskytuje snadný způsob, jak explorer a ladění Azure zdrojů, pojmenování okna do zobrazení"instance" a také zobrazující příkazy Powershellu pro prostředky, které se díváte. Zatímco sady měřítko OM jsou v náhledu, Průzkumníka zdroje se zobrazí nejvíce podrobností sady OM měřítko.

## <a name="next-steps"></a>Další kroky

Jakmile jste úspěšně nasazený sady měřítko OM pomocí aplikace Visual Studio můžete upravit projektu tak, aby vyhovovaly vašim požadavkům na aplikace. Příklad nastavení automatické měřítko přidáním přehledy zdroje, přidání infrastrukturu do šablony jako samostatný VMs nebo nasazení aplikací s příponou vlastní skript. Zdroj dobré příklad šablon najdete v úložišti GitHub [Azure rychlý úvod šablony](https://github.com/Azure/azure-quickstart-templates) (vyhledejte "vmss").

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
