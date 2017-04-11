<properties
   pageTitle="Testování vaši nabídku datové služby pro Marketplace | Microsoft Azure"
   description="Pochopte, jak otestujte vaše nabídky datové služby Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="testing-your-data-service-offer-in-staging"></a>Testování datové služby nabídky v pracovní

>[AZURE.IMPORTANT] **V současné době jsme už rychlého připojení všechny nové vydavatelé datové služby. Nové dataservices nebude získat schváleno seznam.** Pokud máte s aplikací business SaaS chcete publikovat na AppSource můžete najít další informace [v tomto poli](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo vývojář služby rádi byste publikovat na webu Azure Marketplace můžete najít další informace [v tomto poli](https://azure.microsoft.com/marketplace/programs/certified/).

Po dokončení první dva kroky [Vytvoření účtu Microsoft pro vývojáře](marketplace-publishing-accounts-creation-registration.md) a [vytváření vaše Data Service nabízejí v portál publikování](marketplace-publishing-data-service-creation.md) jste připravení týkající se uplatňování vaši nabídku k dispozici v Azure Marketplace. Toto téma vás provede jednotlivými nejdřív intermediate krok s názvem "Pracovní"

Pracovní znamená, že nasazení nabídku vaší osobní "sandboxová", kde můžete otestovat a zkontrolovat jeho funkcí před jeho odesláním výroby. Nabídka se zobrazí v přípravu stejně jako u ji zákazníkovi, který má nasazení.

## <a name="step-1-pushing-your-offer-to-staging"></a>Krok 1. Předání vaši nabídku pracovní
Předání vaši nabídku pracovní umožňuje otestovat nabídky, než bude k dispozici pro budoucí předplatitele.  Můžete zjistit, jak bude vaši nabídku zobrazit a funkce pro můžou být přihlášení k odběru k datům.  

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1.  Přihlášení do [portál publikování](https://publish.windowsazure.com)
2.  V okně levém navigačním panelu vyberte **Datové služby**
3.  Vyberte svůj, který chcete použít pro pracovní. Zobrazí se výše uvedené obrazovce.
4.  Tlačítko **Posunout na pracovní** .  
5.  Pokud jsou problémy s nabídky potřebné k dokončení před předání do pracovní, zobrazí se seznam zobrazit.  Oprava tyto položky kliknutím na každou položku v seznamu. Při všechny provést změny, klikněte na tlačítko **použít pro pracovní** znovu.

Jestliže neexistují problémy s vaší nabídky zobrazí se okno místní.  

Pokud je máte není plánování/není oprávněn ukazovat vaši nabídku Azure portálu (aktuálně má omezenou kapacitu), jednoduše zavřete okno místní.

Chcete-li otestovat datové služby Azure portálu (kromě portálu DataMarket), budete potřebovat ID předplatného Azure k testování.  Toto ID předplatné bude identifikovat účet, který bude moct testovat vaši nabídku.  

Vyjmutí a vložení ID předplatného a klikněte na zaškrtnutí pokračovat.

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [AZURE.NOTE] Tyto Azure předplatné ID je pouze potřebných pro testování a přípravu na [Portálu Správa Azure](https://manage.windowsazure.com). Není nutné a otestujte v Azure Marketplace.

Na další obrazovce, která se zobrazí ukazuje, že se zobrazením na ikonu "Probíhá" Probíhá publikování zvýrazněn žlutá dole. Předání do pracovní trvá mezi 10 až 15 minut.  Pokud trvá déle, nejdřív aktualizujte si prohlížeč (Stisknutím F5 v aplikaci Internet Explorer).  Výjimečně místo, kam vaši nabídku pořád předání do pracovní za hodinu, klikněte na kontakt nám odkaz na dejte nám vědět, že je problém.

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Ikona "Probíhá" dokončení stisknutí na pracovní přesunutí zastavit a stav se aktualizují na "Fázovaná".  Teď jste připraveni k testování vaší nabídky.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Krok 2. Testování fázované nabídky v služby DataMarket

Klikněte na odkaz sledovat text **"V tématu služby nabízejí v..."** Chcete-li zobrazit na obrazovce, že účastníka se zobrazí při přechodu do výrobní vaší nabídky a zobrazí v části DataMarket.

  ![Kreslení](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Otestujte nebo ověřte všechny 12 položky označené nad zajistit všechny loga ceny/transakce, text, obrázky, si přečtěte následující dokumentaci, a odkazy, jsou správné a funkční správně.  Toto je včas zajistěte, aby všechny testovat hodnoty, které jste zadali při vytváření vaši nabídku nahradily správných hodnot.

1. Nabídka loga
2. Nabídka název
3. Publisher název nebo odkaz na web společnosti
4. Kategorie vyhledávání pro vaši nabídku
5. Odkaz na podporu vaši nabídku pomáhat účastníky
6. Kontextová popis vaší nabídky
7. Plán nabídka zobrazující podrobnostmi fakturace
8. Odkaz na implementace
9. Ukázkové obrázky, které ilustrují pomocí nabídky dat
10. Vstupní a výstupní metadat pro každou službu v rámci nabídky
11. Nabídky podmínky použití
12. Náhled dat nabídky


Nakonec zkontrolujte, zda funkčnosti službu se prostřednictvím Datamarket po kliknutí na odkaz "PROZKOUMÁNÍ tento datovou sadu".  Otevře se nové okno v nástroji název "Služby Průzkumníka", můžete zobrazit náhled výsledků dotazu proti služby.  V tomto okně můžete zadat potřebné parametry a zobrazte výsledky zobrazit dotaz na služby.   Zobrazí je také adresu URL pro váš dotaz.  

> [AZURE.NOTE] Ujistěte se, chcete-li zobrazit textový popis služby zobrazí v horní.  A pokud vaší nabídky se skládá z více služeb volání, klikněte na kartách v dolní části přepnutí na další službu zkontrolovat a otestovat.



## <a name="next-step"></a>Další krok
Pokud máte problémy a potřebujete pomoct s jejich řešení kontaktujte prosím [Podporu Publisheru Azure]( http://go.microsoft.com/fwlink/?LinkId=272975).

Pokud si nejste spokojení a je připraven k publikování vaši nabídku prosím dokumentaci [Požádat o schválení nabízená výroby](marketplace-publishing-push-to-production.md) .

## <a name="see-also"></a>Viz taky
- [Začínáme: Jak publikovat nabídka z Azure Marketplace](marketplace-publishing-getting-started.md)
