Skript pro nasazení přeskočí vytvoření virtuální prostředí v Azure zjistí, že již existuje prostředí virtuální kompatibilní.  To můžete urychlit nasazení značně.  Balíčky, které jsou už nainstalované bude přeskočilo tak, že pip.

V určitých případech je vhodné vynutit odstranit tento virtuální prostředí.  Je vhodné provést, pokud se rozhodnete prostředí virtuální jako součást zahrnout úložiště.  Můžete taky udělat, když potřebujete zbavit určitých balíčků nebo test změny requirements.txt.

Správa existující prostředí virtuální na Azure několika způsoby:

### <a name="option-1-use-ftp"></a>Možnost 1: Použití FTP

S klientem FTP připojit k serveru a budete moct odstraňte složku obálka.  Všimněte si, že v některých klientech FTP (například webových prohlížečích) může být jen pro čtení a nebude možné odstranit složky, takže budete chtít zkontrolujte, že klient FTP pomocí této funkce.  Název hostitele FTP a uživatele se zobrazují v zásuvné webovou aplikaci na [Portál Azure](https://portal.azure.com).

### <a name="option-2-toggle-runtime"></a>Možnost 2: Přepnout runtime

Tady je alternativu, který využívá na to, že bude skript pro nasazení Pokud neodpovídá svou požadovanou verzi Python odstraňte složku obálka.  Efektivní to bude odstranit existující prostředí a vytvořte nový účet.

1. Přepnutí na jinou verzi Python (přes runtime.txt nebo zásuvné **Nastavení aplikace** v portálu Azure)
1. Libovolná nabízená některé změny (ignorovat všechny chyby při instalaci pip pokud existuje)
1. Přejděte zpátky k původní verzi Python
1. Libovolná nabízená některé změny znovu

### <a name="option-3-customize-deployment-script"></a>Možnost 3: Přizpůsobení skript pro nasazení

Pokud jste přizpůsobili skript pro nasazení, můžete změnit kód v deploy.cmd vynutit odstraňte složku obálka.
