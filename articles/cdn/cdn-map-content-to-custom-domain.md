<properties
     pageTitle="Postup mapování obsahem sítě (CDN) Azure doručování obsahu na vlastní doméně | Microsoft Azure"
     description="Toto téma ukazuje, jak lze mapování CDN obsahu na vlastní doménu."
     services="cdn"
     documentationCenter=""
     authors="camsoper"
     manager="erikre"
     editor=""/>
<tags
     ms.service="cdn"
     ms.workload="media"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="article"
    ms.date="07/28/2016"
     ms.author="casoper"/>

# <a name="how-to-map-custom-domain-to-content-delivery-network-cdn-endpoint"></a>Jak mapovat vlastní doménu koncový bod obsahu doručení Network (CDN)
Můžete namapovat vlastní doménu koncový bod CDN mohli použít vlastní název domény v adresy URL obsah v mezipaměti, spíše než pomocí poddomény azureedge.net.

Namapujte vaší vlastní domény na koncový bod CDN dvěma způsoby:

1. [Vytvoření záznamu CNAME u svého doménového registrátora a zmapovat vlastní doménu a subdoménu koncový bod CDN](#register-a-custom-domain-for-an-azure-cdn-endpoint)

    Záznam CNAME je do DNS mapy zdrojové domény, třeba `www.contosocdn.com` nebo `cdn.contoso.com`, do cílové doméně. V tomto případě je doména zdroj vlastní doménu a subdoménu (subdoménu, jako **www** nebo **cdn** požaduje vždy). Cílové doméně je koncový bod CDN.  

    Proces mapování vaší vlastní domény koncový bod CDN můžete, ale za následek stručný období výpadek služeb pro doménu registrace domény na portálu Azure.

2. [Přidání kroku intermediate registrace s **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)

    Pokud vlastní domény momentálně podporuje aplikaci smlouvu o úrovni služeb (SLA), která vyžaduje, která tu žádné prostoje, můžete použít Azure **cdnverify** subdoménu k poskytování krok intermediate registrace tak, aby uživatelé budou mít přístup k vaší doméně při DNS mapování probíhá.  

Po registraci vlastní domény pomocí jedné z výše uvedených postupů chcete [ověřit, že vlastní subdomény odkazuje koncový bod CDN](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [AZURE.NOTE] S doménovým registrátorem domény přiřadit koncový bod CDN musí vytvořit záznam CNAME. Záznamy CNAME mapování specifické subdomény, jako `www.contoso.com` nebo `cdn.contoso.com`. Není možné záznam CNAME přiřadit kořenovou doménu, například `contoso.com`.
>    
> Subdoménu lze pouze přidružené k jeden CDN koncový bod. Záznam CNAME, který vytvoříte bude směrovat všechny přenosy adresovanou subdoména zadaný koncový bod.  Řekněme, že spojíte `www.contoso.com` s CDN koncový bod, potom je nelze ji přidružit Azure koncové body, například koncový bod účtu úložiště nebo koncový bod služby cloudu. Však může použít jiný subdomény ze stejné domény pro koncové body různé služby. Můžete taky namapovat různých subdomén stejný CDN koncový bod.
>
> Protože pro koncové body **Azure CDN z Verizon** (Standard a Premium) Všimněte si, že zabírá **90 minut** vlastní doménu změny se rozšíří do CDN okraj uzlů.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Registraci vlastní domény pro koncový bod Azure CDN

1.  Přihlaste se k [portálu Azure](https://portal.azure.com/).
2.  Klikněte na **Procházet**, pak **CDN profily**, potom CDN profil s koncový bod, který chcete namapovat na vlastní doménu.  
3.  V zásuvné **CDN profil** klikněte na koncový bod CDN, ke kterému chcete přidružit subdoména.
4.  V horní části zásuvné koncový bod klikněte na tlačítko **Přidat vlastní doménu** .  V zásuvné **Přidat vlastní doménu** zobrazí se název hostitele koncového bodu odvozeno z vaší koncový bod CDN pro použití při vytváření nový CNAME záznam. Formát adresy název hostitele se zobrazí jako ** &lt;EndpointName >. azureedge.net**.  Můžete zkopírovat tento název hostitele pro použití při vytváření záznamu CNAME.  
5.  Přejděte na webu registrátora vaší domény a najděte v části vytváření záznamů DNS. Je možné v oddílu jako je **Název domény**, **DNS**nebo **Správa název serveru**.
6.  Najděte v části pro správu mají záznamy CNAME. Bude pravděpodobně nutné přejít na stránku upřesňující nastavení a hledejte slova CNAME Alias nebo subdomény.
7.  Vytvořte nový CNAME záznam, který mapy vaší zvolené subdoménu (třeba **www** nebo **cdn**) na název hostitele podle zásuvné **Přidat vlastní doménu** .
8.  Vraťte se na zásuvné **Přidat vlastní doménu** a zadejte vlastní domény, včetně subdoménu, v dialogovém okně. Zadejte název domény ve formátu `www.contoso.com` nebo `cdn.contoso.com`.   

    Azure bude ověřte záznam CNAME pro název domény, kterou jste zadali. Při správné záznamy CNAME je ověřit vaši vlastní doménu.  Protože pro koncové body **Azure CDN z Verizon** (Standard a Premium) může trvat až 90 minut, než se rozšíří do všech uzlů okraj CDN, ale nastavení vlastní domény.  

    Všimněte si, že v některých případech to může trvat čas záznamu CNAME na názvové servery na Internetu. Pokud není okamžitě ověření domény a domníváte správnost záznamu CNAME, počkejte pár minut a zkuste to znovu.


## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain"></a>Registraci vlastní domény pro koncový bod Azure CDN pomocí poddomény zprostředkovatel cdnverify  

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Klikněte na **Procházet**, pak **CDN profily**, potom CDN profil s koncový bod, který chcete namapovat na vlastní doménu.  
3. V zásuvné **CDN profil** klikněte na koncový bod CDN, ke kterému chcete přidružit subdoména.
4. V horní části zásuvné koncový bod klikněte na tlačítko **Přidat vlastní doménu** .  V zásuvné **Přidat vlastní doménu** zobrazí se název hostitele koncového bodu odvozeno z vaší koncový bod CDN pro použití při vytváření nový CNAME záznam. Formát adresy název hostitele se zobrazí jako ** &lt;EndpointName >. azureedge.net**.  Můžete zkopírovat tento název hostitele pro použití při vytváření záznamu CNAME.
5. Přejděte na webu registrátora vaší domény a najděte v části vytváření záznamů DNS. Je možné v oddílu jako je **Název domény**, **DNS**nebo **Správa název serveru**.
6. Najděte v části pro správu mají záznamy CNAME. Bude pravděpodobně nutné přejít na stránku upřesňující nastavení a hledejte slova **CNAME** **Alias**nebo **subdomény**.
7. Vytvořte nový CNAME záznam a zadejte alias subdomain (subdoména), který obsahuje subdoména **cdnverify** . Subdoména, kterou zadáte třeba budete mít ve formátu **cdnverify.www** nebo **cdnverify.cdn**. Zadejte název hostitele, což je vaše koncový bod CDN ve formátu **cdnverify.&lt; EndpointName >. azureedge.net**.
8. Vraťte se na zásuvné **Přidat vlastní doménu** a zadejte vlastní domény, včetně subdoménu, v dialogovém okně. Zadejte název domény ve formátu `www.contoso.com` nebo `cdn.contoso.com`. Všimněte si, že v tomto kroku nepotřebujete subdoména s **cdnverify**začínat.  

    Azure bude ověřte záznam CNAME pro název domény cdnverify, které jste zadali.
9. V tomto okamžiku byly Azure ověřeny vaši vlastní doménu, ale umožnění datových přenosů do vaší domény není zatím směrovány koncový bod CDN. Po čekání dlouho povolit nastavení vlastní domény se rozšíří do uzly okraj CDN (90 minut **Azure CDN z Verizon**, 1-2 minuty pro **Azure CDN z Akamai**), vraťte se na webu registrátora vaší DNS a vytvořte další záznam CNAME, který odpovídá vaší subdoménu koncový bod CDN. Například subdoména určit jako **www** nebo **cdn**a hostname jako ** &lt;EndpointName >. azureedge.net**. Tento krok je dokončeno registraci vlastní domény.
10. Nakonec můžete odstranit záznam CNAME jste vytvořili pomocí **cdnverify**, bylo nutné pouze jako zprostředkovatel krok.  


## <a name="verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Ověřte, že vlastní subdomény odkazuje koncový bod CDN

- Po dokončení zápisu vaší vlastní domény máte přístup k obsahu, který je v mezipaměti na koncový bod CDN používat vlastní doménu.
Nejdřív zajištění veřejné obsah, který je v mezipaměti na koncový bod. Například pokud koncový bod CDN je přidružené k účtu úložiště, CDN ukládání do mezipaměti obsahu ve veřejných objektů blob kontejnery. Otestovat vlastní doménu, ujistěte se, že váš kontejner nastavený na povolit přístup k veřejným a obsahuje alespoň jeden objektů blob.
- V prohlížeči přejděte na adresu objektů blob používat vlastní doménu. Například, pokud je vaše vlastní doména `cdn.contoso.com`, adresu URL do mezipaměti objektů blob bude vypadat podobně jako následující adresu URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Viz taky

[Jak povolit síť pro doručování obsahu (CDN) pro Azure](./cdn-create-new-endpoint.md)  
