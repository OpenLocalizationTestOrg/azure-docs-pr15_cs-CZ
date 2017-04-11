<properties 
    pageTitle="Základní informace o Enterprise Integration Pack | Služba Microsoft Azure aplikací | Microsoft Azure" 
    description="Použití funkcí Enterprise integrace Pack povolit obrázku a integrace scénáře pro firmy pomocí služby Microsoft Azure App" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-xml-transforms"></a>Podnikové integrace se službou transformace XML

## <a name="overview"></a>Základní informace
Konektor podnikové integrace transformace převede data z jednoho formátu do jiného formátu. Například může mít příchozí zprávy, která obsahuje aktuální datum ve formátu YearMonthDay. Formát datum ve formátu MonthDayYear můžete transformace.

## <a name="what-does-a-transform-do"></a>Co dělá transformace?
Transformace, což je označovaná taky jako mapu, tvořeno schéma XML zdroj (vstup) a schéma XML cílové (výstup). Různé předdefinované funkce slouží k manipulaci s nimi či ovládacích prvků data, včetně manipulace s řetězci podmíněné přiřazení, aritmetické výrazů, datum čas formatters a i opakování konstrukce.

## <a name="how-to-create-a-transform"></a>Jak vytvořit transformace?
Transformace/mapě můžete vytvořit pomocí aplikace Visual Studio [SDK podnikové integrace](https://aka.ms/vsmapsandschemas). Po dokončení vytvoření a testování transformace nahrajete transformace ke svému účtu integrace. 

## <a name="how-to-use-a-transform"></a>Jak používat transformace
Když nahrajete transformace ke svému účtu integrace, můžete při vytváření aplikace logiku. Aplikaci logiky bude spusťte svůj transformace pokaždé, když se při spuštění aktivují aplikaci použití logických operátorů (a není zadávání obsah, který má být Transformovat).

**Tady najdete postup, jak použít transformaci**:

### <a name="prerequisites"></a>Zjistit předpoklady pro 
V náhledu musíte:  

-  [Vytvoření kontejneru funkce Azure] (https://ms.portal.azure.com/#create/Microsoft.FunctionApp "Vytvoření kontejneru funkce Azure")  
-  [Přidat funkci do kontejneru funkce Azure] (https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-transform-function%2Fazuredeploy.json "Tato šablona vytvoří webhook na základě C# azure funkci s možnostmi transformace používat v scénáře integrace aplikace logiky")    
-  Vytvořte účet integrace a přidání map jiných k němu  

>[AZURE.TIP] Poznamenejte si jméno container funkcích Azure a Azure funkce, které budete potřebovat v dalším kroku.  

Teď jste stará požadavky, je potřeba vytvořit aplikaci použití logických operátorů:  

1. Vytvoření aplikace logiky a [Připojit se ke svému účtu integrace](./app-service-logic-enterprise-integration-accounts.md "informace o propojení klienta integrace logiky aplikaci") , která obsahuje mapu.
2. Přidat aktivační **přijetí žádosti o - požadavek při HTTP** logiku aplikace  
![](./media/app-service-logic-enterprise-integration-transforms/transform-1.png)    
3. Přidání **Transformace XML** akce tak, že nejprve vyberou **Přidat akci**   
![](./media/app-service-logic-enterprise-integration-transforms/transform-2.png)   
4. Word *transformovat* vyhledávacího pole zadejte za účelem filtrování všechny akce té, kterou chcete použít  
![](./media/app-service-logic-enterprise-integration-transforms/transform-3.png)  
5. Výběr **Transformaci XML** akce   
![](./media/app-service-logic-enterprise-integration-transforms/transform-4.png)  
6. Vyberte **Kontejner FUNKCI** , která obsahuje funkce, které budete používat. To je název, který jste vytvořili dříve v těchto krocích kontejneru Azure funkcí.
7. Vyberte **funkce** , které chcete použít. To je název funkce Azure jste dříve vytvořili.
8. Přidání **obsahu** , který bude transformace XML. Všimněte si, že můžete data XML, která budete dostávat v žádost HTTP jako **obsahu**. V tomto příkladu vyberte text žádost HTTP, který spustil aplikaci použití logických operátorů.
9. Vyberte název **MAPU** , kterou chcete použít k provedení transformace. Mapy již musí být ve vašem účtu integrace. V předchozím kroku už přiřadil logiky aplikace přístup ke svému účtu integrace, která obsahuje mapu.
10. Uložení prezentace  
![](./media/app-service-logic-enterprise-integration-transforms/transform-5.png) 

V tomto okamžiku dokončíte nastavení mapy. V aplikaci reálné je vhodné ukládat Transformovaná data v aplikaci LOB – třeba služby SalesForce. Můžete snadno akce výstup transformace odešlete služby Salesforce. 

Nyní můžete otestovat vaše transformace tím, že žádost HTTP koncový bod.  

## <a name="features-and-use-cases"></a>Funkce a případy použití

- Transformace vytvořené v mapě může být jednoduchý, například kopírování jména a adresy z jednoho dokumentu do jiného. Nebo můžete vytvořit složitější transformace pomocí operace mimo pole mapa.  
- Více mapování operace nebo funkce jsou snadno dostupné, včetně řetězce, funkce čas data a tak dál.  
- Můžete udělat kopii přímé dat mezi schémat. V mapování zahrnutý v sadě SDK je jednoduchá – Stačí nakreslit čáru spojující prvky ve schématu zdroje s jejich protějšky cíle schéma.  
- Při vytváření mapu, zobrazení grafické znázornění mapování, které zobrazit všechny vztahy a odkazy, které vytvoříte.
- Použijte funkci Test Map přidat Ukázka zprávy XML. Jednoduchý kliknutím můžete otestovat mapu, kterou jste vytvořili a najdete v článku generovaný výstup.  
- Nahrát existující mapy  
- Podporuje formát XML.


## <a name="learn-more"></a>Víc se uč
- [Další informace o Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Další informace o Enterprise Integration Pack")  
- [Přečtěte si další informace o mapách] (./app-service-logic-enterprise-integration-maps.md "Přečtěte si víc o enterprise integration mapy")  
 