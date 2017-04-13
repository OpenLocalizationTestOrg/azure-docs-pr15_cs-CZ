<properties
   pageTitle="Konfigurace metodu směrování přenosu přenosy Správce převzetí | Microsoft Azure"
   description="Tento článek vám pomůže konfigurace metodu směrování přenosu převzetí ve Správci přenosy"
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-failover-routing-method"></a>Konfigurace metodu směrování překlopení

Často organizace chce poskytnout spolehlivosti pro své služby. Je to poskytují záložní služby v případě, že jejich primární služby havaruje. Běžné vzor pro službu převzetí je poskytují sadu stejné služby a odešlete přenosy primární služby a zachovat nakonfigurované seznam jedné nebo víc záložní služeb. Tento typ zálohování můžete nakonfigurovat Azure cloudovými službami a weby podle níže uvedeného postupu.

Všimněte si, že Azure weby již obsahuje převzetí přenosy směrování metoda funkcí pro weby v rámci datacentra (neboli oblast), bez ohledu na režim webu. Přenosy správce vám umožní určit převzetí metodu směrování přenosu weby v jiném datacentrech.

## <a name="to-configure-failover-traffic-routing-method"></a>Konfigurace metodu směrování přenosu převzetí:

1. Azure klasické portálu v levém podokně klikněte na ikonu **Přenosy správce** otevřete podokno přenosy správce. Pokud jste ještě nevytvořili svůj profil správce přenosy, najdete v článku [Správa profilů přenosy správce](traffic-manager-manage-profiles.md) postup se dá vytvořit základní profil správce dopravy.
2. V podokně přenosy správce na portálu Azure klasické vyhledejte přenosy správce profil, který obsahuje nastavení, které chcete upravit a potom klikněte na šipku vpravo od názvu profilu. Tím se otevře stránka nastavení profilu.
3. Na stránce vašeho profilu klikněte na **koncové body** v horní části stránky a ověřte, že i cloudových služeb a weby (koncové body), které chcete zahrnout do vaší konfigurace jsou k dispozici. Postup přidání nebo odebrání koncové body najdete v článku [Správa koncové body ve Správci přenosy](traffic-manager-endpoints.md).
4. Na stránce vašeho profilu klikněte na **Konfigurovat** nahoře a otevře se stránka konfigurace.
5. **Způsob nastavení směrování přenosu**zkontrolujte, že metoda směrování přenosu **překlopení**. Pokud není, klikněte v rozevíracím seznamu **překlopení** .
6. **Seznamu priorita převzetí**upravte pořadí převzetí koncových bodů. Po výběru metodu směrování přenosu **převzetí** záleží na pořadí vybraných koncové body. Primární koncový bod je zcela nahoře. Nahoru nebo přesunout dolů změňte pořadí podle potřeby. Další informace o tom, jak nastavte prioritu převzetí pomocí Windows Powershellu najdete v článku [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Ověřte, že je **Sledování nastavení** nakonfigurované řádně podporovat. Sledování zaručuje, že koncové body, které jsou v režimu offline neodesílají přenosy. Abyste mohli sledovat koncové body, zadejte cestu a název souboru. Všimněte si, že lomítka "/" je platnými položkami pro relativní cestu a znamená, že soubor databáze v kořenovém adresáři (výchozí). Další informace o sledování najdete v článku [Správce přenosy sledování](traffic-manager-monitoring.md).
8. Po dokončení změny konfigurace, klikněte na **Uložit** v dolní části stránky.
9. Otestujte změny v konfiguraci. Další informace najdete v článku [Testování nastavení přenosy správce](traffic-manager-testing-settings.md) .
10. Po nastavení a práce se svůj profil správce přenosy upravte záznam DNS na autoritativní server DNS tak, aby ukazovaly na název domény přenosy správce názvu domény společnosti. Další informace o tom, jak to udělat najdete v článku [čárky internetovou doménu společnosti přenosy správce domény](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Další kroky

[Internetovou doménu společnosti přejděte na správce přenos domény](traffic-manager-point-internet-domain.md)

[Metody směrování přenosu správce](traffic-manager-routing-methods.md)

[Konfigurace metodu směrování kruhového](traffic-manager-configure-round-robin-routing-method.md)

[Konfigurace metodu směrování výkonu](traffic-manager-configure-performance-routing-method.md)

[Poradce při potížích přenosy správce stavu sníženou kvalitu.](traffic-manager-troubleshooting-degraded.md)

[Vysílání povolit správce – zakázat, nebo odstranění profilu](disable-enable-or-delete-a-profile.md)

[Přenosy správce – zakázání nebo povolení koncový bod](disable-or-enable-an-endpoint.md)

