Po rozšíření záznamy za svůj název domény, musíte přidružit je Web App. Pomocí následujících kroků umožníte názvy domén pomocí webového prohlížeče.

> [AZURE.NOTE] Může trvat delší dobu vytvořených v předchozích krocích se rozšíří celém systému DNS záznamů TXT. Název domény nemůžete přidat do webové aplikace, dokud rozšíření záznamu TXT. Pokud používáte záznam, nemůžete přidat název záznamu domény A do webové aplikace, dokud se rozšíří záznamu TXT vytvořili v předchozím kroku.
>
> Služby, třeba <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> můžete ověřit, že je k dispozici záznamu TXT.

1. V prohlížeči otevřete [Portál Azure](https://portal.azure.com).

2. Na kartě **Web Apps** klikněte na název svojí webové aplikace a pak vyberte **vlastní domény**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. V zásuvné **vlastní domény** klikněte na **Přidat název hostitele**.
    
4. Pomocí textového pole **název hostitele** zadejte názvy domén chcete přidružit k této webové aplikace.

    ![](./media/custom-dns-web-site/add-custom-domain.png)

6.  Klikněte na **Ověřit**.

7.  Po kliknutí na tlačítko **Ověřit** Azure bude zahájit pracovní postup ověření domény. To kontroloval vlastnictví domény i podrobné chyby s obvyklé guidence o tom, jak opravit chybu nebo název hostitele úspěšné dostupnost a sestavy.    

V tomto okamžiku by měl moct zadejte název vlastní domény v prohlížeči a se, že ho úspěšně přenese vás do webové aplikace.
