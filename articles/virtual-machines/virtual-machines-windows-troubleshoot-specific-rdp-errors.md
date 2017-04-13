<properties
    pageTitle="Konkrétní chybové zprávy RDP pro Azure VMs | Microsoft Azure"
    description="Porozumět tomu, že konkrétní chybové zprávy, které se mohou zobrazit při pokusu pomocí vzdálené plochy připojení k počítači virtuální Windows Azure"
    keywords="Vzdálené plochy chyby, připojení ke vzdálené ploše chyba se nemůže připojit k OM, vzdálené plochy Poradce při potížích s"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/14/2016"
    ms.author="iainfou"/>

# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Řešení problémů s konkrétní chybové zprávy RDP angličtině Windows v Azure
Pokud chcete použít připojení ke vzdálené ploše do virtuálního počítače (OM) Windows Azure, může zobrazit konkrétní chybové zprávy. Tento článek podrobné informace o některých z nejčastěji používaných chybových zpráv narazili spolu s návody na řešení potíží při jejich řešení. Pokud máte problémy s připojením k vaší OM pomocí RDP, ale ne zobrazit konkrétní chybová zpráva, přečtěte si téma [Průvodce pro připojení ke vzdálené ploše pro řešení](virtual-machines-windows-troubleshoot-rdp-connection.md).

Informace o konkrétní chybové zprávy najdete v těchto článcích:

- [Relace byla odpojena, protože neexistují žádné vzdálené plochy licenci servery k dispozici licence](#rdplicense).
- [Vzdálená plocha nenašli počítač "název"](#rdpname).
- [Ověřování došlo k chybě. Místní autorita zabezpečení nelze kontaktovat](#rdpauth).
- [Zabezpečení systému Windows chyby: nefungují svoje přihlašovací údaje](#wincred).
- [Tento počítač nemůže připojit k počítači](#rdpconnect).

<a id="rdplicense"></a>
## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>Relace byla odpojena, protože neexistují žádné vzdálené plochy licenci servery k dispozici licence.

Příčina: 120 dny poskytnuté lhůty licencování role serveru vzdálené plochy vypršela a budete potřebovat k instalaci licence.

Jako alternativu uložení místní kopie souboru RDP z portálu Microsoft a spusťte tento příkaz příkazovém řádku prostředí PowerShell připojení. Tento krok zakáže licencí pro jenom toto připojení:

        mstsc <File name>.RDP /admin

Pokud nepotřebujete skutečně více než dvě souběžné připojení ke vzdálené ploše bude v angličtině, můžete odebrat roli serveru vzdálené plochy správce serveru.

Další informace najdete v tématu v blogovém příspěvku [Azure OM selže s "Vzdálené plochy licence k dispozici žádné servery"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>
## <a name="remote-desktop-cant-find-the-computer-name"></a>Vzdálená plocha nemůžu najít tlačítko "název".

Příčina: Klient vzdálené plochy ve vašem počítači nerozpoznává název počítače v nastavení soubor RDP.

Řešení:

- Pokud jste v síti intranet organizace, ujistěte se, že váš počítač má přístup k proxy serveru a můžete poslat HTTPS přenosy ho.

- Pokud používáte místně uložený soubor RDP, zkuste použít ten, který je generováno aplikací na portálu. Tento krok zajistí, že máte správný název DNS virtuálního počítače nebo Cloudová služba a port koncového bodu OM. Tady je ukázkový soubor RDP generováno aplikací na portálu:

        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Adresa část tento soubor RDP obsahuje:
- Plně kvalifikovaný název domény z cloudové služby, který obsahuje OM ("tailspin-azdatatier.cloudapp.net" v tomto příkladu).

- Externí port TCP koncového bodu přenosů vzdálené plochy (55919).

<a id="rdpauth"></a>
## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Došlo k chybě ověřování. Místní autorita zabezpečení nelze kontaktovat.

Příčina: Do cíle OM nemůžete najít autorita zabezpečení v části uživatelské jméno přihlašovacích údajů.

Kdy je vaše uživatelské jméno v podobě *SecurityAuthority*\\*uživatelské jméno* (Příklad: CORP\User1), část *SecurityAuthority* je OM název počítače (místního úřadu zabezpečení) nebo název domény Active Directory.

Řešení:

- Je-li účet místní bude v angličtině, ujistěte se, že název OM správně napsané.

- Pokud je v doméně služby Active Directory, zkontrolujte název domény.

- Pokud je doménovým účtem služby Active Directory a správně napsaný název domény, zkontrolujte, že řadiče domény k dispozici v této domény. Je běžné potíže v Azure virtuální sítě, které obsahují řadiče domény, aby řadiče domény není k dispozici, protože ještě nezačala. Jako alternativu můžete místního účtu správce místo doménovým účtem.

<a id="wincred"></a>
## <a name="windows-security-error-your-credentials-did-not-work"></a>Chyba zabezpečení systému Windows: nefungují přihlašovacích údajů.

Příčina: Do cíle OM nemůže ověřit název účtu a heslo.

Počítače s Windows můžete ověřit pověření místního účtu nebo účet domény.

- Místní účty, když použijete *název_počítače*\\syntaxe*uživatelské jméno* (Příklad: SQL1\Admin4798).
- Účty domény, použijte *název_domény*\\syntaxe*uživatelské jméno* (Příklad: CONTOSO\peterodman).

Pokud máte propagované OM řadiče domény v nové doménové služby Active Directory, místního účtu správce, kterou jste si zaregistrovali se převedou na odpovídající účet u stejné heslo v nové struktuře a domény. Potom se odstraní místního účtu.

Pokud přihlášenými pomocí místního účtu DC1\DCAdmin a virtuální počítač povýšenými v nové struktuře corp.contoso.com domény řadiče domény, se odstraní místního účtu DC1\DCAdmin a nové doménovým účtem (CORP\DCAdmin) se vytvoří pomocí stejné heslo.

Ujistěte se, že název účtu je název, který virtuálního počítače můžete ověřit jako platný účet a správnost heslo.

Pokud potřebujete změnit heslo místního účtu správce, najdete v článku [Jak obnovit heslo nebo službu ke vzdálené ploše pro virtuálních počítačích Windows](virtual-machines-windows-reset-rdp.md).

<a id="rdpconnect"></a>
## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Tento počítač nemůže připojit k počítači.

Příčina: Účet, který se používá pro připojení právy Vzdálená plocha přihlášení.

Každý počítač Windows má na vzdálené plochy uživatelé místní skupinu, která obsahuje účty a skupiny, které můžete do ní vzdáleně podepsat. Členové skupiny místních správců také mít přístup, i když tyto účty nejsou uvedené v skupiny místních uživatelů Vzdálená plocha. Pro doméně stroje skupiny místních správců obsahuje také správci domény u domény.

Zajistěte účet, který používáte pro připojení ke vzdálené ploše přihlašovací práva. Jako alternativu připojení přes připojení ke vzdálené ploše pomocí domény nebo místního účtu správce. Chcete-li požadovaný účet přidat do skupiny místních uživatelů Vzdálená plocha, použijte modulu snap-in konzoly Microsoft Management Console (**Systémové nástroje > Místní uživatelé a skupiny > skupiny > Uživatelé vzdálené plochy**).


## <a name="next-steps"></a>Další kroky
Pokud žádný z těchto chyb došlo k a máte neznámý problém s připojením pomocí RDP, přečtěte si téma [Průvodce pro připojení ke vzdálené ploše pro řešení](virtual-machines-windows-troubleshoot-rdp-connection.md).

- [Azure balíček diagnostics IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Návody na řešení potíží při přístupu k aplikacím na virtuálního počítače, najdete v článku [Poradce při potížích s přístup k spuštění aplikace v Azure OM](virtual-machines-linux-troubleshoot-app-connection.md).
- Pokud máte problémy se připojujete k Linux OM v Azure pomocí zabezpečené prostředí (SSH), přečtěte si článek [Poradce při potížích s SSH připojení k Linux OM v Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).