<properties
    pageTitle="Mapa vlastního názvu domény k aplikaci pro Azure"
    description="Zjistěte, jak přiřadit vaši aplikaci distribuovali aplikaci služby Azure vlastní název domény (marnivost domény)."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"
    tags="top-support-issue"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/27/2016"
    ms.author="cephalin"/>

# <a name="map-a-custom-domain-name-to-an-azure-app"></a>Mapa vlastního názvu domény k aplikaci pro Azure

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Tento článek popisuje, jak ručně mapovat vlastní název domény webových aplikací, mobilní aplikace back-end nebo rozhraní API aplikace v [Aplikaci služby Azure](../app-service/app-service-value-prop-what-is.md). 

Aplikace již obsahuje jedinečné subdoménu azurewebsites.net. Pokud je v poli Název aplikace **contoso**, pak svůj název domény je například **contoso.azurewebsites.net**. Však můžete mapovat vlastní doménu název, aby se aplikace tak, jeho adresu URL, třeba `www.contoso.com`, odráží vaši značku.

>[AZURE.NOTE] Pokud potřebujete pomoc od Azure odborníků na [Azure fórech](https://azure.microsoft.com/support/forums/). Ještě vyšší úroveň podpory přejděte na [Web podpory Azure](https://azure.microsoft.com/support/options/) a klepněte na **Získání podpory**.

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

## <a name="buy-a-new-custom-domain-in-azure-portal"></a>Koupit novou vlastní doménu Azure portálu

Pokud jste to ještě už jste si koupili vlastní název domény, můžete si nějakou kupte a spravovat přímo do vaší aplikaci nastavení [Azure portálu](https://portal.azure.com). Tato možnost umožňuje snadno mapovat vlastní doménu do aplikace, zda aplikace používá [Správce přenosy Azure](web-sites-traffic-manager-custom-domain-name.md) nebo ne. 

Pokyny najdete v tématu [zakoupení vlastního názvu domény pro aplikaci služby](custom-dns-web-site-buydomains-web-app.md).

## <a name="map-a-custom-domain-you-purchased-externally"></a>Mapovat vlastní doménu, kterou jste si koupili externě

Pokud jste si koupili vlastní doménu z [Azure DNS](https://azure.microsoft.com/services/dns/) nebo z jiného poskytovatele, existují tři hlavní kroky mapovat vlastní doménu do aplikace:

1. [IP adresa *(záznam jenom)* získat aplikaci](#vip).
2. [Vytvoření záznamů DNS, které mapovat vlastní domény do aplikace](#createdns). 
    - **Kde**: svého registrátora vlastního nástroje pro správu domén (například Azure DNS GoDaddy, atd.).
    - **Proč**:, aby věděli svého doménového registrátora výsledkem požadovaný vlastní domény, kterou chcete Azure aplikace.
1. [Povolit vlastní název domény Azure aplikace](#enable).
    - **Kde**: [Azure portálu](https://portal.azure.com).
    - **Proč**: tak, aby aplikace zná odpověď na žádosti vlastní název domény.
3. [Šíření ověření DNS](#verify).

### <a name="types-of-domains-you-can-map"></a>Typy domény, kterou můžete namapovat

Služba Azure aplikací umožňuje mapování následující kategorií vlastní domény do aplikace.

- **Kořenovou doménu** – název domény, který je rezervován u doménového registrátora (představované `@` hostovat záznamy, obvykle). Například **contoso.com**.
- **Subdomain (subdoména)** -domény, který je v části kořenovou doménu. Například, **www.contoso.com** (představované `www` hostovat záznam).  Různé subdomén stejné kořenovou doménu můžete namapovat různých aplikací v Azure.
- **Zástupný znak domény** - [každá subdoména jehož vlevo popisek DNS je `*` ](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (například hostovat záznamy `*` a `*.blogs`). Například ** \*. contoso.com**.

### <a name="types-of-dns-records-you-can-use"></a>Typy záznamů DNS, které můžete použít

Podle potřeby můžete použít dva různé typy záznamů DNS na standardní mapovat vlastní domény: 

- [A](https://en.wikipedia.org/wiki/List_of_DNS_record_types#A) - mapy váš vlastní název domény k aplikaci Azure virtuální IP adresy přímo. 
- [CNAME](https://en.wikipedia.org/wiki/CNAME_record) - namapuje váš vlastní název domény na název Azure domain vaše aplikace * *&lt;*název_aplikace*>. azurewebsites.net**. 

Výhodou CNAME je, že ukládá přes IP adresu změny. Pokud odstraníte a znovu vytvořit aplikaci nebo přejděte na vyšší úroveň ceny zpět osy **sdílené** se vaše aplikace virtuální IP adresu lišit. Pomocí těchto změn je pořád platná záznam CNAME, zatímco záznam vyžaduje aktualizaci. 

Kurzu se dozvíte, jak postup pro používání záznam taky pomocí záznamu CNAME.

>[AZURE.IMPORTANT] Nevytvářejte záznam CNAME pro kořenovou doménu (tedy "záznam kořenové"). Další informace najdete v tématu [Proč se nedají záznam CNAME použít na kořenovou doménu](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain).
Namapujte kořenovou doménu na Azure aplikace, použijte záznam.

<a name="vip"></a>
## <a name="step-1-a-record-only-get-apps-ip-address"></a>Krok 1. *(Záznam pouze)* Získání aplikace IP adresa
Pokud chcete mapovat vlastní název domény pomocí záznamu, musíte Azure aplikace IP adresu. Pokud budete mapovat místo toho použít záznam CNAME, tento krok přeskočit a přesuňte do další části.

1.  Přihlaste se k [portálu Azure](https://portal.azure.com).

2.  Klikněte v nabídce nalevo na **Služby aplikace** .

4.  Klikněte na aplikace a potom klikněte na **vlastní domény**.

6.  Poznamenejte si IP adresu nad názvy hostitelů oddíl.

    ![Mapa vlastní název domény u záznamu: získání IP adresa aplikace pro aplikaci služby Azure](./media/web-sites-custom-domain-name/virtual-ip-address.png)

7.  Nechte toto portálu zásuvné otevřené. Můžete se vrátit k němu po vytvoření záznamů DNS.

<a name="createdns"></a>
## <a name="step-2-create-the-dns-records"></a>Krok 2. Vytvoření záznamů DNS

Přihlaste se na svého doménového registrátora a jejich nástrojem pro přidání A záznam a nebo záznam CNAME. Uživatelské rozhraní každé registrátora je mírně odlišnou, takže si pročtěte si přečtěte následující dokumentaci svého poskytovatele. Tady jsou ale některé obecné pokyny.

1.  Přejděte na stránku pro správu DNS záznamů. Vyhledejte odkazy nebo oblasti webu s názvem **Název domény**, **DNS**nebo **Správa název serveru**. Často můžete najít odkaz zobrazení informací o účtu a potom hledáte odkaz například **Mé domény**.
2.  Hledejte odkaz, který umožňuje přidat nebo upravit záznamy DNS. Může to být **soubor zóny** nebo **Záznamů DNS** odkaz nebo odkazu na **Upřesnit** konfiguraci.
3.  Vytvořte záznam a uložte provedené změny.
    - [Pokyny pro záznam a najdete tady](#a).
    - [Pokyny pro záznam CNAME najdete tady](#cname).

<a name="a"></a>
### <a name="create-an-a-record"></a>Vytvořte záznam

Pomocí záznamu a namapovat na IP adresu Azure aplikaci, musíte skutečně vytvořit záznam a záznam TXT. Záznam je pro služeb překládá názvy DNS sebe sama a je záznam TXT pro Azure k ověření, že vlastníte doménu vlastní název domény. 

Konfigurace záznamů A takto (@ obvykle představuje kořenovou doménu):
 
<table cellspacing="0" border="1">
  <tr>
    <th>Příklad plně kvalifikovaný název domény</th>
    <th>Hostitel</th>
    <th>Hodnota</th>
  </tr>
  <tr>
    <td>contoso.com (kořenové)</td>
    <td>@</td>
    <td>IP adresy z <a href="#vip">kroku 1</a></td>
  </tr>
  <tr>
    <td>www.contoso.com (sub)</td>
    <td>www</td>
    <td>IP adresy z <a href="#vip">kroku 1</a></td>
  </tr>
  <tr>
    <td>*. contoso.com (zástupný znak)</td>
    <td>*</td>
    <td>IP adresy z <a href="#vip">kroku 1</a></td>
  </tr>
</table>

Další záznam TXT převezme konvence, která map z &lt; *subdoménu*>. &lt; *rootdomain*> k &lt; *název_aplikace*>. azurewebsites.net. Konfigurace záznamu TXT následujícím způsobem:

<table cellspacing="0" border="1">
  <tr>
    <th>Příklad plně kvalifikovaný název domény</th>
    <th>Host (hostitel) TXT</th>
    <th>Hodnotu TXT</th>
  </tr>
  <tr>
    <td>contoso.com (kořenové)</td>
    <td>@</td>
    <td>&lt;<i>název_aplikace</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>www.contoso.com (sub)</td>
    <td>www</td>
    <td>&lt;<i>název_aplikace</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>*. contoso.com (zástupný znak)</td>
    <td>*</td>
    <td>&lt;<i>název_aplikace</i>>. azurewebsites.net</td>
  </tr>
</table>

<a name="cname"></a>
###Vytvořit záznam CNAME

Pokud používáte záznam CNAME přiřadit Azure aplikace výchozí název domény, nemusíte dalšího záznamu TXT stejně jako u záznamu. 

>[AZURE.IMPORTANT] Nevytvářejte záznam CNAME pro kořenovou doménu (tedy "záznam kořenové"). Další informace najdete v tématu [Proč se nedají záznam CNAME použít na kořenovou doménu](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain).
Namapujte kořenovou doménu na Azure aplikace, použijte [záznam](#a) .

Konfigurace CNAME záznam takto (@ obvykle představuje kořenovou doménu):

<table cellspacing="0" border="1">
  <tr>
    <th>Příklad plně kvalifikovaný název domény</th>
    <th>Host (hostitel) CNAME</th>
    <th>Hodnota CNAME</th>
  </tr>
  <tr>
    <td>www.contoso.com (sub)</td>
    <td>www</td>
    <td>&lt;<i>název_aplikace</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>*. contoso.com (zástupný znak)</td>
    <td>*</td>
    <td>&lt;<i>název_aplikace</i>>. azurewebsites.net</td>
  </tr>
</table>

<a name="enable"></a>
##Krok 3. Povolení název vlastní domény pro aplikace

Po návratu do zásuvné **Vlastních domén** v portálu Azure (viz [Krok 1](#vip)), budete muset přidat plně kvalifikovaný název domény (FQDN) vaší vlastní domény do seznamu.

1.  Pokud jste nevytvořili, přihlaste se k [portálu Azure](https://portal.azure.com).

2.  Na portálu Azure klikněte v nabídce nalevo na **Služby aplikace** .

3.  Klikněte na aplikace a potom klikněte na **vlastní domény** > **Přidat název hostitele**.

4.  Úplný název vaší vlastní domény přidáte do seznamu (například **www.contoso.com**).

    ![Mapování vlastního názvu domény na Azure aplikace: přidání do seznamu s názvy domén](./media/web-sites-custom-domain-name/add-custom-domain.png)

    >[AZURE.NOTE] Azure se pokusí ověřit název domény, který používáte tady. Ujistěte se, že je stejný název domény, pro kterou jste vytvořili záznam DNS v [kroku 2](#createdns). 

5.  Klikněte na **Ověřit**.

6.  Po kliknutí na tlačítko **Ověřit** Azure bude zahájit pracovní postup ověření domény. To kontroloval vlastnictví domény i podrobné chyby s obvyklé guidence o tom, jak opravit chybu nebo název hostitele úspěšné dostupnost a sestavy.    

7.  Po úspěšném ověření **hostname přidat** tlačítko je aktivní a budou moct hostname přiřadit. 

8.  Po dokončení konfigurace nové vlastní název domény Azure přejděte na název vaší vlastní domény v prohlížeči. V prohlížeči dokončeno otevřete aplikaci Azure, což znamená, že váš vlastní název domény správně nakonfigurované.

> [AZURE.NOTE] Pokud záznam DNS se už používá (aktivní domény sloužící přenosy scénář) a potřebujete preventivně svázat webovou aplikaci k němu pro ověření domény, jednoduše vytvořte záznamy TXT jako příklady uvedené v následující tabulce. Další záznam TXT převezme konvence, která map z &lt; *subdoménu*>. &lt; *rootdomain*> k &lt; *název_aplikace*>. azurewebsites.net. 
> <table cellspacing="0" border="1">
  <tr>
    <th>Příklad plně kvalifikovaný název domény</th>
    <th>Host (hostitel) TXT</th>
    <th>Hodnotu TXT</th>
  </tr>
  <tr>
    <td>contoso.com (kořenové)</td>
    <td>awverify.contoso.com</td>
    <td>&lt;<i>název_aplikace</i>>. azurewebsites.net</td>
  </tr>
  <tr>
    <td>www.contoso.com (sub)</td>
    <td>awverify.www.contoso.com</td>
    <td>&lt;<i>název_aplikace</i>>. azurewebsites.net</td>
  </tr>
    <tr>
    <td>*. contoso.com (sub)</td>
    <td>awverify.*.contoso.com</td>
    <td>&lt;<i>název_aplikace</i>>. azurewebsites.net</td>
  </tr>
</table>
Po vytvoření tohoto DNS záznamu přejděte zpátky na Azure portál a přidejte název vlastní domény do webové aplikace.
 

<a name="verify"></a>
##Ověření šíření DNS

Po dokončení kroků konfigurace, může trvat delší dobu změny rozšířit, v závislosti na svého poskytovatele DNS. Můžete ověřit, že šíření DNS funguje očekávaným pomocí [http://digwebinterface.com/](http://digwebinterface.com/). Když přejdete na web, zadejte názvy hostitelů do textového pole a klikněte na **dostanete**. Zkontrolujte výsledky k potvrzení, pokud poslední změny projevily.  

![Vlastní název domény namapovat aplikace Azure: ověření šíření DNS](./media/web-sites-custom-domain-name/1-digwebinterface.png)

> [AZURE.NOTE] Šíření položky DNS může trvat až 48 hodin (někdy už). Pokud jste nakonfigurovali všechno správně, potřebujete až šíření proběhla úspěšně.

## <a name="next-steps"></a>Další kroky
Zjistěte, jak zajistit váš vlastní název domény s HTTPS [zakoupení certifikátu SSL v Azure](web-sites-purchase-ssl-web-site.md) nebo [pomocí certifikátu SSL od jinde](web-sites-configure-ssl-certificate.md).

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

[Začínáme se službou Azure DNS](../dns/dns-getstarted-create-dnszone.md)  
[Vytvoření záznamů DNS pro web app ve vlastní doméně](../dns/dns-web-sites-custom-domain.md)  
[Delegát Domain Azure DNS](../dns/dns-domain-delegation.md)


<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png
