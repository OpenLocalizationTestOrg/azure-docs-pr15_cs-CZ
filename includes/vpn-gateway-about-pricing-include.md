Zaplatit dvě věci: hodinové výpočet nákladů na virtuální sítě brány a přenosu výstupní dat z brány virtuální sítě. Ceny informace najdete na stránce [ceny](https://azure.microsoft.com/pricing/details/vpn-gateway) .

**Virtuální sítě brány výpočetním nákladů**<br>Brána pro každý virtuální sítě nabízí každou hodinu výpočet nákladů. Cena vychází z brány SKU zadané při vytváření virtuálních síťové brány. Náklady je brány sebe sama a kromě přenos dat, které prochází brány.

**Náklady na převod dat**<br>Data přenos počítá náklady založené na výstupní adres brány virtuální sítě zdroje.

- Jestliže odesíláte přenosy do zařízení místní VPN, strhne příslušná s rychlostí přenosu výstupní Internet.
- Jestliže odesíláte komunikaci mezi virtuálních sítí v různých oblastech, je založená ceny oblasti.
- Pokud odesíláte přenosy pouze mezi virtuální sítí, které jsou ve stejné oblasti, je potřeba žádná data nákladů. Přenos mezi VNets ve stejné oblasti je zadarmo.