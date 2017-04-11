Po rozšíření záznamy za svůj název domény, měli moct ověřte, zda svůj vlastní název domény můžete použít pro přístup k vaší webové aplikace pro aplikaci služby Azure pomocí prohlížeče.

> [AZURE.NOTE] Může trvat delší dobu CNAME se rozšíří celém systému DNS. Služby, třeba <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> můžete ověřit, že záznamy CNAME je k dispozici.

Pokud jste ještě nepřidali svoji webovou aplikaci jako koncový bod přenosy správce, musíte to udělat před překlad fungovalo jako trasy název vlastní domény správci přenosy. Přenosy správce přesměruje do webové aplikace. Pokud chcete přidat webovou aplikaci jako koncový bod ve vašem profilu přenosy správce pomocí informací v [Přidat nebo odstranit koncové body](../articles/traffic-manager/traffic-manager-endpoints.md) .

> [AZURE.NOTE] Pokud váš web appu nevidíte při přidávání koncový bod, ověřte, zda je nakonfigurován pro režim plán **Standardní** aplikaci služby. Chcete-li pracovat s správce přenosy je nutné použít **standardním** režimu pro web app.

1. V prohlížeči otevřete [Portál Azure](https://portal.azure.com).

1. Na kartě **Web Apps** klikněte na název svojí webové aplikace, vyberte **Nastavení**a pak vyberte **vlastní domény**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

1. V zásuvné **vlastní domény** klikněte na **Přidat název hostitele**.
    
1. Zadejte název domény přenosy správce chcete přidružit k této webové aplikace pomocí textového pole **název hostitele** .

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

1. Klikněte na **Ověřit** uložte konfigurace název domény.

7.  Po kliknutí na tlačítko **Ověřit** Azure bude zahájit pracovní postup ověření domény. To kontroloval vlastnictví domény i podrobné chyby s obvyklé guidence o tom, jak opravit chybu nebo název hostitele úspěšné dostupnost a sestavy.    

8.  Po úspěšném ověření **hostname přidat** tlačítko je aktivní a budou moct hostname přiřazení. Teď přejděte na název vaší vlastní domény v prohlížeči. Teď byste měli vidět váš spuštění aplikace používat vlastní název domény. 

    Po dokončení konfigurace vlastní název domény uvedené v části **názvy domén** svojí webové aplikace.

V tomto okamžiku by měl moct zadejte název domény přenosy správce v prohlížeči a se, že ho úspěšně přenese vás do webové aplikace.
