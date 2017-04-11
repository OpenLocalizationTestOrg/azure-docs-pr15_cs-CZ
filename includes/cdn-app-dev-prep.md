## <a name="prerequisites"></a>Zjistit předpoklady pro

Před jsme lze zadat kód správy CDN, potřebujeme určitá příprava povolit naše kód chcete provést interakci s správce prostředků Azure.  K tomuto účelu musíte:

* Vytvoření skupiny prostředků má být CDN profil, který jsme vytvořili v tomto kurzu
* Konfigurace služby Azure Active Directory ověřování pro naše aplikace
* Použití oprávnění ke skupině zdroje, takže jenom uživatelé z naší Azure AD klienta můžete pracovat s naše CDN profilu

### <a name="creating-the-resource-group"></a>Vytvoření skupiny zdrojů

1. Přihlaste se k [portálu Azure](https://portal.azure.com).

2. Klikněte na tlačítko **Nový** v vpravo nahoře a pak **Správa**a **Pole Skupina zdroje**.
    
    ![Vytvoření nové skupiny prostředků](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)

3. Zavolejte na svůj *CdnConsoleTutorial*pole Skupina zdroje.  Vyberte předplatné a zvolte umístění blízkosti.  Pokud chcete, může klepnutím na zaškrtávací políčko **kód Pin pro řídicí panel** připnout skupina zdroje na řídicí panel na portálu.  To usnadní ho později najít.  Po provedení požadované hodnoty, klikněte na **vytvořit**.

    ![Pojmenování skupina zdroje](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)

4. Po vytvoření skupina zdroje je neměli připnout do řídicího panelu, najdete ji kliknutím na **Procházet**a pak **Skupiny zdrojů**.  Klikněte na skupina zdroje a otevřete ho.  Poznamenejte si **ID předplatného**.  Jsme budete ho později potřebovat.

    ![Pojmenování skupina zdroje](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Vytvoření aplikace Azure AD a nastavením oprávnění

Ověřování aplikace pomocí služby Azure Active Directory dvěma způsoby: jednotlivé uživatele nebo hlavní název služby. Služby základní je podobný účtu služby ve Windows.  Místo určitému uživateli udělit oprávnění chcete provést interakci s profily CDN, doporučujeme místo toho udělení oprávnění ke služby základní.  Služba objekty se nejčastěji používají pro automatické, interaktivním procesy.  I když je tento kurz zapisuje aplikace interaktivní konzoly, jsme se zaměřují na služby základní přístup.

Vytváření hlavní název služby obsahuje několik kroků, včetně vytvoření aplikace služby Azure Active Directory.  K tomuto účelu ukážeme [podle kurzu](../articles/resource-group-create-service-principal-portal.md).

> [AZURE.IMPORTANT] Ujistěte se, postupujte podle kroků v [propojených kurz](../articles/resource-group-create-service-principal-portal.md).  Je *velice důležité* dokončit ho přesně tak, jak je popsáno.  Nezapomeňte svoje **ID klienta**, **název domény klienta** (obvykle *. onmicrosoft.com* domény Pokud jste zadali vlastní doménu), **ID klienta**a **klíč ověřování klienta**, jako potřebujeme tyto novější.  Dávejte velmi chránit svoje **ID klienta** a **klíč ověřování klienta**, jak tyto přihlašovací údaje lze kdokoli spouštět operace jako hlavní služby. 
>   
> Až se dostanete ke kroku s názvem [aplikace více klienta konfigurovat](../articles/resource-group-create-service-principal-portal.md#configure-multi-tenant-application), vyberte **Ne**.
> 
> Až se dostanete ke kroku [přiřadit aplikace k roli](../articles/resource-group-create-service-principal-portal.md#assign-application-to-role), používat jsme vytvořili, *CdnConsoleTutorial*skupina zdroje, ale místo roli **Čtenář** přiřazení role **Přispěvatele CDN profilu** .  Po přiřazení aplikace role **Přispěvatele profilu CDN** ve skupině prostředků, vraťte se do tohoto kurzu. 

Po vytvoření služby základní a přiřazené role **Přispěvatele CDN profilu** , **Uživatelé** zásuvné skupiny zdroje by měla vypadat přibližně takto.

![Uživatelé zásuvné](./media/cdn-app-dev-prep/cdn-service-principal-include.png)


### <a name="interactive-user-authentication"></a>Ověření interaktivní uživatele

Pokud místo hlavní název služby, se dají přednost ověření interaktivní jednotlivé uživatele, je velmi podobný pro službu jistinu procesu.  Ve skutečnosti je třeba použijte stejný postup, ale několik menší změny.

> [AZURE.IMPORTANT] Pouze tyto kroky další Pokud zvolíte místo služby základní použít ověřování jednotlivé uživatele.

1. Při vytváření aplikace místo **Webové aplikace**, zvolte **nativní aplikaci**. 
    
    ![Nativní aplikaci](./media/cdn-app-dev-prep/cdn-native-application-include.png)
    
2. Na další stránce zobrazí se výzva pro **přesměrování URI**.  Identifikátor URI nebude ověřený, ale nezapomeňte, co jste zadali.  Je potřeba ho později. 

3. Je potřeba vytvořit **klíč ověřování klienta**.

4. Místo hlavní název služby přiřadit role **Přispěvatele profilu CDN** , ukážeme přidělit jednotlivým uživatelům nebo skupinám.  V tomto příkladu uvidíte, můžu jste přiřadili *CDN ukázku uživatele* do role **Přispěvatele CDN profilu** .  
    
    ![Přístup jednotlivé uživatele](./media/cdn-app-dev-prep/cdn-aad-user-include.png)

