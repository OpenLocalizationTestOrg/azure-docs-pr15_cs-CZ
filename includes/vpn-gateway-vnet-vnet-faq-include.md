- Virtuální sítě může být ve stejném nebo jiném Azure oblasti (umístění).

- Do cloudové služby nebo k načtení vyrovnávání koncového bodu nemůže zahrnovat virtuální sítích, i když jsou připojené společně.

- Připojení více Azure virtuální sítí společně nevyžaduje všechny místní VPN brány, pokud je vyžadováno připojení mezi místní.

- VNet VNet podporuje připojení přes virtuální sítě. Ho nepodporuje připojení virtuálních počítačích nebo cloudových služeb není v síti virtuální.

- VNet VNet vyžaduje Azure VPN bran s RouteBased (dříve nazývanou dynamické směrování) typy VPN. 

- Připojení k síti virtuální lze současně s více webů VPN, maximálně 10 (výchozí/standardní bran) nebo 30 (vysoký výkon bran) VPN propojení brány virtuální sítě VPN připojení k jiné virtuální sítě pro místní weby.

- Nesmí překrývat adresu mezery virtuální sítí a místních webů místní síti. Překrývající se adresa mezery způsobí vystavením VNet VNet připojení k chybě.

- Nadbytečné tunelů mezi dvěma virtuálních sítí nejsou podporované.

- Všechny VPN tunelů virtuální sítě sdílet dostupné šířky pásma na brána Azure VPN a provozu brány stejné VPN SLA v Azure.

- Přenosy VNet VNet prochází přes Microsoft Network, nikoli na Internetu.

- Přenosy VNet VNet ve stejné oblasti je zadarmo obou směrech; křížové výstupní oblasti VNet VNet přenosy Účtovaná s přenos dat odchozí VNet mimo rychlostí podle regionů zdroje. Získáte [ceny stránku](https://azure.microsoft.com/pricing/details/vpn-gateway/) podrobnosti.