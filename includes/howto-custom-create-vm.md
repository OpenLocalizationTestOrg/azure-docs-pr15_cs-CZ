#<a name="how-to-create-a-custom-virtual-machine"></a>Jak vytvořit vlastní virtuálního počítače

*Vlastní* virtuální počítač odkazuje virtuální počítač, který vytvoříte pomocí metody **Z Galerie** , protože umožňuje nastavit další možnosti než metodu **Vytvořit** . Tyto možnosti:

- Další možnosti pro daný obrázek používat k vytvoření virtuálního počítače (OM)
- Připojení k síti virtuální OM
- Přidání OM existující cloudové služby
- Přidání OM do sady dostupnosti

> [AZURE.IMPORTANT] Pokud budete potřebovat virtuálního počítače a použít virtuální sítě, aby se můžete připojit k němu přímo hostname nebo nastavení křížově místní připojení, zkontrolujte, že zadáte virtuální sítě při vytváření virtuálních počítače. Připojení k síti virtuální pouze při vytváření virtuálního počítače je možné konfigurovat virtuálního počítače. Další informace o virtuálních sítí najdete v článku [Azure virtuální síť – přehled](http://go.microsoft.com/fwlink/p/?LinkID=294063).

1. Přihlaste se k [portálu Azure](http://manage.windowsazure.com).

2. Na řádku nabídek klikněte na **Nový**.

3. Klikněte na **Výpočet**, klikněte na **virtuálního počítače**a klikněte na **Z Galerie**.

4. Vyberte obrázek, který chcete použít a potom klikněte na šipku pokračovat.

5. Pokud více verzí obrázku jsou k dispozici ve **Verzi datum vydání**, vyberte verzi, kterou chcete použít.

6. Do pole **Název virtuálního počítače**zadejte název, který chcete použít pro virtuální počítač.

7. Vyberte správné velikosti virtuálního počítače pomocí **osy** a **velikost** . Velikost, kterou vyberete ovlivňuje maximální konfiguraci virtuálního počítače i ceny. Podrobné informace o konfiguraci najdete v článku [virtuální počítač a velikosti cloudové služby Azure](http://go.microsoft.com/fwlink/p/?LinkID=389844).

8. V **Nové uživatelské jméno**zadejte název účtu správce, který chcete použít ke správě serveru.

9. V dialogovém okně **Nové heslo**zadejte silné heslo účtu správce. V dialogovém okně **Potvrdit heslo**zadejte znovu stejné heslo.

10. Klikněte na šipku na pokračovat.

11. Do **Cloudové služby**proveďte jednu z následujících akcí:

    - Pokud je to virtuální počítač první nebo jenom do cloudové služby, vyberte možnost **vytvořit nový cloudové služby**. Do pole **Název DNS cloudové služby**, zadejte název, který používá mezi 3 a 24 malá písmena a číslice. Tento název se změní část identifikátor URI, který slouží ke kontaktování virtuálního počítače prostřednictvím služby cloudu.
    - Pokud tento virtuální počítač se přidá do cloudové služby, vyberte ji v seznamu.

    > [AZURE.NOTE] Další informace o umístění virtuálních počítačích do stejné cloudové služby najdete v článku [jak připojit virtuálních počítačích v cloudové službě](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/).

12. V **Oblasti/spřažení skupiny/virtuální sítě**vyberte oblast, spřažení skupiny nebo virtuální sítě, který chcete použít pro virtuální počítač. Další informace o skupinách spřažení najdete v článku [o spřažení skupiny pro: virtuální sítě](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

13. V **Účtu úložiště**vyberte existující účet úložiště k souboru virtuálního pevného disku nebo použití účtu automaticky generované úložiště. Jen s jedním klientem úložiště jednotlivých oblastech se automaticky vytvoří. Všechny ostatní virtuálních počítačích, vytvořené pomocí tohoto nastavení najdete v tomto účtu úložiště. Můžete se omezuje 20 úložiště účty.

14. Pokud chcete virtuální počítač patří do sady dostupnost, **Nastavte dostupnost**, klikněte na možnost **vytvořit dostupnost sadu** nebo ho přidat do existující sady dostupnosti.

    **Poznámka**: virtuálních počítačích v sadě dostupnost jsou používaný různých poruch domény. Uvedení více virtuálních počítačích v dostupné sadu zajistit, že aplikace neexistuje během selhání sítě, selhání hardwaru místní disk a plánované odstávky.

15.  V části **koncové body**zkontrolujte nový koncové body, které se bude vytvořena povolit připojení, virtuální počítač, například pomocí vzdálené plochy nebo klient zabezpečené prostředí (SSH). Také můžete přidat koncové body, nebo byla vytvořená na pozdější. Pokyny pro vytvoření je později, najdete v článku [jak nastavit koncové body do virtuálního počítače](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

16.  V části **Agent OM**rozhodněte, jestli instalace agenta OM. Tento agent nabízí prostředí pro instalaci přípon souborů, které můžete pracovat s virtuální počítač. Další informace najdete v tématu [Správa rozšíření](http://go.microsoft.com/FWLink/p/?LinkID=390493).

17. Klikněte na šipku vytvořit virtuální počítač.

    ![Vytvoření vlastní virtuální počítač úspěšné](./media/howto-custom-create-vm/VMSuccessWindows.png)

##<a name="next-steps"></a>Další kroky##
Po vytvoření virtuálního počítače je spuštěn automaticky. Když portálu se zobrazí stav jako spuštění, můžete se přihlásit do virtuálního počítače. Pokyny najdete v následujících článcích:

- [Jak se přihlásit do virtuálního počítače se systémem Linux](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)
- [Jak se přihlásit do virtuálního počítače se systémem Windows Server](../articles/virtual-machines/virtual-machines-windows-classic-connect-logon.md)

