# <a name="pull-request-comment-automation"></a>Vyžádat žádost o automatické komentář

Používáme automatizaci komentář v komentářích žádost o vyžádané přispěvatelům povolit a autoři přiřadit popisky, které jednotka žádost vyžádané zkontrolujte obrázku.

| Komentář | Co dělá | Dostupnost|
| -------- |-------------|-------------|
|#odhlášení | Když Autor článek zadá komentář **#sign nepracovní** proudu komentář, přiřadí se nápisem **Připraveno ke sloučení** . | Veřejné a soukromé|
|#odhlášení | Pokud Přispěvatel, který není uvedený Autor snaží odhlášení na žádost o veřejné vložit pomocí komentář **#sign nepracovní** , zprávy se aby došlo k zápisu vyžádané žádost o označovat tak, že jenom autorem je možné stanovit popisek. | Veřejné |
|#blokování vypnutí | Pokud zadáte **#hold nepracovní** v komentáři žádost o vyžádané, odebere nápisem **Připraveno ke sloučení** – v případě náhodou později rozmysleli nebo uděláte chybu. V soukromé repo to přiřadí štítky **hromadné korespondence proveďte není** . | Veřejné a soukromé |
| #Chci se toho naučit-závěr | Autoři **please #-závěr** komentář zadat v toku komentář žádost vyžádané zavřete, pokud se rozhodnete nechcete mít sloučené změny. | Veřejné |

##<a name="troubleshooting-sign-offs-in-the-public-repo"></a>Poradce při potížích ve veřejných repo střídání znak

Znaménko veřejné repo vypnout automatické je umožňuje pouze autorovi odhlášení. Může být potřeba nějaké ruční výjimce zpracování:

- **Článek autoři**: můžete automatizaci komentář veřejné úložiště účtu skutečné GitHub se musí přesně shodovat účtu GitHub uvedených v článku metadata. Záleží na psaní velkých písmen vašeho účtu. Pokud jsou blokovány podepisování kvůli potížím, posílat poštu do azdocprs alias.

- **Ostatní zaměstnance**: Pokud jste zaměstnanci, který je odhlášení jménem autora a jsou blokovány automatizace, obraťte se na azdocprs s odkazem na žádost o vložit. Určete, kdo se – PMs na stejné produktového týmu, kolegové na psaní týmu a psaní vedoucí týmu jsou považovány za důvěryhodný.



## <a name="related"></a>Související

- [Vložit žádost o etiketu a osvědčené postupy pro Microsoft přispěvatelů](contributor-guide-pull-request-etiquette.md)

- [Kritéria kvality vyžádané požadavku na zkontrolovat](contributor-guide-pr-criteria.md)
