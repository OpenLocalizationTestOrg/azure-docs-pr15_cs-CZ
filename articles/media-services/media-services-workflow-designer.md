<properties 
    pageTitle="Vytvoření rozšířeného kódování pracovní postupy s Návrháři pracovního postupu | Microsoft Azure" 
    description="Informace o postupu při vytváření pokročilých kódování pracovních postupů s Návrháři pracovního postupu." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;johndeu;anilmur"/>


#<a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Vytvoření rozšířeného kódování pracovní postupy s Návrháři pracovního postupu

##<a name="overview"></a>Základní informace

**Návrhář pracovního postupu** je ploše nástroj služby Windows, který slouží k navrhování a vytváření vlastních pracovních postupů pro šifrování s **Pracovním postupem Media Encoder Premium**.
Pomocí power nástroj Návrháře pracovního postupu můžete navrhnout a vytvářet složité pracovních postupů, které se spustí v **Media Encoder Premium**.  

Pracovní postupy mohou obsahovat logiky rozhodnutí zákazníka a větvení podle vlastností, zadávání zdrojový soubor. Můžete vytvořit pracovní postupy s overridable vlastnosti a dynamické hodnoty pro snadnou opakujte a upravit v cloudu i nejčastěji složité kódování úkoly.

Příklad pracovních postupů, které můžete vytvářet, patří:

- Rozhodnutí o založené pracovní postupy, které kontrola zdroje obsahu pro překlad a kódovat pouze drah požadované výstupní.  Toto je helfpul odstraněním ztrátě skladeb, které by generovaných upscaling obsahu neúmyslně zdroje.
- Více vstupní souborů mohou sloužit k podpoře titulky, překrytí a více obsahu dohromady. 

Tento nástroj slouží také můžete upravit všechny naše [publikované pracovní postupy](media-services-workflow-designer.md#existing_workflows). 

>[AZURE.NOTE]Chcete-li získat této kopie sady nástrojů Návrháře pracovního postupu, obraťte se prosím mepd@microsoft.com.


Po vytvoření souboru pracovního postupu je možné uložit jako aktivum a pak se dá použít pro kódování mediální soubory. Informace o tom, jak kódovat s **Media Encoder Premium pracovního postupu** pomocí **.NET**najdete v tématu [Upřesnit kódování s pracovním postupem Media Encoder Premium](media-services-encode-with-premium-workflow.md).

##<a id="existing_workflows"></a>Změna existující pracovních postupů

Výchozí [publikované pracovních postupů](media-services-workflow-designer.md#existing_workflows) můžete změnit pomocí návrháře nástroje. Výchozí můžete získat soubory pracovních postupů [v tomto poli](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Složka také obsahuje popis těchto souborů.

Následující video ukazuje, jak lze pomocí návrháře.

###<a name="day-1--getting-started"></a>Den 1 – Začínáme

Video 1 den obsahuje:

- Návrháře přehled
- Základní pracovních postupů – "Vítáme"
- Vytvoření více výstup soubory MP4 pro použití s streamování Azure Media Services

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-1]

###<a name="day-2"></a>Den 2

Den 2 video zahrnuje:

- Různé zdrojový soubor scénáře – zpracování zvuku
- Pracovní postupy s Upřesnit logiky
- Fáze grafu

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-2]

###<a name="day-3"></a>Den 3

Video 3 den obsahuje:

- Skriptování uvnitř pracovních postupů/modrotisků
- Omezení s aktuální Encoder
- Služba Q & A
 
> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-3]


## <a name="next-step"></a>Další krok

Prohlédněte si Media Services naučné stezky.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


Pokud potřebujete podporu nebo mají dotazy týkající se vytváření vlastních pracovních postupů v nástroji Návrhář pracovního postupu, pošlete nám prosím e-mailů na mepd@microsoft.com.

##<a name="see-also"></a>Viz taky

[Azure Premium Encoder pracovního postupu návrháře školicích videí](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)
