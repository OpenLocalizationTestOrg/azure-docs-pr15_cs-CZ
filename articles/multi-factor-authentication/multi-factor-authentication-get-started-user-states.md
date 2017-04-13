<properties 
    pageTitle="Microsoft Azure Multi-Factor Authentication uživatele státy"
    description="Přečtěte si o stavech uživatele v Azure MFA."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="user-states-in-azure-multi-factor-authentication"></a>Uživatel stavy v Azure Vícefaktorové ověřování

Uživatelské účty v Azure Vícefaktorové ověřování mají následující tři odlišné stavy:

Stav | Popis |Vliv na jiné prohlížeče aplikace| Poznámky
:-------------: | :-------------: |:-------------: |:-------------: |
Vypnutí | Ve výchozím stavu pro nové uživatele není zaregistrované v vícefaktorové ověřování.|Ne|Uživatel nepoužívá vícefaktorové ověřování.
Povoleno |Uživatel zaregistrovaná v vícefaktorové ověřování.|Ne.  Jsou i nadále funkční až po dokončení procesu registrace.|Uživatel je povoleno, ale nebyla dokončení procesu registrace. Jim výzva k dokončení procesu na další přihlásit.
Vynucení|Uživatel zaregistrovaná a dokončení procesu registrace pro používání vícefaktorové ověřování.|Ano.  Aplikace vyžadovala hesla aplikace. | Uživatel může nebo nemusí dokončili registrace. Pokud jsou dokončení procesu registrace používají vícefaktorové ověřování. V opačném uživatele se výzva k dokončení procesu na další přihlásit.

## <a name="changing-a-user-state"></a>Změna stavu uživatele
Změny stavu uživatelů podle toho, jestli už uběhlo nastavení pro MFA a jestli má uživatel dokončení procesu.  Pokud zapnete MFA pro uživatele, uživatele, které stát se změní z zakázané aktivní.  Jakmile je uživatel stavem změnil na povoleno, přihlášení a dokončí, jeho stav změní na vynucenou.  

### <a name="to-view-a-users-state"></a>Chcete-li zobrazit stavu uživatele
--------------------------------------------------------------------------------
1.  Přihlaste se k **portálu Azure classic** jako správce.
2.  Na levé straně klikněte na **Služby Active Directory**.
3.  V části **adresář** klikněte v adresáři pro uživatele, kterého chcete povolit.
![Klikněte na adresář](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Nahoře klikněte na **uživatele**.
5.  V dolní části stránky klikněte na **Spravovat Auth Multi-Factor**.
![Klikněte na adresář](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Tím se otevře na nové záložce prohlížeče.  Budou moct uživatelé stavu zobrazení.
![Klikněte na adresář](./media/multi-factor-authentication-get-started-user-states/userstate1.png)

###<a name="to-change-the-state-from-disabled-to-enabled"></a>Chcete-li změnit stav ze zakázané na povoleno
1.  Přihlaste se k **portálu Azure classic** jako správce.
2.  Na levé straně klikněte na **Služby Active Directory**.
3.  V části **adresář** klikněte v adresáři pro uživatele, kterého chcete povolit.
![Klikněte na adresář](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Nahoře klikněte na **uživatele**.
5.  V dolní části stránky klikněte na **Spravovat Auth Multi-Factor**.
![Klikněte na adresář](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Tím se otevře na nové záložce prohlížeče.  Najděte uživatele, který chcete povolit pro vícefaktorové ověřování. Budete muset změnit zobrazení nahoře. Zajistěte, aby byl stav **zakázané.** 
 ![Povolit uživateli](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Zaškrtnutím **zaškrtněte** políčko vedle jeho jména.
7.  Na pravé straně klikněte na tlačítko **Povolit**.
![Povolení uživatele](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Zaškrtněte políčko **Povolit Multi-Factor auth**.
![Povolení uživatele](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  By měl všimněte, že stav uživatele se změnil z **Zakázané** **aktivní**.
![Povolení uživatelům](./media/multi-factor-authentication-get-started-cloud/user.png)
10.  Po povolení uživatelům se doporučuje se upozornění prostřednictvím e-mailu.  Ho musí taky informovat je jak jim umožní anonymní jejich prohlížení aplikace vyhnout zablokovaní.

### <a name="to-change-the-state-from-enabledenforced-to-disabled"></a>Pokud chcete změnit stav povolené/nevynucují zakázáno
1.  Přihlaste se k **Azure klasické portál** jako správce.
2.  Na levé straně klikněte na **Služby Active Directory**.
3.  V části **adresář** klikněte v adresáři pro uživatele, kterého chcete povolit.
![Klikněte na adresář](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Nahoře klikněte na **uživatele**.
5.  V dolní části stránky klikněte na **Spravovat Auth Multi-Factor**.
![Klikněte na adresář](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Tím se otevře na nové záložce prohlížeče.  Najděte uživatele, který chcete zakázat. Budete muset změnit zobrazení nahoře. Zajistěte, aby byl stav buď **povoleno** nebo **nevynucují.**
7.  Zaškrtnutím **zaškrtněte** políčko vedle jeho jména.
7.  Na pravé straně klikněte na tlačítko **Zakázat**.
![Zakázat uživatele](./media/multi-factor-authentication-get-started-user-states/userstate2.png)
8.  Zobrazí se výzva k potvrzení to.  Klikněte na **Ano**.
![Zakázat uživatele](./media/multi-factor-authentication-get-started-user-states/userstate3.png)
9.  Potom byste měli vidět, že byl úspěšný.  Klikněte na **zavřete.** 
 ![Zakázat uživatele](./media/multi-factor-authentication-get-started-user-states/userstate4.png)
