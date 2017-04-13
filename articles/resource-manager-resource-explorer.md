<properties
   pageTitle="Průzkumník Azure zdroje | Microsoft Azure"
   description="Popisuje Azure zdroje Explorer a jak ji můžete použít k zobrazení a aktualizace nasazení prostřednictvím Správce prostředků Azure"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="stuartleeks"
   manager="ankodu"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="stuartle;tomfitz"/>

# <a name="use-azure-resource-explorer-to-view-and-modify-resources"></a>Použití Průzkumníka zdroje Azure zobrazíte a upravíte zdroje
[Průzkumník Azure zdroje](https://resources.azure.com) je skvělým nástrojem pro prohlížíte prostředky, které jste už vytvořili ve vašem předplatném. Pomocí tohoto nástroje lze pochopit, jak jsou strukturovány zdroje a zobrazit vlastnosti přiřazené každému zdroji. Můžete informace o rozhraní REST API operace a rutinách Powershellu, které jsou k dispozici pro typ zdroje a můžete vydávat příkazy prostřednictvím rozhraní. Průzkumník zdroje může být užitečné při vytváření šablon správce prostředků protože umožňuje zobrazit vlastnosti pro existující zdroje.

Zdroje pro nástroj Explorer zdroje je dostupná na [github](https://github.com/projectkudu/ARMExplorer), poskytující důležité odkaz v případě potřeby provádět podobné chování svých aplikací.

## <a name="view-resources"></a>Zobrazení zdrojů
Přejděte na [https://resources.azure.com](https://resources.azure.com) a přihlaste se pomocí stejné přihlašovací údaje, který použijete pro [Portál Azure](https://portal.azure.com).

Po načtení, treeview na levé straně umožňuje přecházet na podrobnější svoje předplatné a skupiny zdrojů:

![TreeView](./media/resource-manager-resource-explorer/are-01-treeview.png)

Při procházení datových krychlí skupina zdroje uvidíte poskytovatelů, pro které nejsou zdroje v dané skupině:

![poskytovatelé](./media/resource-manager-resource-explorer/are-02-treeview-providers.png)

Odtud můžete začít procházení instance zdroje. V následující snímek obrazovky se můžete podívat `sltest` instance serveru SQL Server v ovládacím prvku treeview. Na pravé straně zobrazí se informace o požadavcích rozhraní REST API, které můžete použít pro daný zdroj. Tak, že přejdete na uzel zdroje, zdroje Explorer automaticky provede požadavek GET načítat informace o zdroji. V části velké textu pod adresu URL zobrazí se odpověď rozhraní API. 

Jak seznámit se šablonami správce prostředků text začne najděte známé! V části **Vlastnosti** odpověď vraceny hodnoty zadané v části **Vlastnosti** šablony.

![SQL server](./media/resource-manager-resource-explorer/are-03-sqlserver-with-response.png)

Průzkumník zdroje umožňuje zachovat rozbalování, které můžete prozkoumat podřízené zdrojů v případě databázového serveru SQL, podřízené prostředků pro činnosti, jako jsou databáze a pravidla brány firewall.

Prozkoumání databáze nám zobrazí vlastnosti pro tuto databázi. V následující snímek obrazovky se zobrazují, která databázi `edition` je `Standard` a `serviceLevelObjective` (nebo vrstvy databáze) je `S1`.

![databáze SQL](./media/resource-manager-resource-explorer/are-04-database-get.png)

## <a name="change-resources"></a>Změna zdroje

Jakmile jste přešli zdroji, můžete vybrat tlačítka pro úpravu aby obsah JSON upravovat. Pak můžete zdroje Explorer úpravy ve formátu JSON a pošlete žádost o umístění chcete změnit zdroj. Například na následujícím obrázku vidíte osy databáze změnili `S0`:

![databáze: žádost o umístění](./media/resource-manager-resource-explorer/are-05-database-put.png)

Výběrem **umístění** odešlete žádost. 

Po odeslání žádosti zdroje Explorer znovu odešle požadavek GET aktualizovat stav. V tomto případě vidíme, že `requestedServiceObjectiveId` byl aktualizovaný a se liší od `currentServiceObjectiveId` označující, že probíhá operace změny měřítka. Klikněte na tlačítko získat stav můžete aktualizovat ručně.

![databáze: GET request2](./media/resource-manager-resource-explorer/are-06-database-get2.png)

## <a name="performing-actions-on-resources"></a>Provádění akcí se zdroje

Na kartu **Akce** umožňuje zobrazit a provádět další operace ZBÝVAJÍCÍ. Například vybrán webu zdroje na kartu akce představuje dlouhém operací k dispozici, některé z nich jsou uvedeny níže.

![Web – žádost příspěvku](./media/resource-manager-resource-explorer/are-web-post.png)

## <a name="invoking-the-api-via-powershell"></a>Použití rozhraní API pomocí prostředí PowerShell
Na záložce Powershellu v Průzkumníku zdroje se zobrazí rutiny, aby se slouží k interakci s prostředek, který se aktuálně průzkumu. V závislosti na typu zdroje jste vybrali v zobrazené skript Powershellu rozsahu jednoduché rutiny (například `Get-AzureRmResource` a `Set-AzureRmResource`) pro složitější rutiny (například výměna sloty na webu). 

![Prostředí PowerShell](./media/resource-manager-resource-explorer/are-07-powershell.png)

Další informace o rutinách Powershellu Azure najdete v článku [použití Azure pomocí Správce prostředků Azure](powershell-azure-resource-manager.md)

## <a name="summary"></a>Souhrn
Při práci s správce prostředků, může být Explorer zdroje velmi užitečným nástrojem. Je skvělý způsob, jak najít způsoby, jak pomocí Powershellu dotazu a proveďte změny. Pokud pracujete s rozhraní REST API je skvělý způsob, jak začít a rychle otestovat volání rozhraní API, než začnete psát kód. A pokud píšete šablony, že může být skvělý způsob, jak pochopit hierarchii zdrojů a najděte umístění konfigurace – můžete provést změny v portálu a pak najděte odpovídající položky v Průzkumníku zdroje.

Další informace podívejte se na [kanál 9 videa s Jiří Hanselman a David Ebbo](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Resource-Manager-Explorer-with-David-Ebbo)


