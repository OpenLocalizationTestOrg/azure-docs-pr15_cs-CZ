<properties
    pageTitle="Upozornění v reálném čase Azure CDN | Microsoft Azure"
    description="Data v reálném upozornění v Microsoft Azure CDN. Oznámení o výkonu koncové body ve vašem profilu CDN poskytuje v reálném čase výstraha."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="casoper"/>

# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Data v reálném upozornění v Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]


## <a name="overview"></a>Základní informace

Tento dokument vysvětluje v reálném čase upozornění v Microsoft Azure CDN. Tato funkce zobrazí data v reálném oznámení o výkonu koncové body v CDN profil.  Můžete nastavit e-mailu nebo HTTP upozornění na základě:

* Šířka pásma
* Stavů
* Zobrazit stavy mezipaměti
* Připojení

## <a name="creating-a-real-time-alert"></a>Vytvoření upozornění na v reálném čase

1. Na [Portálu Azure](https://portal.azure.com)procházením CDN profil.

    ![Zásuvné CDN profilu](./media/cdn-real-time-alerts/cdn-profile-blade.png)

2. Na zásuvné CDN profilu klikněte na tlačítko **Spravovat** .

    ![Tlačítko Spravovat zásuvné CDN profilu](./media/cdn-real-time-alerts/cdn-manage-btn.png)

    Na portálu Správa CDN otevře.

3. Přejděte myší na kartě **Analýza** a potom přejděte myší na plovoucí panel **v reálném čase Statistika** .  Klikněte na **data v reálném upozornění**.

    ![Portál Správa CDN](./media/cdn-real-time-alerts/cdn-premium-portal.png)

    Zobrazí se seznam existující upozornění konfigurace (pokud existuje).

4. Kliknutím na tlačítko **Přidat oznámení** .

    ![Přidání tlačítka Výstraha](./media/cdn-real-time-alerts/cdn-add-alert.png)

    Formulář pro vytvoření nového upozornění se zobrazí.

    ![Formulář nové upozornění](./media/cdn-real-time-alerts/cdn-new-alert.png)

5. Pokud chcete tuto výstrahu za aktivní, když kliknete na **Uložit**, zaškrtněte políčko **Povolit upozornění** .

6. Do pole **název** zadejte popisný název výstrahy.

7. V rozevíracím seznamu **Typ média** objekt **HTTP velké**.

    ![Typ média HTTP velké vybrán objekt](./media/cdn-real-time-alerts/cdn-http-large.png)

    > [AZURE.IMPORTANT] **Velké objekt HTTP** je nutné vybrat **Typ média**.  Další možnosti nepoužívá **Azure CDN z Verizon**.  Selhání vyberte **HTTP velkého objektu** způsobí oznámení nikdy aktivaci.

8. Vytvoření **výrazu** sledování tak, že vyberete **míru**, **operátor**a **aktivační událost hodnotu**.

    - **Metriky**vyberte požadovaný typ podmínka, kterou chcete sledované.  **/ Šířky pásma s** je množství využití šířky pásma v MB.  **Celkový počet připojení** označuje počet souběžné HTTP připojení k serverům naše okraje.  Definice různé stavy mezipaměti a stavů najdete v článku [Azure CDN mezipaměti stavů](https://msdn.microsoft.com/library/mt759237.aspx) a [Azure CDN HTTP stavů](https://msdn.microsoft.com/library/mt759238.aspx)
    - **Operátor** je matematický operátor, která vytvářejí vztah mezi metriky a hodnotu aktivační událost.
    - **Aktivační událost hodnota** je mezní hodnota, která musí být splněná, než bude být odesláno oznámení.

    V příkladu, výraz jsem vytvořil(a) označuje, že chci upozorněni počet stavů 404 je větší než 25.

    ![Data v reálném upozornění Ukázkový výraz](./media/cdn-real-time-alerts/cdn-expression.png)

9. Pro **Interval**zadejte, jak často se mají parametrem expression Vyhodnocená.

10. **Oznámení v** rozevíracím seznamu vyberte, kdy se mají být odesláno oznámení při splnění výraz.
    
    - **Podmínka zahájení** označuje, oznámení bude při prvním zjištění zadané podmínky.
    - **Ukončení podmínka** označuje, že oznámení bude při už zjištění zadané podmínky. Toto oznámení je možné spouštět jenom po naší síti systému sledování zjištění, že došlo k zadané podmínky.
    - **Nepřetržitě** označuje, že oznámení odešle pokaždé, když sítě systému sledování rozpozná zadaná podmínka. Mějte na paměti, že sítí systému sledování pouze kontroloval jednou za interval pro zadané podmínky.
    - **Podmínka začátek a konec** označuje, že oznámení odešle první, že je zadaná podmínka zaznamenán a ještě jednou při podmínka už ke zjištění.

11. Pokud chcete dostávat oznámení o e-mailem, zaškrtněte políčko **upozornění e-mailem** .  

    ![Upozornit formulářem e-mailu](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    V poli **Komu** zadejte e-mailovou adresu místo, kam chcete oznámení o odeslání. Pro **Předmět** a **text**může ponechte výchozí hodnotu, nebo mohou přizpůsobit zprávu dynamicky vložení upozornění data po odeslání zprávy pomocí seznamu **dostupných klíčová slova** .

    > [AZURE.NOTE] E-mailového oznámení můžete otestovat kliknutím na tlačítko **Testovat upozornění** , ale až po uložil upozornění konfigurace.

12. Pokud budete potřebovat upozornění, která se publikuje na webový server, zaškrtněte políčko **oznámit poštou HTTP** .

    ![Upozorněte tím, že HTTP Post formuláře](./media/cdn-real-time-alerts/cdn-notify-http.png)

    Do pole **adresa Url** zadejte adresu URL můžete místo, kam chcete HTTP zprávu odeslala. Do textového pole **záhlaví** zadejte záhlaví HTTP nechat zasílat v pozvánce.  U **textu** můžete upravit zprávy dynamicky vložení upozornění data po odeslání zprávy pomocí seznamu **dostupných klíčová slova** .  **Záhlaví** a **textu** výchozího stavu a data XML, která je podobně jako příkladu.

    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```

    > [AZURE.NOTE] Oznámení HTTP příspěvku můžete otestovat kliknutím na tlačítko **Testovat upozornění** , ale až po uložil upozornění konfigurace.

13. Kliknutím na tlačítko **Uložit** uložte konfiguraci upozornění.  Pokud jste zaškrtli **Povolené upozornění** v kroku 5, je nyní aktivní oznámení.

## <a name="next-steps"></a>Další kroky

- Analyzovat [data v reálném stat v Azure CDN](cdn-real-time-stats.md)
- Prozkoumat důkladněji s [Rozšířené HTTP sestavy](cdn-advanced-http-reports.md)
- Analýza [využití vyhledávání](cdn-analyze-usage-patterns.md)

