1. Na portálu přejděte na **Nový**. Zadejte "Virtuální brána sítě" do vyhledávání. Vyhledejte **virtuální sítě brány** do pole Hledat zpáteční a klepněte na položku. Otevře se zásuvné **vytvořit virtuální sítě brány** .
2. Klikněte na **vytvořit** v dolní části zásuvné **virtuální sítě brány** . Otevře se zásuvné **vytvořit virtuální sítě brány** . Vyplňte hodnoty pro brány virtuální sítě.

    ![Vytvořit virtuální sítě brány zásuvné pole] (./media/vpn-gateway-add-gw-rm-portal-include/createvnetgw300.png "Vytvořit virtuální sítě brány zásuvné pole")

3. **Název**: název brány. To je stejný jako pojmenování podsítě brány. Jedná se název brány objekt, který vytváříte.

4. **Typ brány**: vyberte **VPN**. Virtuální privátní sítě brány pomocí typ brány virtuální sítě **VPN**. 

5. **Typ VPN**: Vyberte typ VPN zadané pro konfiguraci. Většina konfigurace vyžadují typu VPN v postupu.

6. **SKU**: Vyberte brány SKU z rozevíracího seznamu. Skladové jednotky uvedené v rozevíracím seznamu závisí na typu VPN, které vyberete.

7. **Umístění**: Upravit pole **umístění** tak, aby ukazovaly na místo, kde je uložena virtuální sítě.
 
8. Zvolte virtuální sítě, ke kterému chcete přidat tuto bránu. Klikněte na **virtuální sítě** otevřete zásuvné **Zvolte virtuální síť** . Vyberte VNet. Pokud nevidíte VNet, ujistěte se, že pole **umístění** odkazuje na oblast, ve kterém se nachází virtuální sítě.

9. Zvolte veřejnou IP adresu. Klikněte na **veřejnou IP adresu** otevřete zásuvné **Zvolit veřejnou IP adresu** . Klikněte na **+ vytvořit nový** a otevřete tak **vytvořit veřejnou IP adresu zásuvné**. Zadejte název pro veřejnou IP adresu. Tento zásuvné vytvoří veřejné objekt IP adres ke které veřejnou IP adresu dynamicky přiřadíte.<br>Klikněte na **OK** uložte provedené změny tohoto zásuvné.

10. **Předplatné**: Zkontrolujte, jestli je vybraná správná předplatného.

11. **Pole Skupina zdroje**: Toto nastavení je určený podle virtuální sítě, kterou jste vybrali. 

12. Nechcete nastavit **umístění** po zadání předchozí nastavení.

13. Ověřte nastavení. **Připnout do řídicího panelu** v dolní části zásuvné můžete vyberte, pokud mají brány se zobrazí na řídicím panelu.

14. Klikněte na **vytvořit** začít vytvářet brány. Nastavení bude ověřena a uvidíte "zavedení virtuální sítě brána" dlaždice na řídicím panelu. Vytvoření brány může trvat až 45 minut. Budete muset aktualizovat stránku portálu zobrazíte stav dokončení.

    ![Nasazení virtuální brána sítě] (./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png "Nasazení virtuální brána sítě")

11. Po vytvoření brány můžete zobrazit na IP adresu, který byl přiřazen k němu pohledem virtuální sítě na portálu. Brány se zobrazí jako připojené zařízení. Klikněte na připojené zařízení (brány virtuální sítě) zobrazíte další informace.



