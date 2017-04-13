<properties 
    pageTitle="Služby Bus a tak mohli ověřovat | Microsoft Azure"
    description="Základní informace o ověřování sdílené přidružení (zabezpečení aplikace Access podpis)."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="service-bus-authentication-and-authorization"></a>Služba Bus a tak mohli ověřovat

Aplikace může ověřovat pomocí některého z přidružení sdílené podpis aplikace Access (zabezpečení) ověřování Bus služby Azure nebo prostřednictvím Azure Active Directory řízení přístupu (označovaná taky jako služba Řízení přístupu nebo ACS). Sdílené aplikace umožňuje ověřování přístup podpis ověření služby Bus pomocí přístupové klávesy nakonfigurovaný na obor nebo entity, ke kterému jsou určitá práva přidružené. Tento klíč pak můžete použít ke generování token sdílí přístup podpis, který klienti můžou pomocí ověřování Bus služby.

>AZURE. Přidružení důležité zabezpečení se doporučuje přes ACS, protože poskytuje jednoduché flexibilní a snadno použitelné ověřování schématu pro Bus služby. Aplikace můžete použít přidružení zabezpečení v situacích, kdy se nemusí ke správě pojmu oprávněnými "uživatelem."

## <a name="shared-access-signature-authentication"></a>Sdílené ověřování přístup podpisu

[Přidružení zabezpečení ověřování](service-bus-sas-overview.md) umožňuje udělit uživatelům přístup k služby Bus zdroje s konkrétní rights. Ověřování přidružení zabezpečení v služby Bus zahrnuje konfigurace šifrovací klíč právy přidružené k služby Bus zdroje. Klienti potom získat přístup k tomuto prostředku prezentace token přidružení zabezpečení, které sestává z identifikátor URI přistupuje a platnosti podepsané klávesu nakonfigurované.

Klávesy pro přidružení zabezpečení můžete nakonfigurovat pro obor Bus služby. Klíč platí pro všechny zpráv entity v této názvů. Můžete taky nakonfigurovat klíče na služby Bus fronty a témata. Přidružení zabezpečení podporuje i na Bus služba přenáší.

Použití přidružení zabezpečení, můžete nakonfigurovat objekt [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) na obor názvů, fronty nebo tématu, které se skládá z následujících akcí:

- *Název_klíče* , který identifikuje pravidlo.

- *Vyhovoval* je šifrovací klíč používaný k znaménko/ověření tokeny přidružení zabezpečení.

- *Sekundární klíč* je šifrovací klíč používaný k znaménko/ověření tokeny přidružení zabezpečení.

- *Práva* představující kolekce poslech, odeslat nebo spravovat oprávnění udělena.

Ověřovací pravidla nakonfigurovaný na úrovni názvů můžete udělit přístup k všechny entity v oboru pro klienty tokeny podepsaných pomocí odpovídající klíče. Až 12 povolení je možné konfigurovat pravidla na služby Bus názvů, fronty nebo tématu. Ve výchozím nastavení je [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) se všemi právy nakonfigurován pro každý názvů při prvním zřizování.

Přístup k entitu, klientský počítač vyžaduje token přidružení zabezpečení generovaný pomocí konkrétní [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx). Přidružení zabezpečení token generováno SHA256 HMAC zdroje řetězcem, který se skládá z URI zdroje, ke které je požadován přístup a platnosti pomocí šifrovací klíč přidružený k ověřovací pravidlo.

Přidružení zabezpečení ověřování podpora služby Bus je součástí Azure .NET SDK verze 2.0 nebo novějším. Přidružení zabezpečení podporuje [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx). Všechna rozhraní API, které podporují připojovací řetězec jako parametr zahrnují podporu přidružení zabezpečení připojovací řetězec.

## <a name="acs-authentication"></a>ACS ověřování

Ověřování Bus služby prostřednictvím ACS se spravuje přes doplňkovém "-sb" ACS názvů. Pokud budete potřebovat obor companion ACS vytvořit obor Bus služby, nelze vytvořit obor názvů Bus služby na portálu Azure classic; obor názvů pomocí rutiny prostředí PowerShell [New-AzureSBNamespace](https://msdn.microsoft.com/library/azure/dn495165.aspx) musí vytvořit. Příklad:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $true
```

Pokud chcete nevytvářejte ACS názvů, zadejte tento příkaz:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $false
```

Například pokud vytváříte služby Bus obor názvů s názvem **contoso.servicebus.windows.net**obor názvů ACS Průvodce vyhledáváním s názvem **contoso sb.accesscontrol.windows.net** máte k dispozici automaticky. Pro všechny obory názvů, které byly vytvořené před srpen 2014 vytvořil také doprovodnou ACS názvů.

Výchozí identity služby "vlastníka," se všemi právy, máte k dispozici ve výchozím nastavení v této názvů ACS Průvodce vyhledáváním. Získat jemně odstupňovaná ovládací prvek na jiné služby Bus prostřednictvím ACS nakonfigurováním odpovídající zabezpečení relace. Můžete nakonfigurovat další služby identit Správa přístupu k služby Bus entity.

Přístup k entitu, klient požaduje SWT token z ACS s odpovídající deklarace prezentuje své přihlašovací údaje. SWT token musí potom posílat jako součást žádosti o služby Bus k povolení autorizace klienta pro přístup ke entity.

ACS ověřování podpora služby Bus je součástí Azure .NET SDK verze 2.0 nebo novějším. Toto ověření podporuje [SharedSecretTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedsecrettokenprovider.aspx). Všechna rozhraní API, které podporují připojovací řetězec jako parametr zahrnují podporu ACS připojovací řetězec.

## <a name="next-steps"></a>Další kroky

Pokračujte ve čtení [sdílí podpis přístup ověřování pomocí služby Bus](service-bus-shared-access-signature-authentication.md) další podrobnosti o přidružení zabezpečení.

Všeobecný přehled přidružení v Bus služby najdete v článku [Sdílí podpisy přístup](service-bus-sas-overview.md).

Můžete najít další informace o ACS tokenů [jak: požádat o Token ACS prostřednictvím protokolu ZALOMIT OAuth](https://msdn.microsoft.com/library/hh674475.aspx).



