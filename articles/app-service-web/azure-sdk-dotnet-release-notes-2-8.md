
<properties 
   pageTitle="Poznámky k verzi Azure SDK pro .NET 2,8" 
   description="Poznámky k verzi Azure SDK pro .NET 2,8" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>
 
# <a name="azure-sdk-for-net-28-281-and-282"></a>Azure SDK pro .NET 2,8, 2.8.1 a 2.8.2

##<a name="overview"></a>Základní informace
 
Tento článek obsahuje poznámky, (které zahrnuje známých problémů a nejnovějších změnách) pro Azure SDK pro .NET 2,8, 2.8.1 a 2.8.2 vydání. 

Úplný seznam nových funkcí a aktualizace provedené v této verzi najdete v tématu oznámení [Azure SDK 2,8 Visual Studio 2013 a Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/) . 

##  <a name="azure-sdk-for-net-28"></a>Azure SDK pro .NET 2,8

### <a name="download-azure-sdk-for-net-28"></a>Stažení Azure SDK pro .NET 2,8

[Azure SDK pro .NET 2,8 Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=699285) 

[Azure SDK pro .NET 2,8 Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkId=699287)
 
### <a name="net-452-support"></a>Podpora .NET 4.5.2 

####<a name="known-issues"></a>Známé problémy

Azure 2,8 SDK .NET umožňuje vytvářet .NET 4.5.2 balíčků cloudové služby. Ale .NET 4.5.2 framework nebude nainstalována na výchozí uvolněte hostovaného OS obrázky do leden 2016 s operačním systémem Host. Před že 4.5.2 .NET framework budou k dispozici prostřednictvím samostatná verze s operačním systémem hosta vydání – listopad 2015 02. Zobrazit stránku [Azure hosta OS verzí a matice kompatibility SDK](../cloud-services/cloud-services-guestos-update-matrix.md) můžete sledovat, kdy budou vydávat obrázku.  Po uvolnění obrázek 2015 02 listopad můžete použít tento obrázek aktualizací cloudové služby konfigurační soubor (.cscfg) soubor. V konfiguraci služby souboru, nastavte atribut osVersion elementu ServiceConfiguration řetězec "WA-hosta-OS-4.26_201511-02". Pokud se rozhodnete vyjádření výslovného souhlasu použít tento obrázek se už získat automatické aktualizace OS Host. Chcete-li získat automatických aktualizací osVersion musí být nastavena na "*" a .NET 4.5.2 bude k dispozici pouze prostřednictvím automatických aktualizací v lednu 2016.

###<a name="azure-data-factory"></a>Azure Data Factory

####<a name="known-issues"></a>Známé problémy 

Při vytváření **Šablona Factory dat** projektu týkající se ukázková data může selhat skriptu prostředí azure power je-li azure power prostředí verze v počítači nainstalován po 0.9.8.

Abyste mohli úspěšně vytvořit tento typ projektu, je nutné nainstalovat [verzi azure power prostředí 0.9.8](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi).


### <a name="azure-resource-manager-tools"></a>Azure nástroje Správce prostředků 

####<a name="breaking-changes"></a>Zrušení změn

V této verzi pro práci s nové rutiny prostředí PowerShell Azure verze 1.0 byl aktualizovaný skript Powershellu dodávané s aplikací project Azure pole Skupina zdroje.  Tento nový skript nefunguje z aplikace Visual Studio při použití verzi SDK před 2,8.  

Skriptů projekty vytvořené v předchozích verzích aplikace v SDK nespustí z aplikace Visual Studio při použití 2,8 SDK.  Všechny skripty zůstanou pracovat mimo Visual Studio odpovídající verze rutin prostředí PowerShell Azure.  

2,8 SDK vyžaduje verze 1.0 rutiny prostředí PowerShell Azure.  Všechny verze sady SDK potřebovat verzi 0.9.8 rutiny prostředí PowerShell Azure.  Další informace najdete v [tomto](http://go.microsoft.com/fwlink/?LinkID=623011) blogu.

###<a name="web-tools-extensions"></a>Rozšíření nástrojů Web

####<a name="known-issues"></a>Známé problémy

Následující známé problémy se řeší následující vydání.

- Aplikace služby souvisejících cloudu a Průzkumník serveru gesto na testovacím prostředí (třeba Čína Azure nebo Azure zásobníku zákazníci) nefungují. Pro zákazníky v těchto oblastech dotčeném stahování profilu publikování z portálu Microsoft Azure umožní publikování možnost. Budoucí vydání bude opravy gesta například "Ladění připojit" a "Zobrazit Streaming protokoly" Azure Číny a zákazníky zásobníku 
- Zákazníci vidět chyby při vytváření při interpretaci aplikace instance kterého jsou nasazením je v oblasti než východoasijských USA aplikaci služby. V těchto případech vytváření služeb aplikací na portálu a stahování profilu publikování umožní publikování scénáře. 

###<a name="azure-hdinsight-tools"></a>Nástroje Azure HDInsight

####<a name="new-updates"></a>Nových aktualizací

- Můžete spuštění dotazu podregistru clusteru prostřednictvím HiveServer2 s téměř žádné režijních a najdete v tématu že projektu zaznamenává do v reálném čase.
- Použití nového podregistru spuštění zobrazení úkolů můžete dostanete do práce hlubší, podrobnější informace a určit potenciální problémy.

Informace najdete v tématu [Azure SDK 2,8 Visual Studio 2013 a Visual Studio 2015](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/). 

## <a name="azure-sdk-for-net-281"></a>Azure SDK pro .NET 2.8.1

### <a name="known-issues-for-visual-studio-2013-and-visual-studio-2015"></a>Známé problémy s Visual Studio 2013 a Visual Studio 2015
 
1. Spouštěné WebJob publikuje sloty bude zobrazit a chyby a nebude sadu plánu, ale budou WebJob Azure. Zákazníci, kteří potřebují plánu projektu můžete používat portál Azure nastavit plán WebJob. 
2. Zákazníci Python mohou vyskytnout problémy ladění. Týmu služby je zavádění řešení pro tuto, ale když zákazníci se to týká, přejděte prosím nechat Microsoft vědět ve fórech nebo v oznámení blogu nebo uvolnění oddíl komentáře poznámky. 
3. Zákazníci v některých oblastech (například Jižní Indie), budou mít chyby zřizování aplikaci služby. Toto je v souladu s portálu a zákazníky, kteří tento problém můžete používat portál Azure požádat o přístup k publikování na tyto oblasti geo. Jakmile vyžádání přístupu k tyto oblasti pomocí Azure portálu zřizování mají pracovat. 

##<a name="azure-sdk-for-net-282"></a>Azure SDK pro .NET 2.8.2

Po instalaci 2.8.2 následující problému může dojít nástroje pro zákazníky.         

- Pokud používáte Windows 10 a nenainstalovali Internet Exploreru, může dojde k chybě "Internet Exploreru nebyl nalezen".
Tento problém vyřešit, nainstalujte aplikaci Internet Explorer pomocí dialogového okna Přidat či odebrat součásti systému Windows.

Pokud sledovat tento problém pomocí funkce Odeslat smajlíka můžete ji nahlásit.

Další informace najdete v tématu [Tento](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-2-for-net/) příspěvek.
##<a name="other-updates"></a>Mezi další aktualizace

Mezi další aktualizace najdete v článku [Azure SDK 2,8 oznámení publikovat](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

##<a name="also-see"></a>Viz také

[Publikovat oznámení Azure 2,8 SDK](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)

[Podpora a vyřazování webů informace o Azure SDK pro .NET a rozhraní API](https://msdn.microsoft.com/library/azure/dn479282.aspx)

