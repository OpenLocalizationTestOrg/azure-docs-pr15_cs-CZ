<properties
   pageTitle="Jak získat AppSource certifikované pro službu Azure Active Directory | Microsoft Azure"
   description="Podrobnosti o tom, jak získat aplikaci AppSource certifikované pro službu Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/28/2016"
   ms.author="skwan;bryanla"/>

#<a name="how-to-get-appsource-certified-for-azure-active-directory-ad"></a>Jak získat AppSource certifikované pro Azure Active Directory (AD) 

Pro příjem AppSource certifikační pro Azure AD aplikace musíte implementovat znaménko více klienta v vzorek s Azure AD pomocí protokolů OpenID připojení, OAuth 2.0 nebo SAML 2.0. 

Pokud znáte není Azure AD přihlásit nebo vývoj aplikací více klienta:

1. Začněte tím, že čtení o [Prohlížeč scénáře v prohlížeči v ověřování scénáře Azure AD][AAD-Auth-Scenarios-Browser-To-WebApp].  
2. Další, podívejte se na Azure AD [webové aplikace rychlého provede][AAD-QuickStart-Web-Apps], které ukazují, jak implementovat přihlašovací a zahrnout ukázky Průvodce vyhledáváním. 

    > [AZURE.TIP] Vyzkoušejte předběžnou verzi našeho nového [portál pro vývojáře](https://identity.microsoft.com/Docs/Web) , které vám pomůžou do začátků s Azure Active Directory za několik okamžiků!  Portálu pro vývojáře vás provede jednotlivými procesu registrace aplikace a integrace Azure AD do kódu.  Když budete mít hotové, bude mít jednoduchou aplikaci, která může ověřovat uživatele do vašeho tenanta a back-end, které můžete přijmout tokeny a ověření.

3. Zjistěte, jak implementovat více klienta přihlašovací vzorek Azure AD, najdete v tématech [jak se přihlásit všichni uživatelé Azure Active Directory (AD) pomocí aplikace více klienta vzorku][AAD-Howto-Multitenant-Overview]

## <a name="related-content"></a>Související obsah
Další informace o vytváření aplikací, které podporují Azure AD přihlásit nebo získat nápovědu a podporu, najdete pod odkazy [Azure AD Developer Guide][AAD-Dev-Guide].

Pomocí oddílu komentáře Disqus sledovat v tomto článku poskytnutí zpětné vazby a Pomozte nám vylepšit a přizpůsobovat naší obsah.

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#web-application-quick-start-guides


<!--Image references-->










