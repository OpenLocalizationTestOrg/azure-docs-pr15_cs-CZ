1. Na portálu ze **všech zdrojů**klikněte na tlačítko **+ Přidat**. V **všechno** zásuvné vyhledávacího pole zadejte **místní síti brány**a potom klikněte na Hledat. Vrátí seznam. Klikněte na **místní síti brány** otevřete zásuvné a potom klikněte na **vytvořit** otevřete zásuvné **vytvořit místní síti brány** .

    ![Vytvoření brány pro místní síti](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. Na **zásuvné brány vytvořit místní síti**zadejte **název** objektu brány místní síti.
 
3. Zadejte platnou veřejnou **IP adresu** pro zařízení virtuální privátní sítě nebo brány virtuální sítě, ke kterému chcete připojit.<br>Pokud tento místní síti představuje místního pracoviště, je to veřejnou IP adresu VPN zařízení, které se chcete připojit. Nesmí být za překladu síťových adres a musí být dostupný Azure.<br>Pokud tento místní síti představuje jiné VNet, zadáte veřejnou IP adresu přiřazené k bráně virtuální sítě pro tuto VNet.<br>

4. **Adresní prostor** odkazuje na adresu oblastí pro síť, která představuje místní síti. Můžete přidat víc místa rozsahy adres. Ujistěte se, že oblasti, který tady zadáte nepřekrývaly se oblastmi jiných sítích, které se chcete připojit.
 
5. **Předplatné**zkontrolujte, že je zobrazený správné předplatného.

6. **Pole Skupina zdroje**vyberte skupina zdroje, který chcete použít. Můžete vytvořit nové skupiny prostředků, nebo vyberte některou, kterou jste vytvořili.

7. **Umístění**vyberte umístění, vytvoří se tento objekt v. Je vhodné vyberte na stejném místě, umístěné ve vaší VNet, ale nemusíte udělat.

8. Klikněte na **vytvořit** vytvořte bránu místní síti.
