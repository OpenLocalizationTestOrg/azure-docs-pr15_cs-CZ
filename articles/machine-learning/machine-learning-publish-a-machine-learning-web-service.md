<properties
    pageTitle="Nasazení webové služby Machine Learning | Microsoft Azure"
    description="Jak převést školení experimentu prediktivní experimentu, Příprava na nasazení a potom nasadit jako webovou službu Azure Machine Learning."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>

# <a name="deploy-an-azure-machine-learning-web-service"></a>Nasazení webové služby Azure Machine Learning

Azure Machine Learning slouží k vytvoření, testování a nasazení řešení prediktivní analytické.

Z vysoké úrovni-z hlediska to se provádí ve třech krocích:

- **[Vytvoření testu školení]** - Azure Machine Learning Studio je spolupráce vizuální vývojové prostředí, které slouží k vlaku a testování prognostické analýzy modelu pomocí školení data, která jste zadali.
- **[Převést na prediktivní experimentu]** - jednou model byl vyzkoušen s existujícími daty a budete chtít použít k získání nových dat, Příprava a zefektivnit váš experiment pro předpovědi.
- **Nasadit jako webovou službu** - můžete nasadit vaše prediktivní experimentu jako [nové] nebo [klasické] Azure webové služby. Uživatelé mohou odesílat data do modelu a zobrazí váš model předpovědi.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Vytvoření testu školení

Na vlak prognostické analýzy modelu, umožňuje Studio učení Azure Machine školení pokus vytvořit pokud zahrnují různé moduly načítání dat školení, připravit data podle potřeby, použít algoritmy machine learning a vyhodnocení výsledků. Můžete iteraci na základě pokusu a zkuste různé strojovém učení algoritmy pro porovnání a vyhodnocení výsledků.

Proces vytváření a správy vzdělávání experimenty se zabývá důkladněji jinde. Další informace naleznete v článcích:

- [Vytvoření jednoduchého experimentu v Azure Machine Learning Studio](machine-learning-create-experiment.md)
- [Vyvinout prediktivní řešení s Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)
- [Naimportujte data školení Azure Machine Learning Studio](machine-learning-data-science-import-data.md)
- [Správa iterací experimentu v Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Převést na prediktivní experiment pokus školení

Jakmile jste vyškolených modelu, jste připraveni převést vaše školení experimentu prediktivní pokus získat nová data.

Převedením prediktivní testu dostáváte vyškolených modelu připravena k nasazení jako bodování webové služby. Uživatelé webové služby odesílat vstupní data modelu a model odešle zpět výsledky předpověď. Jak převést prediktivní experimentu, mějte na paměti jak očekáváte, že váš model používat ostatní uživatelé.

Pokud chcete převést vaše školení experimentu prediktivní experimentu, klepněte na příkaz **Spustit** na konci experimentu plátna a klepněte na tlačítko **Nastavit webové služby**a vyberte **Prediktivní webové služby**.

![Převedení bodového hodnocení testu](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

Další informace o tom, jak provést tento převod v tématu [Převést Machine Learning školení, pokus prediktivní experimentu](machine-learning-convert-training-experiment-to-scoring-experiment.md).

Následující kroky popisují nasazení prediktivní experimentu jako novou webovou službu. Můžete také nasadit experimentu jako klasické webové služby.

## <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>Nasadit prediktivní experimentu jako nové webové služby

Teď prediktivní experimentu bylo připraveno, je možné nasadit jako Azure webové služby. Pomocí webové služby, mohou uživatelé odesílat data do modelu a model vrátí jeho předpovědi.

Chcete-li nasadit vaší prediktivní experimentu, klepněte na tlačítko **Spustit** na konci experimentu plátno. Po dokončení spuštění testu **Nasazení webové služby** klepněte na tlačítko a vyberte **nasazení webové služby [Nový]**.  Otevře se stránka nasazení portálu Machine Learning webové služby.

### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Machine Learning webové služby portálu nasazení stránky experimentu

Na stránce nasazení testu zadejte název webové služby.
Vyberte plán ocenění. Pokud máte existující plán ocenění, že které lze vybrat, jinak musíte vytvořit nový plán ceny služby.

1.  **Cena plánu** rozevírací seznam, vyberte existující plán nebo vyberte možnost **Vybrat nový plán** .
2.  V poli **Název plánu**zadejte název, který bude identifikovat plán ve vašem vyúčtování.
3.  Vyberte jednu z **Vrstev měsíční plán**. Do této oblasti je nasazena výchozí úrovně plán plány výchozí oblasti a webové služby.

Klepněte na tlačítko **instalovat** a stránce **Quickstart** služby otevře váš web.

Quickstart služby webové stránky poskytuje přístup a návod na běžné úkoly prováděné po vytvoření webové služby. Zde jsou snadno přístupné zkušební stránku i stránky spotřebovat.

<!-- ![Deploy the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-web-service"></a>Testování webové služby

Vyzkoušet nové webové služby, klepněte na tlačítko **testovat webové služby** ve skupinovém rámečku Běžné úkoly. Na zkušební stránku můžete otestovat webové služby jako požadavek odpověď Service (RR) nebo služby Batch Execution (BES společnosti).

Zkušební stránka záznamů zobrazí vstupů, výstupů a globální parametry, které jste definovali pro tento pokus. Testování webové služby, můžete ručně zadat příslušné hodnoty pro vstupy nebo zadat čárkami oddělené hodnoty (CSV) formátovaný soubor obsahující zkušební hodnoty.

Chcete-li otestovat pomocí záznamů o prostředku, z režimu zobrazení seznamu, zadejte příslušné hodnoty pro vstupy a klepněte na tlačítko **Test požadavku odezvy**. Výsledky předpověď zobrazit ve výstupním sloupci vlevo.

![Nasazení webových služeb](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

Své struktuře vedle TLAČÍTKA, klepnutím na **dávky**. Na stránce ověření dávkové zadání klepněte na tlačítko Procházet a vyberte soubor CSV obsahující hodnoty odpovídající vzorek. Pokud nemáte soubor CSV a vytvořili vaše prediktivní experimentu pomocí Machine Learning Studio, můžete stáhnout sadu dat pro vaše prediktivní experimentu a jeho použití.

Chcete-li stáhnout sadu dat, otevřete Machine Learning Studio. Otevřete váš prediktivní experimentu a klepněte pravým tlačítkem myši vstup pro váš experiment. Z kontextové nabídky vyberte **dataset** a potom vyberte **Stáhnout**.

![Nasazení webových služeb](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

Klepněte na tlačítko **Test**. Stav spuštění dávkové úlohy se zobrazí vpravo v části **Test dávkových úloh**.

![Nasazení webových služeb](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

Na stránce **Konfigurace** můžete změnit popis, nadpis, aktualizovat klíč úložiště účtů a povolit ukázková data pro webovou službu.

![Konfiguraci webové služby](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Jakmile jste nasadili webovou službu, můžete:

- **Přístup** prostřednictvím rozhraní API webových služeb.
- **Správa** prostřednictvím Azure Machine Learning web služby portálu nebo na portálu Azure klasické.
- **Aktualizace** je-li váš model změní.

### <a name="access-the-web-service"></a>Přístup k webové službě

Po nasazení do webové služby Machine Learning Studio, můžete odeslat data službě a programově přijímat odpovědi.

Stránka **spotřebovat** poskytuje všechny informace, které potřebujete získat přístup k webové službě. Například klíč API uvedená oprávnění přístupu ke službě.

Další informace o přístupu k webové služby Machine Learning v tématu [spotřebovat nasazena webová služba Azure Machine Learning](machine-learning-consume-web-services.md).

### <a name="manage-your-new-web-service"></a>Spravovat nové webové služby

Klasický web portálu služby Machine Learning webové služby můžete spravovat. Z [hlavní stránky portálu](https://services.azureml-test.net/)klepněte na tlačítko **Webové služby**. Z webové stránky služby můžete odstranit nebo zkopírovat do služby. Chcete-li sledovat určité služby, klepněte na službu a potom klepněte na tlačítko **řídicí panel**. Ke sledování dávkových úloh, které jsou přidružené k webové službě, klepněte na tlačítko **Protokolu požádat o dávky**.

## <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>Prediktivní experiment nasadit jako webovou službu Classic

Nyní, prediktivní experimentu byly dostatečně připraveny, je možné nasadit jako Azure webové služby. Pomocí webové služby, mohou uživatelé odesílat data do modelu a model vrátí jeho předpovědi.

Chcete-li nasadit vaší prediktivní experimentu, klepněte na příkaz **Spustit** na konci experimentu plátna a klepněte na **Nasazení webové služby**. Nastavení webové služby a jsou umístěny v řídicím panelu webové služby.

![Nasazení webových služeb](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

Můžete testovat webové služby Machine Learning webové služby portálu nebo Machine Learning Studio.

Odpověď na požadavek webové služby klepnutím na tlačítko **Test** na řídicím panelu webové služby. Dialogové okno se zobrazí, můžete požádat o vstupní data pro službu. Jedná se o sloupce očekává hodnocení experimentu. Zadejte sadu dat a potom klepněte na tlačítko **OK**. Výsledky generované webové služby se zobrazí v dolní části řídicího panelu.

Můžete klepnout na odkaz Náhled **Test** testování služby v portálu Azure Machine Learning webové služby, jak bylo uvedeno výše v sekci nové webové služby.

Dávkové spuštění služby klepnutím na odkaz Náhled **Test** . Na stránce ověření dávkové zadání klepněte na tlačítko Procházet a vyberte soubor CSV obsahující hodnoty odpovídající vzorek. Pokud nemáte soubor CSV a vytvořili vaše prediktivní experimentu pomocí Machine Learning Studio, můžete stáhnout sadu dat pro vaše prediktivní experimentu a jeho použití.

![Testování webové služby](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

Na stránce **Konfigurace** můžete změnit zobrazovaný název služby a a popis. Název a popis se zobrazí v [klasické portálu Azure](http://manage.windowsazure.com/) , kde můžete spravovat webové služby.

Zadáním řetězce pro každý sloupec v seznamu **Vstupní schéma**, **Schéma výstupu**a **Parametr webové služby**, můžete zadat popis pro vstupní data, výstupní data a parametry webové služby. Tyto popisy se používají v dokumentace ukázkový kód pro webovou službu.

Můžete povolit protokolování Diagnostika všechny chyby, které jsou zobrazeny při přístupu k webové službě. Další informace naleznete v tématu [Povolení protokolování webové služby Machine Learning](machine-learning-web-services-logging.md).

![Konfiguraci webové služby](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

Koncové body webové služby můžete také nakonfigurovat na portálu Azure Machine Learning webové služby podobný postup dříve zobrazené v části nové webové služby. Možnosti jsou různé, můžete přidat nebo změnit popis služby, povolit protokolování a povolit ukázková data pro testování.

### <a name="access-the-web-service"></a>Přístup k webové službě

Po nasazení do webové služby Machine Learning Studio, můžete odeslat data službě a programově přijímat odpovědi.

Řídicí panel poskytuje všechny informace, které potřebujete získat přístup k webové službě. Například klíč rozhraní API je k dispozici povolit oprávnění přístupu ke službě a stránky nápovědy rozhraní API poskytované pomoci můžete začít psát kód.

Další informace o přístupu k webové služby Machine Learning v tématu [spotřebovat nasazena webová služba Azure Machine Learning](machine-learning-consume-web-services.md).

### <a name="manage-the-web-service"></a>Spravovat webové služby

Existují různé akce lze provádět sledování webové služby. Můžete aktualizovat a odstranit ji. Můžete také přidat další koncové body webové službě Classic kromě výchozí koncový bod, který je vytvořen při nasazení.

Další informace naleznete v tématu [Správa pracovního prostoru Azure Machine Learning](machine-learning-manage-workspace.md) a [Spravovat webové služby pomocí portálu Azure Machine Learning webové služby](machine-learning-manage-new-webservice.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>Aktualizace webové služby

Můžete provést změny do webové služby, jako je například aktualizace modelu s daty další vzdělávání a nasadit znovu, přepsání původní webové služby.

Chcete-li aktualizovat webovou službu, otevřete původní prediktivní experimentu, který jste použili k nasazení webové služby a vytvořit upravitelnou kopii klepnutím na příkaz **Uložit jako**. Proveďte požadované změny a potom klepněte na tlačítko **Nasazení webové služby**.

Vzhledem k tomu, že jste nainstalovali tento experiment před, budete vyzváni, pokud chcete přepsat (klasická webová služba) nebo aktualizovat existující službu (nové webové služby). Klepnutím na tlačítko **Ano** nebo **aktualizace** zastaví existující webové služby a nasazuje nové prognostické experimentu je nasazen na jeho místo.

> [AZURE.NOTE] Pokud jste provedli změny konfigurace v původní webové služby, například zadáním nového zobrazovaného názvu nebo popisu, musíte znovu zadat tyto hodnoty.

Jednou z možností pro aktualizaci webové služby je jejich rekvalifikace model programově. Další informace naleznete v tématu [programové modely přeškolit strojovém učení](machine-learning-retrain-models-programmatically.md).


<!-- internal links -->
[Vytvoření testu školení]: #create-a-training-experiment
[Převést na prediktivní experimentu]: #convert-the-training-experiment-to-a-predictive-experiment
[nové]: #deploy-the-predictive-experiment-as-a-new-Web-service
[klasické]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
