<properties 
    pageTitle="Data Factory - pojmenování pravidla | Microsoft Azure" 
    description="Popisuje naming pravidla pro Data Factory entity." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - pojmenování pravidel 
Následující tabulka obsahuje naming pravidel pro artefakty Data Factory.



Jméno | Jedinečnost názvu | Ověření Kredibility
:--- | :-------------- | :----------------
Data Factory | Jedinečný přes Microsoft Azure. Názvy velká a malá písmena, tedy MyDF a mydf odkazují na stejné factory dat. |<ul><li>Každý factory dat je svázané se přesně Azure předplatných.</li><li>Názvy objektů, které musí začínat písmenem nebo číslo a může obsahovat jenom písmena, číslice a znak pomlčku (-).</li><li>Každý znak pomlčku (-) musí být okamžitě a následnou písmeno nebo číslo. V názvech kontejneru nejsou povoleny po sobě jdoucí typ čáry.</li><li>Název můžete mít 3 63 znaků.</li></ul>
Propojené služby/tabulek a potrubí | S v jedinečný data factory. V názvech se malá a velká písmena. | <ul><li>Maximální počet znaků v názvu tabulky: 260.</li><li>Názvy objektů, které musí začínat písmenem číslo nebo podtržítka (_).</li><li>Následující znaky nejsou povoleny: ".", "a", "?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul>
Pole Skupina zdroje | Jedinečný přes Microsoft Azure. V názvech se malá a velká písmena. | <ul><li>Maximální počet znaků: 1 000.</li><li>Název může obsahovat písmena, číslice a tyto znaky: "-", "_";";"a".".</li></ul>