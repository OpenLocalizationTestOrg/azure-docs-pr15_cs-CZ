<properties
    pageTitle="Práce s Azure AD aplikace Proxy spojnic | Microsoft Azure"
    description="Popisuje, jak vytvářet a spravovat skupiny konektorů proxy server Azure AD aplikace."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Publikování aplikací na jednotlivé sítě a umístění pomocí konektoru skupiny

> [AZURE.SELECTOR]
- [Azure portálu](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure klasické portálu](active-directory-application-proxy-connectors.md)


Spojnice skupiny jsou vhodné pro různé scénáře, včetně:

- Weby s více propojených datacentrech. V tomto případě chcete zachovat tolik přenosy v rámci datacentra co nejdřív, protože datacentra křížové odkazy se obvykle drahé pomalé. Můžete nasadit spojnic v jednotlivých datacentra sloužit pouze aplikace, které se nacházejí v rámci datacentra. Tento přístup minimalizuje datacentra křížové odkazy a poskytuje úplně průhledné zkušeností uživatelů.
- Správa aplikací nainstalovaných v izolovaném sítích, které nejsou součástí hlavní podnikové sítě. Spojnice skupiny můžete nainstalovat vyhrazené spojnic v izolovaném sítích taky vyčlenění aplikací k síti.
- Pro aplikace je nainstalovaná na IaaS pro přístup ke cloudu představují skupiny spojnice běžné službě zajistit přístup k části všechny aplikace. Spojnice skupin není vytvořte další závislost ve vaší podnikové síti nebo fragmentu prostředí aplikace. Spojnice je možné nainstalovat na každé datacentra cloudu a vytisknout jenom aplikace, které se nacházejí v této sítě. Můžete nainstalovat několik spojnice k dosažení dostupnost.
- Podpora více doménové prostředí, ve kterých lze konkrétní spojnic nasazený za struktury a nastavit vytisknout konkrétní aplikace.
- Spojnice skupiny můžete používat na webech havárie obnovení buď zjistit převzetí nebo jako zálohování pro hlavní web.
- Spojnice skupin lze také vytisknout více společností z jednoho klienta.

## <a name="prerequisite-create-your-connectors"></a>Předpoklady: Vytvoření spojnice
K seskupení spojnice, až po nastavení se jste [nainstalovali více spojnic](active-directory-application-proxy-enable.md)a názvu je a potom seskupte je. Nakonec je potřeba je přiřadit konkrétní aplikace.

## <a name="step-1-create-connector-groups"></a>Krok 1: Vytvoření skupiny spojnice
Můžete vytvořit tolik spojnice skupin. Vytvoření skupiny spojnice dosáhnete Azure klasické portálu.

1. Vyberte svůj adresář a klikněte na **Konfigurovat**.  
    ![Proxy aplikace konfigurace snímek – klikněte na spravovat spojnice skupiny](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)

2. V části aplikace Proxy klikněte na **Spravovat skupiny spojnice** a vytvoření nové skupiny spojnice tím, že název skupiny.  
    ![Snímek obrazovky skupin spojnice proxy aplikace – název nové skupiny](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Krok 2: Přiřazení spojnice do skupin
Po vytvoření skupiny spojnice spojnice přejděte do příslušné skupiny.

1. V části **Aplikace Proxy**klikněte na **Spravovat spojnice**.
2. V části **skupiny**vyberte skupinu, kterou chcete použít pro každou spojnici. Spojnice může trvat až 10 minut aktivuje do nové skupiny.  
    ![Snímek obrazovky spojnic proxy aplikací - vybranou skupinu z rozevírací nabídky](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Krok 3: Přiřadit skupinám spojnice
Posledním krokem je nastavit jednotlivé aplikace connector skupiny, který bude sloužit.

1. Na portálu v adresáři vašeho Azure klasické vyberte aplikaci, chcete-li přiřadit ke skupině a klikněte na **Konfigurovat**.
2. V části **Connector skupiny**vyberte skupinu vy ji chcete použít. Tato změna se projeví okamžitě.  
    ![Snímek obrazovky skupiny connector proxy aplikace - vybranou skupinu z rozevírací nabídky](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)


## <a name="see-also"></a>Viz taky

- [Povolení aplikace Proxy](active-directory-application-proxy-enable.md)
- [Povolit jednotného přihlašování](active-directory-application-proxy-sso-using-kcd.md)
- [Povolení podmíněné přístupu](active-directory-application-proxy-conditional-access.md)
- [Řešení potíží s aplikací Proxy](active-directory-application-proxy-troubleshoot.md)

Nejnovější příspěvky a aktualizace najdete v článku [aplikace Proxy blogu](http://blogs.technet.com/b/applicationproxyblog/)
