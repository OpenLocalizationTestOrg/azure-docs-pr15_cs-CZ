<properties
    pageTitle="Jak koupit název vlastní domény v Azure aplikace služby Web Apps"
    description="Zjistěte, jak koupit název vlastní domény přes web app v aplikaci služby Azure."
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="buy-and-configure-a-custom-domain-name-in-azure-app-service"></a>Koupit a konfigurace vaší vlastní doménou v aplikaci služby Azure

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Když vytvoříte do webových aplikací, přiřadí mu Azure poddomény azurewebsites.net. Pokud váš web appu s názvem **contoso**, je adresa URL například **contoso.azurewebsites.net**. Azure taky přiřadí virtuální IP adresu.

Pro web app výrobní pravděpodobně má uživatelům zobrazit vlastní název domény. Tento článek vysvětluje, jak koupit a vlastní doménu nakonfigurovat [Aplikaci služby Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## <a name="overview"></a>Základní informace

Pokud nemáte názvu domény pro web app, můžete jej snadno zakoupit na [Portálu Azure](https://portal.azure.com/). Při nákupu můžete mít záznamy DNS WWW a kořenové domény namapovat do webové aplikace automaticky. Taky můžete spravovat domény vpravo do portálu Azure.


Pomocí následujících kroků zakoupení názvy domén a přiřazení do webové aplikace.

1. V prohlížeči otevřete [Portál Azure](https://portal.azure.com/).

2. Na kartě **Web Apps** klikněte na název svojí webové aplikace, vyberte **Nastavení**a pak vyberte **vlastní domény**

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. V zásuvné **vlastní domény** klikněte na **koupit domény**.

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. V zásuvné **Nákup domény** pomocí textového pole zadejte název domény, kterou chcete zakoupit a stiskněte klávesu Enter. Navrhované dostupných domén zobrazí jenom pod textovým polem. Vyberte doméně, které chcete koupit. Můžete koupit víc domén najednou. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. Klikněte na **Kontaktní informace** a vyplňte formulář kontaktní informace domény.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

    > [AZURE.NOTE] Je velmi důležité vyplňte všechna povinná pole s tolik přesnost co nejdřív, zejména e-mailovou adresu. V případě nákup domény bez "Ochranu soukromí", můžete být požádáni o ověření e-mailu před aktivuje domény. V některých případech nesprávných dat kontaktních informací způsobí selhání k zakoupení domény. 

6. Nyní můžete se rozhodnout,

    a) "Automatické obnovení" domény každý rok
    
    b) vyjádření výslovného pro sprá "Ochranu soukromí", která je součástí nákupní cena zdarma (s výjimkou domény nejvyšší úrovně kdo registru co nedělat podporují ochrany osobních údajů. For example:. co.in,. co.uk atd.)  
    
    c) "přiřadit výchozí názvy hostitelů" WWW a kořenové domény pro aktuální Web Appu. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
    > [AZURE.NOTE] Možnost C nakonfiguruje DNS vazby a Hostname vazby automaticky za vás.  Tímto způsobem webovou aplikaci dají se otevřít prostřednictvím vlastní doménu hned nákup dokončení (baring DNS šíření zpožděním při několik případy). V případě webovou aplikaci je za Azure přenosy správce, neuvidíte možnost přiřadit kořenovou doménu, jako záznamy o něm něco nefunguje s správce přenosy. Můžete vždy přiřadit domény a dílčí-domains zakoupené prostřednictvím jeden Web Appu na jiný Web App a naopak. Přejděte ke kroku 8 další podrobnosti. 
    
7. Klikněte **Vybrat** na zásuvné **Nákup domény** a pak se bude zobrazovat informace o nákupu na zásuvné **potvrzení nákupní** . Pokud třeba souhlasit s podmínkami právních a klepněte na položku **zakoupit**, bude Odeslat objednávku a můžete sledovat proces nákupu na **oznámení**. Nákup domény může trvat několik minut. 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. Pokud jste úspěšně objednali doménu, můžete spravovat domény a přiřadit do webové aplikace. Klikněte **trojtečkou (...)** v pravé části vaší domény. Pak můžete **Zrušit nákup** nebo **Spravovat domény**. Klikněte na **Spravovat domény**, potom **subdoménu** můžete svázat s naše v prohlížeči na zásuvné **Spravovat domény** . Pokud chcete vytvořit vazbu **subdoménu** k jiné webové aplikace tento krok z v rámci svých Web App proveďte. Přes Tady můžete zvolit, když potřebujete přiřadit domény koncový bod přenosy správce (je-li Web Appu je za TM) správcem přenosy jednoduše vyberou název z rozevírací nabídky. Tímto postupem doménu a subdoménu automaticky se automaticky přiřadí všechny webové aplikace za tohoto koncového bodu přenosy správce. 

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

    > [AZURE.NOTE] Můžete "zrušit nákup" v rámci 5 dnů plnou náhradu. Po 5 dní, po které se dozvíte, jak nemohli "Zrušit nákup", místo toho uvidíte možnost "Odstranit" doménu. Odstranění doménu bude mít za následek uvolnění ze svého předplatného bez náhrady a bude k dispozici domény. 

Po dokončení konfigurace vlastní název domény uvedené v části **vazby Hostname** svojí webové aplikace.

V tomto okamžiku by měl moct zadejte název vlastní domény v prohlížeči a se, že ho úspěšně přenese vás do webové aplikace.
 
## <a name="what-happens-to-the-custom-domain-you-bought"></a>Co se stane s vlastní doménu, kterou jste si koupili

Vlastní doménu, kterou jste si koupili v zásuvné **vlastní domény a SSL** je svázané se Azure předplatného. Jako zdroj Azure tuto vlastní doménu je samostatné a nezávisle na z aplikace aplikaci služby, nejdřív si koupili domény. To znamená, že:

- V portálu Azure můžete použít vlastní doménu, kterou jste si koupili aplikace víc aplikaci služby a nejen pro aplikace, nejdřív si koupili vlastní domény pro. 
- Můžete spravovat všechny vlastní domény, kterou jste si koupili v Azure předplatného tak, že přejdete na **vlastní domény a SSL** zásuvné *Any* aplikaci služby aplikace v tomto předplatného.
- Můžete přiřadit nějakou aplikaci služby aplikaci ze stejného předplatného Azure subdoménu ve vlastní doméně.
- Pokud se rozhodnete odstranit aplikaci pro aplikaci služby, můžete neodstraňujte vlastní domény, který je svázán s, pokud chcete dál používat pro jiné aplikace.

## <a name="if-you-cant-see-the-custom-domain-you-bought"></a>Pokud se nezobrazuje vlastní doménu jste si koupili

Pokud jste si koupili vlastní doméně z **vlastní domény a SSL** zásuvné, ale není vidět vlastní doménu v části **správce domény**, zkontrolujte následující věci:

- Vytvoření vlastní doménu zatím nebylo dokončeno. Zaškrtněte políčko zvonu oznámení v horní části portálu Azure pro průběh.
- Vytvoření vlastní domény může mít nepovedlo z nějakého důvodu. Zaškrtněte políčko zvonu oznámení v horní části Azure portálu pro průběh.
- Může mít vlastní domény úspěšné, ale zásuvné nemusí aktualizovat ještě. Znovu zásuvné **vlastní domény a SSL** .
- Možná jste odstranili vlastní doménu nastane okamžik. V protokolech auditování po kliknutí na **Nastavení** > **Protokolů auditování** z hlavního zásuvné vaše aplikace. 
- **Vlastní domény a SSL** zásuvné, kterou hledáte, můžete v může patří do aplikace, která se vytvoří v jiné Azure předplatné. Přepněte do jiné aplikace v jiné předplatné a zkontrolujte své **vlastní domény a SSL** zásuvné.  
  V rámci portálu nebude moct zobrazit nebo spravovat vlastní domény, které jsou vytvořené v jiné Azure předplatné než aplikace. Ale pokud kliknete na tlačítko **Upřesnit správy** v domény **Spravovat domény** zásuvné budete přesměrovaní na web poskytovatele domény, které byste měli   [Ruční](web-sites-custom-domain-name.md) konfigurace vaší vlastní domény, třeba externí vlastní domény 
   aplikací vytvořené v jiné Azure předplatné. 


