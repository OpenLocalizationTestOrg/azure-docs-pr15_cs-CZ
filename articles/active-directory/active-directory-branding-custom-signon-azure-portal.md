<properties
pageTitle="Přizpůsobení stránky přihlášení v náhledu Azure Active Directory | Microsoft Azure"
description="Naučte se přidávat společnosti branding Azure přihlašovací stránka"
services="active-directory"
documentationCenter=""
authors="curtand"
manager="femila"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/30/2016"
ms.author="curtand"/>

# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Přidání značky na přihlašovací stránku v náhledu Azure Active Directory společnosti

Chcete-li předejít zmatky, mnoho společností chcete použít jednotný vzhled a chování ve všech webů a služby, které spravují. Azure Active Directory náhled obsahuje tato možnost umožňuje přizpůsobit vzhled přihlašovací stránce s firemní logo a vlastní barevná schémata. [Novinky v náhledu](active-directory-preview-explainer.md) Přihlašovací stránka je stránka, která se zobrazí při přihlášení k Office 365 nebo jiné webových aplikací, které používají Azure AD jako poskytovatele identity. Práce s této stránky a zadejte svoje přihlašovací údaje.

Pokud chcete zobrazit vaši firemní značku, barvy a další přizpůsobitelné prvky na této stránce, podívejte se na následujících obrázcích je třeba porozumět rozdílu mezi dvěma prostředími.

Následující obrazovka ukazuje a příklad na přihlašovací stránce Office 365 na stolním počítači **před** vlastní nastavení:

![Přihlašovací stránka s Office 365 před vlastního nastavení](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Následující obrazovka ukazuje a příklad na přihlašovací stránce Office 365 na stolním počítači **Po** vlastní nastavení:

![Přihlašovací stránka s Office 365 po vlastního nastavení](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)


## <a name="customizing-the-sign-in-page"></a>Přizpůsobení přihlašovací stránka

Pokud potřebujete prohlížečový přístup ke cloudové aplikace a služby, které vaše organizace používá, použijete přihlašovací stránka.

Pokud jste použili změny na přihlašovací stránku, může trvat až na hodinu změny se zobrazí.

Firemní stránky přihlášení se zobrazí jenom při návštěvě službu s adresou URL specifické pro klienta ATP https://outlook.com/**contoso**.com, https://mail. **Contoso**. cz.

Při návštěvě službu s adresou URL konkrétní není klienta (například: https://mail.office365.com), zobrazí se stránka bez zobrazení přihlašovací. v tomto případě váš branding se zobrazí po zadání ID uživatele nebo jste vybrali ikony uživatele.

> [AZURE.NOTE]
>
- Název domény musí se zobrazí jako "Aktivní" v části **domény** portálu Azure, ve kterém jste nakonfigurovali značky. Další informace najdete v tématu [Přidání vlastní názvy domén](active-directory-domains-add-azure-portal.md).
- Přihlašovací stránka branding nezabývá spotř přihlašovací stránku společnosti Microsoft. Pokud se přihlásit pomocí účtu Microsoft, zobrazí se značkou seznam uživatelů dlaždic vykreslení Azure AD, ale branding vaší organizaci, nebudou mít vliv na Microsoft přihlašovací stránku účtu.

Na stránce přihlášení políčko **zůstat přihlášení** umožňuje uživateli při zavření a znovuotevření svůj prohlížeč zůstat přihlášeni. 

   ![Zůstat přihlášení se změnami](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Netýká životnost relace. Můžete skrýt zaškrtávacího políčka na přihlašovací stránce Azure Active Directory.
Zda se má zobrazit na zaškrtávací políčko závisí na nastavení **zůstat přihlášení zakázáno**.

   ![Zůstat přihlášení se změnami](./media/active-directory-branding-custom-signon-azure-portal/02.png)


Chcete-li skrýt zaškrtávacího políčka, toto nastavení na hodnotu **Ano**. 

> [AZURE.NOTE] Některé funkce aplikace SharePoint Online a Office 2010, závisí na uživatelé moct zaškrtnutí políčka. Pokud jste nakonfigurovali toto nastavení umožňuje seřadit skryté, uvidí vaši uživatelé další a neočekávané pokynů k přihlášení.




**Chcete-li přidat branding do adresáře vaší společnosti:**

1.  Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2.  Vyberte **Další služby**, zadejte do textového pole **Uživatelé a skupiny** a pak stiskněte **Enter**.

    ![Otevření Správa uživatelů](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)

3. Na zásuvné **Uživatelé a skupiny** vyberte **branding společnosti**.

4. Na **Uživatelé a skupiny – společnosti branding** zásuvné, vyberte příkaz **Upravit** .

    ![Upravit vlastní branding](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)

5. Upravte prvky, které chcete přizpůsobit. Všechny prvky jsou volitelné.

6. Klikněte na **Uložit**.

Ji může trvat až jednu hodinu pro všechny změny, které jste udělali na přihlašovací stránku branding zobrazit.

## <a name="next-steps"></a>Další kroky

[Přidání specifické pro určitý jazyk společnosti branding](active-directory-branding-localize-azure-portal.md)
