<properties 
    pageTitle="Jak spravovat uživatelské účty v Azure rozhraní API správy | Microsoft Azure" 
    description="Zjistěte, jak vytvořit nebo pozvání uživatelů správy rozhraní API Azure" 
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

# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Správa uživatelských účtů správy rozhraní API Azure

V části Správa rozhraní API vývojáři jsou uživatelé rozhraní API, která vystavíte pomocí rozhraní API správy. Tato příručka ukazují pro vytvoření a pozvat vývojářům použití rozhraní API a produkty, které zpřístupníte k nim pomocí rozhraní API správy instance. Informace o správě uživatelských účtů programově najdete v dokumentaci [uživatel](https://msdn.microsoft.com/library/azure/dn776330.aspx) [REST API správy](https://msdn.microsoft.com/library/azure/dn776326.aspx) odkazu.

## <a name="create-developer"> </a>Vytvořit nové vývojář

Pokud chcete vytvořit nové vývojář, klikněte na **Spravovat** Azure klasické portálu správy rozhraní API službě. Tím přejdete na portál správy rozhraní API aplikace publisher. Pokud jste ještě nevytvořili instanci služby správy rozhraní API, v tématu [Vytvoření instanci služby správy rozhraní API][] v kurzu [začít používat správu rozhraní API Azure][] .

![Portál aplikace Publisher][api-management-management-console]

V nabídce **Rozhraní API správy** na levé straně klikněte na **uživatele** a potom klikněte na **Přidat uživatele**.

![Vytvoření vývojář][api-management-create-developer]

Zadejte **e-mailu**, **heslo**a **název** pro nový vývojář a klikněte na **Uložit**.

![Vytvoření vývojář][api-management-add-new-user]

Ve výchozím nastavení účtů nově vytvořený vývojář jsou **aktivní**a přidružené ke skupině **vývojáři** .

![Nové vývojář][api-management-new-developer]

Karta Vývojář účty, které jsou ve stavu **aktivní** mohou sloužit k přístup k rozhraní API, pro které mají předplatná. Nově vytvořený vývojář přidružit další skupiny, najdete v článku [jak přiřadit skupinám s vývojáři][].

## <a name="invite-developer"> </a>Pozvat vývojář

Pozvat vývojář, v nabídce **Rozhraní API správy** na levé straně klikněte na **uživatele** , a potom klikněte na **Pozvat uživatele**.

![Pozvat vývojář][api-management-invite-developer]

Zadejte jméno a e-mailovou adresu vývojář a klikněte na **Pozvat**.

![Pozvat vývojář][api-management-invite-developer-window]

Zobrazí zprávu s potvrzením, ale nově pozvaným vývojář nezobrazují v seznamu až po přijetí pozvání. 

![Pozvat potvrzení][api-management-invite-developer-confirmation]

Pozvání vývojář e-mail odeslaný do vývojář. E-mailu se pomocí šablony a přizpůsobit. Další informace najdete v tématu [Konfigurace e-mailové šablony][].

Po přijetí pozvání aktivaci účtu.

## <a name="block-developer"></a> Deaktivovat nebo znovu aktivovat účet vývojář

Ve výchozím nastavení jsou **aktivní**nově vytvořené nebo hosté pozvaní vývojář účty. Deaktivace vývojář, klikněte na **blok**. Opětovná aktivace účet blokovaných vývojář, klikněte na **Aktivovat**. Blokované vývojář účet nelze přístup k portálu pro vývojáře nebo všechny rozhraní API volat. Odstranění uživatelského účtu, klikněte na **Odstranit**.

![Karta Vývojář v bloku][api-management-new-developer]

## <a name="reset-a-user-password"></a>Resetování hesla uživatele

Resetování hesla k uživatelskému účtu, klikněte na název účtu.

![Resetování hesla][api-management-view-developer]

Klikněte na Odeslat odkaz na uživatele, aby obnovení hesla **Resetovat heslo** .

![Resetování hesla][api-management-reset-password]

Programově pracovat s uživatelských účtů, najdete v dokumentaci [uživatel](https://msdn.microsoft.com/library/azure/dn776330.aspx) [REST API správy](https://msdn.microsoft.com/library/azure/dn776326.aspx) odkazu. K resetování hesla uživatelského účtu na určitou hodnotu, můžete pomocí operace [aktualizace uživatele](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) a zadejte požadované heslo.

## <a name="pending-verification"></a>Pole Čekání na ověření

![Pole Čekání na ověření][api-management-pending-verification]

## <a name="next-steps"> </a>Další kroky

Po vytvoření účtu Vývojář můžete přidružit rolí a přihlášení k odběru ho produkty a rozhraní API. Další informace najdete v článku [jak vytvářet a používat skupiny][].


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
[]: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[Vytvoření a používání skupin]: api-management-howto-create-groups.md
[Jak přidružit vývojáři skupiny]: api-management-howto-create-groups.md#associate-group-developer

[Začít používat správu rozhraní API Azure]: api-management-get-started.md
[Vytvořit instanci služby správy rozhraní API]: api-management-get-started.md#create-service-instance
[Konfigurace e-mailové šablony]: api-management-howto-configure-notifications.md#email-templates