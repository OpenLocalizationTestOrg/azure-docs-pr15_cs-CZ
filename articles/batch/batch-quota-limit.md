<properties
    pageTitle="Dávkové kvóty služby a omezení | Microsoft Azure"
    description="Další informace o výchozí Azure dávku kvóty, limity a omezení a jak požádat o kvóty zvyšuje"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/10/2016"
    ms.author="marsma"/>

# <a name="quotas-and-limits-for-the-azure-batch-service"></a>Kvót a limity pro službu Azure dávku

Jako s jinými službami Azure jsou omezení na některé přidružený ke službě dávky prostředky. V mnoha tyto limity jsou výchozí kvóty použít tak, že Azure na úrovni účet nebo předplatného. Tento článek obsahuje informace o těchto výchozích hodnot a jak můžete požádat o kvóty zvyšuje.

Pokud budete chtít spustit výrobní úloh v listu, budete muset zvětšit jednu nebo více kvót nad výchozí. Pokud chcete zvýšit kvóty, můžete otevřít online [Zákaznická podpora žádost o](#increase-a-quota) zdarma.

>[AZURE.NOTE] Kvóta je úvěr není záruky kapacity. Pokud máte ve velkém měřítku kapacita požadavky, obraťte se prosím Azure podpory.

## <a name="subscription-quotas"></a>Předplatné kvót
**Zdroje**|**Výchozí Limit**|**Maximální Limit**
---|---|---
Účty dávku jednotlivých oblastech jedno předplatné | 1 | 50

## <a name="batch-account-quotas"></a>Dávkové účtu kvót
[AZURE.INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>Ostatní limity
**Zdroje**|**Maximální Limit**
---|---
Počet výpočetní uzly [souběžné úkoly](batch-parallel-node-tasks.md) | 4 x počet jádra uzel
[Aplikace](batch-application-packages.md) podle dávku účtu        | 20
Balíčků aplikací za aplikace  | 40
Velikost balíček aplikace (každý)       | Poli 195GB<sup>1</sup>

Limit úložiště azure <sup>1</sup> pro maximální objektů blob velikost bloku

## <a name="view-batch-quotas"></a>Zobrazit dávku kvóty

Zobrazení kvóty dávku účet [Azure portál][portal].

1. Zvolte **účty dávku** na portálu a pak vyberte dávku účet, který vás zajímá.

2. Vyberte možnost **Vlastnosti** na zásuvné nabídka dávku účtu

3. Vlastnosti zásuvné zobrazí **kvóty** aktuálně používané k tomuto účtu dávku

    ![Dávkové účtu kvót][account_quotas]

## <a name="increase-a-quota"></a>Zvětšení kvóty

Postupujte podle pokynů a požádat o kvóty zvětšit pomocí [Azure portál][portal].

1. Vyberte dlaždici **nápovědy + podpory** na portálu řídicího panelu nebo otazník (****?) v pravém horním rohu na portálu.

2. Vyberte **Nová žádost o podporu** > **Základy**.

3. Na zásuvné **základní informace** :

    na. **Typ problému** > **kvóty**

    b. Vyberte předplatné.

    c. **Typ kvóty** > **dávku**

    d. **Plán podpory** > **kvóty podpory - zahrnuté**

    Klikněte na tlačítko **Další**.

4. Na zásuvné **problému** :

    na. Vyberte **závažnosti** podle toho, [vliv obchodní][support_sev].

    b. V **Podrobnosti**zadejte každé kvóty, který chcete změnit název účtu listu a nové omezení.

    Klikněte na tlačítko **Další**.

5. Na zásuvné **kontaktní informace** :

    na. Vyberte **Upřednostňovaný způsob kontaktování**.

    b. Ověření a zadejte požadovaný kontakt.

    Klikněte na **vytvořit** odešlete žádost o podporu.

Jakmile jste odeslali žádost o podporu, podporu Azure vás kontaktovat. Všimněte si, že dokončení žádost může trvat až 2 pracovních dní.

## <a name="related-topics"></a>Příbuzná témata

* [Azure dávku účet vytvořte pomocí portálu Azure](batch-account-create-portal.md)

* [Přehled funkcí Azure dávku](batch-api-basics.md)

* [Azure předplatné a omezení služby, kvót a omezení](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
