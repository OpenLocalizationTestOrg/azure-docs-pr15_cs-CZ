<properties
    pageTitle="Řízení přístupu na základě rolí | Microsoft Azure"
    description="Začínáme v části Správa přístupu k ovládacímu prvku Azure přístupu na základě rolí v portálu Azure. Přiřazení rolí umožňuje udělit oprávnění v adresáři."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="get-started-with-access-management-in-the-azure-portal"></a>Začít používat správu přístupu na portálu Azure

Orientovaného zabezpečení společnosti by měly zaměřit na pojmenování zaměstnanců přesné oprávnění, které potřebují. Příliš mnoho oprávnění zpřístupňuje účet, který chcete útočníků. Příliš několik oprávnění znamená, že zaměstnanců se nemůžou dostat efektivně pracovat. Tento problém vyřešit nabídkou Správa jemně odstupňovaná přístupu pro Azure vám pomůže Azure na základě rolí přístup ovládacího prvku (RBAC).

RBAC můžete oddělit úkolů v rámci týmu a udělit jenom množství přístup uživatelům, kteří potřebují k provedení své práce. Místo pojmenování každý neomezený oprávnění v Azure předplatného nebo zdroje, můžete povolit jenom určité akce. Příklad použití RBAC k jednoho zaměstnance Správa virtuálních počítačích v předplatné, zatímco jiné Správa SQL databáze ve stejném předplatném.

## <a name="basics-of-access-management-in-azure"></a>Základní informace o řízení přístupu v Azure
Každý Azure předplatné je přidružený jeden adresář Azure Active Directory (AD). Uživatelé, skupiny a aplikací z adresáře můžete spravovat zdroje v Azure předplatného. Přiřazení těchto pomocí Azure portál, Azure příkazového řádku nástroje a rozhraní API Azure správy přístupových práv.

Udělení přístupu přiřazením odpovídající roli RBAC uživatelé, skupiny a aplikací na určitý obor. Obor přiřazování rolí můžou být předplatné, skupina zdroje nebo jeden zdroj. Role přiřazené v nadřazený oboru taky uděluje přístup k podřízeným položkám obsažené v něm obsažené. Uživatel s přístupem ke skupině zdroje například zvládne všechny zdroje, které obsahuje, například weby virtuálních počítačích a podsítí.

![Vztah mezi prvky Azure Active Directory - diagramu](./media/role-based-access-control-what-is/rbac_aad.png)

Přiřazení role RBAC určuje zdrojích uživateli, skupině nebo aplikace můžete spravovat v daném oboru.

## <a name="built-in-roles"></a>Předdefinované role
Azure RBAC má tři základní role, které platí pro všechny typy zdrojů:

- **Vlastník** má plný přístup ke všem materiálů, včetně doprava a přístup delegáta, aby ostatní.
- **Přispěvatel** můžete vytvořit a spravovat všechny typy Azure zdrojů, ale nemůže udělit přístup pro ostatní uživatele.
- **Čtečky** můžete zobrazit existující Azure zdroje.

Zbytek RBAC role v Azure povolit správu konkrétních Azure zdrojů. Například role přispěvatele virtuálního počítače lze vytvářet a spravovat virtuálních počítačích. Ho není udělte jim přístupová oprávnění virtuální sítě nebo podsítě, která virtuální počítač se připojí k.

[Předdefinované role RBAC](role-based-access-built-in-roles.md) uvádí k dispozici v Azure role. Určuje operace a obor, každá předdefinované role uděluje uživatelům. Pokud se chcete dozvědět definovat vlastní role i větší kontrolu nad nastavením, najdete v článku jak vytvářet [vlastní role v Azure RBAC](role-based-access-control-custom-roles.md).

## <a name="resource-hierarchy-and-access-inheritance"></a>Dědičnost zdroje hierarchie a přístup
- Každého **předplatného** v Azure patří pouze jeden adresář.
- Každá **Skupina zdroje** patří jenom jedno předplatné.
- Každý **zdroj** patří do skupiny pouze jeden zdroj.

Přístup, který poskytnout v nadřazené obory přechází v podřízené obory. Příklad:

- Přiřadit roli Čtenář do skupiny Azure AD v oboru předplatného. Členové této skupiny můžete zobrazit každé pole Skupina zdroje a zdroje v předplatného.
- Přiřazení role přispěvatele do aplikace na požadovaný rozsah skupiny zdrojů. Můžete spravovat zdroje všechny typy v této skupině zdrojů, ale ne další skupiny zdrojů v předplatného.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC porovnání správci klasické předplatného
Správci klasické předplatné a dalších správců mají úplný přístup k předplatnému Azure. Spravují zdroje [Azure portál](https://portal.azure.com) pomocí rozhraní API Správce prostředků Azure nebo klasická nasazení modelu [Azure klasické portálem](https://manage.windowsazure.com) a Azure. V části RBAC model klasické správci přiřazené role vlastníka v oboru předplatného.

Nový správce prostředků API Azure a jenom portál Azure domovské stránce podpory Azure RBAC. Uživatelé a aplikace, které jsou přiřazené role RBAC nelze použít na portálu Správa klasických a Azure klasické nasazení modelu.

## <a name="authorization-for-management-vs-data-operations"></a>Povolení pro správu porovnání operace s daty
Azure RBAC podporuje pouze operace správy Azure zdrojů v Azure portálem a rozhraní API Azure správce prostředků. Nelze povolte všechny úrovně operace s daty pro Azure zdroje. Například někomu Správa úložiště účtů povolit nikoli pozor, abyste objektů BLOB nebo tabulek v rámci účtu úložiště. Podobně databázi SQL lze spravovat, ale ne tabulky v něm obsažené.

## <a name="next-steps"></a>Další kroky
- Začínáme s [řízení přístupu na základě rolí v portálu Azure](role-based-access-control-configure.md).
- V tématu [RBAC předdefinované role](role-based-access-built-in-roles.md)
- Definovat vlastní [vlastní role v Azure RBAC](role-based-access-control-custom-roles.md)
