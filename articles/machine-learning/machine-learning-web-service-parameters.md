<properties 
    pageTitle="Výukové parametry webové služby Azure počítač | Microsoft Azure" 
    description="Jak používat parametry služby Azure počítače výukové Web změnit chování modelu při přístupu k webové služby." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="raymondl;garye"/>

#<a name="use-azure-machine-learning-web-service-parameters"></a>Používat Azure počítač výukové parametry webové služby

Webové služby Azure počítače výukové se vytvoří publikováním pokus, která obsahuje moduly s konfigurovatelné parametry. V některých případech může chcete změnit chování modul je spuštěná webové služby. *Parametry webové služby* umožňují k provedení tohoto úkolu. 

Běžné příklad se nastavuje [Importovat Data] [ reader] modul tak, aby uživatel publikované webové služby zadat jiný zdroj dat při přístupu k webové služby. Nebo konfigurace [Exportovat Data] [ writer] modul tak, aby lze zadat jinou cílovou. Další příklady měnit je číslo posunuto pro [Algoritmus hash funkce] [ feature-hashing] modul nebo počet žádoucí funkce pro [Výběr funkcí systémem filtru] [ filter-based-feature-selection] modul. 

Můžete nastavit parametry webové služby a přidružení k jeden nebo více parametrů modul ve vaší testu a můžete určit, zda jsou požadovány nebo volitelné. Uživatel webová služba potom můžete zadat hodnoty pro tyto parametry při volání webové služby. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##<a name="how-to-set-and-use-web-service-parameters"></a>Jak nastavit a používat parametry webové služby

Kliknutím na ikonu vedle parametr modulu a výběrem "Nastavit jako parametr webové služby" definujete parametr webové služby. Vytvoří nový parametr webové služby a připojuje k modul parametru. Potom při přístupu k webové služby, může uživatel zadat hodnotu parametru webové služby a platí pro parametr modul.

Když definujete parametr webové služby, je dostupná pro všechny ostatní parametry modulu v testu. Když definujete parametr webové služby spojené s parametrem pro jeden modul, můžete tuto stejnou parametr webové služby pro všechny ostatní moduly jako parametr očekává stejného typu hodnoty. Například pokud parametr webové služby Neplatná číselná hodnota, pak ji jenom lze modul parametrů, které očekávat číselnou hodnotu. Pokud uživatel nastaví hodnotu pro parametr webové služby, se použije všechny parametry přiřazen modul.

Rozhodnutí o zadání výchozí hodnoty pro parametr webové služby. Pokud neuděláte, je parametr volitelné pro uživatele webové služby. Pokud neposkytnete výchozí hodnotu, je potřeba uživateli zadání hodnoty při přístupu k webové služby.

Rozhraní API dokumentaci k webové službě obsahuje informace pro uživatele služby web o tom, jak při přístupu k webové službě programově zadat parametr webové služby.

>[AZURE.NOTE] Dokumentace rozhraní API pro klasické webová služba je k dispozici prostřednictvím odkazu **stránka nápovědy pro rozhraní API** webové služby **řídicích panelů** v počítači výukové Studio. Pomocí portálu [Azure počítače výukové webové služby](https://services.azureml.net/Quickstart) na stránkách **spotřebě** a **Swagger rozhraní API** pro webová služba není uvedený dokumentaci k rozhraní API pro nové webové služby.


##<a name="example"></a>Příklad

Jako příklad předpokládejme máme pokus [Exportovat Data] [ writer] moduly, které odesílá informace k úložišti objektů blob Azure. Jsme budete definovat s názvem "Objektů Blob cestu" parametr webové služby umožňující web služby uživatelům měnit cestu k základnímu úložišti objektů blob při přístupu k službě.

1.  Ve počítače výukové studiu, klikněte na [Exportovat Data] [ writer] modul ho vyberte. Jeho vlastnosti jsou zobrazené v podokně Vlastnosti napravo od plátno testu.

2.  Určení typu úložiště:

    - V části **prosím určení cíle dat**vyberte "Úložišti objektů Blob Azure".
    - V části **prosím určit typ ověřování**vyberte "Účet".
    - Zadejte informace o účtu úložišti objektů blob Azure. 
    <p />

3.  Klikněte na ikonu napravo od **cesta objektů blob začínající na s parametrem kontejner**. Vypadá takto:

    ![Ikona parametru služby Web][icon]

    Vyberte "Nastavit jako parametr webové služby".

    V části **Webové služby parametrů** v dolní části podokna Vlastnosti s názvem "Cestu k objektů blob začínající na s kontejneru" se přidá položka. Toto je parametr webové služby, který je teď související s těmito [Exportovat Data] [ writer] modul parametr.

4.  Přejmenovat parametr webové služby, klikněte na název, zadejte "Objektů Blob cestu" a stiskněte klávesu **Enter** . 
 
5.  Zadání výchozí hodnoty pro parametr webové služby, klikněte na ikonu napravo od názvu, vyberte "Poskytují rovná hodnotě", zadejte hodnotu (například "container1/output1.csv") a stiskněte klávesu **Enter** .

    ![Parametr webové služby][parameter]

6.  Klikněte na **Spustit**. 

7.  Klikněte na **Nasazení webové služby** a vyberte **Nasazení webová služba [klasické]** **Nasazení [New] webové služby** pro nasazení webové služby.

Webové služby, může uživatel teď zadat nový cíl pro [Export dat] [ writer] modul při přístupu k webové služby.

##<a name="more-information"></a>Další informace

Podrobnější třeba [Parametry webové služby](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) položku zobrazíte v [Počítači výukové blogu](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

Další informace o přístupu k webové službě výukové počítače najdete v článku [jak používat publikované počítače výukové webové služby](machine-learning-consume-web-services.md).



<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/
 
