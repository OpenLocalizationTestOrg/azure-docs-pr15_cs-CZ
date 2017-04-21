<properties title="" pageTitle="Názvy souborů a umístění Azure technické články" description="Tento článek vysvětluje struktura souboru články a názvů, které byste měli postupovat když vytvoříte nový článek." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="tysonn" />

#<a name="file-names-and-locations-for-azure-technical-articles"></a>Názvy souborů a umístění Azure technické články

V naší technické obsahu úložiště používáme jedné složky (složce **články** ) pro všechny články. Existuje bez hierarchie složek: všech článků live ve struktuře plochého souboru. Když vytvoříte složky s články z nich, nedá se publikovat své články.

Nepoužívejte struktura souboru jako uspořádání zásadu používáme striktně názvů, které jednoznačně identifikuje témata a které přispívají vyhledávání na webu.

Tady je co byste měli vědět:

+ [Pravidla]
+ [Vzor]
+ [Standardní příklady]
+ [Obsah Marketplace]
+ [Schválení název souboru]

##<a name="rules"></a>Pravidla

- Názvy souborů může obsahovat pouze malá písmena, číslice a spojovníky. 
- Bez mezer nebo interpunkční znaménka. Použití spojovníky k oddělení slov a čísel do pole název souboru.
- Nesmí přesáhnout 80 znaky – to je publikování limit systému
- Použití akce akce, které jsou specifické, jako je třeba vývoj, nákup, vytvořit, Poradce při potížích s. Žádná - sestav slova.
- Žádná malé slova - Nezahrnovat a a, v a, apod.
- Všechny soubory musí být v markdown a přípony souboru .md.
- Ujistěte se, slova. Vyhněte se neschválené nebo nepotřebných zkratky v názvech souborů

Zkratky a initialisms v názvech souborů - pokyny:

- Není zkrátí názvy Azure služeb – prvních slov názvu souboru musí být standard, napsaný Azure služby nebo technologie název. 
-   Nepovolit SV nebo arm jako zkratky kamkoli do pole název souboru
- Jiné Standardní zkratky jsou přijatelné podle potřeby v názvech souborů. 

##<a name="pattern"></a>Vzor

Tady je obecný vzor:

 **Service-Platform-Language-Content-Product-Version.MD**

Použití částí vzorku, která platí a prohlédněte si seznam článků v úložišti určitou představu existující názvy. Začínající není platformy zařízením nebo název služby se pravděpodobně podezřelé skluzu prostřednictvím.

##<a name="standard-examples"></a>Standardní příklady

Tady je několik příkladů platné názvy, budou převedeny vzorku. :

- Shluk služby – dotnet nepřetržitý delivery.md
- mobilní – služby – ios-get-started.md
- documentdb Správa account.md
- Mobile-Services-DotNet-backend-Get-Started-Settings-Sync.MD
- Active-Directory-Java-Authenticate-Users-Access-Control-Eclipse.MD
- Virtual-Machines-Install-Windows-Server-2008r2.MD
- mezipaměť aspnet relace zprostředkovatele stavu
- Azure-SDK-DotNet-Release-Notes-2-8
- storsimple-Disaster-Recovery-Using-Azure-Site-Recovery

##<a name="marketplace-content"></a>Obsah Marketplace

Rozlišovat obsah, který se zaměřuje na příspěvky partnera na Azure marketplace, začněte názvů souborů s "marketplace". Tento obsah nesmí být příliš běžné, jako by měl být vytvořen většina partnera obsahu na webech partnerů vlastní.

- Marketplace-mongodb-Virtual-Machines-Install-Windows-Server-2008r2.MD

##<a name="file-name-approval"></a>Schválení název souboru

Je úlohy naše okruhem vyžádané žádost o recenzentům názvy souborů po odeslání nového souboru do úložiště poprvé. Vložit žádost o recenzentů zkontrolujte název souboru a poskytnutí zpětné vazby prostřednictvím toku vyžádané žádost o komentář v případě potřeby změny. Název souboru je třeba opravit před přijetím odvolat žádost vložit. Přispěvatelům můžete snadno nabízená aktualizaci na požadavek na zjištění stavu úkolů vložit.

##<a name="folder-names-in-the-repo"></a>Názvy složek v repo

Složky by měl být vytvořen pouze pro služby a názvu souboru musí shodovat popisu služby. Používejte jenom písmena a pomlčky a pomocí malými písmeny. Získání schválení od správce úložiště před vytvořením nové složky, který není vydané služby.

##<a name="changing-case-in-file-names"></a>Změna velikosti písma v názvech souborů

Operační systémy Windows jsou malá a velká písmena, takže pokud potřebujete změnit název souboru opravit velikost písmen, je lepší provádění změn na podstatné, pokud je možné k přechodu na Mac a Linux Příklad:

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

Pomocí následujícího příkazu přejmenování souboru:
```
  git mv <articles/service-folder/current-file-name.md> <articles/service-folder/new-file-name>
```

###<a name="contributors-guide-links"></a>Přispěvatelům Průvodce odkazy

- [Článek Základní informace](./../README.md)
- [Index pokyny články](./contributor-guide-index.md)


<!--Anchors-->
[Pravidla]: #rules
[Vzor]: #pattern
[Standardní příklady]: #standard-examples
[Obsah Marketplace]: #marketplace-content
[Schválení název souboru]: #file-name-approval
