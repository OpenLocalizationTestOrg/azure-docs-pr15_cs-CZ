<properties
   pageTitle="Služby Azure struktury na Linux | Microsoft Azure"
   description="Služba struktury clusterů podporují Linux a Java, což znamená, že byste měli nasazení a hostitelské služby struktury aplikace napsané v Java a C# na Linux."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="SubramaR"/>

# <a name="service-fabric-on-linux"></a>Služba struktury na Linux

Náhled struktury služby na Linux umožňuje vytvářet, nasazení a správa vysoce dostupné důrazně scalable aplikace na Linux stejným způsobem jako v systému Windows. Předlohy služby struktury (spolehlivé služby a spolehlivé objekty actor) jsou dostupné v Java na Linux kromě C# (.NET Core).  Také je možné vytvářet [spustitelný služby hosta](service-fabric-deploy-existing-app.md) se jazyk nebo rámec. Náhled navíc podporuje orchestrating Docker kontejnery. Kontejnery docker by umožnit spuštění spustitelných Host nebo nativní služby struktury služby, které pomocí služby struktury rámce.

Služba struktury na Linux je ekvivalentní struktury služby v systému Windows (s výjimkou zvláštnosti OS a programování podpora jazyků). Proto většina naši [existující si přečtěte následující dokumentaci](http://aka.ms/servicefabricdocs) se vztahuje dokážete se seznámíte s technologií.

> [AZURE.VIDEO service-fabric-linux-preview]

## <a name="supported-operating-systems-and-programming-languages"></a>Podporované operační systémy a jazyky

Omezené preview podporuje vytváření clusterů vývoj jedno pole kromě více počítačů clusterů v Azure systém 16.04 serveru se systémem Ubuntu. Náhled podporuje spolehlivé účastníky a rámce spolehlivé příslušnosti služby Java a C# kromě hosta spustitelných a orchestrating Docker kontejnery.  

>[AZURE.NOTE] Spolehlivé kolekce zatím nejsou podporované v Linux. Stát samotný clusterů nejsou podporované v náhledu jsou podporovány buď - jenom jedno pole a více počítačů clusterů Azure Linux.

## <a name="supported-tooling"></a>Podporované nástrojů

Náhled podporuje interakce s clusteru prostřednictvím rozhraní příkazového řádku Azure. Pro vývojáře Java integrace se službou zatmění a Yeoman je součástí zatmění podporované Linux a OSX. Integrace OSX používá Linux OM rozšířená prostřednictvím vagrant. Vývojářům C# integrace se službou Yeoman slouží k vytvoření šablony aplikace.

## <a name="next-steps"></a>Další kroky


1. Seznámení s [Spolehlivé účastníky](service-fabric-reliable-actors-introduction.md) a [Spolehlivé služby](service-fabric-reliable-services-introduction.md) programovací rámců.

2. [Příprava vývojové prostředí na Linux](service-fabric-get-started-linux.md)

3. [Příprava vývojové prostředí na OSX](service-fabric-get-started-mac.md)

4. [Vytvoření první aplikace služby struktury Java na Linux](service-fabric-create-your-first-linux-application-with-java.md)
