<properties
    pageTitle="Přiřazení uživatelů do vlastní domény v Azure Active Directory | Microsoft Azure"
    description="Jak k naplnění vlastní domény v Azure Active Directory s uživatelským účtům."
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

# <a name="assign-users-to-a-custom-domain"></a>Přiřazení uživatelů do vlastní doménu

Po přidání vlastní domény pro službu Azure Active Directory, musíte přidat uživatelských účtů pro tuto doménu tak, aby mohli začít ověřování je.

## <a name="users-synced-in-from-a-directory-on-your-corporate-network"></a>Uživatelé synchronizovat v z adresáře ve vaší podnikové síti

Pokud jste již vytvořili připojení mezi vaší místní služby Active Directory a Azure Active Directory, můžete naplnit synchronizace účtů. Další informace o tom, jak synchronizovat služby Azure Active Directory s na adresářová služba Active Directory najdete v článku [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

## <a name="users-added-and-managed-in-the-cloud"></a>Uživatelé přidali a spravovaný v cloudu

Chcete-li změnit doménu pro stávající uživatelský účet:

1.  Otevřete portál Azure klasické pomocí účtu, který je globální správce nebo Správce uživatelů.

2.  Otevření adresáře.

3.  Vyberte kartu **uživatelů** .

4.  Vyberte uživatele ze seznamu.

5.  Změna domény uživatele a potom vyberte **Uložit**.

To můžete taky udělat pomocí [Prostředí PowerShell Microsoft](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) nebo rozhraní [API grafu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="select-a-custom-domain-when-creating-a-new-user"></a>Vyberte vlastní doménu při vytváření nového uživatele

1.  Otevřete portál Azure klasické pomocí účtu, který je globální správce nebo Správce uživatelů.

2.  Otevření adresáře.

3.  Vyberte kartu **uživatelů** .

4.  V řádku nabídek klikněte na **Přidat**.

5.  Když přidáte uživatelské jméno, zvolte vlastní domény v seznamu domény.

6.  Vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky

-   [Použití vlastní domény názvů zjednodušit přihlášení pro vaše uživatele](active-directory-add-domain.md)

-   [Správa vlastní názvy domén](active-directory-add-manage-domain-names.md)

-   [Informace o doménách správy v Azure AD](active-directory-add-domain-concepts.md)
