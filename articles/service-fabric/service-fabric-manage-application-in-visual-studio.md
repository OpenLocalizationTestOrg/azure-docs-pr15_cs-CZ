<properties
   pageTitle="Správa aplikací ve Visual Studiu | Microsoft Azure"
   description="Pomocí aplikace Visual Studio k vytváření, vývoj, balíček, nasazení a ladění struktury služeb aplikací a služeb."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/09/2016"
   ms.author="seanmck;mikhegn"/>

# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Pomocí aplikace Visual Studio zjednodušit psaní a Správa aplikací služby struktury

Můžete spravovat struktury služby Azure aplikace a služby prostřednictvím aplikace Visual Studio. Jakmile jste [nastavení vývojové prostředí](service-fabric-get-started.md), můžete vytvářet aplikace služby struktury, přidat služby nebo balíčku, přihlásit a nasazení aplikace v místním vývoj clusteru Visual Studio.

## <a name="deploy-your-service-fabric-application"></a>Nasazení aplikace služby struktury

Ve výchozím nastavení nasazení aplikace sloučí do jedné jednoduché operace podle těchto kroků:

1. Vytvoření balíčku aplikace
2. Nahrání balíčku aplikace na úložiště obrázek
3. Registrace typ aplikace
4. Odebrání některou spuštěné instance aplikace
5. Vytvoření nové instance aplikace

Ve Visual Studiu stisknutím klávesy **F5** bude taky nasazení aplikace a ladění ke všechny instance aplikace. **Ctrl + F5** slouží k nasazení aplikace bez ladění nebo publikováním na místní nebo vzdálené obrázku s použitím profilu publikovat. Další informace najdete v tématu [publikování aplikace vzdáleného clusteru pomocí aplikace Visual Studio](service-fabric-publish-app-remote-cluster.md).

### <a name="application-debug-mode"></a>Režim ladění aplikace

Ve výchozím nastavení Visual Studio odebere existující instance aplikace typu po ukončení ladění nebo (Pokud jste nainstalovali aplikace bez připojení ladění), kdy přeinstalujte aplikace. V takovém případě všechny aplikace je prázdná. Při ladění místně, můžete chtít zachovat data, která jste už vytvořili při testování novou verzi aplikace, chcete-li zachovat spuštění aplikace nebo chcete relací dalších ladění upgradovat aplikaci. Nástroje služby struktury aplikace Visual Studio poskytuje vlastnost s názvem **Režim ladění aplikace**, které ovládací prvky zda **F5** měli odinstalovat aplikaci a zachovat aplikace po relaci ladění končí nebo povolit aplikaci upgradovat na pozdější ladění relace, nikoli odebrat a znovu nasadit.

#### <a name="to-set-the-application-debug-mode-property"></a>Chcete-li nastavit vlastnost režim ladění aplikace

1. V místní nabídce Projekt aplikace vyberte **Vlastnosti** (nebo stisknutím klávesy **F4** ).
2. V okně **vlastností** nastavte vlastnost **Režim ladění aplikace** .

    ![Nastavte vlastnost režimu ladění aplikace][debugmodeproperty]

Toto jsou dostupné možnosti **Ladění režimu aplikace** .

1. **Automatické aktualizace**: aplikace pokračuje po ukončení relace ladění. Na další **F5** bude zacházet s nasazení jako upgrade s použitím sledován automatický režim rychle upgradovat na novější verzi aplikace řetězcem datum přidaným. Proces upgradu zachová všechna data, která jste zadali v relaci předchozí ladění.

2. **Zachovat aplikace**: aplikace pořád v clusteru po ukončení relace ladění. Na další **F5** aplikace odebrána a nově vytvořené aplikace má být nasazené na clusteru.

3. **Odebrání aplikace** způsobí, že aplikace odeberou, když skončí platnost relace ladění.

Pro **Automatické aktualizace** dat zachovají použitím funkce upgrade aplikace služby struktury, ale je nastaven na optimalizaci výkonu, nikoli zabezpečení. Další informace o upgradu aplikací a jak může provést upgrade v prostředí skutečné najdete v článku [upgrade služeb struktury aplikace](service-fabric-application-upgrade.md).

![Příklad novou verzi aplikace s přidaným kalendářních dat][preservedata]

>[AZURE.NOTE] Tato vlastnost neexistuje, před verze 1.1 struktury nástroje služby Visual Studio. Před 1.1 stiskněte klávesovou zkratku vlastnost **Zachovat Data na úvodní** dosáhnout ke stejnému chování. Možnost "Zachovat aplikace" byla zavedená ve verzi 1.2 struktury nástroje služby for Visual Studio.

## <a name="add-a-service-to-your-service-fabric-application"></a>Přidání služby aplikaci služby struktury

Přidání nové služby aplikaci rozšířit jeho funkcí.  Abyste měli jistotu, že služba je součástí balíčku aplikace, přidejte služby prostřednictvím položka nabídky **Nová služba struktury …** .

![Přidat novou službu struktury do aplikace][newservice]

Vyberte typ služby struktury projektu přidat aplikaci a zadejte název pro službu.  V tématu [Výběr rámec službě](service-fabric-choose-framework.md) týkající se rozhodnout, jaký typ služby používat.

![Vyberte typ služby struktury projektu přidat aplikaci][addserviceproject]

Nové služby se přidají do vašeho řešení a existující balíčku aplikace. Odkazy na služby a výchozí instanci služby se přidají do manifestu aplikace. Služba budou vytvořené a začít při příštím nasazení aplikace.

![Nové služby se přidají do vašeho manifestu aplikace][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Balíček aplikace služby struktury

Abyste mohli nasadit aplikaci a jeho služeb clusteru, potřebujete k vytvoření balíčku aplikace.  Balíček uspořádává manifest aplikace, manifest(s) služby a další potřebné soubory určitým způsobem.  Visual Studio nastavoval a spravoval balíček ve složce aplikace project v adresáři "pkg".  Kliknutím na **balíček** z místní nabídky **aplikace** vytvoří nebo aktualizuje balíček aplikace.  Můžete chtít udělat, pokud nasazení aplikace pomocí vlastních skriptů Powershellu.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Odebrání aplikace a typy aplikací pomocí Průzkumníka cloudu

Můžete provádět operace správy základní obrázku z aplikace Visual Studio pomocí Průzkumníka cloudu, které můžete spustit v nabídce **Zobrazit** . Můžete například odstraňování aplikací a zrušit zřízení typy aplikací na místní nebo vzdálené clusterů.

![Odebrání aplikace](./media/service-fabric-manage-application-in-visual-studio/removeapplication.png)

>[AZURE.TIP] Funkce správy bohatší clusteru najdete v článku [vizualizace svůj cluster v Průzkumníkovi struktury služby](service-fabric-visualizing-your-cluster.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Další kroky

- [Model služby struktury aplikace](service-fabric-application-model.md)
- [Nasazení aplikace služby struktury](service-fabric-deploy-remove-applications.md)
- [Správa parametry aplikace na více prostředí](service-fabric-manage-multiple-environment-app-configuration.md)
- [Ladění aplikace služby struktury](service-fabric-debugging-your-application.md)
- [Vizualizace svůj cluster v programu Průzkumník struktury služby](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[preservedata]:./media/service-fabric-manage-application-in-visual-studio/preservedata.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
