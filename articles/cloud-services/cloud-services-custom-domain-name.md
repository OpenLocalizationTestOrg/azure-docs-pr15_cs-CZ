<properties
    pageTitle="Konfigurace vaší vlastní doménou v Cloudovým službám | Microsoft Azure"
    description="Zjistěte, jak vystavit Azure aplikace nebo dat na vlastní doméně konfigurací nastavení DNS."
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="adegeo"/>

# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Konfigurace vlastního názvu domény pro službu Azure cloudu

> [AZURE.SELECTOR]
- [Azure portálu](cloud-services-custom-domain-name-portal.md)
- [Azure klasické portálu](cloud-services-custom-domain-name.md)


Při vytváření do cloudové služby Azure ho přiřadí subdoménu cloudapp.net. Například pokud cloudové služby je s názvem "contoso", uživatelé budou mít přístup k aplikaci na adresu URL, třeba http://contoso.cloudapp.net. Azure taky přiřadí virtuální IP adresu.

Můžete však také vystavit aplikace na vlastní název domény, například contoso.com. Tento článek vysvětluje, jak rezervovat nebo konfigurovat vlastní název domény pro web role cloudové služby.

Máte už undestand jaké záznamy CNAME a A jsou? [Odkaz dřívější explaination](#add-a-cname-record-for-your-custom-domain).

> [AZURE.NOTE]
> Získejte bude rychlejší. Použití Azure [návodu s asistencí](http://support.microsoft.com/kb/2990804). Díky přidružení vlastního názvu domény a zabezpečení komunikace (SSL) s Azure Cloud Services nebo weby Azure velmi snadno.

<p/>

> [AZURE.NOTE]
> Postupy v tomto úkolu lze použít ke Cloudovým službám Azure. Aplikace služby najdete v článku [Toto](../app-service-web/web-sites-custom-domain-name.md). Účty úložiště najdete v článku [Toto](../storage/storage-custom-domain-name.md).


## <a name="understand-cname-and-a-records"></a>Princip záznamy CNAME a a.

CNAME (alias záznamy či) a záznamy obou umožňují přidružit konkrétní server název domény (nebo v tomto případě služby) ale fungují jinak. Jsou taky některé konkrétní co byste měli zvážit při použití záznam v Azure cloudové služby, které byste měli zvážit než se rozhodnete, kterou chcete použít.

### <a name="cname-or-alias-record"></a>Záznam CNAME nebo Alias

Záznam CNAME namapuje *určité* domény, například **contoso.com** nebo **www.contoso.com**, na název kanonických domény. V tomto případě je název kanonický domény **[aplikace] .cloudapp .net** název domény pro váš Azure hostovaný aplikací. Po vytvoření záznamy CNAME pro vytváří alias **[aplikace] .cloudapp .net**. Položka CNAME vyřeší na IP adresu vaší **[aplikace] .cloudapp .net** služby automaticky, takže pokud změnou IP adresu cloudové služby, není třeba provádět žádné akce.

> [AZURE.NOTE]
> Některé doménových registrátorů umožňují mapování subdomény při použití záznam CNAME, například www.contoso.com a ne kořenové názvů, například contoso.com. Další informace o CNAME záznamy najdete v dokumentaci podle svého registrátora, [položce Wikipedie na záznam CNAME](http://en.wikipedia.org/wiki/CNAME_record)nebo [Názvy domén IETF - implementaci a specifikace](http://tools.ietf.org/html/rfc1035) dokument.

### <a name="a-record"></a>Záznam

Záznam a mapy doménu, například **contoso.com** nebo **www.contoso.com**, *nebo pomocí zástupných znaků domény* , jako ** \*. contoso.com**, k IP adrese. V případě Azure cloudové služby, nastavení virtuální IP adresy služby. Tak, aby hlavní výhoda přes záznam CNAME záznam, jestli máte jedna položka používající zástupné znaky, například \* **. contoso.com**, které by zpracovávat požadavky pro více dílčích domén s například **mail.contoso.com**, **login.contoso.com**nebo **www.contso.com**.

> [AZURE.NOTE]
> Protože záznam namapovala statickou IP adresu, ho nerozpoznává automaticky změny k IP adrese cloudové služby. IP adresy používá cloudové služby je přidělený při prvním nasazením prázdné úsek (výrobní nebo pracovní.) Pokud byste odstranili nasazení úsek, vydání IP adresu podle Azure a všechny budoucí nasazení na pozici může být zadán novou IP adresu.
>
> IP adresu dané nasazení úsek (výrobní nebo pracovní) je snadno, zachován při přepnutí mezi pracovní a výrobní nasazení nebo provádění místní upgrade stávajícího nasazení. Další informace o provádění tyto akce najdete v tématu [Správa cloudovým službám](cloud-services-how-to-manage.md).


## <a name="add-a-cname-record-for-your-custom-domain"></a>Přidejte záznam CNAME pro vlastní doménu

K vytvoření záznamu CNAME, musí přidáte novou položku v tabulce DNS pro vaši vlastní doménu pomocí nástroje poskytnuté svého registrátora. Každý registrátora má podobné ale mírně odlišnou způsob, jak určit záznam CNAME, ale pojmy jsou stejné.

1. Použijte jeden z těchto postupů najdete **. cloudapp.net** přiřazené ke cloudové službě název domény.

    * Přihlaste se ke [Azure klasické portál], vyberte vaše cloudové služby, vyberte **řídicího panelu**a potom vyhledejte položku **Adresu URL webu** v části **Rychlý přehled** .
    
        ![Stručný přehled oddíl zobrazenou adresou URL webu][csurl]
    
        **NEBO**  
    
    * Instalace a konfigurace [Prostředí Powershell Azure](../powershell-install-configure.md)a pak použijte tento příkaz:
        
        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```
    
    Uložení název domény použít v adrese URL vrácené obě metody, budete potřebovat při vytváření záznamu CNAME.

1.  Přihlaste se na webu registrátora vaší DNS a přejděte na stránku pro správu DNS. Vyhledejte odkazy nebo oblasti webu textem je označená jako **Název domény**, **DNS**a **Správa název serveru**.

2.  Teď najdete místo, kam můžete vybrat nebo zadat na CNAME. Možná budete muset vyberte typ záznamu z kapky dolů nebo přejít na stránku upřesňující nastavení. By měla vypadat slova **CNAME** **Alias**nebo **subdomény**.

3.  Také je nutné zadat doménu a subdoménu alias pro CNAME, například **www** Pokud chcete vytvořit alias pro **www.customdomain.com**. Pokud chcete vytvořit alias pro kořenovou doménu, může být vedená jako "**@**" symbol nebo u svého registrátora DNS nástroje.

4. Potom je nutné zadat Host (hostitel) kanonický název, který je vaše aplikace **cloudapp.net** doménu v tomto případě.

Například následující záznamy SRV předá všechny přenosy z **www.contoso.com** **contoso.cloudapp.net**, název vlastní domény nasazení aplikace:

| Alias nebo Host (hostitel) jméno/subdomain (subdoména) | Kanonický domény     |
| ------------------------- | -------------------- |
| www                       | contoso.cloudapp.NET |

Návštěvník **www.contoso.com** nikdy uvidí hostiteli true (contoso.cloudapp.net), tedy neviditelné koncovému uživateli procesu přesměrování.

> [AZURE.NOTE]
> V příkladu výše platí jenom pro přenosů na subdoména **www** . Protože se záznamy CNAME nelze použít zástupné znaky, musíte vytvořit jednu CNAME pro každou doménu a subdoménu. Chcete-li směrování adres subdomény, jako například \*. contoso.com, cloudapp.net adrese, můžete nakonfigurovat **Přesměrování adresy URL** nebo **Přesměrování adresy URL** položky v nastavení DNS, nebo vytvořte záznam.


## <a name="add-an-a-record-for-your-custom-domain"></a>Přidání záznamu a pro vaši vlastní doménu

Pokud chcete vytvořit záznam a, musíte nejdřív najít virtuální IP adresu cloudové služby. Potom přidejte novou položku v tabulce DNS pro vaši vlastní doménu pomocí nástroje poskytnuté svého registrátora. Každý registrátora má podobné ale mírně odlišnou způsob, jak určit záznam, ale pojmy jsou stejné.

1. Pomocí jedné z následujících metod IP adresu cloudové služby.
    
    * Přihlaste se ke [Azure klasické portál], vyberte vaše cloudové služby, vyberte **řídicího panelu**a potom vyhledejte položku **adresa veřejného virtuální IP (VIP)** v části **Rychlý přehled** .
    
        ![oddíl rychlým přehledem zobrazující VIP][vip]
    
        **NEBO**  
    
    * Instalace a konfigurace [Prostředí Powershell Azure](../powershell-install-configure.md)a pak použijte tento příkaz:
    
        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```
    
    Pokud máte víc koncové body přidružené cloudové služby, obdrží více řádků obsahujících IP adresu, ale všechny by měl zobrazit použít stejnou adresu.
    
    Uložte na IP adresu, protože budete potřebovat při vytváření záznamu.

1.  Přihlaste se na webu registrátora vaší DNS a přejděte na stránku pro správu DNS. Vyhledejte odkazy nebo oblasti webu textem je označená jako **Název domény**, **DNS**a **Správa název serveru**.

2.  Teď najdete místo, kam můžete vybrat nebo zadat záznam. Možná budete muset vyberte typ záznamu z kapky dolů nebo přejít na stránku upřesňující nastavení.

3. Vyberte nebo zadejte doménu nebo subdoména, kterou použijete tento záznam. Pokud chcete vytvořit alias pro **www.customdomain.com**například vyberte **www** . Pokud chcete vytvořit položku zástupných znaků pro všechny subdomény, zadejte "__*__". Tím se zabývat těmito oblastmi všech dílčích domén s například **mail.customdomain.com**, **login.customdomain.com**a **www.customdomain.com**.

    Pokud chcete vytvořit záznamu a pro kořenovou doménu, může být vedená jako "**@**" symbol nebo u svého registrátora DNS nástroje.

4. Zadejte IP adresu cloudové služby v zadané oblasti. To spojí položce domény použít v záznam s IP adresu nasazení služby cloudu.

Například následující, které záznamu předá všechny přenosy z **contoso.com** **137.135.70.239**, IP adresu nasazení aplikace:

| Host (hostitel) jméno/subdomain (subdoména) | IP adresa     |
| ------------------- | -------------- |
| @                   | 137.135.70.239 |



Tento příklad ukazuje, vytvoření záznamu a pro kořenovou doménu. Pokud chcete vytvořit položku zástupných pokrýval všechny subdomény, který byste zadali "__*__" jako subdoména.

>[AZURE.WARNING]
>Dynamické ve výchozím nastavení nejsou IP adresy v Azure. Pravděpodobně můžete použít [vyhrazené IP adresu](../virtual-network/virtual-networks-reserved-public-ip.md) zajistit, aby se nemění svoji IP adresu.

## <a name="next-steps"></a>Další kroky

* [Jak spravovat Cloudovým službám](cloud-services-how-to-manage.md)
* [Postup mapování CDN obsahu na vlastní doméně](../cdn/cdn-map-content-to-custom-domain.md)
* [Obecné konfigurace cloudové služby](cloud-services-how-to-configure.md).
* Zjistěte, jak [nasadit do cloudové služby](cloud-services-how-to-create-deploy.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).




[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: http://msdn.microsoft.com/library/ee517253.aspx
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure klasické portálu]: https://manage.windowsazure.com
[Validate Custom Domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
[vip]: ./media/cloud-services-custom-domain-name/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png
 