<properties
    pageTitle="Nemůžu se přihlásit k odběru Azure | Microsoft Azure"
    description="Popisuje, jak vyřešit některé nejčastější problémy s Azure předplatné přihlášení."
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="genli"/>

# <a name="i-cant-sign-in-to-manage-my-azure-subscription"></a>Nemůžu se přihlásit ke správě předplatného Azure

Tento článek vás provede některé nejčastější metod k řešení problémů přihlášení.

## <a name="page-hangs-in-the-loading-status"></a>Ve stavu načítání přestane fungovat stránky

Pokud stránku prohlížeče internet se zablokuje, zkuste každé z následujících kroků, dokud se můžete přepnout na [Azure portálu](https://portal.azure.com).

-   Po aktualizaci stránky.
-   Použijte jiný prohlížeč Internet.
-   Pokud používáte Microsoft Internet Exploreru, přejděte na portál Azure pomocí režim procházení InPrivate. 

    ODPOVĚĎ:  Klikněte na **Nástroje** ![tlačítko nástroje](./media/billing-cannot-login-subscription/Toolsbutton.png) > **bezpečnosti** > **Procházení InPrivate**.

    B.  Přejděte na [portál Azure](https://portal.azure.com)a přihlaste se k portálu.

## <a name="error-message-no-subscriptions-found"></a>Chybová zpráva "Bez předplatná nalezen"

Pokud váš účet nemá dostatečná oprávnění, zobrazí se chybová zpráva **nebyl nalezen žádný odběr** . Pouze správce účtu dostali na [Centra účet](https://account.windowsazure.com/)není správci služeb (severní) nebo dalších správců (CA).

**Scénář 1: Chybová zpráva přijetí [Azure portálu](https://portal.azure.com)**

Tento problém můžete vyřešit, [Přidání dalších správce nebo vlastník role](billing-add-change-azure-subscription-administrator.md) pro účet.

**Scénář 2: Chybová zpráva se dostali na stránce [Centrum pro účet Azure](https://account.windowsazure.com/Subscriptions)**

Kontrola, jestli účet, který jste použili účtu správce. Abyste ověřili, který je účtem správce, postupujte takto:

1.  Přihlaste se k [portálu Azure](https://portal.azure.com).
2.  V nabídce centrální vyberte **předplatné**.
3.  Vyberte předplatné, které chcete zkontrolovat a potom vyberte **Nastavení**.
4.  Vyberte **Vlastnosti**. Správce účet předplatného se zobrazí v poli **Účtu správce** .

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Automaticky jste přihlášení jako jiného uživatele

Tomuto problému může dojít, pokud používáte více než jednoho uživatelského účtu v internetového prohlížeče.

Tento problém vyřešit, zkuste jednu z těchto způsobů:

-   Vymazání mezipaměti a odstraňte cookie. V Internet Exploreru klikněte na **Nástroje** ![tlačítko nástroje](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Možnosti Internetu** > **Odstranit**. Ujistěte se, že je zaškrtnuto políčko pro dočasné soubory cookie, heslo a procházení historie a potom klikněte na odstranit.

-   Resetujte nastavení Internet Exploreru se vrátit osobní nastavení, které jste udělali. Klikněte na **Nástroje** ![tlačítko nástroje](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Možnosti Internetu** > **Upřesnit** > zaškrtněte políčko **Odstranit osobní nastavení** > **Obnovit**.

-   Přejděte na portál Azure v režimu procházení InPrivate. Klikněte na **Nástroje** ![tlačítko nástroje](./media/billing-cannot-login-subscription/Toolsbutton.png) > **bezpečnosti** > **Procházení InPrivate**.

## <a name="need-help-contact-support"></a>Potřebujete pomoc? Kontaktujte podporu. 

Pokud potřebujete pomoc, [Kontaktujte podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat problém vyřešit rychle. 