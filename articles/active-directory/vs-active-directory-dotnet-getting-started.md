<properties 
    pageTitle="Seznámení s Azure Active Directory a Visual Studia připojené služby (MVC projekty) | Microsoft Azure" 
    description="Jak začít používat služby Azure Active Directory v MVC projekty po připojení k nebo vytváření Azure AD pomocí aplikace Visual Studio připojené služby" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Začínáme s Azure Active Directory a Visual Studia připojené služby (MVC projektů)

> [AZURE.SELECTOR]
> - [Začínáme](vs-active-directory-dotnet-getting-started.md)
> - [Co se přihodilo](vs-active-directory-dotnet-what-happened.md)
 
##<a name="requiring-authentication-to-access-controllers"></a>Vyžadování ověřování pro přístup zařízení 

Všechny řadiče v projektu byly ozdobené s atributem **udělit oprávnění** . Tenhle atribut budou vyžadovat, aby uživatel ověřit před přístupem těchto zařízení. Umožňuje řadiče anonymní přístup k odebrání Tenhle atribut správce. Pokud chcete nastavit oprávnění na úrovni odstupňovaný, použijte atribut obou metod, která vyžaduje ověření místo použití u řadiče předmětu.
 
##<a name="adding-signin--signout-controls"></a>Přidání přihlašovacího / SignOut ovládacích prvků 

Pokud chcete přidat ovládací prvky přihlašovacího/SignOut do svého zobrazení můžete částečný pohled **_LoginPartial.cshtml** přidat funkci do jedné z vašich zobrazení. Tady je příklad funkčnosti přidaný do zobrazení standardních **_Layout.cshtml** . (Vezměte v úvahu poslední element v div se navigační panel sbalení třídy):

<pre>
    &lt;! DOCTYPE html&gt; 
&lt;html&gt; 
&lt;head&gt; 
&lt;meta charset="utf-8" /&gt; 
&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
&lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
@Styles.Render("~/Content/css") @Scripts.Render("~/bundles/modernizr") &lt;/head&gt; 
&lt;body&gt; 
&lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
&lt;div class="container"&gt; 
&lt;div class="navbar-header"&gt; 
&lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;/button&gt; 
@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) &lt;/div&gt; 
&lt;div class="navbar-collapse collapse"&gt; 
&lt;ul class="nav navbar-nav"&gt; 
&lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
&lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/ div&gt; 
&lt;/div&gt; 
&lt;/div&gt; 
&lt;div class="container body-content"&gt; 
@RenderBody() &lt;hr /&gt; 
&lt;footer&gt; 
&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
&lt;/footer&gt; 
&lt;/div&gt; 
@Scripts.Render("~/bundles/jquery") @Scripts.Render("~/bundles/bootstrap") @RenderSection("scripts", required: false) &lt;/body&gt; 
&lt;/html                                                                                                                                                                                                                                                                                                                                                                                                                                                           &gt;
</pre>

[Další informace o Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 
