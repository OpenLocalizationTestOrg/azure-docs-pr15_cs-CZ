<properties 
    pageTitle="Základní informace o integraci podnikových | Služba Microsoft Azure aplikací | Microsoft Azure" 
    description="Použití funkcí podnikové integrace povolit obrázku a integrace scénáře pro firmy pomocí aplikace logiky" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-the-enterprise-integration-pack"></a>Základní informace o Enterprise Integration Pack

## <a name="what-is-the-enterprise-integration-pack"></a>Co je Enterprise integrace Pack?
Sada integrace organizace je cloudové řešení společnosti Microsoft Bezproblémová povolení komunikace business business (B2B). Sada používá standardních protokolů včetně [AS2](./app-service-logic-enterprise-integration-as2.md), [X12](./app-service-logic-enterprise-integration-x12.md)a [EDIFACT](./app-service-logic-enterprise-integration-edifact.md) na exchange zpráv mezi obchodními partnery. Zprávy můžete volitelně zabezpečit pomocí šifrování a digitální podpisy. 

Sada umožňuje organizacím, které používají různé protokoly a formáty vymění elektronicky pomocí transformace různé formáty do formátu, který systémy obě organizace může interpretovat a čeká na zpracování. 

Pokud znáte Admin.systému nebo Microsoft Azure BizTalk Services, najdete ji snadno se použije že podnikové integrace funkce vzhledem k tomu, že většina pojmy je podobný. Hlavní rozdíl je, že podnikové integrace používá integrace účty zjednodušit ukládání a správa artefaktů používán B2B komunikace. 

Architektonicky Enterprise integrace Pack vychází z **Integrace účty** , které obsahují všechny artefakty, které můžete použít k návrhu, nasazení a udržovat B2B aplikace. Integrace účet je v podstatě kontejneru cloudové kam ukládat artefakty například schémata partnerů, certifikáty, mapy a dohod. Tyto artefakty pak lze v aplikacích pro použití logických operátorů k vytvoření B2B pracovní postupy. Než budete moct použít artefaktům v aplikaci použití logických operátorů, budete muset propojení účtu integrace s aplikací použití logických operátorů. Po jejich propojením aplikace logiky bude mít přístup k účtu integrace artefakty.  

## <a name="why-should-you-use-enterprise-integration"></a>Proč byste měli použít integrace enterprise?
- Podnikové integrace se pro uložení všech artefakty na jednom místě, které je váš účet integrace. 
- Můžete využít modul logiku aplikace a její spojnice k vytváření pracovních postupů B2B a integrace se 3 aplikacích SaaS stran, místní aplikací, jakož i vlastní aplikace
- Můžete taky využít Azure funkcí

## <a name="how-to-get-started-with-enterprise-integration"></a>Jak začít s integrací enterprise?
Můžete vytvářet a spravovat aplikace B2B pomocí Enterprise Pack integrace prostřednictvím aplikace Návrhář logiky na **Azure portálu**.  

Můžete také [prostředí PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "logiky aplikace témata Powershellu") ke správě aplikace použití logických operátorů. 

Tady je přehled kroků budete muset udělat předtím, než budete moct vytvářet aplikace na portálu Azure: ![obrázek základní informace](./media/app-service-logic-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Jaké jsou některé běžné scénáře?

Podnikové integrace podporuje tyto oborové standardy:   

- Upravit - výměny elektronické dat  
- EAI - integraci podnikových aplikací  

## <a name="heres-what-you-need-to-get-started"></a>Tady je, musíte si nejdřív
- Azure předplatné s účtem integrace
- Visual Studio 2015 k vytvoření mapy a schémata
- [Integrace aplikace Enterprise logiky Microsoft Azure Tools for Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it"></a>Vyzkoušejte si to
Nasazení plně funkční výběru AS2 [vyzkoušet teď](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) odeslat a přijímat logiku aplikace využívající funkcemi B2B logiku aplikací.

## <a name="learn-more-about"></a>Další informace o:
- [Smlouvami] (./app-service-logic-enterprise-integration-agreements.md "Přečtěte si víc o podnikové integrace smlouvami")
- [Scénáře pro firmy na firmy (B2B)] (./app-service-logic-enterprise-integration-b2b.md "Naučte se vytvářet aplikace logika s funkcemi B2B")  
- [Certifikáty] (./app-service-logic-enterprise-integration-certificates.md "Přečtěte si víc o podnikové integrace certifikátů")
- [Kódování/dekódování plochému souboru] (./app-service-logic-enterprise-integration-flatfile.md "Zjistěte, jak kódování a dekódovat obsah plochému souboru")  
- [Integrace účty] (./app-service-logic-enterprise-integration-accounts.md "Přečtěte si víc o integration účty")
- [Mapy] (./app-service-logic-enterprise-integration-maps.md "Přečtěte si víc o enterprise integration mapy")
- [Partneři] (./app-service-logic-enterprise-integration-partners.md "Přečtěte si víc o podnikové integrace partnery")
- [Schémata] (./app-service-logic-enterprise-integration-schemas.md "Přečtěte si víc o schémata podnikové integrace")
- [Ověření zprávy XML] (./app-service-logic-enterprise-integration-xml.md "Zjistěte, jak ověřit zprávy XML s aplikacemi jiných logiky")
- [Transformace XML] (./app-service-logic-enterprise-integration-transform.md "Přečtěte si víc o enterprise integration mapy")
- [Podnikové integrace spojnic] (../connectors/apis-list.md "Přečtěte si víc o podnikové integrace pack spojnic")



