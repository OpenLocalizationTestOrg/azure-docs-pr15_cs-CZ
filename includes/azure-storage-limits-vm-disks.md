Azure virtuální počítač podporuje připojení počet disků data. Pro optimální výkon budete chtít omezit počet vysoce utilized disků připojené k virtuálního počítače Chcete-li předejít možné omezení. Pokud všech discích nejsou využívá vysoce ve stejnou dobu, účtu úložiště podporuje větší číslo discích.

- **u účtů standardní úložiště:** Účet standardní úložiště obsahuje maximální celkové žádost výši 20 000 procesorů. Celkové procesorů ve všech disků virtuálního počítače v účtu standardní úložiště neměli překročit toto omezení.

    Přibližně můžete vypočítat počet vysoce utilized disků nepodporuje účet jediný standardní úložiště na základě sazeb limitu žádost. Například pro základní OM osy, maximální počet vysoce utilized disků je o 66 (20 000/300 procesorů na disku) a standardní OM osy, je asi 40 (20 000/500 procesorů na disku), jak je uvedeno v následující tabulce. 
 
- **Účet úložiště premium:** Účet úložiště premium má maximální celkový výkon výši 50 s technologií. Celkový výkon všech disků OM neměli překročit toto omezení.