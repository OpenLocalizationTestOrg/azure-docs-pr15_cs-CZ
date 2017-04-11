<properties
pageTitle="Přidání značky na přihlašovací stránku v náhledu Azure Active Directory společnosti specifické pro určitý jazyk | Microsoft Azure"
description="Naučte se přidávat určité společnosti jazyk branding obrázky a text Azure stránku přihlášení"
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
ms.date="09/12/2016"
ms.author="curtand"/>

# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Přidání značky na přihlašovací stránku v náhledu Azure Active Directory společnosti specifické pro určitý jazyk

Chcete-li předejít zmatky, mnoho společností chcete použít jednotný vzhled a chování ve všech webů a služby, které spravují. Azure Active Directory náhled obsahuje tato možnost umožňuje přizpůsobit vzhled přihlašovací stránce s firemní logo a vlastní barevná schémata. [Novinky v náhledu](active-directory-preview-explainer.md) Přihlašovací stránka je stránka, která se zobrazí při přihlášení k Office 365 nebo jiné webových aplikací, které používají Azure AD jako poskytovatele identity. Práce s této stránky a zadejte svoje přihlašovací údaje.

## <a name="customizing-the-sign-in-page-for-another-language"></a>Přizpůsobení přihlašovací stránce v jiném jazyce

Specifické pro určitý jazyk prvky můžete přidat vlastní přihlašovací stránku jenom v případě, že jste již vytvořili vlastní přihlašovací stránku jako pokynů v tématu [Přidání společnosti branding na přihlašovací stránku](active-directory-branding-custom-signon-azure-portal.md). Konfigurace jedna stránka přihlášení na adresář s výchozí sadu upravitelných prvků. Po dokončení konfigurace sady výchozích prvků stránky, můžete nakonfigurovat další verze pro jiné národní prostředí. Můžete taky kombinovat a odpovídají různé prvky. Můžete například:

- Vytvořte výchozí **přihlašovací stránky obrázek** , který lze použít u všech jazykových verzí a pak vytvořit určité verze pro angličtinu a francouzštině. Při nastavení vašeho prohlížeče na jeden z těchto dvou jazyků obrázek specifické pro určitý jazyk se zobrazí, když se zobrazí výchozí obrázek pro další jazyky.

- Konfigurace různých loga pro vaši organizaci (například japonština nebo hebrejština verze).

Doporučujeme, abyste počet jazyk varianty zhoršeným důvodů výkon a údržba.

**Chcete-li přidat branding do adresáře vaší společnosti:**

1.  Přihlaste se k [Azure portál](https://portal.azure.com) pomocí účtu, který je jako globální správce pro adresář.

2.  Vyberte **Další služby**, zadejte do textového pole **Uživatelé a skupiny** a pak stiskněte **Enter**.

    ![Otevření Správa uživatelů](./media/active-directory-branding-localize-azure-portal/user-management.png)

3. Na zásuvné **Uživatelé a skupiny** vyberte **branding společnosti**.

4. V **Uživatelé a skupiny – společnosti branding** zásuvné, vyberte příkaz **Přidat jazyk** .

    ![Přidání značky prvků specifické pro určitý jazyk](./media/active-directory-branding-localize-azure-portal/add-language.png)

5. Upravte prvky, které chcete přizpůsobit. Všechny prvky jsou volitelné.

6. Klikněte na **Uložit**.

Ji může trvat až jednu hodinu pro všechny změny, které jste udělali na přihlašovací stránku branding zobrazit.

## <a name="next-steps"></a>Další kroky

[Přidání značky na přihlašovací stránku společnosti](active-directory-branding-custom-signon-azure-portal.md)
