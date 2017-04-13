<properties 
    pageTitle="Jaký druh kolekce je potřeba Azure RemoteApp? | Microsoft Azure" 
    description="Další informace o typech kolekce dostupné služby Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="what-kind-of-collection-do-you-need-for-azure-remoteapp"></a>Jaký druh kolekce je potřeba Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Azure RemoteApp umožňuje sdílení aplikací a zdroje s uživateli na libovolném zařízení. Doporučujeme provést vytvořením kolekcí, stiskněte a podržte na aplikace a zdroje a pak sdílet těch kolekcí s uživateli. Způsoby 2 jiné kolekce, s různých sítě a ověření možnosti – které je pro vás nejlepší?

Podívejme se různé aspekty a možnosti, které potřebujete provést k maximálnímu využití kolekci Azure RemoteApp. 


## <a name="quick-differences-between-the-collection-types"></a>Rychlé rozdíly mezi typy kolekce

|           | Cloud | Hybridní |
|-----------|-------|--------|
|Použití existující VNET| Ano| Ano|
|Vyžaduje AD připojené účty (DirSync)| Ne| Ano|
|Vyžaduje připojení k doméně| Ne| Ano|
|Vyžaduje přístupné pro VNET řadiče domény| Ne| Ano|

## <a name="cloud-collections"></a>Kolekce cloudu
- Rychlé vytvoření - kolekci máte rychle k dispozici, což znamená aplikací uživatelům rychlejší získání.
- Přenést vlastní aplikace nebo sdílet náš. Můžete použít vlastní obrázek (součást z Azure OM) nebo jeden z obrázků součástí vašeho předplatného.
- Nemusíte konfigurace připojení mezi kolekci a místní domény.
- Ale pokud chcete pomocí vlastní VNET Azure zajistit přístup do místního prostředí pro sdílení dat nebo použít ověřování systému Windows do zdrojů, jako jsou SQL Server (s využitím ověření databáze).


Dobře jak jedna vytvořit?

- Jenom v cloudu? Vytvoření s možností **Vytvořit** na portálu.
- Shluk + VNET? Pomocí možnosti **vytvořit s VNET** vytvoříte, ale nevybírejte k doméně.

## <a name="hybrid-collections"></a>Hybridní kolekce
- Zadejte úplný přístup k místní síti + Azure VNET.
- Poskytuje přístup spojení domény aplikace a data. Vzdálené aplikace může ověřovat proti na adresářová služba Active Directory: budou pak přístup k prostředkům ve vaší doméně.
- Povolení rozšířené monitorování a správu s existujících řešení System Center a zásad skupiny systému Windows (přes vlastní obrázek založený na systému Windows Server 2012 R2)
- Podpora [ExpressRoute](https://azure.microsoft.com/services/expressroute/) pro připojení k místní VNET Azure VNET.

Pomocí možnosti **vytvořit s VNET** vytvoříte a zvolte k doméně.

## <a name="authentication-options"></a>Možnosti ověřování
Azure RemoteApp podporuje Microsoft účty a účty adresářové služby Azure Active Directory, ale ne všech kolekce podporují všechny metody. 

| Typ účtu                      |                                                             | Cloud | Shluk + VNET | Hybridní |
|-----------------------------------|-------------------------------------------------------------|-------|--------------|--------|
| Účet Microsoft                 |                                                             | Ano   | Ano          | Ne     |
| Azure Active Directory (Azure AD) |                                                             |       |              |        |
|                                   | Azure AD                                               | Ano   | Ano          | Ne     |
|                                   | AD Connect se synchronizací hesel                               | Ano   | Ano          | Ano    |
|                                   | AD Connect bez synchronizaci hesel                            | Ano   | Ano          | Ne     |
|                                   | AD Connect se službou AD FS                                       | Ano   | Ano          | Ano    |
|                                   | 3 – podporované Azure poskytovatelů identit (například Ping) | Ano   | Ano          | Ano    |
| Vícefaktorové ověřování       |                                                             | Ano   | Ano          | Ano    |



### <a name="cloud-and-cloud--vnet"></a>Cloudu a cloudu + VNET 
S kolekcemi cloudu můžete použít účty Microsoft Azure AD účty nebo kombinaci obou. Pomocí účtů, které nejvhodnější pro uživatele.

Je potřeba žádné zvláštní věcí pomocí účtů Microsoft. 

Pokud chcete používat Azure AD účty, budete muset Ujistěte se, že vašeho klienta Azure AD shoduje s některou přidružený k předplatnému. Po vytvoření předplatného Azure RemoteApp Azure AD klienta, který jste používali se automaticky přiřazovat k vašemu předplatnému. Všechny uživatelské Azure AD udělit oprávnění musí být stejném klientovi. V případě potřeby můžete [změnit klienta Azure AD](remoteapp-changetenant.md) přidružený k předplatnému.
 
### <a name="hybrid-or-cloud--azure-ad--ad"></a>Hybridní (nebo cloudu + Azure AD + AD)

Použití Azure AD + místní služby Active Directory je požadována kolekce hybridní webové aplikace. Je potřeba použít AD Connect integrovat dva adresáře. Ale můžete mít několik pokud jde o tom, jak konfigurovat AD Connect. 

Existují 2 AD Connect scénáře – pomocí synchronizace hesel nebo federace AD. Podívejte se na [AD Connect informace](../active-directory/active-directory-aadconnect.md) k určení prodejce, který z těchto works pro vás nejlepší.

Můžete také Azure AD + AD s kolekcí cloudu. Zkontrolujte, že stejný postup nastavení sledujete.

Podívejte se na [Azure AD + požadavky služby Active Directory Azure RemoteApp](remoteapp-ad.md) pro kroky konfigurace Azure AD a služby Active Directory.

## <a name="go-create-your-collection"></a>Přejděte vytvářejí vaši kolekci!
Dobře myslím, že jsme jste sebou je nyní, tedy jediným věc zleva do - vytvořit kolekci první Azure RemoteApp.

[Vytvoření kolekce cloudu](remoteapp-create-cloud-deployment.md) nebo [vytvořit kolekci hybridní](remoteapp-create-hybrid-deployment.md) - získat vytvoření.
