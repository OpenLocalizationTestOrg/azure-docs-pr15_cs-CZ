<properties
   pageTitle="Konfigurace ověřování pomocí webové služby Amazon | Microsoft Azure"
   description="Tento článek popisuje, jak vytvořit a ověřit AWS přihlašovací údaje pro runbooks v Azure automatizaci Správa AWS prostředků."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="ověřování awS konfigurace aws"/>
<tags
   ms.service="automation"
   ms.workload="tbd"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="09/12/2016"
   ms.author="magoedte"/>

# <a name="authenticate-runbooks-with-amazon-web-services"></a>Ověření Runbooks s Amazon webové služby
Automatizace běžných úkolů ke zdrojům v Amazon webové služby AWS lze provést pomocí automatizaci runbooks v Azure.  Automatizovat mnoho úkolů AWS pomocí automatizaci runbooks úplně stejně, jako je možné ke zdrojům v Azure.  Všechny, které je požadován dvěma způsoby:

* Předplatné AWS a sadu přihlašovacích údajů.  Konkrétně vaší AWS přístupové klávesy a tajné klíče.  Další informace najdete v článku [Pomocí přihlašovacích údajů AWS](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Azure předplatné a automatizaci účtu.  Další informace o nastavení účet Azure automatizaci najdete v článku [Konfigurace Azure jako účet spustit](../automation/automation-sec-configure-azure-runas-account.md).  

Ověření s AWS, je nutné zadat sadu přihlašovacích údajů AWS ověření vaší runbooks spuštění z Azure automatizaci. Pokud už máte nastavený účet automatizaci vytvořili a chcete použít k ověření s AWS, můžete postupujte podle pokynů v následující části.  Pokud budete chtít snaží o účtu pro runbooks zaměřen AWS zdroje, by měl nejdřív vytvořit nový [účet automatizaci spustit jako](../automation/automation-sec-configure-azure-runas-account.md) (přeskočit možnost vytvořit hlavní název služby) a pak postupujte podle pokynů.

## <a name="configure-automation-account"></a>Konfigurace účtu automatizaci
Pro automatizaci Azure komunikovat s AWS bude musíte nejdřív načíst AWS pověření a byly ukládány jako aktiva v Azure automatizaci.  Následujících kroků uvedených v dokumentu AWS [Správa přístupových kláves z verze pro váš účet AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) vytvořte přístupové klávesy a zkopírujte **ID klíče přístup** a **Tajná přístupové klávesy** (Pokud chcete stáhnout klíčové souboru a uložte ho jinam, postupujte bezpečných).

Jste již vytvořili a zkopírovali klíče zabezpečení AWS, bude potřeba vytvořit pověření majetku s účtem Azure automatizaci bezpečně ukládány a odkazovat na ně s vaší runbooks.  Postupujte podle kroků uvedených v části **vytvoření nových přihlašovacích údajů** v článku [prostředky přihlašovacích údajů v Azure automatizaci](../automation/automation-certificates.md/###To create a new credential with the Azure portal) a zadejte následující informace:

1. Do pole **název** zadejte **AWScred** nebo hodnotu odpovídající po pojmenování standardy.  
2. Do pole **uživatelské jméno** zadejte svoje **ID přístup** a **Tajná přístupová klávesa** do pole **heslo** a **potvrďte heslo** .   

## <a name="next-steps"></a>Další kroky

- Reivew v článku řešení [Automatizace nasazení OM ve webových službách Amazon](../automation/automation-scenario-aws-deployment.md) se dozvíte, jak vytvořit runbooks k automatizaci úkolů v AWS.
