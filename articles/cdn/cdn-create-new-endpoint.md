<properties
     pageTitle="Použití Azure CDN | Microsoft Azure"
     description="Toto téma ukazuje, jak povolit Network obsahu doručení (CDN) Azure. Kurz provede vytvoření nového profilu CDN a koncového bodu."
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
     ms.topic="get-started-article"
     ms.date="07/28/2016" 
     ms.author="casoper"/>

# <a name="using-azure-cdn"></a>Použití Azure CDN  

Toto téma se provede povolení Azure CDN vytvořením nového profilu CDN a koncového bodu.

>[AZURE.IMPORTANT] Úvod do CDN principu i seznam funkcí najdete v článku [Přehled CDN](./cdn-overview.md).

## <a name="create-a-new-cdn-profile"></a>Vytvoření nového profilu CDN

Profil CDN je kolekce CDN koncové body.  Každý profil obsahuje jeden nebo více CDN koncové body.  Můžete chtít používat více profily uspořádání koncových bodů CDN domény internet, webové aplikace nebo jiných kritérií.

> [AZURE.NOTE] Ve výchozím nastavení se omezí na osm profily CDN jednoho Azure předplatné. Každý CDN profil se omezí na 10 CDN koncové body.
>
> Ceny CDN se používají na úrovni profilu CDN. Pokud chcete použít kombinaci ceny úrovní Azure CDN, budete potřebovat víc CDN profily.

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Vytvoření nového koncového bodu CDN

**Vytvoření nového koncového bodu CDN**

1. Na [Portálu Azure](https://portal.azure.com)přejděte do vlastního profilu CDN.  Se může mít připnuté ho na řídicí panel v předchozím kroku.  Pokud můžete, najdete ji kliknutím na **Procházet**a pak **CDN profily**a kliknete na profil chcete přidat koncový bod k.

    Zobrazí se zásuvné CDN profilu.

    ![CDN profilu][cdn-profile-settings]

2. Klikněte na tlačítko **Přidat koncový bod** .

    ![Přidání tlačítka koncový bod][cdn-new-endpoint-button]

    Zobrazí se zásuvné **Přidat koncový bod** .

    ![Přidání zásuvné koncový bod][cdn-add-endpoint]

3. Zadejte **název** pro tento CDN koncový bod.  Tento název se použije pro přístup k režim cached zdrojů v doméně `<endpointname>.azureedge.net`.

4. V rozevíracím seznamu **Typ Origin** vyberte typ origin.  Vyberte **úložiště** pro účet Azure úložiště, **cloudové služby** pro cloudové služby Azure, **Webové aplikace** pro Azure Web App nebo **vlastní origin** pro všechny ostatní origin serveru veřejně přístupný web (hostované v Azure nebo kdekoli jinde).

    ![Typ origin CDN](./media/cdn-create-new-endpoint/cdn-origin-type.png)
        
5. V rozevíracím seznamu **Origin hostname** vyberte nebo zadejte vaší původní doméně.  Rozevíracího seznamu zobrazí seznam všech dostupných typů text, který jste zadali v kroku 4.  Pokud jste vybrali *vlastní origin* jako svého **Zadejte Origin**, bude zadáván v doméně vlastní origin.

6. Do textového pole **Origin cesta** zadejte cestu k prostředkům chcete ukládat do mezipaměti, nebo nechejte prázdné umožňuje mezipaměti všechny zdroje na doménu, kterou jste zadali v kroku 5.

7. V **záhlaví hostitele Origin**záhlaví hostitele má CDN odeslat při každém požadavku připojí nebo odejdou výchozí.

    > [AZURE.WARNING] Některé typy typů, například Azure úložiště a webových aplikací Web Apps, vyžadují záhlaví hostitele podle původní doméně. Pokud nemáte origin, vyžadovaného záhlaví hostitele liší od svou doménou, nechejte výchozí hodnotu.

8. **Protocol (protokol)** a **Origin portu**zadejte protokoly a porty použitých při přístupu k prostředkům na počátek.  Alespoň jeden protocol (protokol HTTP nebo HTTPS) musí být zaškrtnuté.
    
    > [AZURE.NOTE] Co port používá koncový bod načítat informace z původ ovlivní pouze **Origin port** .  Koncový bod samotné bude k dispozici pouze ukončit klienty na výchozí HTTP a HTTPS porty 80 a 443, bez ohledu na **Origin port**.  
    >
    > Koncové body **Azure CDN z Akamai** nepovolují široká TCP port pro typů.  Seznam origin porty, které nejsou povolené najdete v článku [Azure CDN z Akamai povolené Origin porty](https://msdn.microsoft.com/library/mt757337.aspx).  
    >
    > Přístup k CDN obsahu pomocí HTTPS obsahuje následující omezení:
    > 
    > - Je nutné použít certifikát SSL poskytovanou CDN. Třetí strany certifikáty nejsou podporované.
    > - Musíte používat doménu, pokud CDN (`<endpointname>.azureedge.net`) přístup k obsahu HTTPS. Podpora HTTPS není k dispozici pro vlastní názvy domén (adres CNAME), protože CDN nepodporuje vlastní certifikáty v současné době.

9. Klikněte na tlačítko **Přidat** k vytvoření nové koncový bod.

10. Po vytvoření koncového bodu se zobrazí v seznamu koncové body profilu. Zobrazení seznamu zobrazuje URL použít pro přístup k obsahu mezipaměti, jakož i původní doméně.

    ![Koncový bod CDN][cdn-endpoint-success]

    > [AZURE.IMPORTANT] Koncový bod nebudou okamžitě k dispozici pro použití, jako vyžaduje čas pro registraci se rozšíří prostřednictvím CDN.  <b>Azure CDN z Akamai</b> profilů šíření dokončí obvykle do jedné minuty.  <b>Azure CDN z Verizon</b> profilů šíření obvykle dokončí do 90 minut, ale v některých případech může trvat déle.
    >    
    > Uživatelé, kteří pokusíte použít název domény CDN před konfigurace koncového bodu má rozšíří do body POP dostanou kódů odpovědí HTTP 404.  Pokud už uběhlo několik hodin a od vytvořené koncový bod a jste ještě přijímá 404 odpovědi, přečtěte si téma [Poradce při potížích s CDN koncové body vrácení 404 stavy](cdn-troubleshoot-endpoint.md).


##<a name="see-also"></a>Viz taky
- [Řízení chování mezipaměti žádostí o řetězce dotazu](cdn-query-string.md)
- [Postup mapování CDN obsahu na vlastní doméně](cdn-map-content-to-custom-domain.md)
- [Předem načíst prostředky na koncový bod Azure CDN](cdn-preload-endpoint.md)
- [Odstranění koncového bodu Azure CDN](cdn-purge-endpoint.md)
- [Poradce při potížích s koncové body CDN vrácení 404 stavy](cdn-troubleshoot-endpoint.md)

[cdn-profile-settings]: ./media/cdn-create-new-endpoint/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-create-new-endpoint/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-create-new-endpoint/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-create-new-endpoint/cdn-endpoint-success.png
