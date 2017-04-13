<properties
   pageTitle="Správa partnerských řešení v Centru zabezpečení Azure | Microsoft Azure"
   description="Tento dokument provede vás jak Centrum zabezpečení Azure umožňuje monitor – rychlý přehled stavu partnerských řešení integrovaný s Azure předplatné."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Sledování partnerských řešení pomocí Centra zabezpečení Azure

Tento dokument vás provede sledování stavu partnerských řešení v Centru zabezpečení Azure.

> [AZURE.NOTE] Tento dokument uvádí službu pomocí nasazení příklad. Toto není podrobného průvodce.

## <a name="monitoring-partner-solutions"></a>Sledování partnerských řešení

Na dlaždici **partnerských řešení** na zásuvné **Centrum zabezpečení** umožňuje monitor – rychlý přehled stavu partnerských řešení integrovaný s Azure předplatné.
![Dlaždice partnerských řešení][1]

Dlaždice **partnerských řešení** zobrazí počet partnerských řešení a stav souhrn tato řešení.

**Stav** řešení partnerů může být:

- Chráněné (zelený) – není žádný stav problém.
- Nefunkční (červená) – není problém stavu, které si žádá okamžitou pozornost.
- Zastavit vytváření sestav (oranžovou): řešení přestal vykazování jeho stavu.
- Stav neznámý ochrany (oranžovou) – je stavu řešení v současné době kvůli neúspěšných proces obrazovky pro přidání nového prostředku do existující řešení neznámý.
- Není uvedena (šedý) – řešení nebyla vykázaného nic a zároveň, může být na řešení stav unreported v případě jenom připojení a je pořád nasazení.

Pokud nejsou žádné řešení k vašemu předplatnému bude dlaždici uveďte, že nejsou žádné řešení. Výběr dlaždice **partnerských řešení** vám umožní otevření zásuvné **doporučení** pro nasazení partnerských řešení zabezpečení.
![Žádné partnerských řešení][2]

Zobrazení stavu partnerských řešení:

1. Vyberte dlaždici **partnerských řešení** . Zásuvné otevře se seznamem vaše partnerských řešení připojené k Centru zabezpečení.
![Partnerských řešení][3]

2. Vyberte partnerských řešení. V tomto příkladu umožňuje vybrat řešení **F5 WAF2** .  Zásuvné se otevře zobrazující stav partnerských řešení a na řešení přiřazené zdroje. Vyberte možnost **konzoly řešení** otevřete možnostem správy partnera pro toto řešení.
![Podrobnosti partnerských řešení][4]

3. Přejděte zpátky na zásuvné **F5 WAF2** a vyberte **aplikaci odkaz**. Otevře se zásuvné **Odkaz aplikací** . Tady můžete připojit prostředky k řešení partnerů.
![Propojení zdroje partnerských řešení][5]

## <a name="see-also"></a>Viz taky
V tomto dokumentu, které byly vydané dlaždici **Partnerských řešení** v Centru zabezpečení. Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Správa doporučení týkající se zabezpečení v Centru zabezpečení Azure](security-center-recommendations.md) – zjistěte, jak doporučení pomocí chránit vaše Azure zdroje.
- [Sledování v Centru zabezpečení Azure stavu zabezpečení](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Časté otázky k Centru zabezpečení Azure](security-center-faq.md) – najít nejčastější dotazy k použití služby.
- [Zabezpečení Azure blog](http://blogs.msdn.com/b/azuresecurity/) – získání nejnovější informace Azure zabezpečení a informací.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
