### <a name="prerequisites"></a>Zjistit předpoklady pro
- Účet Twilio
- Ověření Twilio telefonního čísla, který může přijímat zprávy SMS
- Ověření Twilio telefonního čísla, které můžou odesílat zprávy SMS

>[AZURE.NOTE] Pokud používáte zkušební účet Twilio, můžete jenom poslat služby SMS **ověření** telefonní čísla.  

Než budete moct použít účtu Twilio v aplikaci použití logických operátorů, musíte povolit aplikaci logiky pro připojení ke svému účtu Twilio. Naštěstí dosáhnete snadno z v rámci logiky aplikace na portálu Azure. 

Tady je postup povolit přístup ke svému účtu Twilio aplikace použití logických operátorů:

1. Vytvoření připojení k Twilio, v návrháři aplikace použití logických operátorů, vyberte **Zobrazit Microsoft spravované rozhraní API** v rozevíracím seznamu klikněte do pole Hledat zadejte *Twilio* . Vyberte aktivační událost nebo akci, kterou budete chtít použít:  
  ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Pokud jste nevytvořili všechna připojení k Twilio před, se zobrazí výzva k zadání přihlašovacích údajů Twilio. Těchto přihlašovacích údajů se použijí k ověření aplikace logiky pro připojení k a přístup k účtu Twilio dat:  
  ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. Musíte mít **id účtu Twilio** a **Twilio přístupový token** z řídicího panelu v Twilio, takže přihlášení k účtu Twilio teď přitáhnete těmito dvěma datovými informace:  
  ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Aplikace Twilio a použití logických operátorů pomocí různých názvy k identifikaci těchto dvou částí informací. Tady je způsob musí namapujte je do dialogového okna aplikace použití logických operátorů:![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Klikněte na tlačítko **vytvořit připojení** :  
  ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Všimněte si vytvořil připojení a měli byste být bezplatné pokračujte dalšími kroky v aplikaci použití logických operátorů:  
  ![](./media/connectors-create-api-twilio/twilio-5.png)