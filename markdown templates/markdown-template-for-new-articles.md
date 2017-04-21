<properties
   pageTitle="Název stránky, která se zobrazí v prohlížeče karta a výsledky hledání"
   description="Článek popis, který se zobrazí na úvodní stránky a ve většině výsledků hledání"
   services="service-name"
   documentationCenter="dev-center-name"
   authors="GitHub-alias-of-only-one-author"
   manager="manager-alias"
   editor=""/>

<tags
   ms.service="required"
   ms.devlang="may be required"
   ms.topic="article"
   ms.tgt_pltfrm="may be required"
   ms.workload="required"
   ms.date="mm/dd/yyyy"
   ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

# <a name="markdown-template-article-heading-1-or-h1-heading-at-the-top-of-the-article"></a>Šablona markdown (článek Nadpis 1 nebo H1): Nadpis na začátku tohoto článku

Zkopírovat markdown pomocí této šablony, zkopírujte v článku ve vaší místní repo nebo klikněte na tlačítko nezpracovanými v uživatelském rozhraní GitHub a zkopírujte markdown.

  ![Alternativní text. není nechejte prázdné. Popis obrázku.][8]

Úvod k odstavce: Lorem dolor amet adipiscing elit. Phasellus interdum nulla risus lacinia porta nisl imperdiet sed. Mauris dolor mauris tempus sed lacinia nec, není felis euismod. NUNC semper porta ultrices. Maecenas neque nulla condimentum životopis s přehledem ipsum sednout amet dignissim aliquet nisi.

## <a name="heading-2-h2"></a>Nadpis 2 (H2)

Aenean sednout amet leo nec purus placerat fermentum ac gravida odio. Aenean tellus lectus, faucibus v rhoncus v faucibus sed urna.  volutpat mi id purus ultrices iaculis nec není neque. [Text odkazu pro odkaz mimo azure.microsoft.com](http://weblogs.asp.net/scottgu). Nullam dictum dolor na aliquam pharetra. Vivamus ac hendrerit mauris [Příklad text odkazu pro odkaz na článek ve složce služby](../articles/expressroute/expressroute-bandwidth-upgrade.md).

Získat další 10krát přenosy z [Google]  [ gog] než z [Yahoo]  [ yah] nebo [MSN] [msn].

> [AZURE.NOTE] Text s odsazením poznámky.  Během publikace se přidá na slovo "Poznámky". Ut eu pretium lacus. Nullam purus (normální), iaculis sed est vel, euismod vehicula odio. Curabitur lacinia erat tristique iaculis rutrum, erat sem sodales nisi, eu condimentum turpis nisi purus.

1. Aenean sednout amet leo nec **Purus** placerat fermentum ac gravida odio.

2. Aenean tellus lectus, faucius v **Rhoncus** v faucibus sed urna. Suspendisse volutpat mi id purus ultrices iaculis nec není neque.

    ![Alternativní text. není nechejte prázdné. Auto kolekcí v závodní červená.][5]

3. Nullam dictum dolor na aliquam pharetra. Vivamus ac hendrerit mauris. SED dolor dui condimentum a varius a vehicula na nisl.

    ![Alternativní text. Nenechávejte prázdné][6]


Suspendisse volutpat mi id purus ultrices iaculis nec není neque. Nullam dictum dolor na aliquam pharetra. Vivamus ac hendrerit mauris. Otrus informatus: [1 odkaz na další téma si přečtěte následující dokumentaci azure.microsoft.com](virtual-machines-windows-hero-tutorial.md)

## <a name="heading-h2"></a>Nadpis (H2)

Ut eu pretium lacus. Nullam purus (normální), iaculis sed est vel, euismod vehicula odio.

1. Curabitur lacinia erat tristique iaculis rutrum, erat sem sodales nisi, eu condimentum turpis nisi purus.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:
        (NSDictionary *)launchOptions
        {
            // Register for remote notifications
            [[UIApplication sharedApplication] registerForRemoteNotificationTypes:
            UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
            return YES;
        }

2. Vestibulum předchozí ipsum primis v faucibus orci luctus a ultrices posuere cubilia.

        // Because toast alerts don't work when the app is running, the app handles them.
        // This uses the userInfo in the payload to display a UIAlertView.
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
        (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
            [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
            @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }


    > [AZURE.NOTE] Duis sed diam není <i>nisl molestie</i> pharetra eget est. [Odkaz 2 na další téma si přečtěte následující dokumentaci azure.microsoft.com](web-sites-custom-domain-name.md)


Quisque commodo eros vel lectus euismod auctor eget sednout amet leo. Proin faucibus suscipit tellus dignissim ultrices.

## <a name="heading-2-h2"></a>Nadpis 2 (H2)

1. Maecenas sed condimentum nisi. Suspendisse potenti.

  + Fusce
  + Malesuada
  + Sem

2. Nullam v massa eu tellus tempus hendrerit.

    ![Alternativní text. není nechejte prázdné. Příklad video Channel 9.][7]

3. Quisque felis enim fermentum ut aliquam nec pellentesque pulvinaru magna.




<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky

Vestibul předchozí ipsum primis v faucibus orci luctus a ultrices posuere cubilia Curae; Nullam ultricies ipsum životopis s přehledem volutpat hendrerit purus diam pretium eros, životopis s přehledem tincidunt nulla lorem sed turpis: [3 odkaz na další téma si přečtěte následující dokumentaci azure.microsoft.com](storage-whatis-account.md).

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/        
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/    
