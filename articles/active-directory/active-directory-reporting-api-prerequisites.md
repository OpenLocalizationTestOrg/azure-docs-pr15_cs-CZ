<properties
    pageTitle="Požadavky pro přístup k rozhraní API sestav Azure AD. | Microsoft Azure"
    description="Další informace o požadavcích pro přístup k Azure AD vykazování rozhraní API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Požadavky pro přístup k Azure AD vykazování rozhraní API 

[Azure AD vykazování rozhraní API](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) poskytují programový přístup k datům pomocí sady na základě REST API. Tato rozhraní API můžete volat z různých programovacím jazyce a nástroje.

Vytváření sestav rozhraní API používá [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) povolit přístup k webu rozhraní API. 

Příprava přístup k rozhraní API sestav, musíte:

1. Vytvoření aplikace ve vašem klientovi Azure AD 

2. Udělte aplikace příslušná oprávnění pro přístup k datům Azure AD

3. Shromáždění nastavení z adresáře

Otázky, problémy nebo zpětnou vazbu obraťte se prosím [AAD vykazování Nápověda](mailto:aadreportinghelp@microsoft.com).


## <a name="create-an-azure-ad-application"></a>Vytvoření aplikace Azure AD

Konfigurace vašeho adresáře pro přístup k Azure AD vykazování rozhraní API, musíte se přihlásit k portálu Azure klasické pomocí účtu správce Azure předplatné, které je taky členem role adresáře globální správce ve vašem klientovi Azure AD.

> [AZURE.IMPORTANT] Aplikace spuštěné v části přihlašovací údaje s oprávněními "správce" tímto způsobem může být hodně výkonná, protože nezapomeňte zabezpečit aplikace ID/tajná pověření.


1. Na [portálu Azure klasické](https://manage.windowsazure.com), v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Ze seznamu **služby active directory** vyberte adresáře.

3. V nabídce na horní klikněte na **aplikace**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Na dolním panelu klikněte na **Přidat**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/03.png) 

5. Na **Co chcete udělat?** dialogové okno, klikněte na **Přidat aplikaci, které vyvíjí mé organizaci**. 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/04.png) 


6. V dialogovém okně **námi o aplikaci** proveďte následující kroky: 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/05.png) 

    na. Do textového pole **název** zadejte název (například: vytváření sestav aplikace rozhraní API).

    b. Vyberte **Webová aplikace a rozhraní API webových**.

    c. Klikněte na tlačítko **Další**.


7. V dialogovém okně **Vlastnosti aplikace** proveďte následující kroky: 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/06.png) 

    na. Do textového pole **Přihlašovací adresa URL** zadejte `https://localhost`.

    b. Do textového pole **Identifikátor URI ID aplikace** zadejte ```https://localhost/ReportingApiApp```.

    c. Klikněte na **dokončení**.



## <a name="grant-your-application-permission-to-use-the-api"></a>Udělte oprávnění aplikace můžete rozhraní API

1. Na [portálu Azure klasické](https://manage.windowsazure.com/), v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Ze seznamu **služby active directory** vyberte adresáře.

3. V nabídce na horní klikněte na **aplikace**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png)


3. V seznamu aplikací vyberte aplikaci nově vytvořený.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)

4. V nabídce na horní klikněte na **Konfigurovat**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)


5. V části **oprávnění do jiných aplikací** pro zdroj **Azure Active Directory** klikněte na rozevírací seznam **Oprávnění aplikace** a potom vyberte **data adresářů pro čtení**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/09.png)


5. Na dolním panelu klikněte na **Uložit**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/10.png)


## <a name="gather-configuration-settings-from-your-directory"></a>Shromáždění nastavení z adresáře

Tato část popisuje následující nastavení získat z adresáře:

- Název domény
- ID klienta
- Tajná klienta

Při konfiguraci volání podřízenosti rozhraní API musíte tyto hodnoty. 


### <a name="get-your-domain-name"></a>Získejte název domény

1. Na [portálu Azure klasické](https://manage.windowsazure.com), v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Ze seznamu **služby active directory** vyberte adresáře.

3. V nabídce na horní klikněte na **domény**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/11.png) 

4. Ve sloupci **Název domény** zkopírujte váš název domény.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/12.png) 


### <a name="get-the-applications-client-id"></a>Získání aplikace ID klienta

1. Na [portálu Azure klasické](https://manage.windowsazure.com), v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Ze seznamu **služby active directory** vyberte adresáře.

3. V nabídce na horní klikněte na **aplikace**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 

4. V seznamu aplikací vyberte aplikaci nově vytvořený.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)

4. V nabídce na horní klikněte na **Konfigurovat**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)

4. Zkopírujte **Kód klienta**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/13.png)


### <a name="get-the-applications-client-secret"></a>Získání tajná klientské aplikace

Tajná klientské aplikace získáte potřebujete vytvořit nový klíč a jeho hodnotu při ukládání nový klíč, protože načíst tuto hodnotu dál už uložit.

1. Na [portálu Azure klasické](https://manage.windowsazure.com), v levém navigačním podokně klikněte na **Služby Active Directory**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Ze seznamu **služby active directory** vyberte adresáře.

3. V nabídce na horní klikněte na **aplikace**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/02.png) 

4. V seznamu aplikací vyberte aplikaci nově vytvořený.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/07.png)

4. V nabídce na horní klikněte na **Konfigurovat**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/08.png)

5. V části **klíče** proveďte následující kroky: 

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/14.png)

    na. Ze seznamu dobu trvání vyberte dobu trvání.

    b. Na dolním panelu klikněte na **Uložit**.

    ![Registrace aplikace](./media/active-directory-reporting-api-prerequisites/10.png)

    c. Zkopírujte hodnoty klíče.

## <a name="next-steps"></a>Další kroky

- Chcete pro přístup k datům z Azure AD vykazování rozhraní API programové způsobem? Podívejte se na [Začínáme s Azure Active Directory vykazování API](active-directory-reporting-api-getting-started.md).

- Pokud chcete zjistit víc informací o vytváření sestav služby Azure Active Directory, najdete v článku [Azure Active Directory vykazování Průvodce](active-directory-reporting-guide.md).  
