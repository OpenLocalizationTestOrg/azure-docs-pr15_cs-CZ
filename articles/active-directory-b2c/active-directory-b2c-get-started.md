<properties
    pageTitle="Azure Active Directory B2C: Vytvoření tenanta Azure Active Directory B2C | Microsoft Azure"
    description="Téma návod k vytvoření Azure Active Directory B2C klienta"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.topic="article"
    ms.devlang="na"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure Active Directory B2C: Vytvoření Azure AD B2C klienta

Chcete-li začít používat Microsoft Azure Active Directory (Azure AD) B2C, postupujte podle tří kroků uvedených v tomto článku.

## <a name="step-1-sign-up-for-an-azure-subscription"></a>Krok 1: Registrace pro předplatné Azure

Pokud už máte předplatné Azure, tento krok přeskočte. V opačném případě zaregistrovat ke [Azure předplatné](../active-directory/sign-up-organization.md) a získat přístup k Azure AD B2C.

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>Krok 2: Vytvoření Azure AD B2C klienta

Pomocí následujících kroků k vytvoření nového klienta Azure AD B2C. Aktuálně B2C funkce nejde zapnout do stávajících tenantů.

1. Přihlaste se k [Azure klasické portál](https://manage.windowsazure.com/) jako správce předplatného. Toto je stejný pracovní nebo školní účet nebo stejný účet Microsoft, který jste použili k registraci Azure.
2. Klikněte na **Nový** > **aplikace služby** > **služby Active Directory** > **adresáře** > **vytvořit vlastní**.

    ![Snímek obrazovky při zahajování k vytvoření klienta](./media/active-directory-b2c-get-started/new-directory.png)

3. Vyberte **název**, **Název domény** a **zemi nebo oblasti** pro vašeho klienta.
4. Zkontrolujte, že **to je adresář B2C**možnost.
5. Klikněte na značku zaškrtnutí dokončení akce.

    ![Snímek obrazovky s na značku zaškrtnutí k vytvoření B2C adresáře](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. Vašeho klienta se teď vytvoří a zobrazí se v rozšíření služby Active Directory. Jsou taky volání globální správce klienta. Podle potřeby můžete přidat další globální správci.

    > [AZURE.IMPORTANT]
    Pokud se chystáte používat ke klientovi B2C aplikace výrobní, přečtěte si článek stupnice [výrobní porovnání náhled B2C klienti](active-directory-b2c-reference-tenant-type.md). Všimněte si, že jsou známé problémy při odstranění stávajícímu klientovi B2C a opětovné vytvoření se stejným názvem domény. Budete muset vytvoření klienta B2C s jiným názvem domény.

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>Krok 3: Přejděte na zásuvné funkce B2C na portálu Azure

1. Přejděte na linku služby Active Directory na navigačním panelu na levé straně.
2. Vyhledání vašeho klienta na kartě **adresáře** a klikněte na něj.
3. Klikněte na kartu **Konfigurovat** .
4. Klikněte na odkaz **Spravovat B2C nastavení** v části **Správa B2C** .

    ![Snímek obrazovky s adresáře konfigurace B2C](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. Portál Azure se zásuvné funkce B2C zobrazující se otevře v novou kartu nebo okno prohlížeče.

    > [AZURE.IMPORTANT]
    Může trvat až 2 – 3 minuty pro vašeho klienta, aby byly přístupné na portálu Azure. Opakováním kroků po určitou dobu se problém opravíte. Pokud ne, kontaktujte podporu.

6. Připnete tento zásuvné vaší Startboard k nim usnadnili přístup. (Nástroj PIN kód je označen v červené barvě v pravém horním rohu zásuvné funkce.)

    ![Snímek obrazovky s funkcí zásuvné B2C](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    Můžete spravovat uživatelé a skupiny, samoobslužné resetování konfigurace a značky funkce vašeho klienta na [Azure klasické portál](https://manage.windowsazure.com/)společnosti.

## <a name="easy-access-to-the-b2c-features-blade-on-the-azure-portal"></a>Snadný přístup k zásuvné funkce B2C na portálu Azure

Pro zlepšení vyhledávání, jsme přidali zástupce na zásuvné funkce B2C na portálu Azure.

1. Přihlaste se k portálu Azure jako globální správce klienta B2C. Pokud jste již přihlášeni do různých klienta, zapněte klienti (pravý horní roh).
2. Na levém navigačním panelu klikněte na **Procházet** .
3. Klikněte na tlačítko **Azure AD B2C** pro přístup k funkcím zásuvné B2C.

    ![Snímek obrazovky s Procházet na zásuvné funkce B2C](./media/active-directory-b2c-get-started/b2c-browse.png)

## <a name="next-steps"></a>Další kroky

Zjistěte, jak při registraci aplikace Azure AD B2C a k vytvoření aplikace rychlý Start načtením [Azure Active Directory B2C: registrace aplikace](active-directory-b2c-app-registration.md).
