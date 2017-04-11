<properties
   pageTitle="Správa pro správu jednotky v Azure Active Directory"
   description="Použití správy jednotek pro přesnější delegování oprávnění služby Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Správa pro správu jednotky v Azure AD - Public Preview

Tento článek popisuje jednotek pro správu – nový kontejneru Azure Active Directory zdrojů, které se dá použít pro delegování oprávnění pro správu přes podmnožiny uživatelů a použití zásad podmnožinu uživatelů. V Azure Active Directory pro správu jednotky správcům centrální delegovat oprávnění místního správce nebo nastavení zásad na nejnižší úrovni.

To je užitečné v organizacích s nezávisle na oddíly, třeba na velké vysoké škole, která se skládá z mnoha samostatného škol (Business školy, inženýrské školy a tak dále), které jsou nezávislé na sobě. Tato divize mít vlastní správci IT, kteří řízení přístupu uživatelů a spravovat nastavení zásad pro jejich dělení. Udělit tato divize centrální správy chcete mít možnost oprávnění správců přes uživatelů ve své konkrétní dělení. Konkrétně v tomto příkladu centrální správce můžete, například vytvořit pro správu jednotku pro konkrétní školu (Business školy) a doplníte pouze firemních uživatelů škole. Centrální správce můžete přidat škole firmy zaměstnance IT roli obory jinými slovy, udělte zaměstnance IT oprávnění správce škole firmy jenom přes organizační jednotka pro správu škole.

> [AZURE.IMPORTANT]
> Můžete vytvořit a používat pro správu jednotky pouze v případě umožňují Azure Active Directory Premium. Další informace najdete v tématu [Začínáme s Azure AD Premium](active-directory-get-started-premium.md).

Z centrálního správce pro správu jednotka je objekt adresář, který lze vytvořit a vyplněné zdroje. **V této verzi může být tyto materiály pouze uživatelé.** Po vytvoření a vyplnění správní jednotku mohou sloužit jako obor omezení udělené oprávnění pouze u zdroje obsažená v jednotce pro správu.

## <a name="managing-administrative-units"></a>Správa pro správu jednotek

V této verzi preview můžete vytvořit a spravovat používání rutin modul Azure Active Directory pro Windows PowerShell pro správu jednotky.

Další informace o požadavcích na software a nainstalujte modul Azure AD a informace o rutinách modul Azure AD pro správu pro správu jednotky, včetně syntaxe, parametr popisy a příklady přečtěte si téma [Správa Azure AD pomocí Windows Powershellu](https://msdn.microsoft.com/library/azure/jj151815.aspx).


## <a name="next-steps"></a>Další kroky
[Edice Azure Active Directory](active-directory-editions.md)
