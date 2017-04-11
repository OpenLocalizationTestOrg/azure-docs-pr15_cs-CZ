
1. Navštivte [Azure portálu]. Klikněte na **Procházet vše** > **Mobilní aplikace** > back-end, který jste právě vytvořili. V části Nastavení mobilní aplikace klikněte na tlačítko **Rychlý úvod** > **Cordova**. V části **Konfigurovat klientské aplikace**vyberte možnost **vytvořit novou aplikaci**a pak klikněte na **Stáhnout**. To je možné stáhnout celý projekt Cordova aplikace předem nakonfigurován pro připojení k vaší back-end.

2. Rozbalení stažený soubor ZIP do adresáře na pevném disku, přejděte k souboru řešení (SLN) a ho otevřít v aplikaci Visual Studio.

5. Ve Visual Studiu zvolte platformu řešení (iOS nebo Android, Windows) z rozevíracího seznamu vedle šipku start a vyberte zařízení konkrétní nasazení nebo emulátor po kliknutí na rozevírací seznam na zelenou šipku. Všimněte si, že můžete použít výchozí Android platformy a dominový emulátoru. Další rozšířené výukové programy pro bude nutné vyberte podporovaného zařízení nebo emulátoru. 

6. Stisknutím klávesy F5 nebo kliknutím na zelenou šipku vytvářet a a spuštění aplikace Cordova. Pokud se zobrazí dialogové okno zabezpečení v emulátoru požadování přístupu k síti, přijměte ho.   

7. Po spustit aplikaci na zařízení nebo emulátoru, zadejte smysluplný text, **Zadejte nový text**, například _Dokončeno kurzu_ a potom klikněte na tlačítko **Přidat** .  
To, odešle žádost o příspěvek Azure back-end dříve zavedení. Vloží data back-end ze žádosti o je do TodoItem tabulky v databázi SQL a vrátí informace o nově uložené položky zpátky na mobilní aplikaci. Mobilní aplikace tato data zobrazí v seznamu.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)
    
8. Opakujte předchozí tři kroky pro každou platformu zařízení, které chcete podporu.

[Azure portálu]: https://portal.azure.com/
