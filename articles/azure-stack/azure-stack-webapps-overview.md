<properties
    pageTitle="Azure zásobníku webové aplikace přehled | Microsoft Azure"
    description="Přehled webových aplikací Web Apps v Azure zásobníku"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="azure-stack-web-apps-overview"></a>Přehled webových aplikací Azure zásobníku
    
> [AZURE.NOTE] Tyto informace platí jenom pro nasazení TP1 zásobníku Azure.

Azure zásobníku Web Apps je první prvek nabídky aplikace služby Azure přenesený do zásobníku Azure. Instalační služba Azure zásobníku Web Apps se vytvořit instance všech pět typů povinných rolí a bude tvořit souborovém serveru. Přestože můžete přidat další instance u jednotlivých typů role, mějte na paměti, že není zbývajícího prostoru pro VMs v Technical Preview 1. Aktuální funkce Azure zásobníku webových aplikací jsou primárně foundation možnosti, které jsou potřebné pro správu webové aplikace systému a Host (hostitel).

![Azure zásobníku služba aplikací Web Apps v Azure poskládat portálu][1]

## <a name="limitations-of-the-technical-preview"></a>Omezení Technical Preview

Nejsou podporovány náhled verzích aplikace služby Azure vrstvě. V této verzi preview, nevkládejte pracovního vytížení výroby. Je také bez upgrade mezi verzemi náhled Azure zásobníku aplikaci služby. Primární účely tyto verze preview je zobrazit co společnost Microsoft poskytuje a získat názory. 

## <a name="what-is-an-app-service-plan"></a>Co je to služba aplikace plán?

Zprostředkovatel zdroje Azure zásobníku Web Apps používá stejnou kód, který používá funkci Azure Web Apps v aplikaci služby Azure. Některé běžné koncepty v důsledku toho stojí popisující. Ve webových aplikacích ceny kontejner pro webové aplikace se nazývá plán služeb aplikací. Představuje sady vyhrazené virtuálních počítačích používá k ukládání aplikace. V rámci dané předplatné může mít více plány aplikaci služby. To platí také v Azure zásobníku Web Apps. 

V Azure, což jsou sdílené a vyhrazené zaměstnanců. Sdílený pracovní podporuje hostingu HD víceklientské web app a je pouze jednu sadu sdílené dalšími lidmi. Vyhrazené servery pouze používá jednoho klienta a použít tři formáty: malý, střední a velká. Potřeb místní zákazníci nelze vždy popsat pomocí těchto podmínek. V Azure zásobníku webových aplikací Web Apps správci poskytovatele zdrojů můžete definovat úrovní pracovníka, která má být k dispozici tak, aby se mají více sad sdílené dalšími lidmi nebo různé sady vyhrazené zaměstnanců podle jejich jedinečné potřeb pro hostování webů. Pomocí těchto definice osy pracovní, mohou pak definovat vlastní ceny skladové jednotky.

## <a name="portal-features"></a>Funkce portálu

Jak platí taky s back-end, Azure zásobníku Web Apps pomocí stejného uživatelského rozhraní, který používá Azure Web Apps. Některé funkce jsou zakázány a ještě nejsou funkční v Azure vrstvě. Je to proto Azure specifické očekávání nebo služby, které vyžadují tyto funkce nejsou zatím dostupné ve vrstvě Azure. 

## <a name="next-steps"></a>Další kroky

- [Než začnete s Azure zásobníku Web Apps](azure-stack-webapps-before-you-get-started.md)
- [Instalace zprostředkovatele webové aplikace prostředků](azure-stack-webapps-deploy.md)

Můžete taky vyzkoušet jiné [platformy jako služba (PaaS) služby](azure-stack-tools-paas-services.md), jako [zprostředkovatele prostředků serveru SQL Server](azure-stack-sql-rp-deploy-short.md) a [zprostředkovatele MySQL prostředků](azure-stack-mysql-rp-deploy-short.md).

<!--Image references-->
[1]: ./media/azure-stack-webapps-overview/AppService_Portal.png
