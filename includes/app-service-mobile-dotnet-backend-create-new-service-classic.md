1. Přihlaste se na [portál Azure].

2. Klikněte na **+ Nový** > **Web + Mobile** > **Mobilní aplikaci**, potom zadejte název vašeho back-end mobilní aplikaci.

3. **Pole Skupina zdroje**vyberte existující skupinu zdrojů nebo vytvořte nový účet (s využitím stejný název jako aplikace.) 
 
    Můžete vybrat jiný plán služeb aplikací nebo vytvořte nový účet. Další informace o aplikaci služby plány a jak vytvořit nový plán v jiné ceny úroveň a do požadovaného umístění v tématu [Přehled hloubkovou aplikaci služby Azure plány](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. **Plán služeb aplikací**je vybráno výchozí plán (ve [standardní vrstvu](https://azure.microsoft.com/pricing/details/app-service/)). Můžete také vybrat na jiný plán nebo [vytvořte nový účet](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Nastavení aplikace služeb plánu určují [umístění, funkce, náklady a výpočet zdroje](https://azure.microsoft.com/pricing/details/app-service/) přidružený k vaší aplikaci. 

    Pokud se rozhodnete na plán, klikněte na **vytvořit**. Tím vytvoříte back-end mobilní aplikaci. 
    
6. V **Nastavení** zásuvné pro novou back-end mobilní aplikaci, klikněte na **úvodní** > svoji platformu klientské aplikace > **připojení databáze**. 

    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)

7. Na zásuvné **Přidat datové připojení** , klikněte na **Databáze SQL** > **vytvořit novou databázi**, zadejte **název**databáze, zvolte ceny osy a potom klikněte na **serveru**.  Můžete znovu použít toto nové databáze. Pokud už máte databáze na stejném místě, můžete místo toho **použít existující databázi**. Použití databáze do jiného umístění nedoporučuje kvůli náklady na šířku pásma a latence vyšší.
 
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)

8. V zásuvné **Nový server** zadejte název serveru jedinečný v poli **název serveru** , zadejte jméno a heslo, **služby azure povolit přístup k serveru**a klikněte na **OK**. Tím vytvoříte novou databázi.

9. Zpátky v zásuvné **Přidat datové připojení** klikněte na **připojovací řetězec**, zadejte hodnoty přihlašovací jméno a heslo databáze a klikněte na **OK**. Počkejte několik minut databáze, kterou chcete nasadit úspěšně před pokračováním.

<!-- URLs. -->
[Azure portálu]: https://portal.azure.com/
