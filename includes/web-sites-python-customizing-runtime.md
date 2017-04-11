Azure bude určení verze systému Python pro účely jeho virtuální prostředí následující priority (priorita):

1. verze podle runtime.txt v kořenové složce
1. verze zadaná nastavením Python v konfiguraci webové aplikace ( **Nastavení** > zásuvné**Nastavení aplikace** pro web app na portálu Azure)
1. Python 2.7 je výchozí nastavení, pokud nejsou zadány žádná z výše uvedených možností

Platné hodnoty pro obsah 

    \runtime.txt

jsou:

- Python 2.7
- Python 3.4

Pokud malých verzi není zadán (třetí znak), bude ignorována.
