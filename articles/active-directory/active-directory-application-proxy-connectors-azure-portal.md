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


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups---public-preview"></a>Publikování aplikací na jednotlivé sítě a umístění pomocí konektoru skupiny – Public Preview

> [AZURE.SELECTOR]
- [Azure portálu](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure klasické portálu](active-directory-application-proxy-connectors.md)


Spojnice skupiny jsou vhodné pro různé scénáře, včetně:

- Weby s více propojených datacentrech. V tomto případě chcete zachovat tolik přenosy v rámci datacentra co nejdřív, protože datacentra křížové odkazy se drahé pomalé. Můžete nasadit spojnic v jednotlivých datacentra sloužit pouze aplikace, které se nacházejí v rámci datacentra. Tento přístup minimalizuje datacentra křížové odkazy a poskytuje úplně průhledné zkušeností uživatelů.
- Správa aplikací nainstalovaných v izolovaném sítích, které nejsou součástí hlavní podnikové sítě. Spojnice skupiny můžete nainstalovat vyhrazené spojnic v izolovaném sítích taky vyčlenění aplikací k síti.
- Pro aplikace je nainstalovaná na IaaS pro přístup ke cloudu představují skupiny connector běžné službě zajistit přístup k části všechny aplikace. Spojnice skupin není vytvořte další závislost ve vaší podnikové síti nebo fragmentu prostředí aplikace. Spojnice je možné nainstalovat na každé datacentra cloudu a vytisknout jenom aplikace, které se nacházejí v této sítě. Můžete nainstalovat několik spojnice k dosažení dostupnost.
- Podpora více doménové prostředí, ve kterých lze konkrétní spojnic nasazený za struktury a nastavit vytisknout konkrétní aplikace.
- Spojnice skupiny můžete používat na webech havárie obnovení buď zjistit převzetí nebo jako zálohování pro hlavní web.
- Spojnice skupin lze také vytisknout více společností z jednoho klienta.

## <a name="prerequisite-create-your-connectors"></a>Předpoklady: Vytvoření spojnice
Zařadit do skupiny spojnice, budete muset nezapomeňte [nainstalovaný více spojnic](active-directory-application-proxy-enable.md). Při instalaci nového spojnice automaticky spojí skupině **výchozí** spojnice.

## <a name="step-1-create-connector-groups"></a>Krok 1: Vytvoření skupiny spojnice
Můžete vytvořit tolik spojnice skupin. Vytvoření skupiny spojnice dosáhnete [Azure portálu](https://portal.azure.com).

1. Vyberte **Azure Active Directory** , přejděte na řídicí panel pro správu adresáře. Odtud, vyberte **podnikových aplikací** > **proxy aplikace**.

2. Klikněte na tlačítko **Spojnice skupiny** . Zobrazí se nová skupina Connector zásuvné.

3. Zadejte název nové skupiny na spojnici a potom použijte rozevírací nabídky vyberte, které spojovací čáry patří v této skupině.

4. Po dokončení na spojnici skupiny, vyberte možnost **Uložit** .

## <a name="step-2-assign-applications-to-your-connector-groups"></a>Krok 2: Přiřadit skupinám spojnice
Posledním krokem je nastavit jednotlivé aplikace spojnice skupiny, který bude sloužit.

1. Na řídicím panelu správy adresáře vyberte **podnikových aplikací** > **všechny aplikace** > aplikace, které chcete přiřadit ke skupině spojnice > **Proxy aplikace**.
2. V části **skupiny spojnice**umožňuje v rozevírací nabídce vyberte skupinu vy ji chcete použít.
3. Zaškrtněte políčko **Uložit** změny.


## <a name="see-also"></a>Viz taky

- [Povolení aplikace Proxy](active-directory-application-proxy-enable.md)
- [Povolit jednotného přihlašování](active-directory-application-proxy-sso-using-kcd.md)
- [Povolení podmíněné přístupu](active-directory-application-proxy-conditional-access.md)
- [Řešení potíží s aplikací Proxy](active-directory-application-proxy-troubleshoot.md)

Nejnovější příspěvky a aktualizace najdete v článku [aplikace Proxy blogu](http://blogs.technet.com/b/applicationproxyblog/)
