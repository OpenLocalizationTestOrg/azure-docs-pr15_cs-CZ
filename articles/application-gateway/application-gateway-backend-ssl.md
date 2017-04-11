<properties
   pageTitle="Povolení zásady SSL a konce SSL ve bráně aplikace | Microsoft Azure"
   description="Tato stránka obsahuje základní informace o aplikaci brány podporují konce SSL."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>Povolení zásady SSL a konce SSL ve bráně aplikace

## <a name="overview"></a>Základní informace

Brána pro aplikace podporuje ukončení SSL na brány, po který přenos obvykle přecházel bez šifrování serverům back-end. Díky webových serverů je unburdened shora nákladné šifrování/dešifrování. Ale některé zákazníci nešifrovaném komunikace se servery back-end není přijatelné možnost. Může to být kvůli požadavky na zabezpečení a dodržování předpisů nebo aplikace mohou pouze přijímat zabezpečené připojení. Pro tyto aplikace aplikace brány nyní podporuje šifrování SSL konce.

Ukončení za účelem SSL umožňuje bezpečně přenášet citlivá data do back-end šifrované stále při využití výhod funkce Vyrovnávání zatížení vrstvy 7 které bráně aplikace podporuje, jako jsou soubory cookie spřažení založené na adrese URL směrování, pro směrování založené na webech nebo na možnost vloží X-předaných-* záhlaví.

Jestliže nakonfigurována konce SSL jiný režim komunikace aplikace brány relací SSL uživatele na brány a dešifruje provozu. Použije se nakonfigurované pravidla vyberte instance příslušné back-end fondu provoz směrovat na. Brána aplikace pak zahájí nové připojení SSL do back-end serveru a znovu šifruje dat pomocí certifikátu veřejného klíče back-end serveru před odesláním žádosti o back-end. Ukončení ukončit SSL aktivované nastavením nastavení protokolu v BackendHTTPSetting na Https, která se použije k fondu back-end. Každý back-end serveru ve fondu back-end s povoleným zabezpečením SSL konce musí být nakonfigurované pomocí certifikátu umožňuje zabezpečená komunikace.

![scénář konce ssl][1]

V tomto příkladu jsou směrovány požadavky pomocí TLS1.2 serverům back-end v Pool1 protokolem SSL konce.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>Koncová SSL a povolení programů certifikátů

Aplikace brány pouze komunikaci se instance známé back-end s povolené jejich certifikát brány aplikace. Povolit povolení programů certifikátů, musíte nahrajete veřejným klíčem certifikáty serveru back-end aplikace brány (není kořenového certifikátu). Potom jsou povoleny pouze připojení k známé a povolené back-end. Zbývající back-end za následek chybu brány. Podepsaný certifikáty jsou pro účely testování pouze a ne doporučeného pro typ pracovního vytížení výroby. Tato poukázky musí být povolené brána aplikaci popsanou v předchozích krocích před jejich použitím.

## <a name="application-gateway-ssl-policy"></a>Zásady použití brány SSL

Brána pro aplikace podporuje, která dokáže nahradit SSL vyjednávání zásad uživatelů, které umožňují větší kontrolu nad připojení SSL zákazníci v bráně aplikace.

1. Protokol SSL 2.0 a 3.0 zakázané ve výchozím nastavení pro všechny aplikace brány. Nejsou konfigurovatelné vůbec.
2. SSL zásad definice nabízí možnost zakázat některou z následujících 3 protokolů - TLSv1\_0, TLSv1\_1, TLSv1\_2.
3. Pokud je definován bez zásad SSL všechny tři (TLSv1\_0, TLSv1\_1, TLSv1_2) jsou povolené.

## <a name="next-steps"></a>Další kroky

Po získání informací o ukončení za účelem SSL a SSL zásady, přejděte na [Povolit konce SSL ve bráně aplikace](application-gateway-end-to-end-ssl-powershell.md) vytvořit brány aplikační s možnost posílání přenosy back-end v šifrované podobě.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png