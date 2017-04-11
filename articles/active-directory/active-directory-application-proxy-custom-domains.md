<properties
    pageTitle="Práce s vlastních domén v Azure AD aplikace Proxy | Microsoft Azure"
    description="Zahrnuje jak pracovat s vlastních domén v Azure AD aplikace Proxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Práce s vlastních domén v Azure AD aplikace Proxy

Použití výchozí doménu umožňuje nastavit adresu URL jako interní a externí adresy URL pro přístup k aplikaci tak, aby vaši uživatelé pouze jednu adresu URL pamatovat pro přístup k aplikaci, bez ohledu na to, kde jsou prohlížení ze. To umožňuje taky vytvořit zástupce jednoho na panelu aplikace Access pro aplikaci. Pokud používáte výchozí doménu poskytovanou Azure AD aplikace Proxy, je nevyžaduje žádnou další konfiguraci budete muset povolit vaší domény. V případě, že používáte vlastní doménu, obsahuje několik věcí, které musíte udělat, abyste měli jistotu, že aplikace Proxy rozpozná svoji doménu a ověří své certifikáty.

## <a name="selecting-your-custom-domain"></a>Výběr vaší vlastní domény

1. Publikování aplikace podle pokynů v [aplikacích publikovat s Proxy aplikace](active-directory-application-proxy-publish.md).
2. Až se aplikace zobrazí v seznamu aplikací, vyberte ho a klikněte na **Konfigurovat**.
3. V části **Externí adresu URL**zadejte svoji vlastní doménu.
4. Pokud externí adresa URL je https, se výzva k nahrání certifikát, abyste tento Azure můžete ověřit adresu URL aplikace. Můžete taky odeslat certifikát se zástupnými znaky, která odpovídá externí adresa URL aplikace. Tuto doménu musí být v seznamu svého [Azure ověření domény](https://msdn.microsoft.com/library/azure/jj151788.aspx). Azure musí být certifikát pro adresu URL domény aplikace nebo zástupné certifikát, který odpovídá externí adresu URL pro aplikaci.
5. Ujistěte se, chcete-li přidat záznam DNS, který směruje interní adresa URL aplikace, která umožňuje mít stejnou adresu URL pro interní a externí přístup a jeden zástupce aplikace v seznamu aplikací uživatele.

## <a name="frequently-asked-questions-about-working-with-custom-domains"></a>Nejčastější dotazy o práci s vlastní domény

Otázka: Můžu si vybrat certifikát už nahráli bez uložte jej znovu?  
A: dříve nahraný certifikáty jsou automaticky vázaný na aplikace a není právě jeden certifikát odpovídající název hostitele aplikace.  

Otázka: jak se dají přidat certifikát a jaký formát by měl exportovaného certifikátu nahrát v?  
Odpověď: certifikát by ho nahrát na stránce konfigurace aplikace. Certifikát by měl být souboru PFX.  

Otázka: ECC certifikáty slouží?  
Odpověď: je bez explicitní omezení podpis metody.  

Otázka: SAN certifikáty slouží?  
Odpověď: Ano.  

Otázka: zástupných certifikáty slouží?  
Odpověď: Ano.  

Otázka: jiný certifikát možno použít pro jednotlivé aplikace?  
Odpověď: Ano, pokud těmito dvěma aplikacemi sdílení stejného externí hostitele.  

Otázka: můžu zaregistrovat novou doménu, můžete používám tuto doménu?  
Odpověď: Ano, seznamu domén plněn z klienta ověřenou doménou seznamu.  

Otázka: co se stane, když skončí platnost certifikátu?  
Odpověď: Zobrazí se upozornění v části certifikátů na stránce konfigurace aplikace. Pokud se uživatel pokusí o přístup k aplikaci, bude objevovat upozornění zabezpečení.  

Otázka: Co mám dělat, když chcete nahradit certifikátu dané aplikace?  
A: nahrajte nový certifikát na stránce konfigurace aplikace.  

Otázka: můžete odstranit certifikát a nahradit je?  
A: když nahrajete nový certifikát, pokud starý certifikát nepoužívá jinou aplikací, to se automaticky odeberou.  

Otázka: co se stane, když certifikát byl odvolán?  
A: odvolání kontroly nejsou provést certifikáty. Pokud se uživatel pokusí o přístup k aplikaci závislosti na používaném prohlížeči se může zobrazit upozornění zabezpečení.  

Otázka: můžu používat certifikátu podepsaného svým držitelem?  
Odpověď: Ano, jsou povoleny podepsaného certifikáty. Nezapomeňte, že pokud používáte soukromé certifikační autorita, CDP (certifikát odvolání čárky distribuční místo) certifikátu by měl být veřejné.  

Otázka: existuje místo zobrazíte všechny certifikáty k našemu klientovi?  
A: to není podporováno v aktuální verzi.  


## <a name="see-also"></a>Viz taky

- [Publikování aplikací s Proxy aplikace](active-directory-application-proxy-publish.md)
- [Povolení jednotné přihlašování](active-directory-application-proxy-sso-using-kcd.md)
- [Povolení podmíněné přístupu](active-directory-application-proxy-conditional-access.md)
- [Přidání vlastní název domény Azure AD](active-directory-add-domain.md)

Nejnovější příspěvky a aktualizace najdete v článku [aplikace Proxy blogu](http://blogs.technet.com/b/applicationproxyblog/)
