<properties 
    pageTitle="Jak zabezpečené ověřování certifikátů správy rozhraní API Azure pomocí klienta služby back-end" 
    description="Zjistěte, jak zajistit back-end služby pomocí ověřování certifikátu klienta v části Správa rozhraní API Azure." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Jak zabezpečené ověřování certifikátů správy rozhraní API Azure pomocí klienta služby back-end

Rozhraní API Správa poskytuje možnost zabezpečený přístup ke službě back-end rozhraní API pomocí certifikátů. Tato příručka, ukazuje, jak Správa certifikátů na portálu rozhraní API aplikace publisher a postup při konfiguraci rozhraní API použít certifikát pro přístup k jeho back-end služby.

Informace o správě certifikátů pomocí rozhraní API správy REST API najdete v tématu [Azure rozhraní API správy REST API certifikát entity][].

## <a name="prerequisites"> </a>Požadavky

Tato příručka, uvidíte, jak pro nastavení vaší instanci služby správy rozhraní API ověřování certifikátu klienta používat pro přístup ke službě back-end rozhraní API. Před postupovat podle kroků v tomto tématu, by měl back-end služba nakonfigurovány pro ověřování certifikátu klienta ([Konfigurace ověřování pomocí certifikátu v Azure weby najdete pod odkazy v tomto článku][]) a mít přístup k certifikát a heslo pro certifikát pro odeslání na portálu Správa rozhraní API aplikace publisher.

## <a name="step1"> </a>Odeslat certifikát klienta

Začněte tím, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě. Tím přejdete na portál správy rozhraní API aplikace publisher.

![Portál rozhraní API aplikace Publisher][api-management-management-console]

>Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

V nabídce **Rozhraní API správy** na levé straně klikněte na **zabezpečení** a klikněte na tlačítko **Certifikáty klienta**.

![Certifikátů][api-management-security-client-certificates]

Pokud chcete odeslat nový certifikát, klikněte na **Odeslat certifikát**.

![Nahrání certifikátu][api-management-upload-certificate]

Vyhledejte certifikát a potom zadejte heslo pro potvrzení.

>Certifikát musí být v **.pfx** formátu. Jsou povoleny podepsaného certifikáty.

![Nahrání certifikátu][api-management-upload-certificate-form]

Klikněte na **Odeslat** na odeslat certifikát.

>Heslo certifikátu proběhne v současné době. Pokud je nesprávné, zobrazí se chybová zpráva.

![Certifikát nahráli][api-management-certificate-uploaded]

Po nahrání certifikát se zobrazí na kartě **certifikátů** . Pokud ještě několika certifikátů ten zkontrolujte poznámky předmětu nebo poslední čtyři znaky Miniatura, které se používají k vyberte certifikát při konfiguraci rozhraní API používání certifikátů, podle pokynů v následující části [konfigurovat rozhraní API použít certifikát klienta pro ověřování brány][] .

>Vypnout řetězu ověření certifikátů při použití, například certifikátu podepsaného svým držitelem, postupujte podle kroků popsaných v tato nejčastější dotazy týkající se [položky](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).

## <a name="step1a"> </a>Odstranit certifikát klienta

Odstranění certifikátu, klepněte na tlačítko **Odstranit** vedle požadovaného certifikát.

![Odstranění certifikátu][api-management-certificate-delete]

Klikněte na **Ano, odstranění** potvrďte.

![Potvrzení odstranění][api-management-confirm-delete]

Pokud certifikát je nepoužívá rozhraní API, se zobrazí na obrazovce upozornění. Chcete-li odstranit certifikát je třeba nejprve odebrat certifikát API, které jsou nakonfigurované tak, aby se používala.

![Potvrzení odstranění][api-management-confirm-delete-policy]

## <a name="step2"> </a>Konfigurace rozhraní API pro účely ověření brány certifikát klienta

V nabídce **Rozhraní API správy** na levé straně klikněte na **rozhraní API** , klikněte na název požadovaného rozhraní API a klikněte na kartu **zabezpečení** .

![Rozhraní API zabezpečení][api-management-api-security]

Vyberte **certifikátů** z rozevíracího seznamu **pomocí přihlašovacích údajů** .

![Certifikátů][api-management-mutual-certificates]

Vyberte požadovaný certifikát z rozevíracího seznamu **certifikát klienta** . Pokud existuje více certifikátů může samostatně prohlížet předmětu nebo poslední čtyři znaky Miniatura, jak je uvedeno v předchozí části, abyste určili správný certifikát.

![Vyberte certifikát][api-management-select-certificate]

Klepněte na tlačítko **Uložit** uložte změnu konfigurace rozhraní API.

>Tato změna je efektivní okamžitě a volání operace, které rozhraní API použije certifikát ověření na serveru back-end.

![Uložte změny rozhraní API][api-management-save-api]

>Pokud není zadán certifikát brány ověřování pro službu back-end rozhraní API, přestane být součástí zásad, které rozhraní API a můžete zobrazit v editoru zásad.

![Zásady certifikátů][api-management-certificate-policy]

## <a name="next-steps"></a>Další kroky

Další informace o jiných způsobech zabezpečené back-end služby, jako je základní nebo sdílené tajné ověřování HTTP viz následující video.

> [AZURE.VIDEO last-mile-security]

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Začít používat správu rozhraní API Azure]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance

[Azure entity rozhraní API správy REST API certifikátu]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Konfigurace ověřování certifikátů v Azure weby najdete pod odkazy v tomto článku]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Konfigurace rozhraní API pro účely ověření brány certifikát klienta]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps


 
