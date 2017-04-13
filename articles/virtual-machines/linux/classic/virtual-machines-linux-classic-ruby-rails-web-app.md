<properties
    pageTitle="Hostovat skutečné na webu profilů na Linux OM | Microsoft Azure"
    description="Nastavte si a hostovat skutečné na webu profilů založené na Azure pomocí virtuálního počítače Linux."
    services="virtual-machines-linux"
    documentationCenter="ruby"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="web"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Skutečné na profilů webová aplikace na OM Azure

Tento kurz znázorňuje hostovat skutečné na webu profilů na Azure pomocí virtuálního počítače Linux.  

Tento kurz byl ověřen pomocí systémem Ubuntu serveru 14.04 l. Pokud používáte jiného distribučního Linux, můžete zkusit změnit pokynů k instalaci profilů.

> [AZURE.IMPORTANT] Azure obsahuje dva různé nasazení modely pro vytváření grafů a práci s prostředky: [Správce zdrojů a klasické](../../../resource-manager-deployment-model.md).  Tento článek se věnuje pomocí klasické nasazení modelu. Microsoft doporučuje, že většina nových nasazení použití modelu správce prostředků.

## <a name="create-an-azure-vm"></a>Vytvoření Azure OM

Začněte tak, že vytvoříte Azure OM s obrázkem Linux.

Pokud chcete vytvořit OM, můžete portálu Azure klasické nebo Azure rozhraní příkazového řádku (rozhraní příkazového řádku).

### <a name="azure-management-portal"></a>Portál Správa Azure

1. Přihlaste se k [Azure klasické portálu](http://manage.windowsazure.com)
2. Klikněte na **Nový** > **Výpočet** > **virtuálního počítače** > **vytvořit**. Vyberte obrázek Linux.
3. Zadejte heslo.

Poté, co máte k dispozici OM, klikněte na název OM a klikněte na **řídicí panel**. Najdete koncový bod SSH zařazený do kategorie **SSH podrobnosti**.

### <a name="azure-cli"></a>Azure rozhraní příkazového řádku

Postupujte podle pokynů v tématu [Vytvoření virtuálního počítače systém Linux][vm-instructions].

Poté, co máte k dispozici OM, můžete získat koncový bod SSH spuštěním následujícího příkazu:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Skutečné nainstalovat profilů

1. Připojení k OM pomocí SSH.

2. Z relace SSH skutečné nainstalovat OM pomocí následujících příkazů:

        sudo apt-get update -y
        sudo apt-get upgrade -y
        sudo apt-get install ruby ruby-dev build-essential libsqlite3-dev zlib1g-dev nodejs -y

    Instalaci může trvat několik minut. Až se dokončí, použijte k ověření, že je nainstalovaný skutečné tento příkaz:

        ruby -v

    Tento příkaz vrátí verzi skutečné, která je nainstalovaná.

3. Zadejte následující příkaz a nainstalujte profilů:

        sudo gem install rails --no-rdoc --no-ri -V

    Použití – bez rdoc a – bez ri příznaky Přeskočit instalaci dokumentace, která je rychlejší.
    Tento příkaz bude pravděpodobně trvat dlouho chcete provést, tak přidávání -V se zobrazí informace o průběhu instalace.

## <a name="create-and-run-an-app"></a>Vytvoření a spuštění aplikace

Přihlaste se pořád prostřednictvím SSH, spusťte následující příkazy:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

Příkaz [Nová](http://guides.rubyonrails.org/command_line.html#rails-new) vytvoří novou aplikaci profilů. Příkaz [server](http://guides.rubyonrails.org/command_line.html#rails-server) spustí WEBrick webového serveru, které jsou součástí profilů. (Pro použití v praxi, bude pravděpodobně chcete použít jiný server, například Unicorn nebo osobní.)

Měli byste vidět výstup podobně jako tento.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C to shutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Přidání koncový bod

1. Přejděte na [portál Azure klasické] [ management-portal] a vyberte svůj OM.

    ![seznam virtuálního počítače][vmlist]

2. Vyberte **koncové body** v horní části stránky a potom klikněte na tlačítko **+ Přidat koncového bodu** v dolní části stránky.

    ![Stránka Koncové body][endpoints]

3. V dialogovém okně **Přidat koncový bod** vyberte "Přidat koncový bod samostatného" a klikněte na **Další** šipky.

    ![Nový dialog koncový bod][new-endpoint1]

3. V dialogovém okně další dialogové okno zadejte následující informace:

    * **Název**: HTTP

    * **Protocol (protokol)**: TCP

    * **Veřejný PORT**: 80

    * **Soukromé portu**: 3000

    Tím vytvoříte veřejný port 80, které budou směrovat přenosy v síti na soukromé port 3000, pokud je server profilů přijímá.

    ![Nový dialog koncový bod][new-endpoint]

4. Zaškrtněte políčko Uložit koncový bod.

5. Zprávy by se měly oznamující **Aktualizace v průběhu**. Jakmile se tato zpráva zmizí, je aktivní koncový bod. Nyní může testovat aplikaci tak, že přejdete na název DNS virtuálního počítače. Na webu by měla vypadat podobně jako tento:

    ![výchozí stránky profilů][default-rails-cloud]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste provedli většina kroků ručně. V provozním prostředí by psaní aplikace pro na vývoj počítač a ho nasadit do OM Azure. Většina provozním prostředí také hostovat profilů aplikace ve spojení s jiným procesem serveru Apache ATP NginX, která zpracovává žádosti o směrování do několika instancí aplikace profilů podávání statické prostředky. Další informace najdete v tématu http://rubyonrails.org/deploy/.

Další informace o skutečné na profilů, navštěvujte blog o [skutečné na profilů vodítka][rails-guides].

Použití služby Azure skutečné aplikace, najdete tady:

* [Uložit Nestrukturovaná data pomocí objekty BLOB][blobs]

* [Úložiště klíč/dvojice pomocí tabulek][tables]

* [Zobrazování obsahu velkou šířkou pásma s síť pro doručování obsahu][cdn-howto]

<!-- WA.com links -->
[blobs]: ../../../storage/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]: https://azure.microsoft.com/develop/ruby/app-services/
[management-portal]: https://manage.windowsazure.com/
[tables]: ../../../storage/storage-ruby-how-to-use-table-storage.md
[vm-instructions]: ../../virtual-machines-linux-classic-createportal.md

<!-- External Links -->
[rails-guides]: http://guides.rubyonrails.org/
[sqlite3]: http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]: ./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
