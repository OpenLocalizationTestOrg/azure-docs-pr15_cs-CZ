
   * Přihlaste se k Azure účtu zadejte svoje přihlašovací údaje.

     Tento způsob je rychlejší a líp, ale pokud použijete tento postup nebude moct najdete v článku databáze SQL Azure nebo mobilní služby v okně **Průzkumník serveru** .

     V **Průzkumníku serveru**klikněte na tlačítko **připojit k Azure** . Další možností je klikněte pravým tlačítkem myši na uzel **Azure** a potom v místní nabídce klikněte na **připojit k Azure** .

   * Nainstalujte certifikát správy, které umožňují přístup ke svému účtu.

     V **Průzkumníku serveru**klikněte pravým tlačítkem myši na uzel **Azure** a potom v místní nabídce klikněte na **Spravovat předplatná** . V dialogovém okně **Správa odběrů Azure** klikněte na kartu **certifikáty** a potom klikněte na **importovat**. Postupujte podle pokynů ke stažení a pak je naimportujte soubor předplatného (nazývané také soubor *.publishsettings* ) pro váš účet Azure.

     > [AZURE.NOTE] Stáhněte si předplatné soubor do složky mimo vaší zdrojové kód adresáře (například ve složce Downloads) a potom ho odstraňte po dokončení importu. Zlými úmysly, který získá přístup k souboru předplatného můžete upravit, vytvořit a odstranit služby Azure.

    Další informace najdete v tématu [jak připojit k Azure z aplikace Visual Studio](http://go.microsoft.com/fwlink/?LinkId=324796).
