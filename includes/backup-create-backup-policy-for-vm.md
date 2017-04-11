## <a name="defining-a-backup-policy"></a>Definování zásady zálohování

Zásady zálohování definuje matici ze kdy pořízené snímky dat, a jak dlouho se zachovají tyto snímky. Při definování zásad pro zálohování virtuálního počítače, můžete aktivovat úlohy zálohování *jednou za den*. Při vytváření nové zásady se použije do trezoru. Rozhraní zásady zálohování vypadat takto:

![Zásady zálohování](./media/backup-create-policy-for-vms/backup-policy.png)

K vytvoření zásad:

1. Zadejte název pro **její název**.

2. Snímky dat může pořizovat intervalech denně nebo týdně. Pomocí nabídky rozevírací seznam **Zálohování počet_plateb** rozhodnout, jestli jsou data snímky pořizovat denně nebo týdně.

    - Pokud se rozhodnete denní interval, použijte ovládací zvýrazněný vyberte čas, kdy pro snímek. Změna hodiny, zrušte výběr hodiny a vyberte novou hodinu.

    ![Zásady denní zálohování](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>

    - Pokud se rozhodnete týdenní interval, vyberte dny v týdnu a čas, kdy se snímku pomocí zvýrazněný ovládacích prvků. V nabídce den vyberte jeden nebo více dní. V nabídce hodinu vyberte jednu hodinu. Změna hodiny, zrušte výběr vybrané hodiny a vyberte nové hodinu.

    ![Týdenní zásady zálohování](./media/backup-create-policy-for-vms/backup-policy-weekly.png)

3. Ve výchozím nastavení jsou vybrány všechny možnosti **Oblasti uchovávání informací** . Zrušte zaškrtnutí políčka omezení rozsahu uchovávání informací, které nechcete používat. Zadejte interval(s) používat.

    Měsíční a roční uchovávání informací oblastí umožňují určit snímky podle týdenní a denní přírůstek.

    >[AZURE.NOTE] Při ochraně virtuálního počítače, spustí úlohy zálohování jednou za den. Čas při spuštění zálohování je stejný pro každou oblast uchovávání informací.

4. Po nastavení možností pro zásady, v horní části zásuvné klikněte na **Uložit**.

    Nové zásady ihned použije trezoru.
