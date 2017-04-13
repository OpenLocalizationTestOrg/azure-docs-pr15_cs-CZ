<properties
    pageTitle="Práce s Node.js moduly"
    description="Informace o práci s Node.js moduly při používání aplikace služby Azure nebo Cloudovým službám."
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>


# <a name="using-nodejs-modules-with-azure-applications"></a>Používání Node.js moduly s Azure aplikacemi

Tento dokument obsahuje pokyny k používání Node.js moduly s aplikacemi hostitelem Azure. Obsahuje pokyny v zajistit, aby vaše aplikace používá konkrétní verze modulu i pomocí nativního moduly Azure.

Pokud už máte zkušenosti s použití Node.js moduly **package.json** a **npm shrinkwrap.json** , souborů takto je rychlý přehled co popisované v tomto článku:

* Azure aplikaci služby rozumí **package.json** a **npm shrinkwrap.json** soubory a nainstalovat moduly založené na položky v těchto souborů.
* Azure cloudové služby očekávat názevprojektu do vývojové prostředí nainstalovat a **uzel\_moduly** adresář, který má být součástí balíček pro nasazení. Je možné zapnout podporu pro instalaci moduly pomocí **package.json** **npm shrinkwrap.json** souborů na Cloud Services, ale tento postup vyžaduje úpravy výchozí skripty využívané projektů cloudové služby. Příklad k tomu najdete v tématu [spuštění Azure úkol spustit instalaci npm Chcete-li předejít nasazení moduly uzel](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [AZURE.NOTE] Azure virtuálních počítačích nejsou popisované v tomto článku na nasazení zkušenosti OM bude závisí na operačním systému hostitelem virtuálního počítače.

##<a name="nodejs-modules"></a>Moduly Node.js

Moduly jsou načíst balíčků JavaScript, které jsou zdrojem určité funkce aplikace. Modul obvykle nainstalovaný nástrojem **npm** příkazového řádku, ale jako součást balíčku Node.js core jsou uvedené některé (například modul http).

Po nainstalování moduly jsou uložené v **uzel\_moduly** adresáře u původního příspěvku struktury adresáře aplikace. V jednotlivých modulech **uzel\_moduly** directory udržuje vlastní **uzel\_moduly** adresář, který obsahuje nějaké moduly, které závisí a zopakováním opakujte pro každý modul úplně řetězu závislost. Díky jednotlivých modulech je instalována samostatném verze požadavky pro moduly že závisí, ale může vést struktury velmi velkého adresáře.

Při nasazení **uzel\_moduly** adresáři jako součást aplikace ho vzroste velikost nasazení ve srovnání s pomocí souboru **package.json** nebo **npm shrinkwrap.json** ; však zaručit, že verzi modulů používaných v výrobní jsou stejné jako klávesovými zkratkami ve vývoji.

###<a name="native-modules"></a>Nativní moduly

Většina modulů jsou soubory JavaScriptu jednoduše prostého textu, jsou některé moduly specifické pro platformu binární obrázky. Moduly jsou kompilovaný při instalaci, obvykle pomocí Python a uzel gyp. Protože spolehnout Azure Cloud Services **uzel\_moduly** složky nasazení jako součást aplikace, už nativní modul jako součást nainstalovaných modulů by měl pracovat v do cloudové služby, dlouhou, jak byly nainstalované a kompilovaný vývoj systému Windows.

Azure aplikaci služby nepodporuje všechny nativní moduly a může selhat při sestavování s specifických požadavků. Zatímco některé oblíbených moduly jako MongoDB mají volitelné nativní závislostí a práce jenom pořádku bez nich, prokázáno úspěšné s téměř nativní názevprojektu dnes k dispozici dvě možnosti:

* Spusťte **npm nainstalovat** na počítač Windows, která obsahuje všechny nativní modulu požadavky na instalaci. Potom nasazení vytvořeného **uzel\_moduly** složky jako součást aplikace služby Azure aplikace.
* Azure aplikaci služby je možné konfigurovat provést vlastní flám nebo skriptů prostředí během nasazení, která nabízí, kterou možnost vlastní příkazy a přesně nastavit způsob, jak **nainstalovat npm** je spuštěn. Video znázorňující, jak to udělat najdete v tématu [Vlastní webu nasazení skriptů s Kudu].

###<a name="using-a-packagejson-file"></a>Pomocí souboru package.json

Soubor **package.json** je způsob, jak určit závislosti nejvyšší úrovně aplikace vyžaduje, abyste mohli nainstalovat platformu hostingu, závislosti, spíše než bylo potřeba zahrnout **uzel\_balíčků** složky jako součást nasazení. Po nasazení aplikace příkazu **npm instalace** lze analyzovat soubor **package.json** a nainstalujte všechny závislosti uvedené.

Během vývoje, můžete použít **– uložení**, **– Uložit vývojáře**, nebo **– volitelné uložit** parametry při instalaci moduly přidejte novou položku pro modul **package.json** soubor automaticky. Další informace najdete v tématu [instalace npm](https://docs.npmjs.com/cli/install).

Jeden potenciální se souborem **package.json** je to, že jenom Určuje verzi závislostí nejvyšší úrovně. Jednotlivých modulech nainstalovaný může nebo nemusí zadat verzi moduly, které závisí, a proto je možné, že můžete dostat pomocí různých závislost typu řetězec než používali ve vývoji.

> [AZURE.NOTE]
> Při nasazení služby Azure aplikace, pokud soubor <b>package.json</b> odkazuje nativní modulu uvidíte podobnou následující chyba při publikování aplikace pomocí libovolná:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


###<a name="using-a-npm-shrinkwrapjson-file"></a>Pomocí souboru npm shrinkwrap.json

Soubor **npm shrinkwrap.json** je pokus o adresy omezení modul správy verzí souboru **package.json** . Během **package.json** soubor obsahuje pouze verze pro moduly nejvyšší úrovně, **npm shrinkwrap.json** soubor obsahuje požadavky na verzi pro celou modul závislost typu řetězec.

Když je připraven k výrobní aplikace, můžete uzamknout – požadavky na verzi a vytvoření souboru **npm shrinkwrap.json** pomocí příkazu **npm shrinkwrap** . Bude použit verzí nainstalovaný v **uzel\_moduly** složky a zaznamenávat k souboru **npm shrinkwrap.json** . Po nasazení aplikace hostitelské prostředí příkazu **npm instalace** lze analyzovat soubor **npm shrinkwrap.json** a nainstalujte všechny závislosti uvedené. Další informace najdete v tématu [npm shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap).

> [AZURE.NOTE]
>Při nasazení služby Azure aplikace, pokud soubor <b>npm shrinkwrap.json</b> odkazuje nativní modulu uvidíte podobnou následující chyba při publikování aplikace pomocí libovolná:

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


##<a name="next-steps"></a>Další kroky

Teď, když víte, jak pomocí služby Azure Node.js moduly, zjistěte, jak [zadat verze Node.js], [sestavovat a nasazovat Node.js web appu]a [jak používat rozhraní Azure příkazového řádku pro Mac a Linux].

Další informace najdete v tématu [Středisko pro vývojáře Node.js](/develop/nodejs/).

[určení verze Node.js]: nodejs-specify-node-version-azure-apps.md
[Jak používat rozhraní Azure příkazového řádku pro Mac a Linux]: xplat-cli-install.md
[Vytvořte a nasaďte Node.js web appu]: web-sites-nodejs-develop-deploy-mac.md
[Node.js Web Application with Storage on MongoDB (MongoLab)]: store-mongolab-web-sites-nodejs-store-data-mongodb.md
[Build and deploy a Node.js application to an Azure Cloud Service]: cloud-services-nodejs-develop-deploy-app.md
[Skripty nasazení vlastní webu s Kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/
