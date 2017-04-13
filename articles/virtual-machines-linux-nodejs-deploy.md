<properties
   pageTitle="Nasazení aplikace Node.js k Linux virtuálních počítačích v Azure"
   description="Naučte se nasadit aplikaci Node.js Linux virtuálních počítačích v Azure."
   services=""
   documentationCenter="nodejs"
   authors="stepro"
   manager="dmitryr"
   editor=""/>

<tags
   ms.service="multiple"
   ms.devlang="nodejs"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="02/02/2016"
   ms.author="stephpr"/>

# <a name="deploy-a-nodejs-application-to-linux-virtual-machines-in-azure"></a>Nasazení aplikace Node.js k Linux virtuálních počítačích v Azure

Tento kurz ukazuje, jak chcete pořídit aplikace Node.js a nasadit na virtuálních počítačích Linux spuštěné v Azure. Pokyny v tomto kurzu může být zahájen v operačním systému, který může systém Node.js.

Dozvíte, jak:

- Větve a klonovat úložišti GitHub obsahující jednoduché úkol aplikace;
- Vytváření a konfigurace dvou Linux virtuálních počítačích na Azure ke spuštění aplikace.
- Zapracovávat ji do aplikace stisknutím aktualizace web frontend virtuálního počítače.

> [AZURE.NOTE]
> Tento kurz potřebujete GitHub účet a účet Microsoft Azure a možnost použít libovolná z počítače vývoj.

> Pokud nemáte účet GitHub, můžete si jej vytvořit [tady](https://github.com/join).

> Pokud nemáte účet [Microsoft Azure](https://azure.microsoft.com/) , můžete registraci bezplatnou zkušební verzi [tady](https://azure.microsoft.com/pricing/free-trial/). To taky vás provede procesu registrace [Účtu Microsoft](http://account.microsoft.com) Pokud už nemáte jeden. Můžete taky Pokud jsou odběratele Visual Studio můžete [Aktivovat MSDN výhod](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> Pokud nemáte v počítači vývoj libovolná, pokud používáte počítač Macintosh nebo Windows, nainstalujte libovolná z [tady](http://www.git-scm.com). Pokud používáte Linux, nainstalujte libovolná pomocí mechanismus raději, například `sudo apt-get install git`.

## <a name="forking-and-cloning-the-todo-application"></a>Větvení a klonováním úkol aplikace

ÚKOL aplikace v tomto kurzu používá jednoduchý web frontend přes MongoDB instanci, který sleduje seznam úkol. Přihlásit se k GitHub, přejděte [v tomto poli](https://github.com/stepro/node-todo) najít aplikaci a rozvětvené to pomocí odkazu v pravém horním. Úložiště to mají vytvořit ve vašem účtu s názvem /node-todo *název účtu*.

Teď ve vašem počítači vývoj klonovat tohoto úložiště:

    git clone https://github.com/accountname/node-todo.git

Použijeme tento místní klonovat úložiště trochu později při provádění změn zdrojového kódu.

## <a name="creating-and-configuring-the-linux-virtual-machines"></a>Vytváření a konfigurace Linux virtuálních počítačích

Azure obsahuje skvělé podporu nezpracovanými výpočetním pomocí Linux virtuálních počítačích. Tato část výuková ukazuje, jak můžete snadno číselník se dvěma Linux virtuálních počítačích a nasazení aplikace úkol, se systémem web frontend jeden a instance MongoDB na druhé.

### <a name="creating-virtual-machines"></a>Vytváření virtuálních počítačích

Nejjednodušší způsob, jak vytvořit nový virtuální počítač v Azure je používat portál Azure. Klikněte na [zde](https://portal.azure.com) se přihlásit a spuštění portálu Azure ve webovém prohlížeči. Jakmile portálu Azure načetl, proveďte následující kroky:

- Klikněte na odkaz "+ nový";
- Vyberte kategorii "Využití" a zvolte "Systémem Ubuntu serveru 14.04 l";
- "Správce prostředků" nasazení modelu a klikněte na "Vytvořit";
- Vyplňte základní informace o následujících pokynů:
  - Zadejte název, který umožňuje snadno identifikovat později;
  - Tento kurz zvolte ověřování hesla;
  - Vytvoření nové skupiny prostředků identifikovatelné názvem.
- Velikost virtuálního počítače "A1 standardní" je vhodná volba pro účely tohoto návodu.
- Další nastavení zkontrolujte, že na disku typ "Standardní" a přijmout všechny zbývající výchozí hodnoty.
- Zahájit vytváření na souhrnné stránce.

Provedení výše procesu dvakrát vytváření dvou Linux virtuálních počítačích, jeden pro web frontend a druhý MongoDB instance. Vytváření virtuálních počítačích bude trvat přibližně 5 až 10 minut.

### <a name="assigning-a-dns-entry-for-virtual-machines"></a>Přiřazení položky DNS virtuálních počítačích

Virtuálních počítačích vytvořené v Azure jsou ve výchozím nastavení pouze přístupných osobám s postižením přes veřejnou IP adresu, třeba 1.2.3.4. Pojďme vytvořit strojů více snadno identifikovat přiřazením položky DNS.

Jakmile portálu označuje, zda byly vytvořeny virtuálních počítačích, kliknout na odkaz "Virtuálních počítačích" v levé navigační panel a vyhledejte počítače. Pro každý počítač:

- Přejděte na kartu Essentials a klikněte na veřejnou IP adresu.
- Veřejné konfiguraci adresy IP přiřadit popisek název DNS a uložte.

Na portálu zajistí neexistuje název, který určíte. Po uložení konfigurace, bude mít virtuálních počítačích názvy hostitelů podobně jako `machinename.region.cloudapp.azure.com`.

### <a name="connecting-to-the-virtual-machines"></a>Připojení k virtuálních počítačích

Když virtuálních počítačích poskytnuté, byly předkonfigurovaná a povolení vzdáleného připojení přes SSH. Toto je mechanismus, které budeme používat ke konfiguraci virtuálních počítačích. Pokud používáte Windows pro vaše vývoj, musíte získat SSH klienta, pokud už nemáte jeden. Běžné volba tady je nátěrové, které si můžete stáhnout z [tady](http://www.chiark.greenend.org.uk/~sgtatham/putty/). Pomocí verze SSH předinstalované se Macintosh a operační systémy Linux.

### <a name="configuring-the-web-frontend-virtual-machine"></a>Konfigurace webové Frontend virtuálního počítače

SSH do počítače frontend web, který jste vytvořili pomocí nátěrové, ssh příkazového řádku nebo jiných Oblíbené nástroj SSH. Byste měli vidět uvítací zprávu a za ním uveďte příkazový řádek.

Nejdřív Pojďme Ujistěte se, že libovolná a uzel jsou oba nainstalovaný:

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs
    
Protože frontend webové aplikace závisí na některé nativní Node.js moduly, jsme taky potřebují nainstalovat základní sadu nástrojů Tvůrce dotazů:

    sudo apt-get install -y build-essential

Pojďme nainstalujte aplikaci Node.js názvem *trvale*, přispět ke spuštění Node.js serverové aplikace:

    sudo npm install -g forever
    
Toto jsou všechny závislosti, třeba na tomto počítači virtuální moct spouštět frontend webové aplikace, takže nastavíme se systémem. K tomuto účelu vytvoříme nejdřív úplné klonovat úložiště GitHub, které jste předtím forked, takže můžete snadno publikovat aktualizace virtuálního počítače (budeme se zabývat těmito oblastmi tento scénář aktualizace později) a potom klonovat úplné klonovat poskytnout verzi úložištěm, které skutečně jde spouštět.

Spuštění z adresáře Domů (~), spusťte následující příkazy (Nahraďte *název účtu* GitHub uživatelské jméno účtu):

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

Teď zadejte adresáři uzel úkol a spusťte tyto příkazy:

    npm install
    forever start server.js
    
Teď běží frontend webové aplikace, ale je jedním dalším krokem před přistupujete k aplikaci z webového prohlížeče. Virtuální počítač, který jste vytvořili se po zamknutí podle Azure zdroje s názvem *síť skupiny zabezpečení*, které pro vás vytvořil při zřizování virtuální počítač. V současnosti tento zdroj umožňuje pouze externí požadavky na port 22 na směrovány virtuálního počítače, což umožňuje SSH komunikace s počítači, ale nic jiného. Tak abyste mohli zobrazit aplikace úkol, který je nakonfigurovaný na spuštění na portu 8080, tento port také musí být otevřen.

Vraťte se k portálu Azure a proveďte následující kroky:

- Klikněte na "Skupiny zdrojů" v levé navigační panel;
- Vyberte skupinu zdroje, který obsahuje virtuálního počítače;
- Ve výsledném seznamu zdrojů vyberte skupinu zabezpečení síti (tu ikony znaku);
- V okně vlastností zvolte "pravidla příchozí zabezpečení";
- Na panelu nástrojů klikněte na tlačítko "Přidat";
- Zadejte název jako "výchozí povolit úkol";
- Nastavení protokolu "TCP";
- Nastavit oblast cílový port na "8080";
- Klikněte na tlačítko OK a počkejte pravidlo zabezpečení vytvořit.

Po vytvoření tohoto pravidla zabezpečení aplikace úkol je bude veřejně viditelný na Internetu a můžete vyhledat, například pomocí adresy URL:

    http://machinename.region.cloudapp.azure.com:8080

Zjistíte, že i když je ještě nenakonfigurovali MongoDB virtuálního počítače, úkol aplikace zdá být docela funkční. Důvodem je úložiště zdrojů je pevně kódovaná používat předem nasazeném MongoDB instance. Po konfiguraci jsme virtuálního počítače MongoDB Změníme vrátit zpátky a Změna zdrojového kódu pro využití naše soukromé instance MongoDB místo toho.

### <a name="configuring-the-mongodb-virtual-machine"></a>Konfigurace MongoDB virtuálního počítače

SSH na druhý počítač, který jste vytvořili pomocí nátěrové, ssh příkazového řádku nebo jiných Oblíbené nástroj SSH. Po zobrazení uvítací zprávu a příkazový řádek, nainstalujte MongoDB (tyto pokyny byly odebrány z [tady](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

Ve výchozím nastavení MongoDB nakonfigurovaný tak, aby jenom k němu místně. Pro účely tohoto návodu jsme nastaví její konfiguraci MongoDB tak, aby ho můžete k nim získat přístup z počítače virtuální aplikace. V rámci sudo otevřete soubor /etc/mongod.conf a vyhledejte `# network interfaces` oddíl. Změna `net.bindIp` konfigurace hodnotu `0.0.0.0`.

> [AZURE.NOTE]
> Toto nastavení je pro účely pouze v tomto kurzu. **Není si doporučené zabezpečení** a neměla používat ve provozním prostředí.

Teď zajistěte MongoDB spuštěná služba:

    sudo service mongod restart

MongoDB funguje přes port 27017 ve výchozím nastavení. Ano stejným způsobem, potřebujeme otevřete port 8080 na web frontend virtuálního počítače, potřebujeme port 27017 v počítači virtuální MongoDB otevřít.

Vraťte se k portálu Azure a proveďte následující kroky:

* Klikněte na "Skupiny zdrojů" v levé navigační panel;
* Vyberte skupinu zdroje, který obsahuje virtuální počítač MongoDB;
* Ve výsledném seznamu zdrojů vyberte skupinu zabezpečení síti (tu ikony znaku) se stejným názvem, který jste zadali do virtuálního počítače MongoDB;
* V okně vlastností zvolte "pravidla příchozí zabezpečení";
* Na panelu nástrojů klikněte na tlačítko "Přidat";
* Zadejte název jako "výchozí povolit mongo";
* Nastavení protokolu "TCP";
* Nastavit oblast cílový port na "27017";
* Klikněte na tlačítko OK a počkejte pravidlo zabezpečení vytvořit.

## <a name="iterating-on-the-todo-application"></a>Iterace na úkol aplikace
Zatím jsme mít zřízení dvou Linux virtuálních počítačích: tu, která probíhá web frontend a aplikace jednu, která běží MongoDB instance. Ale došlo k potížím - web frontend nepoužívá skutečně zřizování instance MongoDB ještě. Řekněme, opravit aktualizací kód frontend web místo instanci pevně použít proměnná prostředí.

### <a name="changing-the-todo-application"></a>Změna úkol aplikace

V tomto počítači vývoj kde nejdřív klonovat úložišti uzel úkol, otevřete `node-todo/config/database.js` souborů v seznamu Oblíbené editor a změňte hodnotu adresu url z pevně hodnoty jako `mongodb://...` k `process.env.MONGODB`.

Uložte provedené změny a nabízená GitHub předlohy:

    git commit -am "Get MongoDB instance from env"
    git push origin master

Bohužel to není publikovat změny web frontend virtuálního počítače. Pojďme měnit několik dalších počítače virtuální povolení jednoduché, ale působivé mechanismus při publikování aktualizací tak, aby mohli rychle sledovat vliv změny v reálném prostředí.

### <a name="configuring-the-web-frontend-virtual-machine"></a>Konfigurace webové Frontend virtuálního počítače
Odvolání jsme dřív vytvořili úplné klonovat úložišti uzel úkol na web frontend virtuálního počítače. Zjistíte, že tato akce vytvořili nový vzdálené libovolná do kterého můžete posune změny. Však jednoduše předání na tomto vzdáleném úplně nedáte rychlé iterace model, který vývojáři pracují na hledané při práci na svůj kód.

Co byste rádi budete moct udělat je zajistit, aby dojde nabízených k úložišti vzdáleného počítače virtuální pracovního úkol aplikace automaticky aktualizoval. To je naštěstí snadno dosáhnout s libovolná.

Libovolná zpřístupňuje celou řadu zavěšení, které se označují jako v určitou dobu reagovat akce na úložiště. Toto jsou určeny pomocí skriptů prostředí v v úložišti `hooks` složky. Je zavěšení, který nejčastěji platí pro scénář automaticky aktualizován `post-update` události.

V relaci SSH web frontend virtuálního počítače, přejděte `~/node-todo.git/hooks` adresář a přidejte následující obsah do souboru nazvaného `post-update` (nahrazení `machinename` a `region` informacemi virtuálního počítače MongoDB):

    #!/bin/bash
    
    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info
    
Zkontrolujte, že tento soubor spustitelný spuštěním následujícího příkazu:

    chmod 755 post-update

Tento skript zajistí, že je aktuální aplikace serveru, kód v úložišti klonovaný se aktualizuje na nejnovější, všechny aktualizované závislostí a restartování serveru. Také zajišťuje prostředí nakonfigurované při přípravě příjem naše první aktualizace aplikací pro získání MongoDB instance z proměnná prostředí.

### <a name="configuring-your-development-machine"></a>Konfigurace vývoj počítače
Teď nastavíme počítači vývoj připojených k web frontend virtuálního počítače. Toto je jednoduchá – stačí přidání úplné úložiště na počítači virtuální jako vzdálené. Spusťte tento příkaz akce (nahrazení *uživatele* web frontend virtuálního počítače přihlašovací jméno a *nazev_pocitace* a *oblasti* podle potřeby):

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

To je vše, co je potřeba k povolení předání nebo v podstatě publikování, se změní na web frontend virtuálního počítače.

### <a name="publishing-updates"></a>Publikování aktualizace

Pojďme publikovat jedné změny, které byly připíše zatím tak, aby aplikace bude používat vlastní MongoDB instance:

    git push azure master

Měli byste vidět výstup nějak takto:

    Counting objects: 4, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 406 bytes | 0 bytes/s, done.
    Total 4 (delta 1), reused 0 (delta 0)
    remote: info:    Forever stopped processes:
    remote: data:        uid  command         script    forever pid  id logfile                         uptime
    remote: data:    [0] 0Lyh /usr/bin/nodejs server.js 9064    9301    /home/username/.forever/0Lyh.log 0:0:3:17.487
    remote: From /home/username/node-todo
    remote:    5f31fd7..5bc7be5  master     -> origin/master
    remote: From /home/username/node-todo
    remote:  * branch            master     -> FETCH_HEAD
    remote: Updating 5f31fd7..5bc7be5
    remote: Fast-forward
    remote:  config/database.js | 2 +-
    remote:  1 file changed, 1 insertion(+), 1 deletion(-)
    remote: npm WARN package.json node-todo@0.0.0 No repository field.
    remote: npm WARN package.json node-todo@0.0.0 No license field.
    remote: warn:    --minUptime not set. Defaulting to: 1000ms
    remote: warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
    remote: info:    Forever processing file: /home/username/node-todo/server.js
    To username@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

Po dokončení tohoto příkazu, pokuste se aktualizovat aplikaci ve webovém prohlížeči. Je třeba vidět, že je seznam úkol uvedené v této prázdné a už vázané sdílené nasazeném instanci MongoDB.

Dokončete kurzu Pojďme měnit jiné, viditelnější. V počítači vývoj otevřete soubor node-todo/public/index.html editoru Oblíbené. Vyhledejte záhlaví jumbotron a změňte název z "Jsem úkol aholic" k "Jsem aholic úkol na Azure!".

Teď Pojďme potvrdit:

    git commit -am "Azurify the title"

Tento čas, Pojďme změnu publikovat Azure před jeho na GitHub repo odesláním:

    git push azure master

Po dokončení tohoto příkazu aktualizujte na webovou stránku a zobrazí se změny. Protože budou vypadat dobře, nabízená změnu zpět na začátek vzdálené: 

    git push origin master

## <a name="next-steps"></a>Další kroky
Tento článek ukázal, jak chcete pořídit aplikace Node.js a nasadit na virtuálních počítačích Linux spuštěné v Azure. Další informace o Linux virtuálních počítačích v Azure najdete v tématu [Úvod do Linux na Azure](/documentation/articles/virtual-machines-linux-introduction/).
    
Další informace o tom, jak můžete vyvíjet aplikace Node.js v Azure najdete v článku [Středisko pro vývojáře Node.js](/develop/nodejs/).
