<properties
    pageTitle="Správa vlastní názvy domén v náhledu Azure Active Directory | Microsoft Azure"
    description="Správa koncepty a postupy pro správu názvu domény v Azure Active Directory"
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
    ms.date="09/12/2016"
    ms.author="curtand;jeffsta"/>

# <a name="managing-custom-domain-names-in-your-azure-active-directory-preview"></a>Správa vlastní názvy domén v náhledu Azure Active Directory

Název domény je důležitou součástí identifikátor pro mnoho prostředků adresáře: je součástí uživatelské jméno nebo e-mailovou adresu pro uživatele, část adresy pro skupinu a může být součástí aplikace ID URI pro aplikaci. Zdroj v náhledu Azure Active Directory (Azure AD) mohou obsahovat název domény, která se už ověří, vlastněná adresář, který obsahuje daný zdroj. [Novinky v náhledu](active-directory-preview-explainer.md) Jenom globální správce může v Azure AD provádět úlohy správy domény.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Nastavte název primární domény Azure AD adresáře

Po vytvoření adresáře původní název domény, například "contoso.onmicrosoft.com," je také název primární domény. Primární doména je výchozí název domény pro nové uživatele při vytváření nového uživatele. To zjednodušuje proces pro správce k vytvoření nových uživatelů na portálu. Chcete-li změnit název primární domény:

1.  Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2.  Vyberte **Další služby**, zadejte **Azure Active Directory** do textového pole a potom stiskněte **Enter**.

    ![Otevření Správa uživatelů](./media/active-directory-domains-add-azure-portal/user-management.png)

3. Na zásuvné ***název adresáře*** vyberte **názvy domén**.

4. Na zásuvné ** *název adresáře* - názvy domén** vyberte název domény, kterou chcete provést název primární domény.

5.  Na ***název_domény*** zásuvné (to znamená zásuvné, která se otevře, který má v názvu nový název domény) na příkaz **nastavit jako primární** . Potvrďte volbu po zobrazení výzvy.

    ![Zkontrolujte název domény primární](./media/active-directory-domains-manage-azure-portal/make-primary.png)

Můžete změnit název primární domény adresáře je ověření vlastní domény, který není federovaný. Změna primární doména adresáře nezmění uživatelská jména pro stávající uživatele.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Přidání vlastní domény jmen Azure AD

Můžete přidat až 900 vlastní názvy domén do každého Azure AD adresáře. Proces přidáte [Další vlastní název domény](active-directory-domains-add-azure-portal.md) je stejný pro první název vlastní domény.

## <a name="add-subdomains-of-a-custom-domain"></a>Přidávat subdomény vlastní doménu

Pokud chcete přidat název doména třetí úrovně například "europe.contoso.com" do vašeho adresáře, by měl nejdřív přidáním a ověřením doména druhé úrovně, například contoso.com. Subdoména bude automaticky ověřený Azure AD. Ověřil subdoména, kterou jste právě přidali zobrazíte obnovili stránku v prohlížeči, který obsahuje seznam těchto domén.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Co dělat, když změníte registrátora DNS pro vlastní název domény

Pokud změníte registrátora DNS pro vlastní název domény, můžete dál používat vlastní název domény s Azure AD samotné bez přerušování a další konfiguraci. Pokud používáte vlastní název domény s Office 365, Intune nebo jiné služby, které jsou závislé na vlastní názvy domén v Azure AD, naleznete v dokumentaci pro tyto služby.

## <a name="delete-a-custom-domain-name"></a>Odstranění vlastního názvu domény

Vlastní název domény můžete odstranit z Azure AD, pokud vaše organizace už používá tento název domény, nebo pokud je potřeba použít tuto doménu s jinou Azure AD.

Odstranit vlastní název domény, třeba nejprve zajistit, že žádné zdroje v adresáři vašeho závisí na název domény. Název domény nelze odstranit z vašeho adresáře, pokud:

-   Všichni uživatelé obsahuje uživatelské jméno, e-mailovou adresu nebo adresu proxy, která zahrnuje název domény.

-   Všechny skupiny má e-mailovou adresu nebo adresu proxy, která zahrnuje název domény.

-   Všechny aplikace v Azure AD má aplikace Identifikátor URI obsahující název domény.

Musí změnit nebo odstranit tyto zdroje v adresáři Azure AD před odstraněním vlastní název domény.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Správa názvů domén pomocí prostředí PowerShell nebo rozhraní API grafu

Většinu úkolů správy pro názvy domén v Azure Active Directory můžete taky udělat pomocí Powershellu Microsoft nebo programově pomocí rozhraní API Azure AD grafu (ve veřejných verze preview).

-   [Použití Powershellu ke správě názvy domén v Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Spravovat názvy domén v Azure AD pomocí rozhraní API grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Další kroky

-   [Přidání vlastní názvy domén](active-directory-domains-add-azure-portal.md)
