<properties
    pageTitle="Správa vlastní názvy domén v Azure Active Directory | Microsoft Azure"
    description="Správa koncepty a postupy pro správu vlastní domény v Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Správa vlastní názvy domén v Azure Active Directory

Název domény je důležitou součástí identifikátor pro mnoho prostředků adresáře: je součástí uživatelské jméno nebo e-mailovou adresu pro uživatele, část adresy pro skupinu a může být součástí aplikace ID URI pro aplikaci. Zdroj v Azure Active Directory (Azure AD) mohou obsahovat název domény, která se už ověří, vlastněná adresář, který obsahuje daný zdroj. Jenom globální správce může v Azure AD provádět úlohy správy domény.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Nastavte název primární domény Azure AD adresáře

Po vytvoření adresáře původní název domény, například "contoso.onmicrosoft.com," je také název primární domény pro váš adresář. Primární doména je výchozí název domény pro nové uživatele při vytváření nového uživatele v [Azure klasické portál](https://manage.windowsazure.com/)nebo jiných portály jako je portál pro správu Office 365. To zjednodušuje proces pro správce k vytvoření nových uživatelů na portálu.

Chcete-li změnit název primární domény pro váš adresář:

1.  Přihlaste se k [Azure klasické portál](https://manage.windowsazure.com/) k uživatelskému účtu, který je globálního správce služby Azure AD adresáře.

2.  Na levém navigačním panelu vyberte **Služby Active Directory** .

3.  Otevření adresáře.

4.  Vyberte kartu **domény** .

5.  Klikněte na tlačítko **Změnit primární** na panelu s příkazy.

6.  Vyberte doménu, kterou chcete nový primární doména adresáře.

Můžete změnit název primární domény adresáře je ověření vlastní domény, který není federovaný. Změna primární doména adresáře nezmění uživatelská jména pro stávající uživatele.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Přidání vlastní domény jmen Azure AD

Můžete přidat až 900 vlastní názvy domén do každého Azure AD adresáře. Proces přidáte [Další vlastní název domény](active-directory-add-domain.md) je stejný pro první název vlastní domény.

## <a name="add-subdomains-of-a-custom-domain"></a>Přidávat subdomény vlastní doménu

Pokud chcete přidat název doména třetí úrovně například "europe.contoso.com" do vašeho adresáře, by měl nejdřív přidáním a ověřením doména druhé úrovně, například contoso.com. Subdoména bude automaticky ověřený Azure AD. Ověřil subdoména, kterou jste právě přidali zobrazíte obnovili stránku v prohlížeči se seznamem domén v adresáři vašeho.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Co dělat, když změníte registrátora DNS pro vlastní název domény

Pokud změníte registrátora DNS pro vlastní název domény, můžete dál používat vlastní název domény s Azure AD samotné bez přerušování a další konfiguraci. Pokud používáte vlastní název domény s Office 365, Intune nebo jiné služby, které jsou závislé na vlastní názvy domén v Azure AD, naleznete v dokumentaci pro tyto služby.

## <a name="delete-a-custom-domain-name"></a>Odstranění vlastního názvu domény

Vlastní název domény můžete odstranit z Azure AD, pokud vaše organizace už používá tento název domény, nebo pokud je potřeba použít tuto doménu s jinou Azure AD.

Odstranit vlastní název domény, třeba nejprve zajistit, že žádné zdroje v adresáři vašeho závisí na název domény. Název domény nelze odstranit z vašeho adresáře, pokud:

-   Všichni uživatelé má uživatelské jméno, e-mailové adresy a adresy serveru proxy, které obsahují název domény.

-   Všechny skupiny má e-mailovou adresu nebo adresu proxy, která zahrnuje název domény.

-   Všechny aplikace v Azure AD má aplikace Identifikátor URI obsahující název domény.

Musí změnit nebo odstranit tyto zdroje v adresáři Azure AD před odstraněním vlastní název domény.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Správa názvů domén pomocí prostředí PowerShell nebo rozhraní API grafu

Většinu úkolů správy pro názvy domén v Azure Active Directory můžete taky udělat pomocí Powershellu Microsoft nebo programově pomocí rozhraní API Azure AD grafu (ve veřejných verze preview).

-   [Použití Powershellu ke správě názvy domén v Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Spravovat názvy domén v Azure AD pomocí rozhraní API grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Další kroky

-   [Úvod do názvů domén v Azure AD](active-directory-add-domain-concepts.md)

-   [Správa vlastní názvy domén](active-directory-add-manage-domain-names.md)
