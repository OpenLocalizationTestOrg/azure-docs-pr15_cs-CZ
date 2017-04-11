Teď, když jste přidali podmínky, dostanete zajímavý její čas něco udělat s daty, která je generováno aplikací aktivační událost. Tímto postupem přidat akci **služby Salesforce - Get objektu** . Tato akce pošle data pokaždé, když se vytvoří nové zájemce. Pokud přidáte druhý akci, která budou používat data ze služby Salesforce - Get akce objektu odeslat e-mailů pomocí konektoru Office 365.  

Konfigurace tento akce, budete muset zadat následující informace. Zjistíte, že je snadno se použije data generovaná aktivační událost jako vstup pro některé vlastnosti nového souboru:

|Vytvoření vlastnosti souboru|Popis|
|---|---|
|Typ objektu|Toto je typ služby Salesforce, které vás zajímají. Příklady potenciální zákazník, účtu atd.|
|ID objektu|Představuje identifikátor objektu.|


1. Vyberte odkaz **Přidat akci** . Toto se otevře, ke které chcete vyhledávací pole, kde můžete vyhledávat všech akcí můžete udělat. V tomto příkladu služby Salesforce akce jsou potřebné.      
![Obrázek akce služby Salesforce 1](./media/connectors-create-api-salesforce/action-1.png)  
- Zadejte *služby salesforce* vyhledat akce související s služby salesforce.
- Vyberte **služby Salesforce - Get objekt** jako akce.   **Poznámka**: Zobrazí se výzva k ověření aplikace logiky pro přístup k účtu služby Salesforce, pokud jste tak dosud neučinili.    
![Obrázek akce služby Salesforce 2](./media/connectors-create-api-salesforce/action-2.png)    
- Ovládací prvek **získat objekt** otevře.  
- Jako typ objektu vyberte *vést* .
- Vyberte ovládací prvek **ID objektu** .
- Zaškrtnutím tohoto políčka **…** rozbalil seznam klíčovými slovy, která mohou sloužit jako vstup pro akce.       
![Obrázek akce služby Salesforce 3](./media/connectors-create-api-salesforce/action-3.png)    
- Vyberte ovládací prvek **Vést kód** otevře.   
![Obrázek akce služby Salesforce 4](./media/connectors-create-api-salesforce/action-4.png)     
- Všimněte si, že token vést ID je teď v ovládacím prvku ID objektu označující, že akce přidružená k objektu získání vyhledá potenciálního zákazníka se číslo ID, které je rovno pořádat ID potenciální zákazník, který spustil tuto aplikaci logiky.  
![Obrázek akce služby Salesforce 5](./media/connectors-create-api-salesforce/action-5.png)  
- Uložte svou práci. To je vše, akce přidružená k objektu získání jste přidali do aplikace pro použití logických operátorů. Ovládací prvek objekt Get by měl vypadat takto:    
![Obrázek akce služby Salesforce 6](./media/connectors-create-api-salesforce/action-6.png)  

Teď, když jste přidali akci, kterou chcete získat zájemce, můžete udělat něco zajímavé s nově vytvořený zájemce. V podniku můžete poslat e-mailu sdělit distribučního seznamu vytvořené nové zájemce. Odešlete e-mail s některými relevantní informace z nové zájemce objektu v služby Salesforce použijeme konektoru Office 365.  

1. Klikněte na **Přidat akci** a pak zadejte *e-mailu* v ovládacím prvku hledání. Toto pole filtruje akce ty, které se vztahují k odesílání a přijímání e-mailu.  
- Vyberte položku seznamu **Office 365 Outlook – pošlete e-mail** . Pokud jste ještě nevytvořili *připojení* ke svému účtu Office 365, zobrazí se výzva k zadání přihlašovacích údajů Office 365 vytvořit nyní. Až budete hotovi, otevře se ovládací prvek **Odeslat e-mail** .        
![Obrázek akce služby Salesforce 7](./media/connectors-create-api-salesforce/action-7.png)  
- Zadejte e-mailovou adresu, který chcete odeslat e-mailu v ovládacím prvku **pro** .
-  V ovládacím prvku **Předmět** zadejte *nové zájemce vytvořili* - pak vyberte token *společnosti* . Tím zobrazíte pole *společnosti* z nové zájemce vytvořené v služby Salesforce.  
-  V ovládacím prvku **textu** vyberte některou z tokenů z nové zájemce objektu a je rovněž možné zadat jakýkoli text, kterou chcete zobrazit v textu e-mailu. Tady je příklad:  
![Obrázek akce služby Salesforce 8](./media/connectors-create-api-salesforce/action-8.png)   
- Uložte pracovní postup.  

To je vše. Použití logických operátorů aplikace je teď dokončeno.  

Teď můžete otestovat aplikace pro použití logických operátorů: v služby Salesforce, vytvořte nové zájemce, který splňuje podmínky jste vytvořili.  Pokud jste postupovali tento walk-through plně, jednoduše si ji vytvořte potenciálního zákazníka se e-mailovou adresu, která obsahuje *amazon.com* v nich. Po několik sekund, než logiky aplikace spuštěna a výsledky může vypadat nějak takto:  
![Obrázek akce služby Salesforce 9](./media/connectors-create-api-salesforce/action-9.png)  

