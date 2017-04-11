<properties
    pageTitle="Správa trezoru klíč Azure pomocí Azure automatizace | Microsoft Azure"
    description="Informace o použití služby Azure automatizaci ke správě Azure klíč trezoru."
    services="Key-Vault, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

#<a name="managing-azure-key-vault-using-azure-automation"></a>Správa trezoru klíč Azure pomocí automatizace Azure

Tato příručka, bude vám představí služby Azure automatizaci a jak ji můžete použít k zjednodušení správy klíčů a tajemství v Azure klíč trezoru.

## <a name="what-is-azure-automation"></a>Co je Azure automatizaci?

[Automatizace Azure](../automation/automation-intro.md) je služby Azure zjednodušení správy cloudu pomocí automatizaci procesů a konfigurace požadovaný stav. Automatické Azure, ručně, opakovat, dlouho probíhajících a chyby chybám úkoly můžete automatizovat větší spolehlivosti, efektivity a hodnotu time to pro vaši organizaci.

Azure automatizaci poskytuje modul spuštění vysoce spolehlivé, která je velmi dostupná pracovního postupu, který změní podle svých potřeb. V Azure automatizaci procesů můžete být vykazuje postavu ručně, systémů 3rd výrobců nebo plánované intervalech tak, aby úkoly z toho důvodu přesně potřeby.

Snížení režijních provozní a uvolnit tak IT a zaměstnance školy DevOps zaměření na práci, která přidá firmy hodnotu přesunutím úkoly správy cloudu, aby automaticky spouštět Azure automatizaci.


## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Jak lze Azure automatické plánování trezoru klíč Azure?

Klíč trezoru můžete spravovat v Azure automatizaci pomocí [AzureRM klíč trezoru rutiny] (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) a pro [Azure klasické klíč trezoru](https://msdn.microsoft.com/library/azure/dn868052.aspx). Modul Azure pro správu klasické trezoru klíč neexistuje automaticky v Azure automatizaci a [AzureRM KeyVault modul](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) můžete importovat do Azure automatizaci tak, aby bylo možné provádět řadu úkolů správy klíč trezoru v rámci služby. Můžete taky spárování těchto rutin v Azure automatizaci pomocí rutin pro další služby Azure, k automatizaci složitých úkolů přes Azure službami a 3 systémy stran.

Pomocí rutin Azure klíč trezoru může provádět tyto úkoly prvky: 

- Vytváření a konfigurace klíčových trezoru
- Možnost vytvořte nebo importujte klíč
- Vytvoření nebo aktualizace tajná
- Aktualizace atributy klíče
- Získání klíče nebo tajná
- Odstranění klíče nebo tajná

Tady je několik příkladů použití Powershellu ke správě trezoru klíč:  

* [Azure klíčové trezoru - krok za krokem](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Nastavení a konfiguraci Azure trezoru klíče](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)


## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy Azure automatizace a jak ho můžete použít ke správě trezoru klíč Azure, tyto odkazy vedou na další informace o automatizaci Azure.

* Najdete v článku [Začínáme výuková](../automation/automation-first-runbook-graphical.md)Azure automatizaci.
* V tématu [skriptů Powershellu trezoru klíč Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).
