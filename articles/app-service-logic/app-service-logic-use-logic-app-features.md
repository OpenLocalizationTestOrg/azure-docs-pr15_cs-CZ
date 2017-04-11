<properties 
    pageTitle="Použití funkcí aplikace logiky | Microsoft Azure" 
    description="Informace o použití pokročilých funkcí logiky aplikace." 
    authors="stepsic-microsoft-com" 
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
    ms.date="03/28/2016"
    ms.author="stepsic"/> 
    
# <a name="use-logic-apps-features"></a>Použití funkcí aplikace logiky

V [předchozí téma](app-service-logic-create-a-logic-app.md)vytvoření aplikace pro první použití logických operátorů. Teď si ukážeme k vytváření detailní proces, který používá aplikaci služby logiku aplikace. Toto téma uvádí následující nové koncepty aplikace použití logických operátorů:

- Logika s podmínkou, které provede akci pouze při splnění určité podmínky.
- Chcete-li upravit existující aplikace použití logických operátorů o zobrazení kód.
- Možnosti pro spuštění pracovního postupu.

Než se pustíte v tomto tématu, by měl postupujte podle pokynů v tématu [Vytvoření nové aplikace logiky](app-service-logic-create-a-logic-app.md). [Azure portál]přejděte do aplikace pro použití logických operátorů a klikněte na **aktivační události a akce** v podokně Souhrn úprava definice logiky aplikace.

## <a name="reference-material"></a>Referenční materiály

Užitečné tyto dokumenty:

- [Správa a modulu runtime rozhraní REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx) – včetně přímo vyvolat logiky aplikace
- [Referenční příručka jazyka](https://msdn.microsoft.com/library/azure/mt643789.aspx) - úplný seznam všech podporovaných funkcí a výrazů
- [Typy aktivační události a akce](https://msdn.microsoft.com/library/azure/mt643939.aspx) - různé typy akcí a přijmou vstupní hodnoty
- [Základní informace o aplikaci služby](../app-service/app-service-value-prop-what-is.md) – popis jaké komponenty pro výběr, kdy se k vytvoření řešení

## <a name="adding-conditional-logic"></a>Přidání logika s podmínkou

I když původní tok pracuje, existují některé oblasti, které lze zlepšit. 


### <a name="conditional"></a>Podmíněné
Tato aplikace použití logických operátorů, můžete mít se zobrazuje spoustu e-mailů. Následující kroky přidání logiky a ujistěte se, když tweetu pochází od někoho, s počtem sledující pouze dostávat e-mailu. 

1. Klepněte na znaménko plus a najděte akce *Získat uživatele* pro Twitter.

2. Předávání v poli **Tweeted tak, že** z aktivační událost získat informace o uživateli Twitter.

    ![Získání uživatele](./media/app-service-logic-use-logic-app-features/getuser.png)

3. Klikněte na tlačítko plus, ale tentokrát vyberte **Přidat podmínku**

4. V dialogovém okně první kliknutím **** pod **Získat uživatele** vyhledejte pole, **které ho sledují count** .

5. V rozevíracím seznamu vyberte **větší než**

6. Do druhého pole zadejte počet sledující chcete, aby uživatelé měli.

    ![Podmíněné](./media/app-service-logic-use-logic-app-features/conditional.png)

7.  Nakonec a přetažením e-mailu zadejte do pole **Pokud ano** . To znamená to, že jenom dostanete e-mailů při splnění následný count.

## <a name="repeating-over-a-list-with-foreach"></a>Opakující se seznam s forEach

Smyčka forEach určuje maticových opakování akce myší. Pokud ještě není maticových tok se nezdaří. Jako příklad, pokud máte action1, který výstupy maticových zprávy a chcete poslat jednotlivé zprávy, můžete zahrnout tento příkaz forEach ve vlastnostech vzít: forEach:"@action('action1').outputs.messages"
 

## <a name="using-the-code-view-to-edit-a-logic-app"></a>Použití zobrazení kód upravit v aplikaci použití logických operátorů

Kromě Návrháři můžete přímo upravit kód, který definuje v aplikaci logiky následujícím způsobem. 

1. Klikněte na tlačítko **zobrazení Kód** v panelu s příkazy. 

    Otevře se úplné editor zobrazující definici, můžete upravit.

    ![Zobrazení kódu](./media/app-service-logic-use-logic-app-features/codeview.png)

    Pomocí editoru textu můžete zkopírovat a vložit libovolný počet akce v aplikaci stejné logiky nebo mezi aplikacemi logiky. Můžete taky snadno přidat nebo odebrat celé oddíly z definice a definice můžete taky sdílet s ostatními.

2. Po provedení změn v zobrazení kódu jednoduše klikněte na **Uložit**. 

### <a name="parameters"></a>Parametry
Existují některé funkce aplikace použití logických operátorů, které lze použít pouze v zobrazení kódu. Příklad z nich je parametry. Parametry usnadňují opakované použití hodnoty v logických aplikace. Například pokud máte e-mailové adresy, které chcete použít v několik akcí, měli byste ho jako parametr.

Následující aktualizuje existující aplikace logiky termínu dotazu pomocí parametrů.

1. V zobrazení kódu, vyhledejte `parameters : {}` objektu a vložte následující objekt tématu:

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }
    
2. Přejděte `twitterconnector` akce, vyhledejte hodnotu dotazu a nahradit `#@{parameters('topic')}`.
    Je také použít funkci **propojit** ke spojení dvou nebo více řetězce, například: `@concat('#',parameters('topic'))` je stejná jako výše uvedených možností. 
 
Parametry jsou vhodná k vysunout hodnoty, které budete pravděpodobně výrazně měnit. Jsou užitečné zejména pokud nechcete přepsat parametrů v různých prostředích. Další informace o tom, jak přepíší parametry prostředí najdete v článku naše si přečtěte následující [dokumentaci pro rozhraní REST API](https://msdn.microsoft.com/library/mt643787.aspx).

Teď po klepnutí na tlačítko **Uložit**každou hodinu se zobrazí nová tweety, které mají víc než 5 retweets doručeny do složky nazvané **tweety** vaší dropbox.

Další informace o použití logických operátorů aplikace definice, najdete v článku [vytváření definice logiky aplikace](app-service-logic-author-definitions.md).

## <a name="starting-a-logic-app-workflow"></a>Spuštění pracovního postupu aplikace logiky
Existuje několik různých možností pro spuštění pracovního postupu podle logiky aplikace. Pracovní postup můžete vždy lze spustit na vyžádání [Azure portálu].

### <a name="recurrence-triggers"></a>Aktivace opakování
Aktivační události opakování běží v intervalu, který určíte. Logika s podmínkou po aktivaci aktivační událost určuje, zda pracovní postup vyžaduje vrácení spustit. Aktivační označuje by měla běžet vrácením `200` stavový kód. Nemusí spustit, vrátí `202` stavový kód.

### <a name="callback-using-rest-apis"></a>Zpětné pomocí rozhraní REST API
Služby upoutat koncový bod logiky aplikace spuštění pracovního postupu. Další informace najdete v tématu [Použití logických operátorů aplikace jako možné volat koncové body](app-service-logic-connector-http.md) . Tento typ logiku aplikace na vyžádání zahájíte kliknutím na tlačítko **Spustit** na panelu s příkazy. 

<!-- Shared links -->
[Azure portálu]: https://portal.azure.com 