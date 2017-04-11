<properties
    pageTitle="Nakonfigurujte aplikaci služby Azure, která používá přenosy správce pro vyrovnávání zatížení vlastního názvu domény pro web app."
    description="Použití vlastního názvu domény pro do webových aplikací služby Azure aplikace, která zahrnuje přenosy správce pro vyrovnávání zatížení."
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
    ms.date="09/20/2016"
    ms.author="robmcm"/>

# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Konfigurace vlastního názvu domény pro web app v aplikaci služby Azure pomocí přenosy správce

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Tento článek obsahuje obecné pokyny pro použití vlastního názvu domény se službou Azure aplikace, které používají správce dopravy pro vyrovnávání zatížení.

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## <a name="understanding-dns-records"></a>Principy záznamy DNS

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## <a name="configure-your-web-apps-for-standard-mode"></a>Konfigurace webové aplikace pro standardním režimu

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## <a name="add-a-dns-record-for-your-custom-domain"></a>Přidejte záznam DNS pro vaši vlastní doménu

> [AZURE.NOTE] Pokud jste zakoupili doméně přes Azure aplikace služby Web Apps přeskočit kroků a v nápovědě k poslední krok článek [Kupte domény pro Web Apps](custom-dns-web-site-buydomains-web-app.md) .

Pokud chcete přidružit web app v aplikaci služby Azure vaši vlastní doménu, musí přidáte novou položku v tabulce DNS pro vaši vlastní doménu pomocí nástroje poskytnuté, který jste si koupili název domény od doménového registrátora. Pomocí následujících kroků vyhledejte a pomocí nástrojů DNS.

1. Přihlaste se ke svému účtu u svého doménového registrátora a vyhledejte na stránce Spravovat záznamy DNS. Vyhledejte odkazy nebo oblasti webu textem je označená jako **Název domény**, **DNS**a **Správa název serveru**. Často odkaz na této stránce najdete zobrazení informací o účtu a potom hledáte odkaz například **Mé domény**.

1. Když jste našli stránku Správa za svůj název domény, hledejte odkaz, který umožňuje provádět úpravy DNS záznamů. Může být vedená jako **soubor zóny** **DNS záznamy**, nebo jako odkaz **Upřesnit** konfiguraci.

    * Na stránce bude pravděpodobně několik záznamů už vytvořili, jako je položka přidružení "**@**"nebo"\*" se stránkou domény parkování. Mohou také obsahovat záznamy pro běžné subdomény například **www**.
    * Na stránce zmínit **CNAME záznamy**, nebo zadejte rozevíracího seznamu vyberte typ záznamu. Lze také uvést další záznamy jako **záznam** a **záznamy MX**. V některých případech bude názvů jiných například **Alias Record**volat záznamy CNAME.
    * Na stránce bude mít taky zajištěný pole, která vám umožní **mapu** **název hostitele** nebo **název domény** jiný název domény.

1. Během podrobnosti každé registrátora liší, obecně mapujete *z* váš vlastní název domény (třeba **contoso.com**) *pro* přenos správce názvu domény (**contoso.trafficmanager.net**), který se používá pro web app.

    > [AZURE.NOTE] Pokud potřebujete preventivně svázat aplikací k němu záznamu se už používá, můžete taky můžete vytvořit další záznam CNAME. Například **www.contoso.com** preventivně svázat webovou aplikaci, vytvořte záznam CNAME z **awverify.www** k **contoso.trafficmanager.net**. Můžete pak přidáte "www.contoso.com" do webové aplikace beze změny "www" CNAME záznam. Další informace najdete v tématu [Vytvoření záznamů DNS pro web app na vlastní doméně][CREATEDNS].

1. Jakmile dokončíte přidávání nebo úpravě záznamů DNS u svého registrátora, uložte změny.

<a name="enabledomain"></a>
## <a name="enable-traffic-manager"></a>Povolit přenosy správce

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Středisko pro vývojáře Node.js](/develop/nodejs/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
