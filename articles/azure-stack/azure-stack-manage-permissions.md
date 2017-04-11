<properties
    pageTitle="Správa oprávnění k prostředkům za uživatele ve vrstvě Azure (Správce služby a klienta) | Microsoft Azure"
    description="Jako správce služby nebo klienta Naučte se spravovat oprávnění k prostředkům za uživatele."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="manage-user-permissions"></a>Správa uživatelských oprávnění

Uživatele v Azure zásobníku může být čtečky, vlastník nebo skupiny přispěvatelů pro všechny výskyty předplatného, skupina zdroje nebo službu. Například uživatel A může oprávnění čtečka 1 předplatného, ale mít oprávnění vlastníka až 7 virtuálního počítače.

-   Čtečky: Uživatel můžete zobrazit vše, ale nemůžete proveďte požadované změny.

-   Přispěvatel: Uživatel můžete spravovat všechno kromě přístupu k prostředkům.

-   Vlastník: Uživatel můžete spravovat všechno, co, včetně přístupu k prostředkům.


## <a name="set-access-permissions-for-a-user"></a>Nastavení oprávnění pro uživatele

1.  Přihlaste se pomocí účtu, který má oprávnění vlastníka zdroje, který chcete spravovat.

2.  Zásuvné zdroje, klikněte na ikonu **aplikace Access** ![](media/azure-stack-manage-permissions/image1.png).

3.  V zásuvné **Uživatelé** klikněte na položku **role**.

4.  V zásuvné **role** kliknutím na **Přidat** přidejte oprávnění pro uživatele.

## <a name="next-steps"></a>Další kroky

[Přidání tenanta Azure zásobníku](azure-stack-add-new-user-aad.md)
