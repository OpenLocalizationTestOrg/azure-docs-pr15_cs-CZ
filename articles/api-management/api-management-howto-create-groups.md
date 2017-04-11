<properties 
    pageTitle="Jak vytvořit a používat skupiny pro správu účtů vývojář správy rozhraní API Azure" 
    description="Naučte se spravovat účty vývojář pomocí skupin správy rozhraní API Azure" 
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

# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Jak vytvořit a používat skupiny pro správu účtů vývojář správy rozhraní API Azure

V části Správa rozhraní API skupiny slouží ke správě viditelnost produktů pro vývojáře. Produkty nejdřív udělali viditelné pro skupiny, a pak můžete vývojáři v těchto skupinách zobrazení a odběr produkty, které jsou přidružené k skupiny. 

Správa rozhraní API obsahuje následující skupiny neměnný systému.

-   **Správci** - Azure předplatné správci jsou členy této skupiny. Správci spravovat instancí služby správy rozhraní API vytváření rozhraní API, operace a produkty, které využívají vývojáři.
-   **Vývojáři** - ověřené – portál pro vývojáře, které uživatelé spadají do této skupiny. Vývojáři se zákazníky, kteří vytváření aplikací pomocí svého rozhraní API. Vývojáři mají přístup k portálu pro vývojáře a vytváření aplikací, které volají operace rozhraní API.
-   **Hosté** - uživatelé portálu neověřených vývojář, například potenciální zákazníci návštěva portálu pro vývojáře instance rozhraní API správy spadají do této skupiny. Mohou být uděleny určité jen pro čtení přístup, například možnost zobrazit rozhraní API, ale ne jim zavolá.

Kromě těchto skupin systém správci můžete vytvořit vlastní skupiny nebo [využít externí skupin v klienti přidruženými Azure Active Directory][]. Vlastní a externí skupiny mohou sloužit k systému skupin v pojmenování vývojáři viditelnost a přístup k rozhraní API produkty. Můžete například vytvořit jednu vlastní skupinu pro vývojáře přidružen konkrétního partnera organizace a aby mu umožnil přístup k rozhraní API z produktu obsahující jenom relevantní rozhraní API. Uživatel může být členem skupiny víc než jedné skupině.

Tato příručka ukazuje, jak můžou správci správy rozhraní API instance přidávání nových skupin a spojit s produkty a vývojáři.

>[AZURE.NOTE] Kromě vytváření a Správa skupin na portálu Publisheru, můžete vytvořit a spravovat skupiny pomocí rozhraní API správy REST API [skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.

## <a name="create-group"> </a>Vytvoření skupiny

Pokud chcete vytvořit novou skupinu, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě. Tím přejdete na portál správy rozhraní API aplikace publisher.

![Portál aplikace Publisher][api-management-management-console]

>Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

V nabídce **Rozhraní API správy** na levé straně klikněte na **skupiny** a potom klikněte na tlačítko **Přidat skupinu**.

![Přidat novou skupinu][api-management-add-group]

Zadejte jedinečný název skupiny a volitelný popis a klikněte na **Uložit**.

![Přidat novou skupinu][api-management-add-group-window]

Nová skupina se zobrazí na kartě skupiny. Pokud chcete upravit **název** nebo **Popis** skupiny, klikněte na název skupiny v seznamu. Pokud chcete skupinu odebrat, klikněte na **Odstranit**.

![Přidání skupiny][api-management-new-group]

Teď, když se vytvoří skupinu, může být přidružené k produkty a vývojáři.

## <a name="associate-group-product"> </a>Skupiny přidružit k produktu

Pokud chcete přidružit k produktu skupinu, v nabídce **Rozhraní API správy** na levé straně klikněte na **produkty** a potom klikněte na název požadovaného produktu.

![Nastavení viditelnosti][api-management-add-group-to-product]

Vyberte kartu **viditelnost** k přidání a odebrání skupiny a chcete zobrazit aktuální skupiny produktu. Přidání nebo odebrání skupiny, zaškrtněte nebo zrušte zaškrtnutí políček u požadovanými skupinami a klikněte na **Uložit**.

![Nastavení viditelnosti][api-management-add-group-to-product-visibility]

>[AZURE.NOTE] Přidání skupin služby Azure Active Directory, najdete v článku [jak povolit vývojář účty pomocí služby Azure Active Directory správy rozhraní API Azure](api-management-howto-aad.md).
>
>Abyste mohli nakonfigurovat skupiny na kartě **viditelnosti** pro produkt, klikněte na **Spravovat skupiny**.

Jakmile produkt souvisí se skupinou, vývojáři v dané skupině můžete zobrazit a přihlášení k odběru dělený součinem jejich.

## <a name="associate-group-developer"> </a>Přidružit vývojáři skupiny

Pokud chcete přidružit vývojáři skupin, v nabídce **Rozhraní API správy** na levé straně klikněte na **uživatele** a potom zaškrtněte políčko vedle vývojáři, které chcete přidružit k skupiny.

![Přidání vývojář do skupiny][api-management-add-group-to-developer]

Jakmile se požadované vývojáři jsou zaškrtnuté, klikněte na požadovanou skupinu v rozevíracím seznamu **Přidat do skupiny** . Vývojáři je možné odebrat ze skupiny pomocí rozevíracího seznamu **Odebrat ze skupiny** . 

![Vývojáři][api-management-add-group-to-developer-saved]

Po přidání spojení mezi vývojář a ve skupině můžete ji zobrazit na kartě **uživatelů** .


## <a name="next-steps"> </a>Další kroky

-   Po přidání vývojář skupině jejich zobrazení a odběr produkty spojené s danou skupinu. Další informace najdete v článku [jak vytvářet a publikovat produktu v části Správa Azure rozhraní API][],
-   Kromě vytváření a Správa skupin na portálu Publisheru, můžete vytvořit a spravovat skupiny pomocí rozhraní API správy REST API [skupiny](https://msdn.microsoft.com/library/azure/dn776329.aspx) entity.


[api-management-management-console]: ./media/api-management-howto-create-groups/api-management-management-console.png
[api-management-add-group]: ./media/api-management-howto-create-groups/api-management-add-group.png
[api-management-add-group-window]: ./media/api-management-howto-create-groups/api-management-add-group-window.png
[api-management-new-group]: ./media/api-management-howto-create-groups/api-management-new-group.png
[api-management-add-group-to-product]: ./media/api-management-howto-create-groups/api-management-add-group-to-product.png
[api-management-add-group-to-product-visibility]: ./media/api-management-howto-create-groups/api-management-add-group-to-product-visibility.png
[api-management-add-group-to-developer]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer.png
[api-management-add-group-to-developer-saved]: ./media/api-management-howto-create-groups/api-management-add-group-to-developer-saved.png

[api-management-]: ./media/api-management-howto-create-groups/api-management-.png

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[Jak vytvořit a publikovat produkt správy rozhraní API Azure]: api-management-howto-add-products.md

[Začít používat správu rozhraní API Azure]: api-management-get-started.md
[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance
[Využijte externí skupin v klienti přidruženými Azure Active Directory]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
