<properties
   pageTitle="Služba Azure Centrum zabezpečení a databáze SQL Azure | Microsoft Azure"
   description="Tento článek popisuje, jak Centrum zabezpečení můžete zabezpečené databází v databázi SQL Azure."
   services="sql-database"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="terrylan"/>

# <a name="azure-security-center-and-azure-sql-database-service"></a>Služba Azure Centrum zabezpečení a databáze SQL Azure

[Centrum zabezpečení Azure](https://azure.microsoft.com/documentation/services/security-center/) pomáhá zabránit, zjišťování a odpovědět na rizika. Poskytuje integrované zabezpečení sledování a zásady správy přes Azure předplatných, pomáhají zjistit hrozeb, které může jinak všimnout a spolupracuje Rozsáhlá platforma řešení zabezpečení.

Tento článek popisuje, jak Centrum zabezpečení můžete zabezpečené databází v databázi SQL Azure.

## <a name="why-use-security-center"></a>Proč používat Centrum zabezpečení?

Centrum zabezpečení pomáhá chránit data do SQL databáze zadáním přehled o zabezpečení servery a databází. Pomocí Centra zabezpečení máte tyto možnosti:

- Definování zásad pro šifrování databáze SQL a auditování.
- U všech předplatných sledujte zabezpečení databáze SQL zdroje.
- Rychlé vyhledání a nápravě potíže se zabezpečením.
- Integrace upozornění od [zjišťování hrozbou, že databáze SQL Azure](../sql-database/sql-database-threat-detection-get-started.md).

Kromě pomáhá chránit vaše databáze SQL zdroje, Centrum zabezpečení také poskytuje zabezpečení monitorování a správu pro Azure virtuálních počítačích, cloudové služby, aplikací služby, virtuálních sítí a další. Další informace o Centru zabezpečení [tady](security-center-intro.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro

Začít s Centrum zabezpečení, musíte mít předplatné Microsoft Azure. Bezplatné osy Centrum zabezpečení je povolený k vašemu předplatnému. Další informace o volném Centrum zabezpečení a standardní úrovní najdete v článku [Ceny Centrum zabezpečení](https://azure.microsoft.com/pricing/details/security-center/).

Centrum zabezpečení podporuje přístupu na základě rolí. Další informace o řízení přístupu na základě rolí (RBAC) v Azure najdete v tématu [Řízení přístupu na základě rolí Azure Active Directory](../active-directory/role-based-access-control-configure.md). Časté otázky k Centru zabezpečení obsahuje informace o [tom, jak oprávnění fungují v Centru zabezpečení](security-center-faq.md#how-are-permissions-handled-in-azure-security-center).

## <a name="access-security-center"></a>Centrum zabezpečení aplikace Access

Máte přístup k Centru zabezpečení z [Azure portálu](https://azure.microsoft.com/features/azure-portal/). [Přihlaste se k portálu](https://portal.azure.com/) a vyberte **možnost Centrum zabezpečení**.

![Možnost Centrum zabezpečení][1]

Otevře se zásuvné **Centrum zabezpečení** .
![Centrum zabezpečení zásuvné][2]

## <a name="set-security-policy"></a>Nastavení zásad zabezpečení

Zásady zabezpečení definuje sadu ovládacích prvků, které jsou vám doporučené zdrojů v zadaném předplatné nebo skupina zdroje. V Centru zabezpečení definování zásad pro předplatné nebo skupiny zdrojů podle potřebám vaší společnosti zabezpečení a typ aplikace nebo citlivosti dat v každé předplatného.

Můžete nastavit zásady zobrazíte doporučení pro auditování SQL a SQL průhledné šifrování (TDE).

- Po zapnutí **auditování SQL a hrozbou, že zjišťování**Centrum zabezpečení doporučuje, aby se zákonnými požadavky, Upřesnit zjišťování a vyšetřování účely povoleno auditování přístup k databázi Azure.
- Po zapnutí **SQL průhledné šifrování**Centrum zabezpečení doporučuje že šifrování v klidu povolit pro databázi SQL Azure, přidružené zálohování a transakce protokoly.

Pokud chcete nastavit zásady zabezpečení, vyberte dlaždici **zásad** na zásuvné Centrum zabezpečení. Na zásuvné **zásady zabezpečení** vyberte předplatné, na které chcete povolit zásady zabezpečení. Výběr **zásad pro prevenci** a **zapněte zabezpečení doporučení, která chcete použít na toto předplatné** .
![Zásady zabezpečení][3]

Další informace najdete v tématu [nastavení zásad zabezpečení](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Správa zabezpečení doporučení

Centrum zabezpečení pravidelně analyzuje stav zabezpečení Azure prostředků. Když Centrum zabezpečení identifikuje potenciální zabezpečením, vytvoří doporučení. Doporučení vás provede procesem konfigurace potřebných ovládacích prvků.

Po nastavení zásad zabezpečení Centrum zabezpečení analyzuje stav zabezpečení prostředky k identifikaci potenciální chyby. Doporučení se zobrazují ve formátu tabulky, kde každý řádek představuje jeden konkrétní doporučení. Pomocí následující tabulky jako odkaz na vám pomůže pochopit dostupné doporučení pro databázi SQL Azure, a co každý doporučení pokud ji použijete. Výběr doporučení přejdete na článek vysvětluje, jak implementovat doporučení v Centru zabezpečení.

| Doporučení | Popis |
| ----- | ----- |
| [Povolení zjišťování auditování a hrozbou, že na serverech SQL](security-center-enable-auditing-on-sql-servers.md) | Doporučuje zapnout auditování a hrozeb rozpoznávání pro databáze SQL servery. (Pouze služby SQL databáze. Neobsahuje Microsoft SQL Server na virtuálních počítačích.) |
| [Povolení zjišťování auditování a hrozbou, že na SQL databáze](security-center-enable-auditing-on-sql-databases.md) | Doporučuje zapnout auditování a hrozeb rozpoznávání pro databáze SQL databáze. (Pouze služby SQL databáze. Neobsahuje Microsoft SQL Server na virtuálních počítačích.) |
| [Povolit šifrování průhledné dat](security-center-enable-transparent-data-encryption.md) | Doporučuje povolit šifrování databáze SQL. (Databáze SQL služba pouze.) |

Doporučení pro Azure zdroje zobrazíte vyberte dlaždici **doporučení** na zásuvné Centrum zabezpečení. Na zásuvné **doporučení** vyberte doporučení na Zobrazit podrobnosti. V tomto příkladu vybereme **Povolit auditování & zjišťování hrozbou, že na serverech SQL**.

![Doporučení][4]

Viz níže Centrum zabezpečení se dozvíte, kde nejsou povoleny auditování a hrozeb zjišťování servery SQL. Po zapnutí auditování, můžete nakonfigurovat nastavení hrozbou, že vyhledávání a nastavení e-mailu výstrahy zabezpečení. Zjišťování hrozbou, že vás upozorní, když zjistí neobvyklých databáze činnosti, které označují potenciální hrozeb zabezpečení databáze. Upozornění na se zobrazují v řídicím panelu Centra zabezpečení.
![Sestavy auditování a hrozeb zjišťování][5]

Postupujte podle pokynů v tématu [Začínáme s SQL databáze hrozbou, že zjišťování](../sql-database/sql-database-threat-detection-get-started.md) zapnout a konfigurace zjišťování hrozbou, že a konfiguraci seznamu e-mailů, které se zobrazí upozornění zabezpečení při zjišťování neobvyklých činností.

Další informace o doporučení, najdete v článku [Správa zabezpečení doporučení](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Sledování stavu zabezpečení

Po povolení [zásady zabezpečení](security-center-policies.md) pro přihlášení k odběru zdroje Centrum zabezpečení analyzovat zabezpečení prostředky k identifikaci potenciální chyby.  V této dlaždici **zdroje zabezpečení stavu** můžete zobrazit stav zabezpečení prostředků. Po kliknutí dlaždici **stavu zabezpečení zdroje** **dat** , otevře zásuvné **Zdroje dat** s SQL doporučení pro problémy například auditování a průhledné šifrování dat nejsou povolené. Je také doporučení pro obecné stav databáze.
![Stav zabezpečení zdroje][6]

Další informace najdete v tématu [Sledování stavu zabezpečení](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Správa zpráv a odpovídání na výstrahy zabezpečení

Centrum zabezpečení automaticky shromažďují analyzuje a integruje protokolu dat z [Azure SQL hrozbou, že zjišťování](../sql-database/sql-database-threat-detection-get-started.md), stejně jako další Azure zdroje ke zjištění skutečné hrozeb a zmenšení falešně pozitivní. Spolu s informací, které potřebujete rychle prošetřit problému a doporučení týkající se nápravě útok je zobrazený seznam uspořádaný zabezpečení upozornění v Centru zabezpečení.

Pokud chcete zobrazit upozornění, vyberte dlaždici **výstrah zabezpečení** na zásuvné Centrum zabezpečení. Na zásuvné **výstrah zabezpečení** vyberte upozornění Další informace o události, které spuštěná výstrahu a co, pokud existuje, je potřeba udělat pro nápravě útok kroků. V tomto příkladu vybereme **vkládání potenciální SQL**.
![Upozornění zabezpečení][7]

Jak vidíte dole, Centrum zabezpečení poskytuje další podrobnosti, které nabízejí pochopení co spouštěný upozornění na cílovém zdroje, případně Zdrojová IP adresa a doporučení o tom, jak nápravě.
![Potenciální vkládáním příkazu SQL][8]

Další informace najdete v tématu [Správa a reagovat na výstrahy zabezpečení](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Další kroky

- [Nejčastější dotazy týkající se Centrum zabezpečení](security-center-faq.md) – najít nejčastější dotazy k použití služby.
- [Příručka pro plánování a operace Centrum zabezpečení](security-center-planning-and-operations-guide.md) – sledovat sadu kroky a úkoly optimalizovat použití centra zabezpečení na základě vaší organizace požadavkům na zabezpečení a model správy cloudu.
- [Centrum zabezpečení – bezpečnost dat](security-center-data-security.md) – zjistěte, jak shromažďuje Centrum zabezpečení a zpracovává data o Azure zdrojů, včetně informací o konfiguraci, metadata, protokoly událostí, soubory se stavem systému apod.
- [Zpracování incidentem zabezpečení](security-center-incident.md) – zjistěte, jak používat funkci upozornění zabezpečení v Centru zabezpečení při zpracování zabezpečení incidentem.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
