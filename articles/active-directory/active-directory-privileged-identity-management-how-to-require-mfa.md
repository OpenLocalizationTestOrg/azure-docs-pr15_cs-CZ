<properties
   pageTitle="Jak vyžadují vícefaktorové ověřování | Microsoft Azure"
   description="Zjistěte, jak má požadovat vícefaktorové ověřování (MFA) privilegovaných identit s Azure Active Directory privilegovaných Správa identit rozšíření."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="how-to-require-mfa-in-azure-ad-privileged-identity-management"></a>Jak vyžadují MFA správy identit Azure AD privilegovaných

Doporučujeme vyžadovat Multi-Factor authentication (MFA) pro všechny správce. Sníží riziko útok kvůli hostují hesla.

Můžete požadovat, aby uživatelé mohou vyplnit MFA výzva při přihlášení. Co je součástí předplatného Office a Azure s funkcemi obsažené v Microsoft Azure Vícefaktorové ověřování nabízení ve srovnání příspěvku na blogu [MFA pro Office 365 a MFA Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/) .

Můžete taky požadovat, aby uživatelé mohou vyplnit MFA výzva při aktivaci role v Azure AD správce osobních informací. Tímto způsobem, pokud uživatel neproběhla MFA výzvu, když jsou přihlášení, budou se výzva k tomu nevyzve tak, že správce osobních informací.

## <a name="requiring-mfa-in-azure-ad-privileged-identity-management"></a>Vyžadování MFA správy identit Azure AD oprávněními

Při vlastní správě identit v osobních jako privilegovaných roli správce, zobrazí se upozornění, které doporučit MFA privilegovaných účty. Klikněte na upozornění zabezpečení v řídicím panelu Správce osobních informací a otevře se nové zásuvné se seznamem, které by měl vyžadují MFA účtů správce.  Vyžadujete MFA výběr více rolí a potom klikněte na tlačítko **opravit** , nebo klikněte na tři tečky u jednotlivých rolích a potom klikněte na tlačítko **opravit** .

> [AZURE.IMPORTANT] Vpravo nyní Azure MFA pouze spolupracuje pracovní nebo školní účty, nikoli Microsoft účty (obvykle osobní účet, který se používá pro přihlášení ke službám Microsoft jako Skype Xbox, Outlook.com, atd.). Z toho důvodu kdokoliv pomocí účtu Microsoft nemůže být možný správce, protože MFA není možné používat k aktivaci jejich role. V případě tyto uživatele můžete pokračovat ve správě pracovního vytížení pomocí účtu Microsoft, zvýšit je trvalý správcům nyní.

Kromě toho můžete změnit MFA požadavku na určitou roli kliknutím v části role řídicího panelu Správce osobních informací. Klikněte na **Nastavení** v zásuvné roli a potom v části vícefaktorové ověřování vyberte možnost **Povolit** .

## <a name="how-azure-ad-pim-validates-mfa"></a>Jak Azure AD osobních ověří MFA

Ověřování MFA při aktivaci role dvěma způsoby.

Nejjednodušší možností je spolehnout Azure MFA pro uživatele, kteří jsou aktivace privilegovaných roli. K tomuto účelu nejdřív zkontrolujte, zda tito uživatelé mají licenci, podle potřeby a jsou registrované pro Azure MFA. Další informace o tom, jak to udělat je [Začínáme s Azure Multi-Factor Authentication v cloudu](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md). Je vhodné, ale není povinné, konfigurace Azure AD k jejímu vynucení MFA pro tyto uživatele, když se přihlaste. Je to proto kontroly MFA provádějí Azure AD správce osobních informací samotné.

Můžete taky uživatele ověřit místní můžete mít poskytovatele identity za MFA. Například pokud jste nakonfigurovali AD Federation Services vyžadovat ověřování na základě čipové karty před přístupem Azure AD [zabezpečení cloudové zdroje s Azure Vícefaktorové ověřování a služby AD FS](../multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud.md) obsahuje pokyny ke konfiguraci služby AD FS odeslat deklarací Azure AD. Při pokusu o aktivaci role, přijímá Azure AD správce osobních informací, že MFA už ověřena pro uživatele, jakmile obdrží odpovídající deklarace.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
