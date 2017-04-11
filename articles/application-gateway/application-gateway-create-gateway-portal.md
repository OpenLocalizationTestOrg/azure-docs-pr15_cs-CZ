<properties
   pageTitle="Vytvoření brány aplikační na portálu | Microsoft Azure"
   description="Naučte se vytvářet aplikace brány tak, že na portálu"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-by-using-the-portal"></a>Vytvoření brány aplikace pomocí portálu

> [AZURE.SELECTOR]
- [Azure portálu](application-gateway-create-gateway-portal.md)
- [Azure prostředí PowerShell správce prostředků](application-gateway-create-gateway-arm.md)
- [Azure klasické prostředí PowerShell](application-gateway-create-gateway.md)
- [Azure správce prostředků šablony](application-gateway-create-gateway-arm-template.md)
- [Azure rozhraní příkazového řádku](application-gateway-create-gateway-cli.md)

Azure aplikace brána je Vyrovnávání zatížení vrstvy-7. Poskytuje selhání směrování výkonu požadavků HTTP mezi jiné servery, ať už jde o v cloudu a místní. Aplikace brány nabízí spoustu z funkcí aplikace doručení řadiče ADC včetně Vyrovnávání zatížení HTTP, na základě souborů cookie relace spřažení, převzít Secure (Sockets Layer SSL), vlastní stavu sond podporu více webů a mnoha dalších. Úplný seznam podporovaných funkcí naleznete [Přehled brány aplikace](application-gateway-introduction.md)

## <a name="scenario"></a>Scénář

V tomto scénáři je Naučte se vytvářet aplikace brány pomocí portálu Azure.

Tento scénář udělejte toto:

- Vytvoření brány pro střední aplikace s dvě instance.
- Vytvořte virtuální sítě s názvem AdatumAppGatewayVNET s rezervovaná CIDR blokem 10.0.0.0/16.
- Vytvoření podsítě s názvem Appgatewaysubnet používající 10.0.0.0/28 jako jeho blokování CIDR.
- Konfigurace certifikátu SSL převedení.

![Příklad scénáře][scenario]

>[AZURE.IMPORTANT] Další konfiguraci brány aplikace, včetně vlastní stavu sond, back-end fondu adresy a další pravidla nakonfigurované po konfiguraci brány aplikace a nikoli během počáteční nasazení.

## <a name="before-you-begin"></a>Než začnete

Azure brány aplikace vyžadují vlastní podsítě. Při vytváření virtuální sítě, zajistěte ponechejte dostatek místa adresu mít několik podsítí. Po nasazení bráně aplikace do podsítě pouze další aplikace bran se přidají do podsítě.

## <a name="create-the-application-gateway"></a>Vytvoření brány pro aplikace

### <a name="step-1"></a>Krok 1

Přejděte na portál Azure, klikněte na **Nový** > **sítě** > **Bráně aplikace**

![Vytváření bráně aplikace][1]

### <a name="step-2"></a>Krok 2

Vyplňte další základní informace o bráně aplikace. Až budete hotovi, klikněte na tlačítko **OK**

Informace potřebné pro základní nastavení je:

- **Název** - název brány aplikace.
- **Osy** – to je vrstva brány aplikace. Dvě úrovně, jsou k dispozici **WAF** a **Standardní**. WAF umožňuje funkce webové aplikace bránu firewall.
- **Velikost SKU** - toto nastavení odpovídá aplikaci brány, dostupné možnosti jsou (**malá**, **Střední**a **velké**). *Malé není k dispozici, když je vybrán WAF vrstvy*
- **Počet instancí** - počet instancí, tato hodnota musí být číslo 2 až 10.
- **Pole Skupina zdroje** – skupina zdroje pro uložení brány aplikací může být existující nebo nové zdroje.
- **Umístění** - oblasti brány aplikace je na stejném místě skupiny zdrojů. *Umístění je důležité jako virtuální sítě a veřejnou IP musí být ve stejném umístění jako brány*.

![Základní nastavení zásuvné zobrazující][2]

>[AZURE.NOTE] Počet instancí 1 lze vybrat pro účely testování. Je důležité vědět, že všechny instance počet v části dvě instance nevztahuje SLA a tedy nedoporučuje. Malé bran mají být použity pro vývojáře test a ne pro účely výroby.

### <a name="step-3"></a>Krok 3

Po definování základní nastavení dalším krokem je definovat virtuální sítě se nemusí používat. Virtuální sítě máte aplikaci Vyrovnávání zatížení brány aplikace.

Klikněte na **Zvolit virtuální sítě** a konfigurace virtuální sítě.

![zásuvné s nastavením brány aplikace][3]

### <a name="step-4"></a>Krok 4

V zásuvné *Zvolte virtuální sítě* klikněte na **Vytvořit nový**

Když není popsaným v tomto scénáři, může v tomto okamžiku vybrané existující virtuální sítě.  Při použití existující virtuální síti, je důležité vědět, že virtuální sítě musí prázdné podsítě nebo podsítě prostředků pouze aplikace brány se nemusí používat.

![Zvolte zásuvné virtuální sítě][4]

### <a name="step-5"></a>Krok 5

Vyplňte informace o sítě v zásuvné **Vytvořit virtuální síť** podle popisu v předchozí [scénář](#scenario) popis.

![Vytvoření zásuvné virtuální sítě s zadaných informací][5]

### <a name="step-6"></a>Krok 6

Po vytvoření virtuální sítě dalším krokem je definovat front-end IP brány aplikace. V tomto okamžiku volba je mezi veřejné nebo soukromé IP adresu front-end. Volba závisí na tom, jestli je aplikace internet vystaveného nebo interní pouze. Tento scénář předpokládá pomocí veřejné adresy IP. Vyberte soukromá IP adresu, můžete kliknutí na tlačítko **soukromé** . Zvolili automaticky přiřazené IP adresa nebo můžete klepnutím na zaškrtávací políčko **vybrat konkrétní soukromou IP adresu** zadat ručně.

### <a name="step-7"></a>Krok 7

Klikněte na tlačítko **Zvolit veřejnou IP adresu**. Pokud je k dispozici existující veřejnou IP adresu lze vybrat v tomto okamžiku, v tomto scénáři vytvoříte novou veřejnou IP adresu. Klikněte na **vytvořit nový**

![Zvolte veřejnou IP adresu zásuvné][6]

### <a name="step-8"></a>Krok 8

Potom zadejte popisný název veřejnou IP adresu a klikněte na tlačítko **OK**

![Vytvoření veřejné zásuvné IP adresa][7]

### <a name="step-9"></a>Krok 9

Poslední nastavení konfigurace při vytváření aplikace brány je konfigurace posluchače.  Pokud **http** nic ještě zbývá ke konfiguraci a můžete kliknutí na **OK** . Konfigurace pro používání **https** dál je potřeba.

Pro používání **https**, je potřeba certifikát. Soukromý klíč certifikátu není potřeba tak k exportu .pfx certifikát a hesla muset být k dispozici.

### <a name="step-10"></a>Krok 10

Klikněte na **HTTPS**, klikněte na ikonu **složky** vedle textového pole **Odeslat PFX certifikát** .
Přejděte na soubor .pfx certifikát v systému souborů. Po výběru, dejte certifikát popisný název a zadejte heslo pro soubor .pfx.

Po dokončení klikněte na tlačítko **OK** a zkontrolujte nastavení pro bráně aplikace.

![Oddíl konfigurace posluchače na zásuvné nastavení][9]

### <a name="step-11"></a>Krok 11

Zkontrolujte souhrnnou stránku a klikněte na **OK**.  Brána aplikace je teď ve frontě a vytvořit.

### <a name="step-12"></a>Krok 12

Po vytvoření brány aplikace přejděte k němu na portálu pokračujte konfigurace brány aplikace.

![Zobrazení zdrojů aplikace brány][10]

Tento postup vytvoření brány pro základní aplikace s výchozím nastavením pro posluchače, back-end fondu, nastavení protokolu http back-end a pravidla. Můžete změnit nastavení tak, aby vyhovovala nasazení po úspěšném zřizování.

## <a name="next-steps"></a>Další kroky

Tento scénář vytvoří výchozí aplikace bránu. Další kroky jsou konfigurace brány aplikace přidání členů fondu, úpravou nastavení a nastavení pravidel v brány mohlo fungovat správně.

Zjistěte, jak se dá vytvořit vlastní stav sond návštěva [vytvořit vlastní stav zkušební](application-gateway-create-probe-portal.md)

Zjistěte, jak konfigurovat odstranění SSL a udělejte nákladné dešifrování SSL vypnout webových serverů tím, že návštěva [Konfigurace převzít SSL](application-gateway-ssl-portal.md)

Zjistěte, jak chránit aplikací pomocí [Webové aplikace Firewall](application-gateway-webapplicationfirewall-overview.md) funkce aplikace brány.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[6]: ./media/application-gateway-create-gateway-portal/figure6.png
[7]: ./media/application-gateway-create-gateway-portal/figure7.png
[8]: ./media/application-gateway-create-gateway-portal/figure8.png
[9]: ./media/application-gateway-create-gateway-portal/figure9.png
[10]: ./media/application-gateway-create-gateway-portal/figure10.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
