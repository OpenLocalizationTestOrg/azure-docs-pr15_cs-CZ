I když je možné MongoLab URI vložte do kódu, doporučujeme nakonfigurováním v prostředí pro snadnější správu. Tímto způsobem, pokud identifikátor URI změní, můžete aktualizovat ho přes Azure portál nemusíte přecházet na kód.


1. Na portálu Azure vyberte **Web Apps**.
1. Klikněte na název webové aplikace v seznamu aplikací Web Apps.  
![WebAppEntry][entry-website]  
Řídicí panel v prohlížeči se zobrazí.

1. V řádku nabídek klikněte na **Konfigurovat** .  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. Přejděte dolů do části připojovací řetězec.  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1. Pole **název**zadejte MONGOLAB_URI.
1. **Hodnota**zkopírovat připojovací řetězec, který jsme získáte v předchozí části.
1. V rozevíracím seznamu **Typ** (místo výchozí **SQLAzure**) vyberte položku **vlastní** .
1. Na panelu nástrojů klikněte na **Uložit** .  
![SaveWebApp][button-website-save]

**Poznámka:** Přidá Azure **CUSTOMCONNSTR\_ ** předpona na tuto proměnnou, což je důvod, proč kód nad odkazy **CUSTOMCONNSTR\_MONGOLAB_URI.**

[entry-website]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[focus-mongolab-websitedashboard-config]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[focus-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[button-website-save]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png
