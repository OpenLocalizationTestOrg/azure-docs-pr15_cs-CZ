Azure bude určovat, že vaše aplikace používá Python **pravdivosti obou těchto podmínek**:

- soubor Requirements.txt v kořenové složce
- libovolný soubor .py v kořenové složce nebo runtime.txt, která určuje python

Když to je případ, použije skriptu konkrétní nasazení Python, která zajišťuje standardní synchronizaci souborů, jakož i další Python operací, jako:

- Automatická správa virtuální prostředí
- Instalace balíčků uvedené v requirements.txt pomocí pip
- Vytvoření odpovídající web.config založené na vybraných Python verzi.
- Shromáždit statické soubory Django aplikací

Některé aspekty kroků nasazení výchozí můžete určit, aniž byste museli vlastní skript.

Pokud chcete přeskočit všechny kroky konkrétními Python, můžete vytvořit prázdný soubor:

    \.skipPythonDeployment

Větší kontrolu nad nasazení je možné přepsat výchozí nasazení skript vytvořením následující soubory:

    \.deployment
    \deploy.cmd

[Azure rozhraní příkazového řádku][] můžete použít k vytvoření souborů.  Pomocí následujícího příkazu ze složky projektu:

    azure site deploymentscript --python

Jestliže neexistují tyto soubory, Azure vytvoří skript dočasné nasazení a spustí ho.  Je shodný s tu, kterou vytvoříte pomocí příkazu výše.

[Azure rozhraní příkazového řádku]: http://azure.microsoft.com/downloads/
