<properties
   pageTitle="Nasazení Azure virtuálního počítače s Chef | Microsoft Azure"
   description="Naučte se používat Chef automatické virtuálního počítače nasazení a konfiguraci na Microsoft Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="diegoviso"
   manager="timlt"
   tags="azure-service-management,azure-resource-manager"
   editor=""/>

<tags ms.service="virtual-machines-windows" ms.workload="infrastructure-services"
ms.tgt_pltfrm="vm-multiple"
ms.devlang="na"
ms.topic="article"
ms.date="05/19/2015"
ms.author="diviso"/>

# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Automatizace nasazení Azure virtuálního počítače s Chef

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Chef je skvělým nástrojem pro doručování automatizaci a vyplňování konfigurace stavu.

Náš nejnovější verzi cloudu rozhraní api Chef nabízí Bezproblémová integrace s Azure, poskytuje možnost zřízení a nasazení konfigurace státy pomocí jednoho příkazu.

V tomto článku se budete ukazují, jak nastavit prostředí Chef zřízení Azure virtuálních počítačích a provede vás vytvořením zásadu nebo "Kuchařka" a potom s nasazováním ve tento kuchařka Azure virtuálního počítače.

A jdeme!

## <a name="chef-basics"></a>Základní informace o Chef

Než začnete, můžu byste že zkontrolovat základních koncepcí Chef. Je skvělé materiálu <a href="http://www.chef.io/chef" target="_blank">tady</a> a můžu doporučujeme, abyste před pokusem tohoto návodu máte snadné pro čtení. Základní informace se bude recap ale před můžeme začít pracovat.

Následující obrázek znázorňuje uceleném architektura Chef.

![][2]

Chef má tři hlavní architektonické součásti: Chef Server, Chef klienta (uzly) a Chef Workstation.

Chef Server je naše bod pro správu a pro Chef Server dvěma způsoby: hostovanou řešení nebo místní řešení. Použijeme hostovanou řešení.

Klient Chef (uzly) je agent, který je umístěn na serverech, mezi nimiž spravujete.

Chef Workstation je naše správce workstation kde můžeme vytvořit naše zásady a provést naše příkazy pro správu. Jsme spuštění příkazu **nůž** z Chef Workstation ke správě naší infrastruktury.

Je také pojem "Cookbooks" a "Recepty". Jsou efektivní zásady jsme definice a použití naše servery.

## <a name="preparing-the-workstation"></a>Příprava workstation

Umožňuje nejdřív Příprava stanice. Používám standardní workstation Windows. Potřebujeme vytvořit adresář, který chcete uložit naši stránku věnovanou konfigurace soubory a cookbooks.

Nejdřív vytvořte adresář s názvem C:\chef.

Vytvořte druhý adresář s názvem c:\chef\cookbooks.

Teď potřebujeme naše Azure nastavení soubor stáhnout, aby Chef mohli komunikovat s naše Azure předplatného.

Stáhněte si vaše nastavení z [tady](https://manage.windowsazure.com/publishsettings/)publikování.

Uložte soubor nastavení publikování v C:\chef.

##<a name="creating-a-managed-chef-account"></a>Vytvoření spravované Chef účtu

Hostovanou účet Chef [tady](https://manage.chef.io/signup).

Během procesu registrace se zobrazí výzva k vytvoření nové organizace.

![][3]

Po vytvoření organizaci stáhněte starter kit.

![][4]

> [AZURE.NOTE] Pokud se zobrazí upozornění, že klíče, nastaví se výzva, je ok postupuje se podle máme bez existující infrastruktury nakonfigurované ještě.

Tento soubor zip kit starter obsahuje vaše organizace konfigurace soubory a klíče.

##<a name="configuring-the-chef-workstation"></a>Konfigurace Chef workstation

Extrahuje obsah starter.zip chef k C:\chef.

Zkopírujte všechny soubory v části chef starter\chef repo\.chef do adresáře c:\chef.

Adresáře by nyní vypadat podobně jako v následujícím příkladu.

![][5]

Teď byste měli mít čtyři souborů včetně souboru Azure publikování v kořenovém c:\chef.

Soubory PEM obsahují vaší organizace a privátních klíčů správce pro komunikaci během knife.rb soubor obsahuje konfiguraci nůž. Je třeba upravit tento soubor knife.rb.

Otevřete soubor v editoru podle výběru a změnit "cookbook_path" odebráním /. / z cesty, takže vypadá tak, jak ukazuje následující.

    cookbook_path  ["#{current_dir}/cookbooks"]

Přidejte následující text také řádek uvědomění si název svého Azure publikování souboru nastavení.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Soubor knife.rb nyní by měla vypadat podobně jako v následujícím příkladu.

![][6]

Tyto řádky zajistí, že nůž odkazuje adresáři cookbooks pod c:\chef\cookbooks a taky používá naše nastavení publikování Azure soubor během Azure operací.

## <a name="installing-the-chef-development-kit"></a>Instalace Chef Development Kit

[Stažení a instalace](http://downloads.getchef.com/chef-dk/windows) ChefDK (Chef Development Kit) nastavit počítači Chef.

![][7]

Nainstalujte výchozího umístění c:\opscode. Tuto instalaci bude trvat zhruba 10 minut.

Potvrďte, že proměnná PATH obsahuje položek C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Pokud nejsou nějaké, ujistěte se, že přidáte stezky!

*POZNÁMKA: POŘADÍ CESTA JE DŮLEŽITÉ!* Není-li opscode cesty ve správném pořadí budete mít problémy.

Restartujte počítači, než budete pokračovat.

Dále jsme nainstaluje koncovku nůž Azure. Díky tomu nůž s "Modul plug-in Azure".

Spusťte tento příkaz.

    chef gem install knife-azure ––pre

> [AZURE.NOTE] Argument – pre zajišťuje, že dostáváte RC nejnovější verzi Azure nůž modul plug-in, které poskytuje přístup k sadě nejnovější rozhraní API.

Je pravděpodobné, že počet závislostí také nainstaluje ve stejnou dobu.

![][8]


Abyste měli jistotu, že všechno, co je správně nastavený, spusťte tento příkaz.

    knife azure image list

Pokud všechno správně nakonfigurovaný, zobrazí se seznam dostupných Azure obrázky a projděte si.

Blahopřejeme. Stanice nastavenou!

##<a name="creating-a-cookbook"></a>Vytvoření kuchařky

Kuchařky použijete Chef ke definují množinu příkazy, které chcete spustit na spravované klientem. Vytvoření kuchařky je jednoduchý a jsme naše kuchařka šablony pomocí příkazu **chef generovat kuchařka** . Můžu budou telefonicky žádat webovém serveru kuchařka jako libovolný text zásadu, která automaticky nasadí IIS.

V části adresáři C:\Chef spusťte tento příkaz.

    chef generate cookbook webserver

To vygeneruje sadu souborů v adresáři C:\Chef\cookbooks\webserver. Teď potřebujeme sadu příkazy, které že budeme rádi naše Chef klienta na naše spravovaných virtuálního počítače provést definovat.

Příkazy jsou uložené v souboru default.rb. V tomto souboru můžu budete definice sady příkazů, nainstaluje služby IIS, spustí IIS a zkopíruje soubor šablony do složky kořenových.

Upravit soubor C:\chef\cookbooks\webserver\recipes\default.rb a přidejte následující řádky.

    powershell_script 'Install IIS' do
        action :run
        code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
        action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
        source 'Default.htm.erb'
        rights :read, 'Everyone'
    end

Až budete hotovi, uložte soubor.

## <a name="creating-a-template"></a>Vytvoření šablony

Jak můžeme jsme zmínili dříve, potřebujeme Generovat šablonu souboru, který bude sloužit jako stránce default.html.

Spusťte tento příkaz Generovat šablonu.

    chef generate template webserver Default.htm

Teď přejděte k souboru C:\chef\cookbooks\webserver\templates\default\Default.htm.erb. Upravte soubor přidáním jednoduchý kód "Vítáme" HTML a potom soubor uložte.



## <a name="upload-the-cookbook-to-the-chef-server"></a>Nahrát kuchařka Chef server

V tomto kroku jsou pořizovat kopii kuchařka, kterou jsme vytvořili v naší místním počítači jsme nahrávání pro službu hostované Chef. Po nahrání kuchařka se zobrazí na kartě **zásady** .

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Nasazení virtuálního počítače s nůž Azure

Nyní změníme nasazení Azure virtuálního počítače a použít kuchařka "Webserver", které bude nainstalovat naší služby IIS služby a výchozí webovou stránku.

K tomuto účelu použijte příkaz **server azure nůž vytvořit** .

Mám příklad příkazu, zobrazí se další.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

Parametry jsou zřejmé. Nahraďte konkrétní proměnných a spustit.

> [AZURE.NOTE] Až příkazového řádku se mi taky automatizace pravidel koncový bod sítě filtru pomocí parametr – tcp koncové body. Byla otevřená porty 80 a 3389 zajistit přístup k Moje webovou stránku a RDP relace.

Po spuštění příkazu Přejít na portál Azure a zobrazí se v počítači docházet k poskytování.

![][13]

Do příkazového řádku se zobrazí další.

![][10]

Po dokončení nasazení jsme by se připojit k webové službě přes port 80 jako jsme to port otevře, když jsme zřízení virtuálního počítače pomocí příkazu nůž Azure. Tento virtuální počítač je pouze virtuálního počítače do cloudové služby, budete ji připojit pomocí adresy url služby cloudu.

![][11]

Jak vidíte, získali kreativní s kód HTML.

Nezapomeňte, že jsme také můžete připojit přes RDP relaci z Azure klasické portálu prostřednictvím port 3389.

Můžu ať že bylo užitečné! Přejděte a spuštěním infrastrukturu jako kód cestu s Azure dnes!


<!--Image references-->
[2]: ./media/virtual-machines-windows-chef-automation/2.png
[3]: ./media/virtual-machines-windows-chef-automation/3.png
[4]: ./media/virtual-machines-windows-chef-automation/4.png
[5]: ./media/virtual-machines-windows-chef-automation/5.png
[6]: ./media/virtual-machines-windows-chef-automation/6.png
[7]: ./media/virtual-machines-windows-chef-automation/7.png
[8]: ./media/virtual-machines-windows-chef-automation/8.png
[9]: ./media/virtual-machines-windows-chef-automation/9.png
[10]: ./media/virtual-machines-windows-chef-automation/10.png
[11]: ./media/virtual-machines-windows-chef-automation/11.png
[13]: ./media/virtual-machines-windows-chef-automation/13.png


<!--Link references-->
