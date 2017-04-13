<properties
   pageTitle="Centrum zabezpečení Azure a Azure virtuálních počítačích | Microsoft Azure"
   description="Tento dokument pomůže pochopit, jak Centrum zabezpečení Azure můžete chránit je Azure virtuálních počítačích."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-and-azure-virtual-machines"></a>Centrum zabezpečení Azure a Azure virtuálních počítačích

[Centrum zabezpečení Azure](https://azure.microsoft.com/services/security-center/) pomáhá zabránit, zjišťování a odpovědět na rizika. Poskytuje integrované zabezpečení sledování a zásady správy přes Azure předplatných, pomáhají zjistit hrozeb, které může jinak všimnout a spolupracuje Rozsáhlá platforma řešení zabezpečení.

Tento článek popisuje, jak Centrum zabezpečení můžete zabezpečené vaší Azure virtuálních počítačích (OM).

## <a name="why-use-security-center"></a>Proč používat Centrum zabezpečení?

Centrum zabezpečení vám pomáhá chránit dat v Azure zadáním přehled o nastavení zabezpečení virtuálního počítače. Centrum zabezpečení zabezpečuje vaše VMs, nebudou mít k dispozici následující možnosti:

- Nastavení zabezpečení operační systém (s operačním systémem) s pravidly doporučená konfigurace
- Zabezpečení systému a důležité aktualizace, které nebyly nalezeny
- Endpoint protection doporučení
- Ověření šifrování disku
- Hodnocení chybu a remediation
- Zjišťování hrozbou, že

Kromě ochrana Azure VMs Centrum zabezpečení také poskytuje zabezpečení monitorování a správu pro cloudové služby, aplikací služby, virtuálních sítí a další. 

>[AZURE.NOTE] V tématu [Úvod k Centru zabezpečení Azure](security-center-intro.md) zobrazíte další informace o Azure Centrum zabezpečení.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Začít s Centrum zabezpečení Azure, musíte znát a zvažte následující skutečnosti:

- Pokud nemáte předplatné Microsoft Azure. Další informace o Centrum zabezpečení bezplatné a standardní úrovní najdete v článku [Ceny Centrum zabezpečení](https://azure.microsoft.com/pricing/details/security-center/) .
- Plánování zavádění centra zabezpečení najdete v článku [plánování Azure Centrum zabezpečení a operace Průvodce](security-center-planning-and-operations-guide.md) Další informace o aspektech plánování a operací.
- Informace týkající se operační systém podpory najdete v tématu [Centrum zabezpečení Azure nejčastější dotazy](security-center-faq.md). 

## <a name="set-security-policy"></a>Nastavení zásad zabezpečení

Shromažďování dat, musí být povolený tak tento Centrum zabezpečení Azure můžete získat informace potřebné k poskytnutí doporučení a výstrahy, které se vytvářejí podle zásady zabezpečení, které jste nakonfigurovali. Na následujícím obrázku můžete se podívat, aby byl **shromažďování dat** **zapnutá**.

Zásady zabezpečení definuje sadu ovládacích prvků, které jsou vám doporučené zdrojů v zadaném předplatné nebo skupina zdroje. Před povolením zásady zabezpečení, musí mít shromažďování dat povolený, Centrum zabezpečení shromažďuje údaje o virtuálních počítačích k posouzení stavu zabezpečení, poskytovat doporučení týkající se zabezpečení a upozornění na hrozeb. V Centru zabezpečení definování zásad pro Azure předplatných nebo skupiny zdrojů podle potřebám vaší společnosti zabezpečení a typ aplikace nebo citlivosti dat v každé předplatného. 

![Zásady zabezpečení](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

>[AZURE.NOTE] Další informace o jednotlivých **zásad pro prevenci** k dispozici, projděte si článek [nastavení zásad zabezpečení](security-center-policies.md) .

## <a name="manage-security-recommendations"></a>Správa zabezpečení doporučení

Centrum zabezpečení analyzuje stav zabezpečení Azure prostředků. Když Centrum zabezpečení identifikuje potenciální zabezpečením, vytvoří doporučení. Doporučení vás provede procesem konfigurace potřebných ovládacích prvků.

Po nastavení zásad zabezpečení, Centrum zabezpečení analyzuje stav zabezpečení prostředky k identifikaci potenciální chyby. Doporučení jsou zobrazeny ve formátu tabulky, kde každý řádek představuje jeden konkrétní doporučení. Následující tabulka obsahuje příklady některých doporučení pro Azure VMs a co každý z nich bude dělat, když ho použijete. Když vyberete doporučení, můžete zadat informace, které vám ukáže, jak implementovat doporučení v Centru zabezpečení.

|Doporučení|Popis|
|-----|-----|
|[Povolení shromažďování dat u předplatného](security-center-enable-data-collection.md)|Doporučuje zapnutí shromažďování dat v zásady zabezpečení pro platnost předplatného a všechny virtuálních počítačích (VMs) v předplatné.|
|[Nápravě chyby OS](security-center-remediate-os-vulnerabilities.md)|Doporučuje zarovnání konfiguraci OS s pravidly doporučená konfigurace například nepovolují ukládání hesel.|
|[Použití aktualizací systému](security-center-apply-system-updates.md)|Doporučuje nasazení chybějící systém zabezpečení a důležité aktualizace pro VMs.|
|[Restartujte po aktualizace systému](security-center-apply-system-updates.md#reboot-after-system-updates)|Doporučuje restartovat OM dokončete proces instalace aktualizací systému.|
|[Instalace Endpoint Protection](security-center-install-endpoint-protection.md)|Doporučuje zřízení antimalware programů VMs (pouze Windows VMs).|
|[Řešení Endpoint Protection stavu upozornění](security-center-resolve-endpoint-protection-health-alerts.md)|Doporučuje vyřešit chyby ochrany koncového bodu.|
|[Povolení OM agenta](security-center-enable-vm-agent.md)|Slouží k zobrazení, které vyžadují VMs agenta OM. V VMs musí být nainstalované agenta OM, aby bylo možné vytvořit oprava prohledávání vyhledávání podle směrného plánu a antimalware programy. Agent OM nainstalovaný ve výchozím nastavení pro VMs, které jsou nasazeny z Azure Marketplace. V článku [rozšíření – část 2 a OM Agent](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) obsahuje informace o tom, jak nainstalovat agenta OM.|
| [Použití šifrování disku](security-center-apply-disk-encryption.md) |Doporučuje šifrování disků OM šifrovanou Azure disku (Windows a Linux VMs). Šifrování doporučujeme OS a data svazky na vaše OM.|
| [Chyba hodnocení Nenainstalováno](security-center-vulnerability-assessment-recommendations.md) | Doporučuje nainstalovat řešení zhodnocení chybu na vaše OM. |
| [Nápravě chyby](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Umožňuje zobrazit chyby systému a aplikací zjistit řešení zhodnocení chybu na vaše OM nainstalovaný. |

>[AZURE.NOTE] Další informace o doporučení, najdete v článku [Správa zabezpečení doporučení](security-center-recommendations.md) .

## <a name="monitor-security-health"></a>Sledování stavu zabezpečení

Po povolení [zásady zabezpečení](security-center-policies.md) pro přihlášení k odběru zdroje Centrum zabezpečení analyzovat zabezpečení prostředky k identifikaci potenciální chyby.  Můžete zobrazit stav zabezpečení svých prostředcích spolu s jakékoliv problémy se v zásuvné **stavu zabezpečení zdroje** . Po kliknutí na dlaždici stavu **zabezpečení prostředků** **virtuálních počítačích** , otevře se s doporučení pro vaše VMs zásuvné **virtuálních počítačích** . 

![Stav zabezpečení](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Správa zpráv a odpovídání na výstrahy zabezpečení

Centrum zabezpečení automaticky shromažďují analyzuje a integruje protokolu dat z Azure zdrojů, sítě a připojené partnerských řešení (třeba bránu firewall a koncový bod řešení ochrany), zjistit skutečné hrozeb a zmenšení falešně pozitivních výsledků. Využitím různorodého agregační [funkce rozpoznávání](security-center-detection-capabilities.md)je moci vygenerování výstrah zabezpečení uspořádaný můžete snadno prozkoumat problém poskytují doporučení, jak k nápravě útoky Centrum zabezpečení.

![Upozornění zabezpečení](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Vyberte upozornění zabezpečení k další informace o události, který spustil, oznámení a co, pokud existuje, je potřeba udělat pro nápravě útok kroků. Upozornění zabezpečení jsou seskupená podle [typu](security-center-alerts-type.md) a datum.


## <a name="see-also"></a>Viz taky

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
