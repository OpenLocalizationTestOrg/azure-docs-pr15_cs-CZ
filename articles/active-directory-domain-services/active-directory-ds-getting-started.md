<properties
    pageTitle="Azure AD Domain Services: Vytvoření skupiny Administrators Datacentrum AAD | Microsoft Azure"
    description="Začínáme s Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="get-started-with-azure-ad-domain-services"></a>Začínáme se službou Azure AD domény

Tento článek provede konfiguraci potřebná k povolení služby Azure AD domén pro vašeho klienta Azure AD.

## <a name="task-1-create-the-aad-dc-administrators-group"></a>Úkol 1: Vytvoření skupiny "AAD Datacentrum Administrators"
Vytvoření skupiny pro správu klienta služby Azure Active Directory je první úkol. Tento speciální skupiny pro správu se nazývá **AAD Datacentrum správci**. Členové této skupiny mít příslušná oprávnění správce v počítačích, které jsou doméně doménu spravovaný Azure AD Domain Services. V doméně počítačích se přidá tuto skupinu ke skupině "Správci". Členové této skupiny navíc můžete použít Vzdálená plocha vzdáleně připojovat k doméně počítačích.  

> [AZURE.NOTE] Nemáte oprávnění správce domény nebo podnikový správce ve spravovaných doméně vytvořené pomocí Azure AD Domain Services. Spravované domén tato oprávnění jsou vyhrazená službou a nejsou k dispozici uživatelům v rámci klienta. Speciální správce skupiny vytvořené v tomto úkolu konfigurace však může použít k provedení některých privilegovaných operací. Tyto operace patří připojení počítače k doméně, patřící do skupiny Administrators v doméně počítačích konfigurace zásad skupiny atd.

V tomto úkolu konfigurace vytvoření skupiny pro správu a přidat jeden nebo víc uživatelů v adresáři vašeho do skupiny. Proveďte následující postup vytvoření skupiny pro správu služby Azure AD domény:

1. Přejděte na **portál Azure klasické** ([https://manage.windowsazure.com](https://manage.windowsazure.com))

2. Vyberte uzel **Služby Active Directory** v levém podokně.

3. Vyberte Azure AD klienta (adresář), pro kterou chcete povolit Azure AD Domain Services. Můžete vytvořit jenom jednu doménu pro každý Azure AD adresář.

    ![Vyberte Azure AD adresář](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klikněte na kartu **skupiny** .

5. Přidání skupiny do vašeho tenanta Azure AD, klikněte na **Přidat skupinu** z podokna úloh v dolní části stránky.

    ![Přidat tlačítko Seskupit](./media/active-directory-domain-services-getting-started/add-group-button.png)

6. Vytvoření skupiny s názvem **AAD Datacentrum správci**. Nastavit **Typ skupiny** **zabezpečení**.

    > [AZURE.WARNING] Povolení přístupu v rámci služby Azure AD domény spravovat domény, vytvoření skupiny s tímto názvem přesné.

    ![Vytvoření skupiny správců](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. Přidejte popis této skupiny, aby ostatní porozumět tomu, že tuto skupinu využívá udělit oprávnění správce v rámci Azure AD Domain Services.

8. Po vytvoření skupiny klikněte na název skupiny, pokud chcete zobrazit vlastnosti této skupiny. Přidání uživatelů jako členové této skupiny, klikněte na tlačítko **Přidat členy** na dolním panelu.

    ![Přidání tlačítka členů skupiny](./media/active-directory-domain-services-getting-started/add-group-members-button.png)

9. V dialogovém okně **Přidat členy** vyberte uživatele, kteří mají mít členové této skupiny a zaškrtněte políčko Po dokončení.

    ![Přidání uživatelů do skupiny správců](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## <a name="task-2-create-or-select-an-azure-virtual-network"></a>Úkol 2: Vytvoření nebo výběr Azure virtuální sítě
Dalším úkolem konfigurace je [Vytvořit nebo vybrat Azure virtuální sítě](active-directory-ds-getting-started-vnet.md).
