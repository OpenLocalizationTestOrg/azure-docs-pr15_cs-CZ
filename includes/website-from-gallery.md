Azure Marketplace zpřístupní do široké řady oblíbených webových aplikacích vyvinutý společností Microsoft, společnosti třetích stran a iniciativy software otevřít zdroj. Webové aplikace vytvořená z Azure Marketplace nevyžadují instalaci softwaru než prohlížeč připojuje k [Portálu náhled Azure](http://go.microsoft.com/fwlink/?LinkId=529715). 

V tomto kurzu se dozvíte:

- Jak vytvořit novou webovou aplikaci pomocí Azure Marketplace.

- Postup pro nasazení webové aplikace pomocí portálu Azure náhled.
 
Budete vytvářet WordPress blogu, který používá výchozí šablonu. Následující obrázek znázorňuje dokončené aplikace:


![WordPress blogu][13]

>[AZURE.NOTE] Pokud chcete začít pracovat s aplikaci služby Azure před registrací účet Azure, přejděte na [Zkuste aplikaci služby](http://go.microsoft.com/fwlink/?LinkId=523751), které můžete okamžitě vytvořit web appu krátkodobý starter v aplikaci služby. Žádné povinné; kreditní karty žádné závazky.

## <a name="create-a-web-app-in-the-portal"></a>Vytvoření webové aplikace na portálu

1. Přihlaste se k portálu Azure náhled.

2. Otevřete Azure Marketplace klepnutím na ikonu **Marketplace** :

    ![Ikona Marketplace][marketplace]

    Nebo kliknutím na ikonu **Nový** v pravém horním rohu na řídicím panelu a výběrem **Marketplace** na bottow seznamu.
    
    ![Vytvořit nový][5]
    
3. Vyberte **Web + Mobile**. Vyhledejte **WordPress** a klikněte na ikonu **WordPress** .

    ![WordPress ze seznamu][7]
    
5. Po přečtení popis aplikaci WordPress, vyberte možnost **vytvořit**.

6. Klikněte na **Web appu**a zadejte požadované hodnoty pro konfiguraci webové aplikace.
    
    ![Konfigurace aplikace][8]

7. Klikněte na **databáze**a zadejte požadované hodnoty pro konfiguraci databáze MySQL. 

    ![Konfigurace databáze][database]

8. Zadejte název nové skupiny prostředků.

    ![Nastavení pole Skupina zdroje][groupname]

8. V případě potřeby klikněte na **PŘEDPLATNÉ**a zadejte předplatné pro použití. 

7. Po definování web appu klikněte na **vytvořit**a počkejte, dokud se vytvoří nový web appu.

   Po vytvoření aplikace uvidíte skupina zdroje obsahující web app a databáze.

   ![Skupina zobrazit][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Spuštění a spravovat svoji WordPress webovou aplikaci
    
1. Klikněte na nový web appu zobrazit podrobné informace o aplikaci.

    ![řídicí panel Snadné spuštění][10]

2. Na stránce **Essentials** klikněte na **Procházet** nebo na odkaz v části **adresa Url** otevřete úvodní stránku web appu.

    ![Adresa URL webu][browse]

3. Pokud jste nenainstalovali WordPress, zadejte příslušnou konfiguraci informace vyžadované WordPress a klikněte na **Nainstalovat WordPress** dokončení konfigurace a otevřete stránku pro přihlášení web appu.

4. Klikněte na **přihlásit** a zadejte svoje přihlašovací údaje.  

5. Budete mít novou WordPress webovou aplikaci, která vypadá podobně jako v aplikaci web.    

    ![WordPress webu][13]






[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png
