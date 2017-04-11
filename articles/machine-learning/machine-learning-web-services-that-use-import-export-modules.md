<properties
    pageTitle="Nasazení Azure ML webové služby, které používají moduly importu a exportu dat | Microsoft Azure"
    description="Naučte se používat moduly dat Import a Export dat pro odesílání a přijímání data z webové služby."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondlaghaeian"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="v-donglo"/>



# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a>Nasazení Azure ML webové služby, které používají moduly importu a exportu dat 

Při vytváření prediktivní experiment přidáte obvykle webové služby vstupní a výstupní. Při nasazení testu, které se zobrazují koncovým můžete odesílat a přijímat data z webové služby prostřednictvím vstupů a výstupů. Datové připojení uživatele pro některé aplikace může být k dispozici z datového kanálu nebo již nacházejí v externí zdroj dat jako je třeba úložiště objektů Blob Azure. V těchto případech nepotřebují čtení a zápis dat pomocí webové služby vstupů a výstupů. Budou místo toho můžete umožňuje číst data ze zdroje dat pomocí modulu importovat Data služby spuštění dávky (BES) a k různým datovým umístění pomocí modulu exportovat Data zápisu hodnocení výsledků.

Import dat a Export dat moduly, můžete číst a zapisovat na počet data, která poskytují umístění, třeba adresu URL webu přes HTTP dotazu podregistru, databáze Azure SQL, úložiště tabulek Azure, úložiště objektů Blob Azure datového kanálu nebo databáze SQL místní.

Toto téma používá "vzorku 5: vlaku, testu vyhodnotit pro binární klasifikace: dospělého datovou sadu" ukázky a předpokládá datové již byly načetl do tabulky Azure SQL s názvem censusdata.

## <a name="create-the-training-experiment"></a>Vytvoření testu školení 
 
Při otevření "vzorku 5: vlaku, testu vyhodnotit pro binární klasifikace: dospělého datovou sadu" ukázkové používala datovou sadu dospělého soupis příjem binární klasifikace vzorku. A testu v plátno bude vypadat podobně jako na následujícím obrázku.

![Počáteční konfigurace testu.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)
  

Číst data z tabulky Azure SQL:

1.  Odstranění modulu datovou sadu.
2.  Do pole Hledat součásti zadejte importovat.
3.  Ze seznamu výsledků přidáte modul *Importovat Data* do plátno testu.
4.  Připojte výstup modulu *Importovat Data* vstupní modulu *Vyčistit chybějící Data* .
5.  V podokně Vlastnosti vyberte v rozevíracím seznamu **Zdroj dat** **Databáze SQL Azure** .
6.  **Název databázového serveru**, **název databáze**, **uživatelské jméno**a **heslo** zadejte příslušné informace pro databázi.
7.  V poli dotaz databáze zadejte následující dotaz.

        select [age],
           [workclass],
           [fnlwgt],
           [education],
           [education-num],
           [marital-status],
           [occupation],
           [relationship],
           [race],
           [sex],
           [capital-gain],
           [capital-loss],
           [hours-per-week],
           [native-country],
           [income]
        from dbo.censusdata;

8.  V dolní části testu plátna, klikněte na příkaz **Spustit**.

## <a name="create-the-predictive-experiment"></a>Vytvoření prediktivní testu

Potom nastavíte prediktivní experiment, ze které budete nasazovat webové služby.

1.  V dolní části plátno testu klikněte na **Nastavení webové služby** a vyberte **Prediktivní webové služby (doporučeno)**.
2.  *Web služby vstupní* a *výstupní webové služby moduly* odebráním prediktivní testu. 
3.  Do pole Hledat součásti zadejte exportovat.
4.  Ze seznamu výsledků přidáte modul *Exportovat Data* na plátno testu.
5.  Připojte výstup modulu *Skóre modelu* vstupní modulu *Exportovat Data* . 
6.  V podokně Vlastnosti vyberte v rozevíracím seznamu cíl dat **Databáze SQL Azure** .
7.  Do pole **název databázového serveru** **název databáze**, **název serveru uživatelského účtu**a pole **hesla uživatelského účtu serveru** zadejte příslušné informace pro databázi.
8.  V poli **čárkou oddělený seznam sloupce, které chcete ukládat** zadejte dosáhne popisky.
9.  Do **pole název tabulky dat**zadejte dbo. ScoredLabels. Pokud v tabulce neexistuje, se vytvoří při spuštění testu nebo s názvem webové služby.
10. V poli **čárkou oddělený seznam sloupců datové tabulky** zadejte ScoredLabels.

Při psaní aplikace, která volá konečný webové služby, je vhodné zadat jinou vstupní dotazu nebo cílovou tabulku za běhu. Pro nastavení těchto vstupů a výstupů, můžete použít funkci parametry webové služby nastavit vlastnosti *zdroje dat* modul *Importovat Data* a *Export dat* režimu dat cíl.  Další informace o parametry webové služby najdete v článku [položku parametry AzureML webové služby](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) na Cortana měřítka a počítač výukové blogu.

Konfigurace parametry webové služby pro import dotaz a cílové tabulce:

1.  V podokně Vlastnosti modulu *Importovat Data* klikněte na ikonu nahoře napravo od pole **databázových dotazů** a vyberte **nastavit jako parametr webové služby**.
2.  V podokně Vlastnosti modulu *Export dat* klikněte na ikonu nahoře napravo od pole **název tabulky dat** a vyberte **nastavit jako parametr webové služby**.
3.  V dolní části podokna Vlastnosti modul *Exportovat Data* v části **Parametry webové služby** klikněte na databázových dotazů a přejmenujte ho na dotaz.
4.  Klikněte na **název tabulky dat** a přejmenujte ho na **tabulky**.

Až budete hotovi, by měl vypadat váš experiment vidíte na následujícím obrázku.
 
![Konečného vzhledu testu.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

Teď můžete nasadíte testu jako webové služby.

## <a name="deploy-the-web-service"></a>Nasazení webové služby 
Můžete nasadit Classic nebo nové webové služby.

### <a name="deploy-a-classic-web-service"></a>Nasazení klasické webové služby

Můžete nasadit jako klasická webové služby a vytvářet aplikace ho zpracovat:

1.  V dolní části experiment plátna klikněte na spustit.
2.  Po dokončení spustit, klikněte na **Nasazení webové služby** a vyberte **Nasazení webová služba [klasické]**.
3.  Na řídicím panelu webové služby vyhledejte rozhraní API klíč. Kopírování a uložit pro pozdější použití.
4.  V tabulce **Výchozí koncový bod** klikněte na odkaz **Spuštění dávky** otevřete stránku nápovědy rozhraní API.
5.  Ve Visual Studiu vytvořte aplikace konzoly C#.
6.  Na stránce Nápověda rozhraní API najděte v části **Ukázkový kód** v dolní části stránky.
7.  Zkopírujte a vložte kód ukázkové C# do souboru Program.cs a odeberete všechny odkazy k základnímu úložišti objektů blob.
8.  Aktualizujte hodnotu proměnné *apiKey* klávesu rozhraní API předtím uložili.
9.  Vyhledejte žádost o prohlášení a aktualizovat hodnoty webové služby parametrů, které jsou do *Dat Import* a *Export dat* moduly. V tomto případě se používat původní dotaz, ale definovat název nové tabulky.

        var request = new BatchExecutionRequest() 
        {   
            GlobalParameters = new Dictionary<string, string>() {
            { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
            { "Table", "dbo.ScoredTable2" },
            }
        };

10. Spusťte aplikaci. 

Po skončení spustit nová tabulka se přidá k databázi obsahující hodnocení výsledků.

### <a name="deploy-a-new-web-service"></a>Nasazení nové webové služby

Můžete nasadit jako nové webové služby a vytvářet aplikace ho zpracovat:

1.  V dolní části testu plátna, klikněte na příkaz **Spustit**.
2.  Po dokončení spustit, klikněte na **Nasazení webové služby** a vyberte **Nasazení [New] webové služby**.
3.  Na stránce nasazení Experiment zadejte název pro webové služby a vyberte plán ceny a klikněte na **nasazení**.
4.  Na stránce **Rychlý úvod** na **spotřebě**.
5.  V části **Ukázka kód** klikněte na **dávku**.
6.  Ve Visual Studiu vytvořte aplikace konzoly C#.
7.  Zkopírujte a vložte kód ukázkové C# do souboru Program.cs.
8.  Aktualizujte hodnotu proměnné *apiKey* **Primární klíč** , najdete v části **informace o základní spotřeba** .
9.  Vyhledejte deklaraci *scoreRequest* a aktualizovat hodnoty webové služby parametrů, které jsou do *Dat Import* a *Export dat* moduly. V tomto případě se používat původní dotaz, ale definovat název nové tabulky.

        var scoreRequest = new
        {
            Inputs = new Dictionary<string, StringTable>()
            {
            },
            GlobalParameters = new Dictionary<string, string>() {
                 { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable3" },
            }
        };

10. Spusťte aplikaci. 
 

