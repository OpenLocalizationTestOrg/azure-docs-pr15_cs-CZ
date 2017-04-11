Zdroje|Výchozí Limit|Maximální Limit
---|---|---
Azure Media Services (AMS) účty v jedné předplatného||25
Aktiva za účet AMS||1,000,000<sup>1</sup>
Zřetězené úkolů na projekt||30
Prostředky jednoho úkolu||50
Prostředky na projekt||100
Práce podle AMS účtu ||více než 50 000<sup>2</sup>
Jedinečný Locator přidružené aktivum najednou||5<sup>4</sup>
Kanály živé podle AMS účtu </p></td>|5</p></td>|Není k dispozici<sup>1</sup>
Programy v přestal stavu na kanál </p></td>|50</p></td>|Není k dispozici<sup>1</sup>
Programy spuštěné stavu na kanál </p></td>|3</p></td>|3
Přenos koncové body v stav každý účet AMS po spuštění</p></td>|2</p></td>|Není k dispozici<sup>1</sup>
Přenos jednotek na streamování koncový bod </p></td>|10 </p></td>|Není k dispozici<sup>1</sup>
Kódování jednotek na AMS účtu </p></td>|25</p></td>|Není k dispozici<sup>1</sup>
Účty úložiště | |1 000<sup>5</sup>
Zásady || 1,000,000<sup>6</sup>

<sup>1</sup> můžete požadovat aktualizovat limity pro tuto kvótu otevřením požadavek podpory můžete. Nevytvářejte další účty AMS zvětšit omezení, místo toho odeslat požadavek podpory můžete.

<sup>2</sup> toto číslo obsahuje úlohy ve frontě dokončení, aktivní a zrušené. Neobsahuje odstraněný úlohy. Můžete odstranit staré úlohy pomocí **IJob.Delete** nebo **Odstranit** žádost HTTP.

<sup>3</sup> při provádění žádost seznamu úlohy entity, než 1 000, bude vrácena na žádost o. Pokud potřebujete ke sledování všech odeslaných úloh, můžete použít horní/přeskočit popsanou v [dotazu OData systému](http://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> Locator nejsou určeny pro správu řízení přístupu uživatelů. Různé přístupových práv k jednotlivým uživatelům, použijete řešení Správa digitálních práv (DRM).

<sup>5</sup> úložiště účty musí být ze stejného Azure předplatného.

<sup>6</sup> má limit 1,000,000 zásad pro různé zásady AMS (například pro Locator zásad nebo ContentKeyAuthorizationPolicy). Pokud používáte vždy stejné dny / přístupová oprávnění byste měli použít stejné ID zásad / atd.
