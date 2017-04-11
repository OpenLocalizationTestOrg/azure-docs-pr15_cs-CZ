<properties
    pageTitle="Povolení spřažení relace pomocí nástrojů Azure pro Eclipse"
    description="Zjistěte, jak povolit spřažení relace pomocí nástrojů Azure pro Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->

# <a name="enable-session-affinity"></a>Povolení spřažení relace #

V rámci Azure sada nástrojů pro Eclipse můžete povolit HTTP relace spřažení nebo "rychlé relací" pro vaše role. Následující obrázek znázorňuje dialogové okno lze povolit funkci spřažení relace vlastností **Vyrovnávání zatížení** :

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>Chcete-li povolit spřažení relaci pro svoji roli ##

1. Klikněte pravým tlačítkem roli v prvku zatmění Průzkumník projektu, klikněte na **Azure**a potom klikněte na **Vyrovnávání zatížení**.
1. V dialogovém okně **Vlastnosti pro vyrovnávání zatížení WorkerRole1** :
    1. Kontrola **Povolit HTTP relace spřažení (rychlé relace) pro tuto roli.**
    1. **Koncový bod vstupní používat**vyberte zadávání koncový bod použít, například **http (veřejné: 80, soukromé: 8080)**. Aplikace musíte použít tento koncový bod jako jeho HTTP koncový bod. Povolení více koncové body pro vaše role, ale můžete vybírat pouze z nich podporuje rychlé relace.
    1. Opětovné sestavení aplikace.

Po povolení, pokud máte víc než jednou instancí role, požadavky HTTP pocházejících z určitého klienta zůstanou v uskutečněných jednotlivými stejné instanci rolí.

Sada nástrojů zatmění povoluje toto instalací zvláštní modul služby IIS s názvem aplikace požádat o směrování (ARR) do každé instancí role. ARR přesměrovává požadavků HTTP instanci odpovídající roli. Tato sada automaticky změní konfiguraci vybraný koncový bod tak, aby příchozí přenosy protokolu HTTP nejdřív směrován k softwaru ARR. Tato sada také vytvoří nový vnitřní koncový bod, který je Java server nakonfigurovaný k poslechu. To je koncový bod používaný ARR přesměrovat přenosy protokolu HTTP instanci odpovídající roli. Tímto způsobem pokaždé role ve více instancí nasazení slouží jako reverzního proxy pro všechny další instance povolení rychlé relace.

## <a name="notes-about-session-affinity"></a>Poznámky k spřažení relace ##

* Relace spřažení ve výpočetním emulátoru nefunguje. Nastavení se dají použít v emulátoru výpočetním bez ovlivnění proces vytváření nebo výpočet emulátoru spuštění, ale samotnou funkcí ve výpočetním emulátoru nefunguje.
* Povolení spřažení relace bude mít za následek zvýšení místa na disku zabírají nasazením v Azure, další software bude stáhnout a nainstalovat do role instance při spuštění služby v Azure cloudu.
* Čas inicializace každou roli bude trvat déle.
* Vnitřní koncový bod, budou fungovat jako přenosy rerouter uvedené výše, bude added.ss

Příklad spravovat relace data při zapnuté funkci spřažení relace najdete v článku [jak zachovat Data relace s spřažení relace][].

## <a name="see-also"></a>Viz taky ##

[Azure sada nástrojů pro Eclipse][]

[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění][]

[Instalace Azure sada nástrojů pro Eclipse][] 

[Jak zachovat Data relace s spřažení relace][]

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure][].

<!-- URL List -->

[Středisko pro vývojáře Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění]: http://go.microsoft.com/fwlink/?LinkID=699533
[Jak zachovat Data relace s spřažení relace]: http://go.microsoft.com/fwlink/?LinkID=699539
[Instalace Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png
