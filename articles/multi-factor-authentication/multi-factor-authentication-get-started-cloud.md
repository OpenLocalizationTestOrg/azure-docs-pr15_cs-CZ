<properties
    pageTitle="Získání v cloudu spuštěná Azure MFA | Microsoft Azure"
    description="Tohle je stránka Microsoft Azure Multi-Factor ověřování, který popisuje, jak začít s Azure MFA v cloudu."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Začínáme s Azure Vícefaktorové ověřování v cloudu
Tento článek provede jak začít používat Azure Vícefaktorové ověřování v cloudu.

> [AZURE.NOTE]  Následující dokumentaci obsahuje informace o tom, jak umožnit uživatelům pomocí **Klasické portál Azure**. Pokud hledáte informace o tom, jak nastavit Azure Vícefaktorové ověřování pro O365 uživatele, přečtěte si článek [Nastavení vícefaktorové ověřování pro Office 365.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MFA v cloudu](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisites"></a>Zjistit předpoklady pro
Dříve než povolíte Azure Vícefaktorové ověřování pro uživatele jsou požadovány následující požadavky.


1. [Registrace pro předplatné Azure](https://azure.microsoft.com/pricing/free-trial/) – Pokud už nemáte předplatné Azure, budete muset registrace pro jednu. Pokud jste právě spuštění a pomocí Azure MFA můžete zkušební předplatné
2. [Vytvoření zprostředkovatele Auth Multi-Factor](multi-factor-authentication-get-started-auth-provider.md) a přiřadit ji někomu adresáři nebo [přiřazovat licence uživateli](multi-factor-authentication-get-started-assign-licenses.md)

> [AZURE.NOTE]  Licence jsou dostupné pro uživatele, kteří mají Azure MFA, Azure AD Premium nebo Enterprise mobilita sadu EMS ().  MFA je součástí Azure AD Premium a EMS. Pokud máte dost licencí, nepotřebujete vytvořil poskytovatel Auth.


## <a name="turn-on-two-step-verification-for-users"></a>Zapnutí dvoustupňového ověření pro uživatele
Vyžadování dvě počáteční ověření na pro uživatele zahájíte změňte o stavu uživatele z zakázáno aktivní.  Další informace o uživateli v USA najdete v článku [Států uživatele v Azure Vícefaktorové ověřování](multi-factor-authentication-get-started-user-states.md)

Chcete-li povolit MFA pro uživatele pomocí následujícího postupu.

### <a name="to-turn-on-multi-factor-authentication"></a>Pokud chcete vypnout vícefaktorové ověřování

1.  Přihlaste se k [Azure klasické portal](https://manage.windowsazure.com) jako správce.
2.  Na levé straně klikněte na **Služby Active Directory**.
3.  V části adresáře vyberte adresář pro uživatele, kterého chcete povolit.
![Klikněte na adresář](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Nahoře klikněte na **uživatele**.
5.  V dolní části stránky klikněte na **Spravovat Auth Multi-Factor**. Otevře na nové záložce prohlížeče.
![Klikněte na adresář](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Najděte uživatele, který chcete povolit pro dvoustupňového ověření. Budete muset změnit zobrazení nahoře. Zajistěte, aby byl stav **zakázané.** 
 ![Povolit uživateli](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  Zaškrtnutím **zaškrtněte** políčko vedle jeho jména.
7.  Na pravé straně klikněte na tlačítko **Povolit**.
![Povolení uživatele](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Zaškrtněte políčko **Povolit Multi-Factor auth**.
![Povolení uživatele](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Všimněte si, že o stavu uživatele změnil z **Zakázané** **aktivní**.
![Povolení uživatelům](./media/multi-factor-authentication-get-started-cloud/user.png)

Po povolení uživatelům oznamte je prostřednictvím e-mailu. Při příštím pokusu o přihlášení, budou budete vyzváni k zápisu jejich účtu pro dvoustupňového ověření. Po zahájení používání dvoustupňového ověření, budete taky potřebovat nastavit aplikace hesla Chcete-li předejít uzamčení mimo jiné prohlížeče.


## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>Použití Powershellu ke automatizovat zapnutí dvoustupňového ověření

Pokud chcete změnit [Stav](multi-factor-authentication-whats-next.md) pomocí [Prostředí PowerShell Azure AD](../powershell-install-configure.md), můžete takto.  Můžete změnit `$st.State` tak, aby odpovídal jednu z následujících stavů:

- Povoleno
- Vynucení
- Vypnutí  

> [AZURE.IMPORTANT]  Jsme zamezilo proti přesunutí uživatelů přímo ze státu zakázat stavu vynucené. Nejsou webovými aplikacemi přestanou se vám práce, protože uživateli nebyl pryč prostřednictvím MFA zápisu a získat [aplikaci heslo](multi-factor-authentication-whats-next.md#app-passwords). Pokud máte jiné webovými aplikacemi a vyžadovala hesla aplikace, doporučujeme přejít z stavu zakázané povoleno. To umožňuje zaregistrovat a získat aplikaci hesla. Až to můžete přesouvat vynucené.

Pomocí prostředí PowerShell by možnost pro hromadné umožňují uživatelům. Aktuálně existuje žádná funkce povolit hromadné Azure portálu a musíte vybrat každý uživatel jednotlivě. Pokud máte většího počtu uživatelů to může být docela úkolu. Vytvořením prostředí PowerShell skriptů pomocí následujícího, je projděte seznam uživatelů a povolíte.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Tady je příklad:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Další informace najdete v tématu [států uživatele v Azure Vícefaktorové ověřování](multi-factor-authentication-get-started-user-states.md)

## <a name="next-steps"></a>Další kroky
Teď, když nastavíte Azure Vícefaktorové ověřování v cloudu, můžete nakonfigurovat a nastavit nasazení. Další informace najdete v článku [Konfigurace Azure Vícefaktorové ověřování](multi-factor-authentication-whats-next.md) .
