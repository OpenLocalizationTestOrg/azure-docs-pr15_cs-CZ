<properties
    pageTitle="Vytvoření poznámkového bloku Jupyter/IPython | Microsoft Azure"
    description="Naučte se nasadit Jupyter/IPython Poznámkový blok na počítači virtuální Linux vytvořená pomocí nasazení modelu správce zdrojů v Azure."
    services="virtual-machines-linux"
    documentationCenter="python"
    authors="crwilcox"
    manager="wpickett"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="python"
    ms.topic="article"
    ms.date="11/10/2015"
    ms.author="crwilcox"/>

# <a name="jupyter-notebook-on-azure"></a>Jupyter poznámkového bloku na Azure

[Jupyter projektu](http://jupyter.org), dřív [IPython projektu](http://ipython.org)obsahuje sadu nástrojů pro matematickém výpočetních pomocí výkonné interaktivní prostředí, které kombinují spuštění kódu při vytváření živou výpočetní dokumentu. Tyto soubory poznámkových bloků může obsahovat libovolný text, matematické vzorce, zadávání kódu, výsledky, grafika, videa a jiný druh médií může zobrazení moderní webového prohlížeče. Jestli novinka opravdu Python a chcete další informace v tématu zábavnou interaktivní prostředí nebo proveďte některé vážně výpočetních paralelní/technické Jupyter poznámkového bloku se obzvlášť vyplatí.

![Snímek obrazovky](./media/virtual-machines-linux-jupyter-notebook/ipy-notebook-spectral.png) pomocí SciPy a Matplotlib balíčků a analyzujte data struktuře záznamu zvuku.


## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter dvěma způsoby: Azure poznámkových bloků nebo vlastního nasazení
Azure poskytuje služby, které můžete [rychle začít používat Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Pomocí služby Azure Poznámkový blok můžete snadno získat přístup k jeho Jupyter webu přístupné rozhraní pro scalable systémové prostředky s výkonem Python a mnoho knihoven.  Protože instalace zpracovány službou, uživatelé přístup k tyto materiály bez nutnosti pro správu a konfiguraci uživatelem.

Pokud poznámkový blok služby nefunguje k danému nám prosím dál v tomto článku, který se vám ukáže, jak pro nasazení Jupyter poznámkového bloku na Microsoft Azure pomocí Linux virtuálních počítačích (VMs).

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Vytváření a konfigurace virtuálního počítače na Azure

Cílem prvního kroku je vytvořit virtuální počítač (OM) se systémem Azure.
Tento OM dokončení operační systém v cloudu a se použijí k spuštění Jupyter Poznámkový blok. Je možné spuštění virtuálních počítačích Linux a Windows Azure a budeme se zabývat těmito oblastmi nastavení Jupyter na obou typů virtuálních počítačích.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Vytvoření OM Linux a otevřete port pro Jupyter

Postupujte podle pokynů vzhledem k tomu, [tady] [ portal-vm-linux] vytvoření virtuálního počítače *se systémem Ubuntu* rozdělení. Tento kurz používá se systémem Ubuntu serveru 14.04 l. Budete předpokladu, že jméno uživatele *azureuser*.

Po virtuální počítač nasadí potřebujeme otevřete pravidlo zabezpečení na skupiny zabezpečení sítě.  Z portálu Microsoft Azure přejděte do **Skupiny zabezpečení sítě** a otevřete kartu odpovídá vaší OM skupiny zabezpečení. Budete muset přidat pravidlo příchozí zabezpečení s tímto nastavením: u protokolu **TCP** **\*** pro zdroj (veřejné) portů a **9999** pro port cíl (soukromá).

![Snímek obrazovky](./media/virtual-machines-linux-jupyter-notebook/azure-add-endpoint.png)

Ve skupině zabezpečení sítě, klikněte na **Síť rozhraní** a Všimněte si na **Veřejnou IP adresu** , jak bude potřeba se připojit k OM v dalším kroku.

## <a name="install-required-software-on-the-vm"></a>Vyžaduje software nainstalovat OM

Spuštění poznámkového bloku Jupyter na naše OM, jsme třeba nejprve nainstalovat Jupyter a jeho závislosti. Připojení k vaší linux OM pomocí ssh a uživatelského jména a hesla spárování jste zvolili při vytváření OM. V tomto kurzu budeme pomocí nátěrové a připojit se z Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Instalace Jupyter systémem Ubuntu
Nainstalujte Anaconda, rozdělení python vědy Oblíbené data, používat jeden z odkazů uvedených z [Poměrně analýzy](https://www.continuum.io/downloads).  K zápisu tohoto dokumentu následujících odkazů jsou nejaktuálnější verze datum.

#### <a name="anaconda-installs-for-linux"></a>Instalace anaconda Linux
<table>
  <th>Python 3.4</th>
  <th>Python 2.7</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64-bit</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64-bit</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32bitové</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32bitové</href>
    </td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a>Instalace Anaconda3 2.3.0 64-bit na systémem Ubuntu
Jako příklad jedná, jak lze nainstalovat Anaconda systémem Ubuntu

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Snímek obrazovky](./media/virtual-machines-linux-jupyter-notebook/anaconda-install.png)


### <a name="configuring-jupyter-and-using-ssl"></a>Konfigurace Jupyter a protokolem SSL
Po instalaci potřebujeme instalační soubory konfigurace pro Jupyter chvíli trvat. Pokud vám to přinést potíže při s konfigurace Jupyter mohou být užitečné najdete v [Dokumentaci Jupyter serverem Poznámkový blok](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).

Další jsme `cd` k adresáři profilu vytvořit certifikát SSL a úpravy konfiguračního souboru profily.

Na Linux zadejte následující příkaz.

    cd ~/.jupyter

Pomocí následujícího příkazu vytvořit certifikát SSL (Linux a Windows).

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Poznámka: protože vytváříme certifikátu podepsaného svým držitelem SSL při připojování k poznámkovému bloku prohlížeči získáte upozornění zabezpečení.  Dlouhodobé výrobní používat budete chtít použít správně podepsané certifikát přidružený k vaší organizace.  Protože správa certifikátů je nad rámec Tato ukázka, jsme bude aby se na certifikát podepsaný svým držitelem pro tuto.

Kromě použití certifikát, musíte taky poskytují hesla můžete chránit před neoprávněným použitím poznámkového bloku.  Z bezpečnostních důvodů Jupyter používá šifrovaných hesel v souboru konfigurace, budete muset nejdřív šifrování svoje heslo.  IPython poskytuje nástroj k tomu nevyzve; na příkazovém řádku spusťte tento příkaz.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Zobrazí výzvu k potvrzení a heslo a potom vytiskne heslo. Poznámka: pro podle následujících pokynů.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

Dále jsme upravit konfigurační soubor na profil, který je `jupyter_notebook_config.py` soubor v adresáři se.  Poznámka: Tento soubor nemusí existenci – stačí vytvořit případě.  Tento soubor obsahuje počet polí a ve výchozím nastavení všechny jsou změněna na komentář.  Tento soubor můžete otevřít v jakémkoli textovém editoru titulků a měli zajistěte, aby měl aspoň následující obsah. **Nezapomeňte místo c.NotebookApp.password v konfiguraci s sha1 v předchozím kroku**.

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a>Spuštění poznámkového bloku Jupyter

V tomto okamžiku jsme připraveni k vytvoření poznámkového bloku Jupyter. K tomuto účelu přejděte na adresář, který chcete ukládání poznámkových bloků v a spusťte Poznámkový blok Jupyter server pomocí následujícího příkazu.

    /anaconda3/bin/jupyter-notebook

Teď byste měli být mít přístup k Jupyter poznámkového bloku na adrese `https://[PUBLIC-IP-ADDRESS]:9999`.

Při prvním přístup k poznámkovému bloku, na přihlašovací stránku zeptá svoje heslo. A jakmile se přihlásíte, zobrazí se "Jupyter Notebook řídicí panel", což je kontrole Centrum pro všechny operace Poznámkový blok.  Na této stránce můžete vytvořit nové poznámkové bloky a otevřít existující.

![Snímek obrazovky](./media/virtual-machines-linux-jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a>Jupyter poznámkového bloku

Když kliknete na tlačítko **Nová** , zobrazí se následující úvodní stránka.

![Snímek obrazovky](./media/virtual-machines-linux-jupyter-notebook/jupyter-untitled-notebook.png)

Do oblasti označené `In []:` Dotázat se oblasti pro zadávání a Tady můžete zadat libovolný platný Python kód a se spustí, když kliknete `Shift-Enter` nebo klikněte na ikonu "Přehrávání" (trojúhelník směřující vpravo na panelu nástrojů).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>Výkonné paradigma: live výpočetní dokumenty s multimediálních

Poznámkový blok samotné by měli mít pocit velmi natural všem uživatelům, kteří pořídil Python a textovém editoru, protože se některé způsoby kombinaci obou: spustíte bloky Python kódu, ale můžete taky ponechat poznámek a další text změníte nastavením stylu buňky z "Kódu" k "Markdown" pomocí rozevírací nabídky na panelu nástrojů.

Jupyter je víc než textový editor jako umožňuje společným výpočtu a rozsáhle médií (text, grafiku, video a prakticky cokoliv můžete zobrazit moderní webový prohlížeč). Můžete používat, text, kód, videa a další!

![Snímek obrazovky](./media/virtual-machines-linux-jupyter-notebook/jupyter-editing-experience.png)

A s výkonem je Python mnoho pracovníků knihovny vědeckých a technických computing, následující snímek obrazovky, že jednoduchého výpočtu lze provádět s stejně snadné jako analýzy složitých sítě, všechny v jednom prostředí.

Tento paradigma společným power moderní webu s živou výpočtu nabízí mnoho možností a je ideální pro cloudu; Poznámkový blok můžete použít:

* Jako výpočetní zápisník pořizovat záznam průzkumné pracujte na problém.

* Sdílení výsledků s kolegy, v "live" výpočetní formuláři nebo v výtisk formáty (HTML, PDF).

* Distribuce a předvádění živou výukových materiálů, které zahrnují výpočtu, takže studenty můžete okamžitě experimentovat s reálným kód, upravit a znovu ho spusťte interaktivně.

* Zajištění "spustitelný papírů", které vedou k výsledků výzkumu způsobem, který lze okamžitě reprodukovat, ověření a prodloužilo o ostatním.

* Jako platformu pro spolupráci výpočetních: více uživatelé mohou přihlásit na stejný server Poznámkový blok sdílet výpočetní relaci.


Přejděte do zdrojového kódu IPython [úložiště][]najdete celý adresář s příklady Poznámkový blok, které si můžete stáhnout a pak vyzkoušet na vlastních OM Jupyter Azure.  Jednoduše Stáhnout `.ipynb` soubory z webu a nahrajte je na řídicím panelu Azure OM poznámkového bloku (nebo stáhnout přímo do OM).

## <a name="conclusion"></a>Uzavření

Poznámkový blok Jupyter poskytuje výkonné rozhraní pro přístup k interaktivní power ekosystému Python na Azure.  Zahrnuje širokou škálu případy použití včetně jednoduché průzkum a výukových Python, analýza dat a vizualizaci, simulace a paralelní výpočty. Výsledný Poznámkový blok dokumenty obsahovat úplný záznam výpočty, které provádí a můžete sdílet s ostatními uživateli Jupyter.  Poznámkový blok Jupyter mohou sloužit jako místní aplikace, ale je je ideální pro nasazení cloudu na Azure

Základní funkce Jupyter je také dostupná ve Visual Studiu prostřednictvím [Python Tools for Visual Studio][] (PTVS). PTVS je bezplatného a od Microsoftu, který změní Visual Studio upřesnit Python vývojové prostředí obsahující rozšířeném editoru pomocí technologie IntelliSense ladění, vytváření profilů nebo paralelní modul plug-in otevřít zdroje computing integrace.

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu [Středisko pro vývojáře Python](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[úložiště]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
