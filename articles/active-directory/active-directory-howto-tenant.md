<properties
    pageTitle="Jak získat tenanta Azure AD | Microsoft Azure"
    description="Jak získat tenanta služby Azure Active Directory pro registraci a vytváření aplikací."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# <a name="how-to-get-an-azure-active-directory-tenant"></a>Jak získat tenanta služby Azure Active Directory

V Azure Active Directory (Azure AD) je [klienta](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) příslušným zástupcem dané organizace.  Je vyhrazený instanci služby Azure AD, který organizaci přijímá a vlastní při zaregistruje pro cloudové službě Microsoftu, jako je Azure, Microsoft Intune nebo Office 365.  Každého klienta Azure AD je odlišné a odděleně od jiných Azure AD klienti.  

Ke klientovi máte uživatelům ve společnosti a informace o těchto - jejich hesla dat profilů uživatelů, oprávnění a tak dál.  Obsahuje také skupiny, aplikace a další informace týkající se organizace a zabezpečení.

Umožňuje uživatelům Azure AD přihlášení do aplikace musíte zaregistrovat aplikace v klientovi vlastní.  Publikování aplikace v klientovi Azure AD je **naprosto zadarmo**.  Ve skutečnosti Většina vývojářů vytvoří několika klientů a žádosti o pokusů, vývoj, přípravu a testování.  Organizacím, které si zaregistrovat a používání aplikace můžete volitelně Nákup licencí, pokud chtějí využít funkce Upřesnit adresář.

Ano jak přejdete získání Azure AD klienta?  Proces může být malé různých if můžete:

- [Máte stávajícímu předplatnému Office 365](#use-an-existing-office-365-subscription)
- [Máte předplatné existující Azure přidružené k Account společnosti Microsoft](#use-an-msa-azure-subscription)
- [Máte předplatné existující Azure přidružený k účtu organizace](#use-an-organizational-azure-subscription)
- [Žádný z výše uvedených možností a chcete začít úplně od začátku](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Použití stávajícímu předplatnému Office 365
Pokud máte stávajícímu předplatnému Office 365, máte už tenanta Azure AD! Můžete přihlásit k [portálu Azure](https://portal.azure.com) pomocí svého účtu O365 a začněte používat Azure AD.

## <a name="use-an-msa-azure-subscription"></a>Použití předplatné MSA Azure
Pokud jste dříve registraci předplatné Azure Account jednotlivé Microsoft, máte už klienta, kterého!  Při přihlášení k [Portálu Azure](https://portal.azure.com)se bude automaticky být přihlášení ke tenantovi výchozí. Máte volno podle potřeby - použití tohoto klienta, ale můžete chtít vytvořit účet organizace správce.

K tomu, postupujte takto:  Můžete taky můžete chtít vytvořit nového klienta a jejich vytváření správce v tomuto klientovi sledovat podobný proces.

1.  Přihlaste se k [Portálu Azure](https://portal.azure.com) jednotlivé účtem
2.  Přejděte do části "Azure Active Directory" portálu (nachází v levém navigačním panelu v části **Další služby**)
3.  Abyste by měl automaticky se přihlásit k "Výchozí adresáři", v opačném případě přejděte adresářů kliknutím na název účtu v pravém horním rohu.
4.  V oddílu **Rychlé úkoly** zvolte **Přidat uživatele**.
5.  Ve formuláři přidat uživatele zadejte tyto údaje:

    - Název: (vyberte hodnotu odpovídající)
    - Uživatelské jméno: (vyberte uživatelské jméno pro tohoto uživatele)
    - Profil: (zadejte příslušné hodnoty pro křestního jména, příjmení jméno, funkce a oddělení)
    - Role: Globální správce

6.  Pokud jste dokončili formulář přidat uživatele a přijímání dočasné heslo pro nové uživatele pro správu, je potřeba k zaznamenání toto heslo, jak budete muset přihlásit pomocí tomuto novému uživateli-li změnit heslo. Můžete taky poslat heslo přímo uživatele pomocí alternativní e-mailu.
7.  Klikněte na **vytvořit** nového uživatele.
8.  Chcete-li změnit dočasné heslo, přihlaste se k [https://login.microsoftonline.com](https://login.microsoftonline.com) s Tento nový uživatelský účet a změnit heslo při požadavku na.


## <a name="use-an-organizational-azure-subscription"></a>Použití předplatné organizační Azure
Pokud jste dříve registraci Azure předplatné s účtem organizace, máte už klienta, kterého!  Na [Portálu Azure](https://portal.azure.com)by měl nenajdete klienta při přechodu na "Další služby" a "Azure Active Directory."  Máte volno použití tohoto klienta podle potřeby. 


## <a name="start-from-scratch"></a>Začněte úplně od začátku
Pokud neužitečné vám stačí, nemusíte dělat starosti.  Jednoduše navštivte [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) zaregistrujte Azure s novou organizaci.  Po dokončení procesu, bude mít vlastní velmi Azure AD klienta se název domény, který jste zvolili během Odhlásit se.  Na [Portálu Azure](https://portal.azure.com)můžete najít vašeho klienta tak, že přejdete do "Služby Azure Active Directory" v levém horním nav.

Jako součást procesu registrace pro Azure bude požádáni o zadání podrobnosti platební karty.  Můžete přejít spolehlivé – můžete se nezapočítávají pro publikování aplikací v Azure AD nebo vytvoříte nové klienti.
