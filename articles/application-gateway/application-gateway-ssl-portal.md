<properties
   pageTitle="Konfigurace brány aplikace pro převedení SSL pomocí portálu | Microsoft Azure"
   description="Tato stránka obsahuje pokyny k vytvoření brány aplikační s SSL převzít pomocí portálu"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Konfigurace aplikace bráně pro převedení SSL pomocí portálu

> [AZURE.SELECTOR]
-[Azure portál](application-gateway-ssl-portal.md)
-[Azure správce prostředků PowerShell](application-gateway-ssl-arm.md)
-[Klasické prostředí PowerShell Azure](application-gateway-ssl.md)

Ukončit relaci Secure (Sockets Layer SSL) po bránu vyhnout nákladné úkoly dešifrování SSL stát na farmě web je možné konfigurovat Azure aplikace brány. Převedení SSL také zjednodušuje instalační program serveru front-end a správu webové aplikace.

## <a name="scenario"></a>Scénář

Následující scénář prochází konfigurace SSL převzít ve stávající bráně aplikace. Scénář předpokládá, že už jste postupovali podle kroků k [Vytvoření bráně aplikace](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Než začnete

Převedení SSL nakonfigurovat aplikaci brány, je požadován certifikát. Certifikát se načíst na brány aplikace a slouží k šifrování a dešifrování přenosů prostřednictvím SSL. Certifikát musí být ve formátu osobních informací Exchange (pfx). V tomto formátu umožňuje privátním klíčem k exportu umožňující brána aplikace slouží k šifrování a dešifrování provoz.

## <a name="add-an-https-listener"></a>Přidání posluchače HTTPS

Posluchače HTTPS vyhledá přenosy na základě jeho konfigurace a pomáhá provoz směrovat na fondů back-end.

### <a name="step-1"></a>Krok 1

Přejděte na portál Azure a vyberte existující brány aplikace

### <a name="step-2"></a>Krok 2

Klikněte na posluchače a klikněte na tlačítko Přidat přidat posluchače.

![aplikace brány přehled zásuvné][1]

### <a name="step-3"></a>Krok 3

Vyplňte požadované informace pro posluchače a nahrát .pfx certifikát, až budete hotovi, klikněte na OK.

**Název** - hodnota je popisný název posluchače.

**Konfigurace Frontend IP** – tato hodnota je konfigurace frontend IP, který se používá pro posluchače.

**Frontend port (jméno/Port)** - popisný název port použitý na front-end aplikace brány a skutečné port použitý.

**Protocol (protokol)** - přepínače a zjistit, pokud https nebo http se používá pro klientskou.

**Certifikát (jméno a heslo)** – Pokud SSL převedení se používá, certifikát .pfx je nutný pro toto nastavení a popisný název a heslo jsou potřeba.

![Přidání zásuvné posluchače][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Vytvoření pravidla a přidružit k posluchače

Byla vytvořena posluchače. Je potřeba vytvořit pravidlo, které zpracovávat přenosy z posluchače.

### <a name="step-1"></a>Krok 1

Klikněte na položku **pravidla** brány aplikace a potom klikněte na Přidat.

![aplikace brány pravidla zásuvné][3]

### <a name="step-2"></a>Krok 2

Na zásuvné **Přidat základní pravidlo** zadejte do pole popisný název pravidla a zvolte posluchače vytvořili v předchozím kroku. Vyberte odpovídající back-end fondu a nastavení protokolu http a klikněte na tlačítko **OK**

![protokol HTTPS v okně Nastavení][4]

Nastavení se uloží ho bráně aplikace. Uložení obrázku pro nastavení může chvíli trvat, než jsou dostupné pro zobrazení pomocí portálu nebo pomocí Powershellu. Po uložení brány aplikace zpracovává šifrování a dešifrování provoz. Všechny přenosy mezi aplikací brány a webových serverů back-end se bude přistupovat prostřednictvím protokolu http. Veškeré komunikace zpátky ke klientovi v případě zahájená přes protokol https budou vráceny klientovi šifrovaná.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak konfigurovat vlastní stavu zkušební s bráně aplikace Azure, najdete v článku [vytvořit vlastní stav zkušební](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png