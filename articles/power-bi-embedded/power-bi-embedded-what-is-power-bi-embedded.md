<properties
   pageTitle="Co je Microsoft Power BI vložený?"
   description="Power BI vložený umožňuje integrovat sestavách Power BI na web nebo mobilní aplikace, takže nemusíte vytvářet vlastní řešení vizualizace dat pro vaše uživatele"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="what-is-microsoft-power-bi-embedded"></a>Co je Microsoft Power BI vložený?

S **Power BI vložený**můžete integrovat sestavách Power BI přímo na webu nebo mobilní aplikace.

![](media\powerbi-embedded-whats-is\what-is.png)

Power BI vložený je **Azure služby** , který umožňuje dodavateli softwaru a aplikace vývojářům povrchový prostředí Power BI dat v rámci svých aplikací. Vývojář nevytvoříte aplikací a tyto aplikace vlastní uživatelů a odlišné sadu funkcí. Tyto aplikace taky dochází, mít několik předdefinovaných dat prvků, jako jsou grafy a sestavy, které můžete nyní být technologii Microsoft Power BI vložený. Uživatelé, nemusíte účet Power BI používat aplikaci. Můžete dál se přihlásit k aplikaci stejně jako před a zobrazit a pracovat s Power BI vytváření sestav bez nutnosti žádné další licence.

## <a name="licensing-for-microsoft-power-bi-embedded"></a>Vložené licencí pro Microsoft Power BI

V části **Microsoft Power BI vložený** model použití licencí pro Power BI není zodpovědnost koncový uživatel.  Místo toho **vykreslí** jsou zakoupené tak, že vývojář aplikace, spotřebovává vizuálních prvků a bude účtovaná k předplatnému, který vlastní zdroje.

## <a name="microsoft-power-bi-embedded-conceptual-model"></a>Microsoft Power BI vložený konceptuální Model

![](media\powerbi-embedded-whats-is\model.png)

Stejně jako jakékoli jiné služby v Azure jsou materiály k Power BI vložený zřízení prostřednictvím rozhraní [API ARM Azure](https://msdn.microsoft.com/library/mt712306.aspx). V tomto případě prostředek, který zřizujete je **Shromažďování pracovního prostoru v Power BI**.

## <a name="workspace-collection"></a>Kolekce pracovního prostoru

**Pracovní prostor kolekce** je kontejner nejvyšší úrovně Azure pro zdroje, který obsahuje 0 nebo další **pracovní prostory**.  **Pracovní prostor** **kolekce** obsahuje všechny standardní Azure vlastnosti, jakož i takto:

-   **Přístupové klávesy** – zkratek používaných při bezpečně volání rozhraní API Power BI (popsané v další části).
-   **Uživatelé** – Azure Active Directory (AAD) uživatelů, kteří mají oprávnění správce pro správu kolekce pracovního prostoru Power BI pomocí portálu Azure nebo ARM API.
-   **Oblast** – jako součást zřizování **Kolekce pracovního prostoru**, můžete vybrat oblast, kterou chcete v zřízení. Další informace najdete v tématu [Azure oblastí](https://azure.microsoft.com/regions/).

## <a name="workspace"></a>Pracovní prostor

**Pracovní prostor** je kontejner Power BI obsahu, který může obsahovat datové sady, sestavy a řídicí panely. Je prázdná při prvním vytvoření **pracovního prostoru** . Během zobrazení náhledu budete vytvářet veškerý obsah pomocí Power BI Desktop a budete ho nahrajte do jedné z vašich pracovních prostorů pomocí rozhraní [REST API Power BI](http://docs.powerbi.apiary.io/reference).

## <a name="using-workspace-collections-and-workspaces"></a>Použití pracovního prostoru kolekce a pracovní prostory
**Pracovní prostor kolekce** a **pracovní prostory** jsou kontejnery obsahu, který používá se seřazenými do ten nejlepší způsob vešel návrhu aplikace, kterou vytváříte. Nastane mnoha různými způsoby, aby mohl uspořádání obsahu v nich. Můžete umístit veškerý obsah v rámci jedné pracovní prostor a pak později pomocí aplikace tokeny dál rozdělit obsah mimo vaši zákazníci. Můžete taky tak, aby některé oddělení všechno vaši zákazníci dát v samostatném pracovních prostorech. Nebo si můžete uspořádat uživatelům oblasti neinstaluje z zákazníka. Této flexibilní návrh můžete zvolit nejlepší způsob, jak uspořádat obsah.

## <a name="cached-datasets"></a>Režim Cached datové sady

V náhledu lze použít režim Cached datové sady.  Po načtení do **Aplikace Microsoft Power BI vložený**však nelze aktualizovat data uložená v mezipaměti.

## <a name="authentication-and-authorization-with-app-tokens"></a>A mohli ověřovat pomocí aplikace tokenů

**Microsoft Power BI vložený** podřídí se aplikace provádět všechny potřebné uživatelské ověřování a se tak mohli ověřovat. Neexistuje žádný explicitní požadavek se svým koncovým uživatelům zákazníci služby Azure Active Directory (Azure AD).  Místo toho vyjadřuje aplikace **Microsoft Power BI vložený** registraci k vykreslování sestavy Power BI pomocí **Tokeny ověřování aplikace (aplikace tokeny)**.  Tyto **Aplikace tokeny** vytvořené v případě potřeby když aplikace chce k vykreslování sestavy.  V tématu [tokeny aplikace](power-bi-embedded-get-started-sample.md#key-flow).

![](media\powerbi-embedded-whats-is\app-tokens.png)

**Tokeny ověřování aplikace (aplikace tokeny)** slouží k **Microsoft Power BI vložený**ověřování.  Existují tři typy tokenů **Aplikace**:

1.  Zřízení tokeny - při vytváření nového **pracovního prostoru** do **Pracovního prostoru kolekce**
2.  Tokeny vývoj - vyvolají volání přímo do **Power BI REST API**
3.  Vložení tokeny - vyvolají volání se vykreslují pomocí sestavy v vložené rámce iframe

Těchto tokenů se používají pro různé fáze vašich interakcí s **Microsoft Power BI vložený**.  Tokeny jsou navržené tak, aby k Power BI můžete delegovat oprávnění u aplikace. Další informace najdete v tématu [Tokenu tok aplikace](power-bi-embedded-app-token-flow.md).

## <a name="see-also"></a>Viz taky
- [Obvyklé scénáře Microsoft Power BI vložený](power-bi-embedded-scenarios.md)
- [Začínáme s Microsoft Power BI vložený](power-bi-embedded-get-started.md)
