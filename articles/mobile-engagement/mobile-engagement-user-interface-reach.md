<properties 
   pageTitle="Azure mobilní zapojení uživatelské rozhraní - Reach" 
   description="Naučte se Kontaktujte uživatele aplikace s nabízená oznámení pomocí zapojení Mobile Azure" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>


# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a>Jak kontaktovat uživatelům aplikace s nabízená oznámení

Tento článek popisuje kartě **DOSÁHLA** portálu **Mobile Engagement** . Portál **Mobile zapojení** umožňuje sledovat a spravovat své mobilní aplikace. Všimněte si, že začít používat portál musíte nejdřív vytvořit účet **Azure Mobile Engagement** . Další informace najdete v tématu [Vytvoření účet Azure Mobile Engagement](mobile-engagement-create.md).

Části Reach uživatelského rozhraní je na nabízených kampaně nástroj pro správu kde můžete vytvořit nebo upravit/aktivovat/dokončit/monitor a získání statistických údajů o kampaní nabízená oznámení a funkce, které lze také k nim získat přístup prostřednictvím rozhraní API dosáhla (a některé prvky nízké nabízená rozhraní API). Mějte na paměti, jestli používáte rozhraní API nebo uživatelského rozhraní, budete muset integrace Azure Mobile zapojení a Reach do aplikace pro každou platformu s SDK před použitím dosáhla kampaní.

>[AZURE.NOTE] Mnoho oddíly portálu **Mobile zapojení** uživatelského rozhraní včetně tlačítka **Zobrazit NÁPOVĚDU** . Klepnutím na toto tlačítko zobrazíte další kontextové informace o oddílu.

## <a name="four-types-of-push-notifications"></a>Čtyři typy nabízená oznámení
1.    Oznámení – umožňují k odesílání zpráv reklamě uživatelům, které je přesměruje do jiného umístění v aplikaci nebo jim můžete poslat do webové stránky nebo úložiště mimo aplikaci. 
2.    Hlasování – umožňují shromážděte informace ze koncoví uživatelé tak, že je klást otázky.
3.    Posune dat – umožňuje odeslat datového souboru ve formátu Base 64 nebo binární. Informace obsažené v dat nabízených odeslaný do aplikace pro úpravy vašich uživatelů aktuální prostředí v aplikaci. Aplikace potřebujete zpracování dat v nabízených data.

## <a name="campaign-details"></a>Podrobnosti kampaně

Můžete upravit, kopírovat, odstranit nebo aktivovat kampaně, které nebyly dosud aktivováno tak, že umístíte ukazatel myši na jejich jméno nebo kterou můžete kliknout a ty si pak otevřít. Můžete vytvořit kopii kampaně, které již byly aktivovány tak, že umístíte ukazatel myši na jejich jméno nebo kterou můžete kliknout a ty si pak otevřít. Jakmile již aktivován však nelze změnit s kampaní.
 
![Reach1][18]

## <a name="reach-feedback"></a>Dosažení zpětné vazby

Klikněte na **Statistika** zobrazit podrobnosti o Reach kampaní. **Jednoduché** zobrazení obsahuje vizuální znázornění ve formě pruhového grafu sloupec o příčinách po aktivaci s kampaní. Zobrazení **Upřesnit** najdete podrobnější informace o kampaně připínáčku. Tyto údaje nebudou k dispozici při odesílání zkušební kampaně tedy nabízených poslané na test zařízení. Tady je, jak by měl interpretovat tyto podrobnosti:

1. **Pushed** - určuje počet zpráv posune zařízení. Toto číslo závisí na cílovou skupinu, které jste zadali při vytváření kampaně připínáčku. Pokud nezadáte všechny cílovou skupinu, potom tento nabízených bude být odesláno registrovaných zařízení. Jako všechny ostatní služby nabízených můžeme udělat není nabízená oznámení přímo do zařízení, ale místo toho použít pro platformu odpovídajících určité nabízená oznámení služby (PNS - APNS/GCM/WNS) tak, aby dodají oznámení na zařízeních. 

2.  **Dodáno** - určuje počet zpráv, které jsou úspěšně doručen PNS zařízení a potvrzeno jako přijatých SDK zapojení Mobile. 
        
    *Důvody pro dodáno spočítat, je menší než po počet:*
    
    1. Pokud uživatel odinstaloval aplikace ze zařízení, ale PNS poslal, neví, k němu v době, kdy chcete PNS odeslat push se nezobrazí zprávu.
    2. Pokud zařízení má aplikace, ale zařízení sami byly offline dlouhou dobu, PNS nebude možné zprávu doručit zařízení. 
    3. Pokud k zařízení doručení zprávy, ale SDK zapojení mobilní aplikaci nerozpozná obsah zprávy, pak vynechává zprávy. Zhroucení může dojít, pokud přizpůsobení oznámení v aplikaci vygeneruje výjimku, které jsme zachytit SDK a přetažení zprávu. Tato situace může nastat také Pokud aplikaci na zařízení je pomocí verze SDK zapojení Mobile, která není nerozumí novější verze technologie nabízená zprávy odeslané z platformu, ale toto je pouze v případě, že aplikace při upgradu po oznámení byla odeslána z platformy služby. Na kartě **Upřesnit** se dozvíte, jak velké množství zpráv vlastním vynechán. 
    4. Na zařízení s iOS zprávy někdy není doručen Pokud buď zařízení na zhoršeným battery nebo aplikaci spotřebovává značné množství power při zpracování vzdálené oznámení. Jedná se o omezení zařízení s iOS.   

3.  **Zobrazí se** – toto určuje počet zpráv, které jsou uvedeny úspěšně uživateli aplikace na zařízení ve formě oznámení nabízených/out z aplikace systému v Centru pro upozornění nebo oznámení o v aplikaci v mobilní aplikaci.  Na kartě **Upřesnit** se dozvíte, kolik byly upozornění systému a kolik byly v aplikaci oznámení. 
    
    *Důvody pro zobrazené spočítat, je menší než počet dodáno (čekání zobrazovat)*
    
    1. Pokud kampaně oznámení měli koncové datum v něm je možné, že oznámení byla Doručená, ale když čas na Otevřít a zobrazit uživateli aplikace dostali, ji už vypršelo tak, aby nikdy zobrazí.   
    2. Pokud je oznámení oznámení v aplikaci pak oznámení se zobrazí jenom, když uživatel aplikace otevře aplikace. V případech, kdy uživatel aplikace neotevřel aplikace bude v SDK hlásit, aby byl oznámení Doručená, ale ještě nejste nezobrazí, dokud opětovném otevření v aplikaci. 
    2. Je-li oznámení oznámení v aplikaci a nakonfigurován pro zobrazení na konkrétní aktivity/obrazovky a pak taky bude oznámení hlášené jako Doručená, ale ještě Doručená do otevření aplikace na určitou obrazovku. 
    
4.  **Interakce uživatelů** – určuje počet zpráv, které má uživatel aplikace serveru a bude obsahovat zprávy, které jsou actioned nebo ukončen. 

    - *Uživatel aplikace může akce oznámení některým z následujících způsobů:*
            
        1. Pokud oznámení oznámení systém/out z aplikace nebo oznámení o v aplikaci odeslané jen oznámení a aplikace uživatel klikne na oznámení.
        2. Pokud oznámení je v aplikaci oznámení s textu nebo zobrazení webového nebo hlasování aplikace uživatel klikne na tlačítko akce v oznámení.
        3. Pokud je oznámení v aplikaci oznámení s zobrazení webového pak aplikace uživatel klikne na adresu URL webu pouze v zobrazení [Android]
    
    - *Uživatel aplikace můžete opustit oznámení některým z následujících způsobů:*
    
        1. Kliknutím na tlačítko Zavřít oznámení o přímo. 
        2. Pryč prstem nebo odstranění oznámení. 
        3. V aplikaci oznámení s textem nebo webového obsahu a hlasování se obvykle zobrazují uživateli aplikace ve dvou krocích. Nejdřív vidí oznámení a když kliknou na něm, vidí další obsah textu/web/hlasování. Uživatel aplikace můžete opustit oznámení některým z těchto kroků a podrobností v zobrazení Upřesnit zaznamenává to. 

5.  **Actioned** - určuje počet zpráv, které byly explicitně actioned uživatelem aplikace. Toto je číslo zajímavé, jak to ukazuje počet uživatelů, kteří aplikace byly zajímají o zprávu, kterou použiti v oznámení. 
 
> [AZURE.NOTE] V iOS a oken otevřete platformy, pokud má uživatel aplikace a kampaně byl kampaně "Kdykoli" a je možné, že i mimo aplikaci a v aplikaci oznámení jsou zobrazeny ve stejnou dobu. Zobrazí počet vyššími, než dodáno může dojít. Pokud uživatel pracuje nebo akce oznámení a potom i počet uživatelů interakcí/Actioned může být větší než dodáno. 


![Reach2][19]

## <a name="see-also"></a>Viz taky

- [Koncepty][Link 6]
- [Odstraňování potíží se službou Průvodce][Link 24]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
