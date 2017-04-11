<properties
    pageTitle="Azure Active Directory B2C: Samoobslužné resetování hesla | Microsoft Azure"
    description="Téma, které demonstrují nastavíte samoobslužné resetování hesla pro uživatele v Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Nastavení samoobslužné resetování hesla pro uživatele

Pomocí funkce Samoobslužné resetování uživatele (kteří se zaregistrují místních účtů) obnovit své heslo ve své vlastní. Značně sníží zatížení pracovníkům podpory zvlášť když aplikace má miliony spotřebitele s ním pracovat v pravidelných intervalech. V současné době pouze podporují pomocí ověřenou e-mailovou adresu jako prostředek k obnovení. Přidáme metody další obnovení (ověřenou telefonního čísla, dotazy zabezpečení, atd.) v budoucnosti.

> [AZURE.NOTE]
Tento článek se týká samoobslužné resetování používána v kontextu zásadu přihlášení. Pokud potřebujete plně přizpůsobitelného resetování hesla zásady volá z aplikace, najdete v [tomto článku](./active-directory-b2c-reference-policies.md#create-a-password-reset-policy).

Ve výchozím nastavení adresáře něčím samoobslužné resetování zapnutá. Zapnout pomocí následujících kroků:

1. Přihlaste se k [Azure klasické portál](https://manage.windowsazure.com/) jako správce předplatného. Toto je stejný pracovní nebo školní účet nebo stejný účet Microsoft, který jste použili k vytvoření adresáře.
2. Přejděte na linku služby Active Directory na navigačním panelu na levé straně.
3. Najděte adresáře na kartě **adresáře** a klikněte na něj.
4. Klikněte na kartu **Konfigurovat** .
5. Přejděte dolů do části **resetování hesla k uživatelskému zásady** a zapnutím **uživatelé povoleno pro heslo resetovat** možnost **Ano**. Všimněte si, že je zaškrtnuta možnost **Alternativní e-mailovou adresu** ; Ponechejte uvedený ho.

    ![Resetování hesla samoobslužných funkcí](./media/active-directory-b2c-reference-sspr/sspr.png)

6. Klepněte na tlačítko **Uložit** v dolní části stránky. Hotovo.

Chcete-li otestovat, funkci "Spustit" na všechny zásady přihlášení, která má místní účty jako zprostředkovatelem identit. Na přihlašovací místního účtu stránky (místo, kam zadáte e-mailovou adresu a heslo, nebo uživatelské jméno a heslo), klikněte na **nemáte přístup ke svému účtu?** k ověření prostředí pro uživatele.

> [AZURE.NOTE]
Samoobslužné resetování stránky můžete přizpůsobit pomocí [funkce brandingu společnosti](../active-directory/active-directory-add-company-branding.md).
