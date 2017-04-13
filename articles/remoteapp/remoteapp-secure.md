
<properties
    pageTitle="Zabezpečení aplikace a prostředků v Azure RemoteApp | Microsoft Azure"
    description="Zjistěte, jak uzamknout aplikace a zdroje v Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Zabezpečení aplikace a prostředků v Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Azure RemoteApp umožňuje uživatelům přístup k Centrální správa přístupových práv aplikacím systému Windows, který umožňuje určit, co vaši uživatelé můžete a nemůžete dělat.  To je užitečné, když uživatel připojuje z nespravované zařízení (třeba osobní Macbooku) a chcete řízení přístupu uživatelů nebo dojít.

Například pokud používáte služby Active Directory pro ověřování uživatelů a chcete-li zabránit uživatelům v kopírování dat z aplikace, můžete nakonfigurovat vzdálené plochy zásad skupiny k blokování uživatelů v kopírování dat.

Jiný příklad se, jestli chcete blokovat přístup k Internetu pro určité aplikaci ve vaší kolekci. Můžete vytvořit pravidlo Brána Firewall systému Windows, které blokuje aplikace access při vytvoření obrázku ve vaší kolekci.

## <a name="implementation-options"></a>Možnosti implementace

  Tady jsou klíčové implementaci možnosti, které lze použít jednotlivě nebo ve spojení v případě potřeby:

1.  Pokud kolekci RemoteApp je doména připojen můžete vynutit libovolný [Zásad skupiny](https://technet.microsoft.com/library/cc725828.aspx) (s výjimkou nečinnosti a odpojit vypršení časového limitu zásady popsané [sem](../azure-subscription-service-limits.md)).
2.  Jako alternativu k zásad skupiny (Pokud kolekci není domény připojen nebo nemáte správné oprávnění ve službě Active Directory) můžete nakonfigurovat [Místní zásady](https://technet.microsoft.com/library/cc775702.aspx) do vaší šablony.  Poznámka: tuto skupinu zásady místní zásady trumfy po ke konfliktu.
3.  Některá nastavení OS/aplikace nejsou, která dokáže nahradit prostřednictvím zásad, ale může být prostřednictvím klíče registru pomocí [nástroje Editor](./remoteapp-hybridtrouble.md) při konfiguraci obrázek šablony.
4.  Můžete [Brána Firewall systému Windows](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) pro řízení přístupu k síti z počítače a pokud je spuštěná aplikace. Jenom zkontrolujte, jestli že nechcete blokovat adresy URL a porty definice tady.
5.  [Nástroje AppLocker](https://technet.microsoft.com/library/hh831440.aspx) slouží k určení, které aplikace a soubory uživatelů mohlo by umožnit spuštění. Uživatele můžete například zjistit, jak spouštění aplikací, že not publikovat, ale, které jsou k dispozici v obrázek, který jste použili k vytvoření kolekci - nástroje AppLocker můžete blokovat to.

## <a name="detailed-information"></a>Podrobné informace

- Tyto zásady služby Remote Data Service by mohly být nejvíc vyhovovat:
    - [Zařízení a prostředků](https://technet.microsoft.com/library/ee791794.aspx)
    - [Přesměrování tiskárny](https://technet.microsoft.com/library/ee791784.aspx)
    - [Profily](https://technet.microsoft.com/library/ee791865.aspx).
- Všimněte si, že konfigurace přesměrování pomocí prostředí RemoteApp PowerShell modul (jako příručky [tady](./remoteapp-redirection.md)) závisí na klientském počítači k jejímu vynucení zásady, takže je-li zabezpečení primární cíle je vhodné k jejímu vynucení zásady prostřednictvím místních zásad obrázek šablony nebo prostřednictvím zásad skupiny.
- [Windows serveru 2012 R2 zásady](https://technet.microsoft.com/library/hh831791.aspx).
- [Zásady Office 2013](https://technet.microsoft.com/library/cc178969.aspx) (včetně [Přizpůsobení panelu nástrojů Office](https://technet.microsoft.com/library/cc179143.aspx)).
