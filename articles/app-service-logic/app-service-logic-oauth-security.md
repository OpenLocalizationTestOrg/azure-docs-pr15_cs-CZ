<properties
    pageTitle="Zabezpečení OAUTH SaaS spojnic a rozhraní API aplikace | Azure"
    description="Přečtěte si o zabezpečení OAUTH spojnic a rozhraní API aplikace v aplikaci služby Azure; microservices architektury, saas"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="mandia"/>


# <a name="learn-about-oauth-security-in-saas-connectors"></a>Informace o zabezpečení OAUTH v SaaS spojnic

>[AZURE.NOTE] Tuto verzi článku platí pro použití logických operátorů aplikace 2014 12 01 náhled schématu verze.

V mnoha Software jako spojnice služby (SaaS) jako Facebook, Twitter, Dropboxu a tak dál vyžadují ověřování protokolem OAUTH.  Pokud použijete tyto spojnice SaaS z logických aplikací, nabízíme zjednodušené uživatelské prostředí kliknete na místo, kam "Povolení" v Návrháři logiky aplikace. Kdy můžete **Ověřit**, budete vyzváni k podpisu v (Pokud není již) a zadejte souhlas pro připojení ke službě SaaS vaším jménem. Po zadání souhlas a povolte, aplikací logiku potom službám můžete přistupovat SaaS.

## <a name="create-your-own-saas-app"></a>Vytvoření vlastní aplikace SaaS
Tento zjednodušené prostředí je možné, protože jsme dříve vytvořena a registraci naše aplikace v těchto služeb SaaS.  V některých případech se můžete zaregistrovat a používat vlastní aplikaci.  Toto je potřeba, například když budete chtít použít tyto spojnice SaaS ve vlastních aplikací. Tento příklad používá konektoru Dropboxu, ale proces je stejný pro všechny spojnice, které jsou závislé na OAUTH.

I v souvislosti s aplikací použití logických operátorů můžete použít vlastní aplikaci místo použití výchozí aplikace, který poskytneme. Pokud tlačítko "Povolení" nepodaří připojit, můžete zkusit vytvoření vlastní aplikace. Následující seznam obsahuje postup pro konektoru Twitter:

1. Otevřete na Twitter spojnici na portálu Azure náhled. Přejděte na **Procházet** > **rozhraní API aplikace**. Vyberte spojnici Twitter:  
    ![][1]

2. Vyberte **Nastavení** > **ověření**:  
    ![][2]

3. Zkopírujte hodnotu **Přesměrovat URI** :  
    ![][3]

4. Přejděte na [Twitter](http://apps.twitter.com) a **Vytvoření nové aplikace**. Ve vlastnosti **Zpětné URL** vložte hodnotu **Přesměrovat URI** zkopírovali z Twitter spojnice: ![][4]  
5. Po vytvoření aplikace pro Twitter vyberte **spolu s tokeny přístupu**. Zkopírujte následující hodnoty.
6. V nastavení ověření spojnice Twitter vložte tyto hodnoty ve vlastnostech **ID klienta** a **Tajná klienta** :   
    ![][5]  
7. Uložení nastavení spojnic.  

Teď by měla nebudou moct používat na spojnici z logických aplikací. Při použití tohoto spojnice z logických aplikací používá aplikaci místo výchozí aplikace.  

> [AZURE.NOTE] Pokud jste určili aplikace dříve, bude pravděpodobně chcete aplikaci-li.


<!--Image references-->
[1]: ./media/app-service-logic-oauth-security/TwitterConnector.png
[2]: ./media/app-service-logic-oauth-security/Authentication.png
[3]: ./media/app-service-logic-oauth-security/RedirectURI.png
[4]: ./media/app-service-logic-oauth-security/TwitterApp.png
[5]: ./media/app-service-logic-oauth-security/TwitterKeys.png
