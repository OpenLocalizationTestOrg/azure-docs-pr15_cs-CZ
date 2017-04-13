<properties 
    pageTitle="Vzdálené plochy brány a Azure Multi-Factor ověřování serveru pomocí RADIUS"
    description="Tohle je stránka Azure Multi-Factor ověřování, které vám pomohou při nasazení vzdálené plochy (RD) brány a Azure Multi-Factor ověřování serveru pomocí RADIUS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>

# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Vzdálené plochy brány a Azure Multi-Factor ověřování serveru pomocí RADIUS

V mnoha případech Brána vzdálené plochy používá místní NPS k ověřování uživatelů. Tento dokument popisuje, jak chcete směrovat RADIUS požadavky se z vzdálené plochy brány (přes místní NPS) na multi-Factor Authentication Server.

Ověřování serveru Multi-Factor měli byste mít nainstalované na samostatném serveru, které se pak proxy požadavek RADIUS zpátky na NPS na serveru brány vzdálené plochy. Po NPS ověří uživatelské jméno a heslo, vrátí odpověď na multi-Factor ověřování serveru, který provádí druhý faktor ověřování než vrátí výsledek brány.





## <a name="configure-the-rd-gateway"></a>Konfigurace brány RD

RD brány musí být nakonfigurované pro ověřování RADIUS odešlete Server Azure Multi-Factor Authentication. Po instalaci RD brány nakonfigurován a je pracujete, přejděte do vlastnosti RD brány. Přejděte na kartu RD Zakončení úložiště a změňte ho na použití centrální serveru NPS místo místního serveru. Přidání jednoho nebo více Azure Multi-Factor Authentication serverů mezi servery RADIUS a určete sdílené tajná u každého serveru.





## <a name="configure-nps"></a>Konfigurace NPS

Brána RD používá NPS odešlete žádost o RADIUS Azure Vícefaktorové ověřování. Časový limit je třeba změnit nechcete, aby RD brány z vypršení časového limitu před dokončením vícefaktorové ověřování. Následující postup používá ke konfiguraci NPS.

1. V NPS rozbalte nabídku klienti a RADIUS serveru v levém sloupci a klikněte na vzdálený Server RADIUS, skupiny. Přejděte do vlastnosti skupiny TS brány serveru. Upravit servery RADIUS zobrazí a přejděte na kartu Vyrovnávání zatížení. Změna "počet sekund bez odpovědi požadavek je považován za nezobrazí" a "Počet sekund mezi požadavky, když server je považován za nedostupný" 30 60 sekund. Klikněte na kartu ověřování/účet a ujistěte se, že porty RADIUS určené podle porty, které ověřování serveru Multi-Factor bude listening na.
2. NPS musí se konfigurovat taky pro příjem RADIUS ověřování zpět ze serveru Azure Multi-Factor Authentication. Klikněte na klientů RADIUS v nabídce nalevo. Přidání Server Azure Multi-Factor Authentication jako klienta RADIUS. Zvolte popisný název a určete sdílené tajná.
3. Rozbalte oddíl zásady v levém navigačním panelu a klikněte na požádat o zásady připojení. By měl obsahovat jen TS brány se tak mohli OVĚŘOVAT zásad vytvořenou RD brány bylo při konfiguraci zásady žádost o připojení. Tuto zásadu předá RADIUS požadavky na server Multi-Factor Authentication.
4. Zkopírujte tuto zásadu vytvořte nový účet. V nové zásady přidáte podmínku, která odpovídá popisný název klienta s popisným názvem nastavit v předchozím kroku 2 pro klienta RADIUS Server Azure Multi-Factor Authentication. Změňte poskytovatele ověřování s místním počítačem. Tuto zásadu zaručuje, že po přijetí žádosti o RADIUS ze serveru Azure Multi-Factor Authentication dojde k ověření místně místo odesílání žádost RADIUS zpět na Server Azure Multi-Factor ověřování, která by vedla v obraze podmínky. Nechcete, aby podmínka smyčky, nové zásady vznikl nad původní zásady, předá ho Multi-Factor ověřování serveru.

## <a name="configure-azure-multi-factor-authentication"></a>Konfigurace Azure Vícefaktorové ověřování


--------------------------------------------------------------------------------



Server Azure Multi-Factor Authentication nakonfigurovaný proxy serveru RADIUS mezi RD brány a NPS.  Měli byste mít nainstalované na serveru doméně, která je nezávislý serveru RD brány. Následující postup používá ke konfiguraci Server Azure Multi-Factor Authentication.

1. Otevřete Server Azure Multi-Factor Authentication a klikněte na ikonu ověřování. Zaškrtněte políčko Povolit RADIUS ověřování.
2. Na kartě klienti zajistěte, aby že porty odpovídají konfigurovat NPS a klikněte na Přidat. tlačítko. Přidání IP adresu serveru RD brány, název aplikace (nepovinné) a sdílené tajná. Sdílené tajná bude potřeba stejné na Azure Multi-Factor Authentication Server a brána RD.
3. Klikněte na kartě cíl a zvolte přepínač RADIUS servery.
4. Klikněte na Přidat. tlačítko. Zadejte IP adresu, sdílené tajná a portů serveru NPS. Pokud používáte centrální NPS, klienta RADIUS a cílové RADIUS budou stejné. Sdílené tajná musí odpovídat jeden vzhled v části RADIUS klienta serveru NPS.

![Ověřování](./media/multi-factor-authentication-get-started-server-rdg/radius.png)
