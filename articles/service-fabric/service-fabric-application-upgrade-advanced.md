<properties
   pageTitle="Upgrade aplikace: rozšířené témata | Microsoft Azure"
   description="Tento článek popisuje některé pokročilé témata vztahující se k upgradu služeb struktury aplikace."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="service-fabric-application-upgrade-advanced-topics"></a>Upgrade aplikace služby struktury: rozšířené témata

## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Přidání nebo odebrání služby během upgradu aplikace

Pokud do aplikace, která je už nasazeném a publikován jako upgrade se přidá nové služby, nová služba přibude nasazení aplikace.  Tyto upgrade nemá vliv na libovolnou z těchto služeb, které již byly část aplikace. Však musí být instanci služby, který byl přidaný spuštěna nové služby za aktivní (pomocí `New-ServiceFabricService` rutina).

Také je možné odebrat služby z aplikace v rámci upgradu. Však nutné všechny aktuální instance služby k-be odstraněné zastaveno před pokračováním upgrade (pomocí `Remove-ServiceFabricService` rutina). 

## <a name="manual-upgrade-mode"></a>Ruční režim upgradu

> [AZURE.NOTE]  Považovat za sledován ruční režim jen pro upgrade neúspěšných nebo pozastavené. Sledované režim je doporučený upgradu režim pro službu struktury aplikace.

Azure struktury služby obsahuje více upgradu režimy podporuje vývoj a výroba clusterů. Možnosti nasazení zvolili můžou být odlišná na různých prostředích.

Sledované inovace aplikace je některé běžné upgradovat na použití. Pokud není zadán upgradu zásad, struktury služby zajišťuje, aby aplikace správný před upgrade vytvářejí.

 Správce aplikace umožňuje ruční postupné upgradu režim aplikace mají úplnou kontrolu nad průběh upgradu pomocí různých upgradu domén. Tento režim je užitečná, když požaduje zásad hodnocení přizpůsobený nebo složitě strukturovanou stavu nebo se děje netradiční upgrade (například aplikace je už ztrátu dat).

Nakonec je užitečné pro vývoj nebo testování prostředí poskytnout rychlé iterace obrázku průběhu vývoje služby Automatická inovace aplikace.

## <a name="change-to-manual-upgrade-mode"></a>Změna na ruční režim upgradu
**Ruční**– zastavení upgrade aplikace na aktuální UD a změňte režim upgradu sledován ručně. Ručně zavolat **MoveNextApplicationUpgradeDomainAsync** a pokračujte v upgradu aktivace vrácení zpět spuštěním nový upgrade se správcem. Po upgradu vloží do režimu ručního, zůstane v režimu ručního dokud je zahájená nový upgrade. Příkaz **GetApplicationUpgradeProgressAsync** vrátí struktury\_aplikace\_UPGRADE\_stavu\_kolejová\_vpřed\_čekání.

## <a name="upgrade-with-a-diff-package"></a>Upgrade se balíčku změn

Aplikace služby struktury lze přidat zřizování s balíčku úplné, samostatné aplikace. Aplikaci můžete taky upgradovat pomocí změn balíček, který obsahuje pouze soubory aktualizované aplikace, aktualizovaného manifestu aplikace a služby seznamu souborů.

Úplné verze aplikace balíček obsahuje všechny soubory a spustíte aplikaci služby struktury. Balíček změn obsahuje pouze soubory, které rozdílů mezi poslední poskytování a aktuální upgrade plus manifest úplné verze aplikace a služby manifest soubory. Každý odkaz v manifestu aplikace nebo manifest služby, který není uveden v rozložení Tvůrce dotazů hledat v úložišti obrázek.

Úplné verze aplikace balíčků jsou potřeba při první instalaci aplikace obrázku. Následné aktualizace může být balíčku úplné verze aplikace nebo balíčku změn.

Příležitosti při použití balíčku změn bude Dobrá volba, pokud:

* Balíček změn je upřednostňované, pokud máte velké aplikace balíčku, který odkazuje na několik služby seznamu souborů a/nebo několik balíčků kód, config balíčků nebo balíčků data.

* Balíček změn je upřednostňované obsahujících nasazení systému, který generuje rozložení sestavení přímo z vašeho procesu Tvůrce dotazů aplikace. V tomto případě Ačkoli kód nezměnil, nově vytvořené sestavení získání různých součtu. Použití balíčku úplné verze aplikace by vyžadují aktualizaci verze na všechny balíčky kód. Použití změn balíčku, pouze poskytnete kde verzi změnilo soubory, které měnit a seznamu.

Při upgradu aplikace pomocí Visual Studia, změn balíčku je publikován automaticky. Ruční vytvoření balíčku změn, musí být aktualizovány manifest aplikace a služby manifesty, ale jenom změněné balíčků měl by být součástí balíček konečné aplikace. 

Například Začněme následující aplikace (verze čísel pro snadnější Principy):

```text
app1        1.0.0
  service1  1.0.0
    code    1.0.0
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Teď Předpokládejme jste chtěli aktualizovat jenom kód balíček service1 pomocí balíčku změn pomocí Powershellu. Teď aktualizované aplikace obsahuje následující struktura složek:

```text
app1        2.0.0      <-- new version
  service1  2.0.0      <-- new version
    code    2.0.0      <-- new version
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

V tomto případě aktualizujete manifest aplikace a 2.0.0 instalované služby service1 tak, aby odrážely balíček aktualizace kódu. Ve složce balíčku aplikace má následující strukturu:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Další kroky

[Upgrade vaší aplikace pomocí aplikace Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí aplikace Visual Studio.

[Upgrade vaší aplikace pomocí Powershellu](service-fabric-application-upgrade-tutorial-powershell.md) vás provede aplikace upgradovat pomocí prostředí PowerShell.

Řídit, jak aplikace aktualizuje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).

Proveďte upgrade aplikace kompatibilní naučit používat [Serializaci dat](service-fabric-application-upgrade-data-serialization.md).

Řešení běžných problémů v aplikaci inovací odkazem kroků v tématu [Řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).
 
