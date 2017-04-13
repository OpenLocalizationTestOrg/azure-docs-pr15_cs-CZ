<properties 
    pageTitle="Pomocí kláves SSH systému Windows pro Linux VMs | Microsoft Azure" 
    description="Naučte se generovat a klávesami se SSH na počítači s Windows se připojit k počítači virtuální Linux na Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="squillace" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="rasquill"/>

# <a name="how-to-use-ssh-keys-with-windows-on-azure"></a>Jak SSH pomocí kláves systému Windows Azure

> [AZURE.SELECTOR]
- [Windows](virtual-machines-linux-ssh-from-windows.md)
- [Linux nebo Mac](virtual-machines-linux-mac-create-ssh-keys.md)

Při připojení k Linux virtuálních počítačích (VMs) v Azure byste měli použít [šifrování veřejného klíče](https://wikipedia.org/wiki/Public-key_cryptography) poskytnout bezpečnější způsob, jak se přihlásit k OM Linux. Tenhle proces zahrnuje veřejných a privátních klíčů exchangi pomocí příkazu zabezpečené prostředí (SSH) ověření sami namísto uživatelské jméno a heslo. V heslech se ohroženo hrubou silou útoky, zejména v internetového VMs například webových serverů. Tento článek obsahuje přehled SSH klíče a jak k vygenerování klíčů odpovídající na počítači s Windows.


## <a name="overview-of-ssh-and-keys"></a>Základní informace o SSH a klíče

Můžete zabezpečené přihlášení k Linux OM pomocí veřejné a privátní klíče:

- **Veřejný klíč** je umístěn na Linux OM nebo jiné služby, který chcete použít s veřejným klíčem kryptografický.
- Co budete prezentovat Linux OM při přihlášení se k ověření vaší identity je **privátním klíčem** . Ochrana privátním klíčem. Nesdílet ho.

Tyto veřejné a privátní klíče lze použít na více VMs a služeb. Páru klíčů, nemusí pro každý OM nebo službu, kterou chcete připojit. Podrobnější informace najdete v tématu [Kryptografický veřejným klíčem](https://wikipedia.org/wiki/Public-key_cryptography).

SSH je protokol šifrované připojení, který umožňuje zabezpečené přihlášení nezabezpečenou připojení. Je výchozí protokol připojení pro hostované v Azure VMs Linux. Přestože SSH samotné poskytuje šifrované připojení, používání hesel s připojením k SSH pořád ponechá OM ohroženo hrubou silou útoky nebo uhodnout hesel. Bezpečnější a upřednostňované, způsob, jak připojení k OM pomocí SSH je pomocí těchto veřejných a privátních klíčů, nazývaný také SSH klíče.

Pokud si nepřejete klávesami SSH, můžete pořád přihlášení k vaší Linux VMs pomocí hesla. Pokud vaše OM, nebude vystaven na Internetu, může být dostatečná pomocí hesla. Ale potřebujete ke správě hesla pro každou Linux OM a Udržovat zásady správný hesel a postupy, jako jsou minimální délka hesla a pravidelná aktualizace. Použití SSH klíčů snižuje složitost správy jednotlivé pověření napříč několika VMs.


## <a name="windows-packages-and-ssh-clients"></a>Balíčky systému Windows a SSH klientů

Připojení k a správa Linux VMs v Azure pomocí **ssh** klienta. Počítače se systémem Windows nemají žádné obvykle **ssh** klientem instalaci. Běžné Windows SSH klienty, se kterými můžete nainstalovat jsou součástí následující balíčky:

- [Libovolná pro Windows](https://git-for-windows.github.io/)
- [nátěrové](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
- [MobaXterm](http://mobaxterm.mobatek.net/)
- [Softwaru Cygwin](https://cygwin.com/)

> [AZURE.NOTE] Nejnovější aktualizace Windows 10 výročí obsahuje flám pro Windows. Tato funkce umožňuje spuštění Windows Subsystem Linux a přístup nástroje, například SSH klienta. Flám pro Windows je stále ve vývoji a se považuje za beta verzi. Další informace o flám pro systém Windows najdete v článku [flám na systémem Ubuntu v systému Windows](https://msdn.microsoft.com/commandline/wsl/about).


## <a name="which-key-files-do-you-need-to-create"></a>Klíčové soubory, které je potřeba vytvořit?

Azure vyžaduje aspoň 2048bitový, **ssh-rsa** formátovat veřejné a privátní klíče. Pokud spravujete Azure zdrojů pomocí modelu Klasický nasazení, bude potřeba generovat PEM (`.pem` soubor).

Toto jsou scénáře nasazení a typy souborů, které používáte v každém:

1. klíče **SSH-rsa** jsou potřeba pro jiné typy nasazení pomocí [Azure portál](https://portal.azure.com)a nasazení Správce prostředků pomocí [Rozhraní příkazového řádku Azure](../xplat-cli-install.md).
    - Klávesy se většinou používá musím všechny většina lidí.
2. `.pem`soubor je potřebný k vytvoření VMs pomocí [klasického portálu](https://manage.windowsazure.com). Klávesy taky podporuje klasické nasazení, které používají [Azure rozhraní příkazového řádku](../xplat-cli-install.md).
    - Potřebujete vytvořit tyto další klávesy a certifikáty, pokud spravujete zdroje vytvořené pomocí klasického nasazení modelu.


## <a name="install-git-for-windows"></a>Instalace libovolná pro Windows

Výše uvedené několik balíčků, které obsahují `openssl` nástroj pro systém Windows. Tento nástroj slouží k vytvoření veřejné a privátní klíče. Následující příklady podrobností jak nainstalovat a používat **Libovolná pro systém Windows**, když vyberete ten balíčku dáváte přednost. **Libovolná pro Windows** vám umožňuje přístup k některé další software otevřít zdroj ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) nástrojů a nástrojů, které mohou být užitečné při práci s Linux VMs.

1. Stažení a instalace **Libovolná pro Windows** z následujících umístění: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).

2. Pokud potřebujete konkrétně nezměníte, přijměte výchozí nastavení během instalace.

3. Spuštění **Flám libovolná** z **nabídky Start** > **Libovolná** > **Libovolná flám**. Konzole vypadá podobně jako v následujícím příkladu:

    ![Prostředí libovolná flám pro Windows](./media/virtual-machines-linux-ssh-from-windows/git-bash-window.png)


## <a name="create-a-private-key"></a>Vytvoření privátním klíčem

1. V okně aplikace **Libovolná flám** pomocí `openssl.exe` vytvořit privátním klíčem. Následující příklad vytvoří klíč s názvem `myPrivateKey` a certifikát s názvem `myCert.pem`:

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    Výstup vypadá podobně jako v následujícím příkladu:

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key to 'myPrivateKey.key'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

2. Příjem pokynů pro země název, umístění, název organizace, atd.

3. V aktuálním pracovním adresáři se vytvoří nový privátním klíčem a certifikát. Doporučené postupy zabezpečení měli nastavit oprávnění na privátním klíčem tak, aby pouze je můžete používat:

    ```bash
    chmod 0600 myPrivateKey
    ```

4. Pokud potřebujete přidávání a používání zdrojů klasické, převést `myCert.pem` k `myCert.cer` (X509 s kódováním DER certifikát). Tento krok proveďte jenom v případě, že budete potřebovat pro správu konkrétně starší klasické zdroje. 

    Převedení certifikátu pomocí následujícího příkazu:

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a>Privátní klíč vytvořen nátěrové

Nátěrové je běžné SSH klient pro systém Windows. Máte volno používat všechny SSH klienta, který chcete. Pokud chcete použít nátěrové, je potřeba vytvořit další typ klíč - privátní klíč nátěrové (PPK). Pokud nechcete používat nátěrové, tuto část přeskočte.

Následující příklad vytvoří tento další soukromý klíč pro nátěrové používat:

1. K převodu privátním klíčem privátní klíč RSA PuTTYgen srozumitelný použijte **Libovolná flám** . Následující příklad vytvoří klíč s názvem `myPrivateKey_rsa` z existující klíč s názvem `myPrivateKey`:

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    Doporučené postupy zabezpečení měli nastavit oprávnění na privátním klíčem tak, aby pouze je můžete používat:

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```

2. Stáhnout a spustit PuTTYgen z následujících umístění: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

3. Klikněte na nabídku: **souboru** > **zatížení soukromý klíč**

4. Vyhledejte privátním klíčem (`myPrivateKey_rsa` v předchozím příkladu). Výchozí adresář při spuštění **Libovolná flám** je `C:\Users\%username%`. Změna filtru soubor zobrazíte **všechny soubory (\*.\*)**:

    ![Načtení existujícího privátním klíčem do PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/load-private-key.png)

5. Klikněte na **Otevřít**. Výzva označuje, že klávesu úspěšně importovaných:

    ![Úspěšně importovaných klávesy PuTTYgen](./media/virtual-machines-linux-ssh-from-windows/successfully-imported-key.png)

6. Kliknutím na **OK** zavřete okno příkazového řádku.

7. Veřejný klíč zobrazený v horní části okna **PuTTYgen** . Zkopírujte a vložte tento veřejným klíčem do portálu Azure nebo správce prostředků Azure šablonu při vytváření OM Linux. Můžete taky kliknout **Uložit veřejným klíčem** můžete uložit kopii do počítače:

    ![Uložit nátěrové veřejné klíče](./media/virtual-machines-linux-ssh-from-windows/save-public-key.png)

    Následující příklad ukazuje, jak by zkopírujte a vložte tento veřejným klíčem do portálu Azure při vytváření OM Linux. Veřejný klíč obvykle potom uložený ve `~/.ssh/authorized_keys` na nové OM.

    ![Veřejný klíč použití při vytváření virtuálního počítače na portálu Azure](./media/virtual-machines-linux-ssh-from-windows/use-public-key-azure-portal.png)

7. Po návratu do **PuTTYgen**, klikněte na **uložit soukromý klíč**:

    ![Uložte soubor nátěrové privátním klíčem](./media/virtual-machines-linux-ssh-from-windows/save-ppk-file.png)

    > [AZURE.WARNING] Výzva zeptá, pokud chcete pokračovat bez zadání přístupové heslo pro kód. Přístupové heslo je stejné jako heslo připojené k privátním klíčem. I kdyby uživatel získat privátním klíčem, bude pořád nebude moct ověření pomocí jenom klávesu. Potřebují byste taky heslo. Bez přístupové heslo Pokud uživatel obdrží privátním klíčem se můžete přihlásit k OM nebo služby, který používá tuto klávesu. Doporučujeme, že jste vytvořili heslo. Ale pokud zapomenete heslo, nejde žádným způsobem jeho obnovení.

    Pokud chcete zadat přístupové heslo, klikněte na **Ne**, zadejte heslo v hlavním okně PuTTYgen a klikněte na tlačítko **Uložit privátním klíčem** . Jinak klikněte na **Ano** pokračujte bez zadání volitelné heslo.

8. Zadejte název a umístění pro uložení souboru PPK.


## <a name="use-putty-to-ssh-to-a-linux-machine"></a>Použití nátěrové SSH k počítači Linux

Znovu nátěrové je běžné SSH klient pro systém Windows. Máte volno používat všechny SSH klienta, který chcete. Podle těchto kroků podrobnosti o použití privátním klíčem k ověření vaší OM Azure pomocí SSH. Postup je podobný jiných SSH klíčové z hlediska museli načíst privátním klíčem ověření klientů SSH připojení.

1. Stáhnout a spustit nátěrové z následujících umístění: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

2. Zadejte název hostitele nebo IP adresu OM z portálu Microsoft Azure:

    ![Otevření nové připojení nátěrové](./media/virtual-machines-linux-ssh-from-windows/putty-new-connection.png)

3. Nejdřív **Otevřít**, klikněte na **připojení** > **SSH** > **Auth** kartu. Vyhledejte a vyberte soukromý klíč:

    ![Vyberte svůj nátěrové soukromý klíč pro ověřování](./media/virtual-machines-linux-ssh-from-windows/putty-auth-dialog.png)

4. Klikněte na **Otevřít** pro připojení k virtuálního počítače
 

## <a name="next-steps"></a>Další kroky
Můžete taky generovat veřejné a privátní klíče [pomocí OS X a Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Další informace o flám pro Windows a o výhodách OSS nástroje snadno dostupné na počítači s Windows najdete v článku [flám na systémem Ubuntu v systému Windows](https://msdn.microsoft.com/commandline/wsl/about).

Pokud máte potíže při používání SSH se připojit k Linux VMs, najdete v tématu [Poradce při potížích s SSH spojení angličtině Linux Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).