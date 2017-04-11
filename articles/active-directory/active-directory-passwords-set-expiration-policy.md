<properties
    pageTitle="Nastavení zásad vypršení platnosti hesla v Azure Active Directory | Microsoft Azure"
    description="Zjistěte, jak lze zkontrolovat zásad vypršení platnosti a změnit vypršení platnosti uživatelského hesla jednotlivě nebo hromadně Azure Active directory hesel"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="set-password-expiration-policies-in-azure-active-directory"></a>Nastavení zásad vypršení platnosti hesla v Azure Active Directory

> [AZURE.IMPORTANT] **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).

Jako globální správce pro cloudové službě společnosti Microsoft můžete modul služeb Microsoft Azure Active Directory pro Windows PowerShell pro nastavení uživatelská hesla není platnosti. Můžete také rutin prostředí Windows PowerShell odebrat nikdy-vypršení platnosti konfigurace, nebo pokud chcete zjistit, které uživatel hesla nastavená není na vypršení platnosti. Tento článek poskytuje pomoc pro cloudové služby, jako je Microsoft Intune a Office 365, které jsou závislé na Microsoft Azure Active Directory pro služby identit a adresář.

  > [AZURE.NOTE] Pouze hesla u uživatelských účtů, které nejsou synchronizované prostřednictvím synchronizace adresářů můžete nakonfigurován tak, aby platnost. Další informace o synchronizaci adresářů najdete v tématu seznam témat v [Přehled synchronizace adresáře](https://msdn.microsoft.com/library/azure/hh967642.aspx).

Pro používání rutin prostředí Windows PowerShell, nejprve nainstalujte je.

## <a name="what-do-you-want-to-do"></a>Co chcete udělat?

- [Jak se dá zjistit zásad vypršení platnosti hesla](#how-to-check-expiration-policy-for-a-password)

- [Nastavení omezené platnosti hesla](#set-a-password-to-expire)

- [Nastavení hesla tak, aby ho nevyprší](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>Jak se dá zjistit zásad vypršení platnosti hesla

1.  Připojte se k prostředí Windows PowerShell pomocí svých přihlašovacích údajů správce společnosti.

2.  Udělejte jednu z následujících akcí:

    - Pokud chcete zobrazit, jestli je nastavená Neomezená platnost hesla jednoho uživatele, spusťte následující rutinu pomocí hlavního názvu uživatele (UPN) (například aprilr@contoso.onmicrosoft.com) nebo ID uživatele, který chcete zkontrolovat:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - Pokud chcete zobrazit nastavení "Nikdy vypršení platnosti hesla" u všech uživatelů, spusťte následující rutinu:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>Nastavení omezené platnosti hesla

1.  Připojte se k prostředí Windows PowerShell pomocí svých přihlašovacích údajů správce společnosti.

2.  Udělejte jednu z následujících akcí:

    - Pokud chcete nastavit heslo jednoho uživatele tak, že vyprší platnost hesla, spusťte následující rutinu pomocí hlavního názvu uživatele (UPN) nebo ID tohoto uživatele:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`

    - Pokud chcete nastavit hesla všem uživatelům v organizaci tak, aby vyprší platnost, použijte následující rutinu:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>Nastavení neomezené platnosti hesla

1. Připojte se k prostředí Windows PowerShell pomocí svých přihlašovacích údajů správce společnosti.

2.  Udělejte jednu z následujících akcí:

    - Pokud chcete nastavit heslo jednoho uživatele neomezené platnosti, spusťte následující rutinu hlavní jméno uživatele (UPN) nebo ID tohoto uživatele:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`

    - Pokud chcete nastavit hesla všem uživatelům v organizaci neomezené platnosti, spusťte následující rutinu:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Další kroky

* **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).
