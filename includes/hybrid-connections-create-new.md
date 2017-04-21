
1. V místním počítači přihlaste se k [Portálu Správa Azure](http://manager.windowsazure.com) (Toto je původní portál).

2. V dolní části navigačním podokně vyberte **+ Nový** > **Aplikace služby** > **Služba BizTalk** > **Vytvořit vlastní**.

3. Zadejte **Název služby BizTalk** a vyberte **Edition**. 

    Tento kurz používá **mobile1**. Je třeba zadejte jedinečný název pro nová služba BizTalk.

4. Po vytvoření služba BizTalk vyberte kartu **Hybridní připojení** a potom klikněte na **Přidat**.

    ![Přidání připojení pro hybridní](./media/hybrid-connections-create-new/3.png)

    Tím vytvoříte nové připojení hybridní.

5. Zadejte **název** a **Název hostitele** pro vaše hybridní připojení a je nastavena **Port** pro `1433`. 
  
    ![Konfigurace hybridního připojení](./media/hybrid-connections-create-new/4.png)

    Název hostitele je název místní server. To nakonfiguruje připojení hybridní pro přístup k SQL serveru na port 1433. Pokud používáte pojmenovanou instanci systému SQL Server, použijte místo toho statické port, který jste definovali dříve.

6. Po vytvoření nové připojení stav nové připojení zobrazuje **místní nastavení neúplné**.

7. Přejděte zpátky do mobilního služby, klikněte na **Konfigurovat**, posuňte se dolů na **hybridní připojení** klikněte na **Přidat hybridní připojení**, vyberte hybridní připojení, které jste právě vytvořili a klikněte na **OK**.

    Umožňuje Mobilní služba použít nové připojení hybridní.

Pak budete muset nainstalovat Connection Manager hybridní na místním počítači.