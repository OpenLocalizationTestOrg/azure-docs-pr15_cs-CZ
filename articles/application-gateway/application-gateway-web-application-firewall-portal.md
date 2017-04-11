<properties
   pageTitle="Vytvoření aplikace brány firewall webové aplikace pomocí portálu | Microsoft Azure"
   description="Naučte se vytvářet aplikace brány firewall webové aplikace pomocí portálu"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"
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

# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Vytvoření brány aplikací pomocí webové aplikace firewall pomocí portálu

> [AZURE.SELECTOR]
- [Azure portálu](application-gateway-web-application-firewall-portal.md)
- [Azure prostředí PowerShell správce prostředků](application-gateway-web-application-firewall-powershell.md)

Webové aplikace bránu firewall (WAF) v bráně aplikace Azure chrání před běžné web útoky jako vkládáním příkazu SQL, útoky skriptování webů a hijacks relace webových aplikací. Webová aplikace chrání před spoustu OWASP horní 10 běžné web chyby.

Azure aplikace brána je Vyrovnávání zatížení vrstvy-7. Poskytuje selhání směrování výkonu požadavků HTTP mezi jiné servery, ať už jde o v cloudu a místní.
Aplikace nabízí spoustu z funkcí aplikace doručení řadiče ADC včetně HTTP Vyrovnávání zatížení, na základě souborů cookie relace spřažení, převzít Secure (Sockets Layer SSL), vlastní stavu sond podporu více webů a mnoha dalších.
Úplný seznam podporovaných funkcí naleznete [Přehled brány aplikace](application-gateway-introduction.md)

## <a name="scenarios"></a>Scénáře

V tomto článku jsou dvou situacích:

V první scénář Naučte se [Přidat web aplikace firewall k existující bráně aplikace](#add-web-application-firewall-to-an-existing-application-gateway).

Ve druhém scénáři Naučte se [vytvořit aplikace brány firewall webové aplikace](#create-an-application-gateway-with-web-application-firewall)

![Příklad scénáře][scenario]

>[AZURE.NOTE] Další konfiguraci brány aplikace, včetně vlastní stavu sond, back-end fondu adresy a další pravidla nakonfigurované po konfiguraci brány aplikace a nikoli během počáteční nasazení.

## <a name="before-you-begin"></a>Než začnete

Azure brány aplikace vyžadují vlastní podsítě. Při vytváření virtuální sítě, zajistěte ponechejte dostatek místa adresu mít několik podsítí. Po nasazení aplikace brány do podsítě pouze další aplikace bran se přidají do podsítě.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Přidání webových aplikací firewall k existující bráně aplikace

Tento scénář se aktualizuje existující brány aplikace podporuje webové aplikace firewall v režimu prevence.

### <a name="step-1"></a>Krok 1

Přejděte na portál Azure, vyberte existující brány aplikace.

![Vytvoření brány pro aplikace][1]

### <a name="step-2"></a>Krok 2

Klikněte na položku **Konfigurace** a aktualizovat nastavení brány aplikace. Po dokončení klikněte na tlačítko **Uložit**

Nastavení k aktualizaci brány existující aplikace podporuje webové aplikace firewall jsou:

- **Osy** – osy vybraná musí být **WAF** podporuje webové aplikace firewall
- **Velikost SKU** - toto nastavení odpovídá aplikaci brány firewall webové aplikace, jsou dostupné možnosti (**Medium** a **velikost velké**).
- **Stav brány firewall** - toto nastavení zakáže nebo povolí webové aplikace firewall.
- **Režim brány firewall** - toto nastavení je, jak webové aplikace firewall zabývá útokům. Režim **zjišťování** pouze protokoly události, kde režim **Zabránění** protokoly událostí a přestane škodlivým přenosy.

![Základní nastavení zásuvné zobrazující][2]

>[AZURE.NOTE] Zobrazit webové aplikace brány firewall protokoly, musí být povolený diagnostických nástrojů a ApplicationGatewayFirewallLog vybraná. Počet instancí 1 lze vybrat pro účely testování. Je důležité vědět, že všechny instance počet v části dvě instance nevztahuje SLA a tedy nedoporučuje. Malé brány nejsou k dispozici, pokud používáte bránu firewall webové aplikace.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Vytvoření aplikace brány firewall webové aplikace

Tento scénář udělejte toto:

- Vytvoření brány aplikace střední webové aplikace brány firewall s dvě instance.
- Vytvořte virtuální sítě s názvem AdatumAppGatewayVNET s rezervovaná CIDR blokem 10.0.0.0/16.
- Vytvoření podsítě s názvem Appgatewaysubnet používající 10.0.0.0/28 jako jeho blokování CIDR.
- Konfigurace certifikátu SSL převedení.

### <a name="step-1"></a>Krok 1

Přejděte na portál Azure, klikněte na **Nový** > **sítě** > **Aplikace brány**

![Vytvoření brány pro aplikace][1-1]

### <a name="step-2"></a>Krok 2

Vyplňte další základní informace o bráně aplikace. Ujistěte se, že zvolte **WAF** jako úrovně. Až budete hotovi, klikněte na tlačítko **OK**

Informace potřebné pro základní nastavení je:

- **Název** - název brány aplikace.
- **Osy** – osy brány aplikace jsou dostupné možnosti (**Standardní** a **WAF**). Brána firewall webové aplikace je dostupná jenom v WAF osy.
- **Velikost SKU** - toto nastavení odpovídá aplikaci brány, jsou dostupné možnosti (**Medium** a **velikost velké**).
- **Počet instancí** - počet instancí, tato hodnota musí být číslo **2** až **10**.
- **Pole Skupina zdroje** – skupina zdroje pro uložení brány aplikací může být existující nebo nové zdroje.
- **Umístění** - oblasti brány aplikace je na stejném místě skupiny zdrojů. *Umístění je důležité jako virtuální sítě a veřejnou IP musí být ve stejném umístění jako brány*.

![Základní nastavení zásuvné zobrazující][2-2]

>[AZURE.NOTE] Počet instancí 1 lze vybrat pro účely testování. Je důležité vědět, že všechny instance počet v části dvě instance nevztahuje SLA a tedy nedoporučuje. Malé brány nejsou podporované pro webové aplikace brány firewall scénáře.

### <a name="step-3"></a>Krok 3

Po definování základní nastavení dalším krokem je definovat virtuální sítě se nemusí používat. Virtuální sítě máte aplikaci Vyrovnávání zatížení brány aplikace.

Klikněte na **Zvolit virtuální sítě** a konfigurace virtuální sítě.

![zásuvné s nastavením brány aplikace][3]

### <a name="step-4"></a>Krok 4

V zásuvné **Zvolte virtuální sítě** klikněte na **Vytvořit nový**

Když není popsaným v tomto scénáři, může v tomto okamžiku vybrané existující virtuální sítě.  Při použití existující virtuální síti, je důležité vědět, že virtuální sítě vyžaduje prázdné podsítě nebo podsítě prostředků pouze aplikace brány se nemusí používat.

![Zvolte zásuvné virtuální sítě][4]

### <a name="step-5"></a>Krok 5

Vyplňte informace o sítě v zásuvné **Vytvořit virtuální síť** podle popisu v předchozí [scénář](#scenario) popis.

![Vytvoření zásuvné virtuální sítě s zadaných informací][5]

### <a name="step-6"></a>Krok 6

Po vytvoření virtuální sítě dalším krokem je definovat front-end IP brány aplikace. V tomto okamžiku volba je mezi veřejné nebo soukromé IP adresu front-end. Volba závisí na tom, jestli je aplikace internet vystaveného nebo interní pouze. Tento scénář předpokládá pomocí veřejné adresy IP. Vyberte soukromá IP adresu, můžete kliknutí na tlačítko **soukromé** . Zvolili automaticky přiřazené IP adresa nebo můžete klepnutím na zaškrtávací políčko **zvolit konkrétní soukromou IP adresu** zadat ručně.

Klikněte na tlačítko **Zvolit veřejnou IP adresu**. Pokud je k dispozici existující veřejnou IP adresu lze vybrat v tomto okamžiku, v tomto scénáři vytvoříte novou veřejnou IP adresu. Klikněte na **vytvořit nový**

![Zvolte veřejnou IP adresu zásuvné][6]

### <a name="step-7"></a>Krok 7

Potom zadejte popisný název veřejnou IP adresu a klikněte na tlačítko **OK**

![Vytvoření veřejné zásuvné IP adresa][7]

### <a name="step-8"></a>Krok 8

Pak nastavení konfigurace posluchače.  Pokud **http** nic ještě zbývá ke konfiguraci a můžete kliknutí na **OK** . Konfigurace pro používání **https** dál je potřeba.

Pro používání **https**, je potřeba certifikát. Soukromý klíč certifikátu není potřeba tak do .pfx exportujte certifikát musí být k dispozici a heslo k souboru.

Klikněte na **HTTPS**, klikněte na ikonu **složky** vedle textového pole **Odeslat PFX certifikát** .
Přejděte na soubor .pfx certifikát v systému souborů. Po výběru, dejte certifikát popisný název a zadejte heslo pro soubor .pfx.

Po dokončení klikněte na tlačítko **OK** a zkontrolujte nastavení brány aplikace.

![Oddíl konfigurace posluchače na zásuvné nastavení][8]

### <a name="step-9"></a>Krok 9

Konfigurace nastavení **WAF** .

- **Stav brány firewall** - toto nastavení: Zapne nebo vypne WAF.
- **Režimu brány firewall** - tohle nastavení určuje že WAF akce převezme útokům. Pokud je vybrána **zjišťování** , budou návštěvníci je pouze přihlášení k lyncu.  Pokud je vybrána **Zabránění** , přenosy přihlášení k lyncu a zastavit s 403 Neautorizováno.

![nastavení brány firewall webové aplikace][9]

### <a name="step-10"></a>Krok 10

Zkontrolujte souhrnnou stránku a klikněte na **OK**.  Brána aplikace je teď ve frontě a vytvořit.

### <a name="step-12"></a>Krok 12

Po vytvoření brány aplikace přejděte k němu na portálu pokračujte konfigurace brány aplikace.

![Zobrazení zdrojů aplikace brány][10]

Tento postup vytvoření brány pro základní aplikace s výchozím nastavením pro posluchače, back-end fondu, nastavení protokolu http back-end a pravidla. Můžete změnit nastavení tak, aby vyhovovala nasazení po úspěšném zajišťování

## <a name="next-steps"></a>Další kroky

Naučíte se konfigurovat protokolování diagnostiky protokolování událostí, které jsou nerozpoznal nebo zabránit pomocí webové aplikace Firewall navštivte [Diagnostiky brány aplikace](application-gateway-diagnostics.md)

Zjistěte, jak se dá vytvořit vlastní stav sond návštěva [vytvořit vlastní stav zkušební](application-gateway-create-probe-portal.md)

Zjistěte, jak konfigurovat odstranění SSL a udělejte nákladné dešifrování SSL vypnout webových serverů tím, že návštěva [Konfigurace převzít SSL](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[1-1]: ./media/application-gateway-web-application-firewall-portal/figure1-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[4]: ./media/application-gateway-web-application-firewall-portal/figure4.png
[5]: ./media/application-gateway-web-application-firewall-portal/figure5.png
[6]: ./media/application-gateway-web-application-firewall-portal/figure6.png
[7]: ./media/application-gateway-web-application-firewall-portal/figure7.png
[8]: ./media/application-gateway-web-application-firewall-portal/figure8.png
[9]: ./media/application-gateway-web-application-firewall-portal/figure9.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png