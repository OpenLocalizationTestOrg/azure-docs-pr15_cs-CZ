<properties 
   pageTitle="Azure Advisor databáze SQL Azure portálu | Microsoft Azure" 
   description="Poradce databáze SQL Azure Azure portálu umožňuje zkontrolovat a provádění doporučení pro existující databáze SQL, který vám pomůže aktuální výkonu dotazu." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="sql-database-advisor-using-the-azure-portal"></a>Konference Advisor databáze SQL Azure portálu

> [AZURE.SELECTOR]
- [Přehled Advisor databáze SQL](sql-database-advisor.md)
- [Portál](sql-database-advisor-portal.md)

Poradce databáze SQL Azure Azure portálu umožňuje zkontrolovat a provádění doporučení pro existující databáze SQL, který vám pomůže aktuální výkonu dotazu.

## <a name="viewing-recommendations"></a>Zobrazení doporučení

Stránka doporučení je, kde zobrazit nejvyšší doporučení podle svého účinku aby se zvýšil výkon. Můžete taky zobrazit stav historických operací. Vyberte doporučení nebo stav zobrazíte více podrobností.

Pokud chcete zobrazit a použít doporučení, musíte oprávnění správného [řízení přístupu na základě rolí](../active-directory/role-based-access-control-configure.md) v Azure. **Reader**, který je oprávnění **SQL databáze přispěvatele** podporují doporučení zobrazení a **vlastník**, oprávnění **SQL databáze přispěvatele** vyžadovaných provádět žádné akce; Vytvoření nebo zrušit indexy a zrušit vytváření indexů.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/).
2. Klikněte na **Další služby** > **SQL databáze**a vyberte databázi.
5. Klikněte na **výkon doporučení** zobrazíte dostupná doporučení pro vybranou databázi.

> [AZURE.NOTE] Chcete-li získat doporučení databázi není potřeba o den použití a je třeba některé aktivity. Je také třeba některé konzistentní aktivity. Poradce SQL databáze můžete snadněji optimalizovat pro konzistentní dotazu vzorků než můžete pro náhodné spotty roztržení aktivity. Pokud doporučení nejsou k dispozici, **doporučení výkonu** stránky by měl obsahovat zprávu s vysvětlením, proč.

![Doporučení](./media/sql-database-advisor-portal/recommendations.png)

Tady je příklad "Problém schématu" doporučení Azure portálu.

![Řešení problému schématu](./media/sql-database-advisor-portal/sql-database-advisor-schema-issue.png)

Doporučení se seřazenými podle jejich potenciální dopad na výkon do následujících čtyř kategorií:

| Dopad | Popis |
| :--- | :--- |
| Vysoký | Doporučení vysoké dopad by měl obsahovat nejdůležitější vliv na výkon. |
| Střední | Střední dopad doporučení by měla zlepšit výkon, ale ne podstatně. |
| Nízká | Slabý dopad doporučení by měl obsahovat vyšší výkon než bez, ale nebudete významná vylepšení. 


### <a name="removing-recommendations-from-the-list"></a>Odebrání doporučení ze seznamu

Obsahuje-li seznam doporučení položky, které chcete odebrat ze seznamu, můžete zrušit doporučení:

1. Doporučení vyberte v seznamu **doporučení**.
2. Kliknutím na **Zrušit** na zásuvné **Podrobnosti** .


Pokud budete chtít, můžete přidat vyřazené položky zpátky do seznamu **doporučení** :

1. Na zásuvné **doporučení** klikněte na **Zrušit zobrazení**.
1. Výběr vyřazené položky ze seznamu zobrazíte její podrobnosti.
1. Pokud chcete klikněte na tlačítko **Zpět zahodit** ji rejstřík přidat zpátky do hlavního seznamu **doporučení**.



## <a name="applying-recommendations"></a>Použití doporučení

Advisor databáze SQL umožňuje úplnou kontrolu nad v čem se doporučení povolené některým z následujících tří možností: 

- Použití jednotlivé doporučení jeden po druhém.
- Povolení Poradce-li automaticky použít doporučení (aktuálně platí pro index doporučení pouze).
- Implementace doporučení ručně, spusťte doporučené skript T-SQL databáze.

Vyberte libovolnou doporučení zobrazit podrobnosti a pak klikněte na **Zobrazit skript** přesné podrobnosti o vytváření doporučení.

Databáze zůstane online během Poradce platí doporučení – pomocí SQL databáze Advisor nikdy trvá databáze offline.

### <a name="apply-an-individual-recommendation"></a>Použití jednotlivé doporučení

Můžete zobrazit a přijmout doporučení jeden po druhém.

1. Na zásuvné **doporučení** klikněte na doporučení.
2. Na zásuvné **Podrobnosti** klikněte na **použít**.

    ![Použití doporučení](./media/sql-database-advisor-portal/apply.png)

### <a name="enable-automatic-index-management"></a>Povolit správu automatické indexu

Můžete nastavit Poradce SQL databáze automaticky provádět doporučení. Jakmile doporučení k dispozici se bude automaticky použita. Jako s všechny operace indexování spravováno službou při negativní vliv na výkon doporučení se vrátit zpět.

1. Na zásuvné **doporučení** klepněte na **Automatické**:

    ![Konference Advisor nastavení](./media/sql-database-advisor-portal/settings.png)

2. Nastavte Poradce automaticky **vytvořit** nebo **přetáhnout** indexů:

    ![Doporučené indexy](./media/sql-database-advisor-portal/automation.png)


### <a name="manually-run-the-recommended-t-sql-script"></a>Ruční spuštění doporučené skriptu T-SQL

Vyberte libovolnou doporučení a klikněte na **Zobrazit skript**. Spusťte tento skript vůči databáze pro ručně přiřaďte doporučení.

*Indexy, které jsou ručně spustit nejsou sledovány a ověřit pro vliv na výkon službou* tak, aby se navrhlo sledovat tyto indexy po vytvoření ověřit, zda budou poskytují zlepšení výkonu a upravte nebo odstraňte je v případě potřeby. Podrobnosti o vytváření indexů najdete v tématu [Vytvoření indexu (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).


### <a name="canceling-recommendations"></a>Zrušení doporučení

Doporučení, která jsou ve stavu **pole Čekání na** **ověření**a **Úspěch** můžete zrušit. Doporučení se stavem **zpracování** se nedá zrušit.

1. V oblasti **Optimalizace historie** otevřete zásuvné **doporučení podrobnosti** vyberte doporučení.
2. Klikněte na **Zrušit** proces použití doporučení.



## <a name="monitoring-operations"></a>Sledování operace

Použití doporučení situaci, nemusí proběhnout okamžitě. Na portálu obsahuje podrobné informace o stavu operace doporučení. Následující seznam uvádí možné stavy, které můžou být indexu v:

| Stav | Popis |
| :--- | :--- |
| Pole Čekání na | Použití doporučení příkaz byla přijata a by měla proběhnout spuštění. |
| Spuštění | Doporučení se nepoužijí. |
| Úspěch | Doporučení byla úspěšně použita. |
| Chyba | Došlo k chybě během procesu použití doporučení. Může to být přechodná problém, nebo případně schéma změnit do tabulky a skript už není platný. |
| Vrácení zpět | Doporučení byl použit, ale je považována za nenáročných a bude automaticky obnoven. |
| Vrátit zpět | Doporučení byl obnoven. |

Klikněte na doporučení v procesu ze seznamu zobrazíte více podrobností:

![Doporučené indexy](./media/sql-database-advisor-portal/operations.png)


### <a name="reverting-a-recommendation"></a>Návrat doporučení

Pokud jste použili Poradce použít doporučení (což znamená, že jste nespustili ručně skript T-SQL) se automaticky vrátí ho Pokud najde negativní vliv na výkon. Pokud z nějakého důvodu chcete pouze vrátit doporučení můžete udělat následující:


1. V oblasti **Optimalizace historie** vyberte úspěšně použita doporučení.
2. Příkaz **Obnovit** na zásuvné **doporučení podrobnosti** .

![Doporučené indexy](./media/sql-database-advisor-portal/details.png)


## <a name="monitoring-performance-impact-of-index-recommendations"></a>Sledování vliv na výkon index doporučení

Po úspěšném provádění doporučení (v současnosti operace indexování a parametrizovat dotazy doporučení pouze) můžete kliknout na **Přehledy dotazu** na zásuvné podrobnosti doporučení [Přehledy výkonu dotazu](sql-database-query-performance.md) otevřít a podívat dopad na výkon nejčastější dotazy.

![Vliv na výkon monitoru](./media/sql-database-advisor-portal/query-insights.png)



## <a name="summary"></a>Souhrn

Konference Advisor SQL databáze obsahuje doporučení pro zvýšení výkonu databáze SQL. Zadáním T-SQL skripty i osoby a plně – automatické (aktuálně pouze index), obsahuje Poradce užitečné pomoc při optimalizace databáze a nakonec zvýšení výkonu dotazu.



## <a name="next-steps"></a>Další kroky

Sledování vaší doporučení a i nadále je Upřesnit výkonu. Databáze úloh dynamické a změňte nepřetržitě. Databáze SQL advisor zůstanou sledovat a poskytují doporučení, které potenciálně dosáhnout zvýšení výkonu vaší databáze. 

 - Základní informace o Advisor databáze SQL naleznete v tématu [Poradce databáze SQL](sql-database-advisor.md) .
 - V tématu [Přehledy výkonu dotazu](sql-database-query-performance.md) se naučit používat zobrazení dopad na výkon nejčastější dotazy.

## <a name="additional-resources"></a>Další zdroje informací

- [Úložiště dotazu](https://msdn.microsoft.com/library/dn817826.aspx)
- [VYTVOŘENÍ INDEXU](https://msdn.microsoft.com/library/ms188783.aspx)
- [Řízení přístupu na základě rolí](../active-directory/role-based-access-control-configure.md)






