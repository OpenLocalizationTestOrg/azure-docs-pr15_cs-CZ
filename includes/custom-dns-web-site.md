#<a name="configuring-a-custom-domain-name-for-an-azure-website"></a>Konfigurace vlastního názvu domény pro web na Azure

Při vytváření webu Azure obsahuje popisné subdoménu domény azurewebsites.net aby uživatelé měli přístup k webu pomocí adresu URL, třeba http://&lt;webů >. azurewebsites.net. Ale pokud konfigurace vaše weby pro sdílený nebo standardním režimu můžete namapovat váš web vlastní název domény.

Volitelně můžete Azure přenosy správce načíst Zůstatek příchozí přenosy na váš web. Další informace o fungování přenosy správce s weby, najdete v článku [Řízení Azure weby přenosy v síti s Azure přenosy správce][trafficmanager].

> [AZURE.NOTE] Postupy uvedené v tomto úkolu platí pro weby Azure; cloudové služby najdete v článku <a href="/develop/net/common-tasks/custom-dns/">konfigurace vaší vlastní doménou v Azure</a>.

> [AZURE.NOTE] Kroky v tomto úkolu vyžadují, abyste konfigurace vaše weby pro sdílený nebo standardním režimu, což může změnit kolik je faktura pro vaše předplatné. Další informace najdete v tématu <a href="/pricing/details/web-sites/">Weby ceny podrobnosti</a> .

V tomto článku:

-   [Principy záznamy CNAME a a.](#understanding-records)
-   [Konfigurace webů pro sdílené nebo standardním režimu](#bkmk_configsharedmode)
-   [Přidání webů správci přenosy](#trafficmanager)
-   [Přidejte záznam CNAME pro vlastní doménu](#bkmk_configurecname)
-   [Přidání záznamu a pro vaši vlastní doménu](#bkmk_configurearecord)

<h2><a name="understanding-records"></a>Princip záznamy CNAME a a.</h2>

CNAME (alias záznamy či) a záznamy o obou umožňují přiřazení názvu domény k webu, ale každý fungují jinak.

###<a name="cname-or-alias-record"></a>Záznam CNAME nebo Alias

Záznam CNAME namapuje *určité* domény, například **contoso.com** nebo **www.contoso.com**, na název kanonických domény. V tomto případě je název kanonický domény buď ** &lt;aplikace >. azurewebsites.net** název domény z webu Azure nebo ** &lt;aplikace >. trafficmgr.com** název domény pro svůj profil správce dopravy. Po vytvoření záznamy CNAME pro vytváří alias ** &lt;aplikace >. azurewebsites.net** nebo ** &lt;aplikace >. trafficmgr.com** název domény. Položka CNAME vyřeší na IP adresu vašeho ** &lt;aplikace >. azurewebsites.net** nebo ** &lt;aplikace >. trafficmgr.com** název domény automaticky, takže pokud změnou IP adresu webu, není třeba provádět žádné akce.

> [AZURE.NOTE] Některé doménových registrátorů umožňují mapování subdomény při použití záznam CNAME, například www.contoso.com a ne kořenové názvů, například contoso.com. Další informace o CNAME záznamy najdete v dokumentaci podle svého registrátora, <a href="http://en.wikipedia.org/wiki/CNAME_record">položce Wikipedie na záznam CNAME</a>nebo <a href="http://tools.ietf.org/html/rfc1035">Názvy domén IETF - implementaci a specifikace</a> dokument.

###<a name="a-record"></a>Záznam

Záznam a mapy doménu, například **contoso.com** nebo **www.contoso.com**, *nebo pomocí zástupných znaků domény* , jako ** \*. contoso.com**, k IP adrese. V případě web na Azure nastavení virtuální IP adresy služby nebo konkrétní IP adresy, které jste zakoupili pro váš web Tak, aby hlavní výhodou přes záznam CNAME záznam, jestli máte jedna položka používající zástupné znaky, například * **. contoso.com**, které by zpracovávat požadavky pro více dílčích domén s jako * *mail.contoso.com**; * *login.contoso.com**, nebo * *www.contso.com**.

> [AZURE.NOTE] Protože záznam namapovala statickou IP adresu, ho nerozpoznává automaticky změny na IP adresu vašeho webu. IP adresu pro použití s záznam není uvedený při konfiguraci nastavení názvu vlastní domény pro váš web; Tato hodnota však může změnit Pokud odstraníte a znovu vytvořit web nebo změna režimu webu do pozadí a uvolnit.

> [AZURE.NOTE] Záznamy a není možné použít pro s správce přenosy Vyrovnávání zatížení. Další informace najdete v tématu [Řízení Azure weby přenosy v síti s Azure přenosy správce][trafficmanager].

<a name="bkmk_configsharedmode"></a><h2>Konfigurace webů pro sdílené nebo standardním režimu</h2>

Nastavení vlastního názvu domény na web je dostupný jenom u sdílené a standardní režimy Azure weby. Před přepnutím na webu z bezplatné webu režim sdílené nebo režim standardní webu, je třeba nejprve odebrat výdajů caps na místě pro vaše předplatné webu. Další informace o Shared a standardní ceny režimu, najdete v článku [Ceny podrobnosti][PricingDetails].

1. V prohlížeči otevřete na [Portálu Správa][portal].
2. Na kartě **weby** klikněte na název vašeho webu.

    ![][standardmode1]

3. Klikněte na kartu **Měřítko** .

    ![][standardmode2]


4. V části **Obecné** nastavení režimu webu po kliknutí na **SDÍLENÉ**.

    ![][standardmode3]

    > [AZURE.NOTE] Pokud použijete přenosy správce s Tento web, je nutné použít vyberte standardním režimu místo sdílené.

5. Klikněte na **Uložit**.
6. Po zobrazení výzvy o zvýšení náklady sdílený režim (nebo standardním režimu, pokud se rozhodnete standardní), klepněte na tlačítko **Ano** Pokud souhlasíte.

    <!--![][standardmode4]-->

    **Poznámka:**<br />
   Pokud se zobrazí chyba "Konfigurace měřítko pro web webu název, který se nezdařil", můžete získat další informace na tlačítko Podrobnosti.

<a name="trafficmanager"></a><h2>(Volitelné) Přidání vaše weby správci přenosy</h2>

Pokud chcete použít svůj web pomocí Správce přenosy, proveďte následující kroky.

1. Pokud už nemáte profil správce přenosy, použijte informace v tématu [Vytvoření profil správce přenosy pomocí vytvořit] [ createprofile] a vytvořte si ho. Poznámka **. trafficmgr.com** název domény přidružený profilu přenosy správce. Používá se později.

2. Pomocí informací v [Přidat nebo odstranit koncové body] [ addendpoint] přidat váš web jako koncový bod ve vašem profilu přenosy správce.

    > [AZURE.NOTE] Pokud váš web nenajdete při přidávání koncový bod, ověřte, zda je nakonfigurován pro standardním režimu. Chcete-li pracovat s správce přenosy je nutné použít standardním režimu pro váš web.

3. Přihlaste se k webu registrátora vaší DNS a přejděte na stránku pro správu DNS. Vyhledejte odkazy nebo oblasti webu textem je označená jako **Název domény**, **DNS**a **Správa název serveru**.

4. Teď najdete místo, kam můžete vybrat nebo zadat záznamy CNAME. Možná budete muset vyberte typ záznamu z kapky dolů nebo přejít na stránku upřesňující nastavení. By měla vypadat slova **CNAME** **Alias**nebo **subdomény**.

5. Také je nutné zadat doménu a subdoménu alias pro záznamy CNAME. Například **www** Pokud chcete vytvořit alias pro **www.customdomain.com**.

5. Také je nutné zadat název hostitele, který je název kanonický domény pro tento CNAME alias. Toto je **. trafficmgr.com** název vašeho webu.

Například následující záznamy SRV předá všechny přenosy z **www.contoso.com** **contoso.trafficmgr.com**, název domény webu:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias nebo Host (hostitel) jméno/subdomain (subdoména)</strong></td>
<td><strong>Kanonický domény</strong></td>
</tr>
<tr>
<td>www</td>
<td>contoso.trafficmgr.com</td>
</tr>
</table>

Návštěvník **www.contoso.com** nikdy uvidí hostiteli true (contoso.azurewebsite.net), tedy neviditelné koncovému uživateli procesu přesměrování.

> [AZURE.NOTE] Pokud používáte přenosy správce webu, nepotřebujete postupujte podle pokynů v následující části, "**Přidat záznam CNAME pro vaší vlastní domény**" a "**Přidat záznam pro vaší vlastní domény**". Záznam CNAME vytvořili v předchozích krocích se příchozí provoz směrovat přenosy správci, který přesměrovává přenosy na endpoint(s) webu.

<a name="bkmk_configurecname"></a><h2>Přidejte záznam CNAME pro vlastní doménu</h2>

K vytvoření záznamu CNAME, musí přidáte novou položku v tabulce DNS pro vaši vlastní doménu pomocí nástroje poskytnuté svého registrátora. Každý registrátora má podobné ale mírně odlišnou způsob, jak určit záznam CNAME, ale pojmy jsou stejné.

1. Použijte jeden z těchto postupů najdete **. azurewebsite.net** přiřazené k vašemu webu název domény.

    * Přihlášení k [Portálu Správa Azure][portal]vyberte svůj web, vyberte **řídicího panelu**a potom vyhledejte položku **Adresu URL webu** v části **můžete rychle podívat** .

    * Instalace a konfigurace [Prostředí Powershell Azure](/manage/install-and-configure-windows-powershell/)a pak použijte tento příkaz:

            get-azurewebsite yoursitename | select hostnames

    * Instalace a konfigurace [rozhraní Azure příkazového řádku](/manage/install-and-configure-cli/)a potom použijte tento příkaz:

            azure site domain list yoursitename

    Uložit **. azurewebsite.net** pojmenovat, jak se bude používat v následujících krocích.

3. Přihlaste se k webu registrátora vaší DNS a přejděte na stránku pro správu DNS. Vyhledejte odkazy nebo oblasti webu textem je označená jako **Název domény**, **DNS**a **Správa název serveru**.

4. Teď najdete místo, kam můžete vybrat nebo zadat záznamy CNAME. Možná budete muset vyberte typ záznamu z kapky dolů nebo přejít na stránku upřesňující nastavení. By měla vypadat slova **CNAME** **Alias**nebo **subdomény**.

5. Také je nutné zadat doménu a subdoménu alias pro záznamy CNAME. Například **www** Pokud chcete vytvořit alias pro **www.customdomain.com**. Pokud chcete vytvořit alias pro kořenovou doménu, může být vedená jako "**@**" symbol nebo u svého registrátora DNS nástroje.

5. Také je nutné zadat název hostitele, který je název kanonický domény pro tento CNAME alias. Toto je **. azurewebsite.net** název vašeho webu.

Například následující záznamy SRV předá všechny přenosy z **www.contoso.com** **contoso.azurewebsite.net**, název domény webu:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Alias nebo Host (hostitel) jméno/subdomain (subdoména)</strong></td>
<td><strong>Kanonický domény</strong></td>
</tr>
<tr>
<td>www</td>
<td>contoso.azurewebsite.NET</td>
</tr>
</table>

Návštěvník **www.contoso.com** nikdy uvidí hostiteli true (contoso.azurewebsite.net), tedy neviditelné koncovému uživateli procesu přesměrování.

> [AZURE.NOTE] V příkladu výše platí jenom pro přenosů na subdoména __www__ . Protože se záznamy CNAME nelze použít zástupné znaky, musíte vytvořit jednu CNAME pro každou doménu a subdoménu. Chcete-li směrování adres subdomény, jako například *. contoso.com, azurewebsite.net adrese, můžete nakonfigurovat __Přesměrování adresy URL__ nebo __Přesměrování adresy URL__ položky v nastavení DNS, nebo vytvořte záznam.

> [AZURE.NOTE] Může trvat delší dobu CNAME se rozšíří celém systému DNS. Rozšíření CNAME, dokud se nedá nastavit záznamy CNAME webu. Služby, třeba <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> můžete ověřit, že záznamy CNAME je k dispozici.

###<a name="add-the-domain-name-to-your-website"></a>Přidání názvu domény na web

Po rozšíření záznam CNAME pro název domény, musíte ho spojit s webem. Můžete přidat vlastní název domény definované záznam CNAME pro svůj web pomocí některého z Azure rozhraní příkazového řádku (Azure rozhraní příkazového řádku) nebo na portálu Správa Azure.

**Chcete-li přidat název domény pomocí nástroje příkazového řádku**

Instalace a konfigurace [rozhraní Azure příkazového řádku](/manage/install-and-configure-cli/)a pak použijte tento příkaz:

    azure site domain add customdomain yoursitename

Například následující přidáte vlastní název domény od **www.contoso.com** **contoso.azurewebsite.net** web:

    azure site domain add www.contoso.com contoso

Můžete zkontrolovat, že vlastní název domény přidal na web pomocí následujícího příkazu:

    azure site domain list yoursitename

Seznam vrácených tento příkaz by měl obsahovat vlastní název domény, stejně jako výchozí **. azurewebsite.net** položky.

**Chcete-li přidat název domény na portálu Správa Azure**

1. V prohlížeči otevřete na [Portálu Správa Azure][portal].

2. Na kartě **weby** klikněte na název vašeho webu, vyberte **řídicího panelu**a pak vyberte **Spravovat domény** v dolní části stránky.

    ![][setcname2]

6. Do textového pole **Názvy DOMÉN** zadejte název domény, kterou jste nakonfigurovali.

    ![][setcname3]

6. Klikněte na značku zaškrtnutí potvrďte název domény.

Po dokončení konfigurace vlastní název domény uvedené v části **názvy domén** na stránce **Konfigurovat** webu.

<a name="bkmk_configurearecord"></a><h2>Přidání záznamu a pro vaši vlastní doménu</h2>

Pokud chcete vytvořit záznam a, musíte nejdřív najít IP adresu vašeho webu. Pomocí nástroje poskytnuté svého registrátora zadejte údaj v tabulce DNS pro vaši vlastní doménu. Každý registrátora má podobné ale mírně odlišnou způsob, jak určit záznam, ale pojmy jsou stejné. Kromě vytváření záznam, musíte taky vytvořit záznam CNAME, který Azure používá k ověření záznamu A.

1. V prohlížeči otevřete na [Portálu Správa Azure][portal].

2. Na kartě **weby** klikněte na název vašeho webu, vyberte **řídicího panelu**a pak vyberte **Spravovat domény** v dolní části obrazovky.

    ![][setcname2]

5. V dialogovém okně **Spravovat vlastní domény** najděte **IP adresa používat při konfiguraci záznam**. Zkopírujte IP adresu. Se používá při vytváření záznam.

5. V dialogovém okně **Spravovat vlastní domény** Poznámka: název domény awverify na konci textu v horní části dialogového okna. Je třeba **awverify.mysite.azurewebsites.net** kde **osobního webu** je název vašeho webu. Zkopírujte, jako je název domény použít při vytváření ověření záznam CNAME.

6. Přihlaste se k webu registrátora vaší DNS a přejděte na stránku pro správu DNS. Vyhledejte odkazy nebo oblasti webu textem je označená jako **Název domény**, **DNS**a **Správa název serveru**.

6. Najdete, kde můžete vybrat nebo zadat A a CNAME záznamy. Možná budete muset vyberte typ záznamu z kapky dolů nebo přejít na stránku upřesňující nastavení.

7. Provedení následujících kroků můžete vytvořit záznam:

    1. Vyberte nebo zadejte doménu nebo subdoména, kterou budou používat záznam. Pokud chcete vytvořit alias pro **www.customdomain.com**například vyberte **www** . Pokud chcete vytvořit položku zástupných znaků pro všechny subdomény, zadejte "__*__". Tím se zabývat těmito oblastmi všech dílčích domén s například **mail.customdomain.com**, **login.customdomain.com**a **www.customdomain.com**.

        Pokud chcete vytvořit záznamu a pro kořenovou doménu, může být vedená jako "**@**" symbol nebo u svého registrátora DNS nástroje.

    2. Zadejte IP adresu cloudové služby v zadané oblasti. To spojí položce domény použít v záznam s IP adresu nasazení služby cloudu.

        Například následující, které záznamu předá všechny přenosy z **contoso.com** **137.135.70.239**, IP adresu naše nasazení aplikace:

        <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
        <tr>
        <td><strong>Host (hostitel) jméno/subdomain (subdoména)</strong></td>
        <td><strong>IP adresa</strong></td>
        </tr>
        <tr>
        <td>@</td>
        <td>137.135.70.239</td>
        </tr>
        </table>

        Tento příklad ukazuje, vytvoření záznamu a pro kořenovou doménu. Pokud chcete vytvořit položku zástupných pokrýval všechny subdomény, který byste zadali "__*__" jako subdoména.

7. Dále vytvořte záznam CNAME alias **awverify**a kanonický doménu **awverify.mysite.azurewebsites.net** , který jste získali dříve.

    > [AZURE.NOTE] Alias awverify může práci pro některé registrátorů, jiné vyžadují úplné aliasu názvu domény awverify.www.customdomainname.com nebo awverify.customdomainname.com.

    Například následující vytvoří záznam CNAME, který Azure slouží k ověření A záznam konfigurace.

    <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
    <tr>
    <td><strong>Alias nebo Host (hostitel) jméno/subdomain (subdoména)</strong></td>
    <td><strong>Kanonický domény</strong></td>
    </tr>
    <tr>
    <td>awverify</td>
    <td>awverify.contoso.azurewebsites.NET</td>
    </tr>
    </table>

> [AZURE.NOTE] Může trvat delší dobu awverify CNAME se rozšíří celém systému DNS. Název vlastní domény definované záznam webu rozšíření awverify CNAME, dokud se nedá nastavit. Služby, třeba <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> můžete ověřit, že záznamy CNAME je k dispozici.

###<a name="add-the-domain-name-to-your-website"></a>Přidání názvu domény na web

Po rozšíření **awverify** záznam CNAME pro název domény, můžete přiřadit potom vlastní doménu definované záznam s webem. Můžete přidat vlastní název domény definované záznam na web pomocí buď Azure rozhraní příkazového řádku nebo na portálu Správa Azure.

**Chcete-li přidat název domény pomocí rozhraní příkazového řádku Azure (Azure rozhraní příkazového řádku)**

Instalace a konfigurace [Azure rozhraní příkazového řádku](/manage/install-and-configure-cli/)a potom použijte tento příkaz:

    azure site domain add customdomain yoursitename

Například následující přidáte vlastní název domény **contoso.com** **contoso.azurewebsite.net** web:

    azure site domain add contoso.com contoso

Můžete zkontrolovat, že vlastní název domény přidal na web pomocí následujícího příkazu:

    azure site domain list yoursitename

Seznam vrácených tento příkaz by měl obsahovat vlastní název domény, stejně jako výchozí **. azurewebsite.net** položky.

**Chcete-li přidat název domény na portálu Správa Azure**

1. V prohlížeči otevřete na [Portálu Správa Azure][portal].

2. Na kartě **weby** klikněte na název vašeho webu, vyberte **řídicího panelu**a pak vyberte **Spravovat domény** v dolní části stránky.

    ![][setcname2]

6. Do textového pole **Názvy DOMÉN** zadejte název domény, kterou jste nakonfigurovali.

    ![][setcname3]

6. Klikněte na značku zaškrtnutí potvrďte název domény.

Po dokončení konfigurace vlastní název domény uvedené v části **názvy domén** na stránce **Konfigurovat** webu.

> [AZURE.NOTE] Po přidání vlastní domény názvu definovaného záznam na web, můžete odebrat záznam CNAME awverify pomocí nástroje poskytnuté svého registrátora. Ale pokud chcete přidat jiný A záznamu v budoucnu, budete muset znovu awverify záznam před můžete přiřadit název nové domény definované nový záznam s webem.

## <a name="next-steps"></a>Další kroky

-   [Jak spravovat weby](/manage/services/web-sites/how-to-manage-websites/)

-   [Konfigurace certifikátu SSL pro weby](/develop/net/common-tasks/enable-ssl-web-site/)


<!-- Bookmarks -->

[Configure your web sites for shared mode]: #bkmk_configsharedmode
[Configure the CNAME on your domain registrar]: #bkmk_configurecname
[Configure a CNAME verification record on your domain registrar]: #bkmk_configurecname
[Configure an A record for the domain name]:#bkmk_configurearecord
[Set the domain name in management portal]: #bkmk_setcname

<!-- Links -->

[PricingDetails]: /pricing/details/
[portal]: http://manage.windowsazure.com
[digweb]: http://www.digwebinterface.com/
[cloudservicedns]: ../articles/custom-dns.md
[trafficmanager]: ../articles/app-service-web/web-sites-traffic-manager.md
[addendpoint]: ../articles/traffic-manager/traffic-manager-endpoints.md
[createprofile]: ../articles/traffic-manager/traffic-manager-manage-profiles.md

<!-- images -->

[setcname1]: ../media/dncmntask-cname-5.png


<!-- images -->
[standardmode1]: ./media/custom-dns-web-site/dncmntask-cname-1.png
[standardmode2]: ./media/custom-dns-web-site/dncmntask-cname-2.png
[standardmode3]: ./media/custom-dns-web-site/dncmntask-cname-3.png
[standardmode4]: ./media/custom-dns-web-site/dncmntask-cname-4.png


[setcname2]: ./media/custom-dns-web-site/dncmntask-cname-6.png
[setcname3]: ./media/custom-dns-web-site/dncmntask-cname-7.png
