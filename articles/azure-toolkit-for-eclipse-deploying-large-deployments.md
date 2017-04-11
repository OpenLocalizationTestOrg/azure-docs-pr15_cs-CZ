<properties
    pageTitle="Nasazení velké nasazení"
    description="Naučte se nasadit velké nasazení pomocí nástrojů Azure pro Eclipse."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268601.aspx -->

# <a name="deploying-large-deployments"></a>Nasazení velké nasazení #

Pokud nasazení je příliš velký mají být obsaženy v approot výchozí složky, můžete použít místní úložiště zdroje jako kořenové složce nasazení pro vaše JDK a aplikační server.

## <a name="to-use-a-local-storage-resource-as-the-deployment-root-folder-for-large-deployments"></a>Použít místní úložiště zdroje jako kořenové složce nasazení u velkých nasazení ##

1. Vytvoření nového prostředku Místní úložiště. Název zdroje, nezáleží. Úložiště zdroje jsou definovány na úrovni role. Nejrychlejším způsobem, jak přejít do místní úložiště konfigurace dialogového okna, ze kterého můžete vytvořit nový zdroj místní úložiště je pomocí následujících kroků: klikněte pravým tlačítkem myši roli v zobrazení **Aplikace Project Explorer** (Pokud se nezobrazuje roli, rozbalte uzel projektu Azure), klikněte na **Azure**a potom klikněte na **Místní úložiště**. V dialogovém okně **Místní úložiště** klikněte na tlačítko **Přidat** nový zdroj místní úložiště.
1. Nastavte požadovanou velikost aspoň 2048 MB (všechno, co menší způsobit stejného souboru velikost problémy, jako by setkáte v approot).
1. Ujistěte se, že je zaškrtnuté **vyčistit obsah po z koše instanci rolí** ; Tím se zabránili logických spouštěcí nasazení spuštěný do konflikty s předem existujících souborů v zdroje instanci rolí je z koše.
1. Ověřte, zda je řetězec **DEPLOYROOT**hodnotu **proměnné ukládání zdroje adresář po nasazení** . Zdroje dialogu místní úložiště bude vypadat podobně jako tento.
    ![][ic667943]

Můžete taky když **DEPLOYROOT** jako *název* místní zdroj se nezmění automaticky generované prostředí proměnná název (nastaví se **DEPLOYROOT_PATH** v takovém případě), který pro vhodná i aplikace.

Další informace o vytváření zdroje místní úložiště najdete v [místním úložišti vlastnosti][].

## <a name="see-also"></a>Viz taky ##

[Azure sada nástrojů pro Eclipse][]

[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění][]

[Instalace Azure sada nástrojů pro Eclipse][] 

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure][].

<!-- URL List -->

[Středisko pro vývojáře Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalace Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Místní uložení vlastnosti]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties

<!-- IMG List -->

[ic667943]: ./media/azure-toolkit-for-eclipse-deploying-large-deployments/ic667943.png
