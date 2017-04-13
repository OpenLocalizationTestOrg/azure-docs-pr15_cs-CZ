<properties
   pageTitle="Přidání bránu firewall webové aplikace v Centru zabezpečení Azure | Microsoft Azure"
   description="V tomto dokumentu se dozvíte, jak implementovat Azure Centrum zabezpečení doporučení **Přidat bránu firewall webové aplikace** a **Ochrana aplikace dokončit**."
   services="security-center"
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Přidání bránu firewall webové aplikace v Centru zabezpečení Azure

Centrum zabezpečení Azure může doporučit přidat bránu firewall webové aplikace (WAF) od partnera společnosti Microsoft pro zajištění webových aplikací. Tento dokument provede vás provede příklad toho, jak můžete to udělat.

> [AZURE.NOTE] Tento dokument zavádí službu pomocí nasazení příklad.  Toto není podrobného průvodce.

## <a name="implement-the-recommendation"></a>Implementace doporučení

1. **Doporučení** zásuvné vyberte **zabezpečené webové aplikace pomocí webové aplikace firewall**.
![Zabezpečený web aplikace][1]

2. V zásuvné **zabezpečené webových aplikací pomocí webové aplikace firewall** vyberte Webová aplikace. Otevře se zásuvné **Přidat bránu Firewall webové aplikace** .
![Přidání bránu firewall webové aplikace][2]
3. Můžete použít existující web aplikace brány firewall pro síť, pokud je k dispozici nebo vytvořte novou. V tomto příkladu nejsou žádné existující WAFs k dispozici tak vytvoříme nové WAF.

4. Pokud chcete vytvořit nové WAF, vyberte ze seznamu integrované partnerů řešení. V tomto příkladu jsme vybereme **Barracuda webové aplikace Firewall**.
5. Zásuvné **Barracuda webové aplikace Firewall** otevře poskytuje informace o řešení partnera. Výběrem možnosti **vytvořit** v zásuvné informace.
![Informace o zásuvné brána firewall][3]

6. Otevře se zásuvné **Nová brána Firewall webové aplikace** , kde můžete provést kroky **Konfigurace OM** a zadejte **WAF informace**. Vyberte **OM konfiguraci**.

7. V zásuvné **OM konfigurace** zadáte informace potřebné k číselník přes virtuální počítač, který se spustí WAF.
![Konfigurace OM][4]
8. Vraťte se do zásuvné **Nová brána Firewall webové aplikace** a vyberte **WAF informace**. V zásuvné **WAF informace** nakonfigurujete WAF samotné. Krok 7 umožňuje konfigurace virtuálního počítače, na které se spustí WAF a krok 8 umožňuje zajistit WAF samotné.

## <a name="finalize-application-protection"></a>Dokončení Ochrana aplikace

1. Vraťte se do zásuvné **doporučení** . Nová položka vygenerované po vytvoření WAF s názvem **Ochrana aplikace dokončit**. Tento údaj značí, že budete muset dokončit proces skutečně vedení nahoru WAF v rámci virtuální sítě Azure tak, že můžete chránit aplikace.
![Dokončení Ochrana aplikace][5]

2. Vyberte **Dokončit Ochrana aplikace**. Otevře se nové zásuvné. Uvidíte, že je webovou aplikaci, která není potřeba přenosů přesměrována.
3. Vyberte webová aplikace. Zásuvné otevře se vám postup pro dokončení nastavení webové aplikace bránu firewall. Postupujte podle pokynů a vyberte **přenosy omezit**. Centrum zabezpečení pak provede vedení zdola nahoru.
![Omezit provoz][6]

> [AZURE.NOTE] Přidáním tyto aplikace do stávajících nasazení WAF můžete chránit více webových aplikací v Centru zabezpečení. WAF spotřebiče (vytvořené pomocí Správce prostředků nasazení modelu) musí být nasazené na samostatném virtuální sítě. WAF spotřebiče (vytvořená pomocí klasické nasazení modelu) jsou používat skupiny zabezpečení sítě. Toto odborné pomoci bude prodlužuje na plně přizpůsobené nasazení (klasické) WAF zařízení v budoucnu. Další informace o [klasickém a modelů nasazení Správce prostředků](../azure-classic-rm.md) pro Azure zdroje.

Nyní jsou plně integrovaný protokoly z této WAF. Centrum zabezpečení můžete začít automaticky shromažďování a analýza protokoly tak, aby vám která může ukazovat výstrah zabezpečení důležité.

## <a name="see-also"></a>Viz taky

Tento dokument ukázal, jak implementovat Centrum zabezpečení doporučení "Přidat webovou aplikaci." Další informace o konfiguraci brány firewall webové aplikace, najdete v těchto článcích:

- [Konfigurace brány Firewall webové aplikace (WAF) pro aplikaci služby prostředí](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Sledování stavu zabezpečení v Centru zabezpečení Azure](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Správa zabezpečení doporučení v Centru zabezpečení Azure](security-center-recommendations.md) – zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Časté otázky k Centru zabezpečení azure](security-center-faq.md) – najít často kladené otázky týkající se pomocí služby.
- Příspěvky [azure zabezpečení blog](http://blogs.msdn.com/b/azuresecurity/) – najít blog týkající se Azure zabezpečení a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
