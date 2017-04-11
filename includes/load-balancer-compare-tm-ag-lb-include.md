## <a name="load-balancer-differences"></a>Rozdíly Vyrovnávání zatížení

Existují různé možnosti distribuce pomocí Microsoft Azure v síti. Tyto možnosti fungují odlišně od sebe, s jinou funkci nastavení a podpora různých scénářích. Každý mohou použít samostatně, nebo jejich sloučením.

- **Vyrovnávání zatížení Azure** pracuje na vrstva (4 vrstvy ve vrstvě odkaz OSI sítě). Poskytuje rozdělení úrovni sítě přenosy všech instancí aplikace ve stejném Azure datovém centru.

- **Bráně aplikace** funguje ve vrstvě aplikací (vrstvy 7 ve vrstvě odkaz OSI sítě). Funguje jako služba obrácené pořadí proxy, ukončení připojení klienta a přeposílání žádostí o back-end koncové body.

- **Přenosy správce** funguje na úrovni DNS.  Použije odpovědi DNS směrování adres koncových uživatelů k globálně distribuované koncové body. Klienti připojte k těmto koncové body přímo.

Následující tabulka shrnuje funkce poskytované každé služby:

| Služba | Vyrovnávání zatížení Azure | Aplikace brány | Přenosy správce |
|---|---|---|---|
|Technologie| Přenosová úroveň (vrstvy 4) | Úroveň aplikace (Layer 7) | Úroveň DNS |
| Protokoly aplikace podporované | Všechny | Nastavit informace HTTP a HTTPS |  Některé (koncový bod HTTP je nutný pro sledování koncový bod) |
| Koncové body | Azure instance role VMs a Cloudovým službám | Azure interní IP adresa nebo veřejných internet IP adresa | Azure VMs, cloudovými službami, Azure Web Apps a externí koncové body |
| Podpora Vnet | Lze použít pro obě aplikace Internet vystaveného a interní (Vnet) | Lze použít pro obě aplikace Internet vystaveného a interní (Vnet) |    Podporuje pouze internetové aplikace |
Sledování koncový bod | Podporované prostřednictvím sond | Podporované prostřednictvím sond | Podporované prostřednictvím protokolu HTTP/HTTPS GET | 

Azure Vyrovnávání zatížení a aplikace brány směrování sítě přenosy pro koncové body, ale mají různých scénářů využití které provoz na zpracovat. V následující tabulce pomáhá rozdíl mezi Vyrovnávání zatížení dvě:

| Typ | Vyrovnávání zatížení Azure | Aplikace brány |
|---|---|---|
| Protokoly | UDP/TCP | NASTAVIT INFORMACE HTTP / HTTPS |
| Rezervace IP | Podporované | Nepodporovaná funkce | 
| Režim vyrovnávání zatížení | 5 – tuple(source IP, source port, destination IP, destination port, protocol type) | Kruhové<br>Směrování podle adresy URL | 
| Režim pro vyrovnávání zatížení (Zdrojová IP adresa / rychlé relace) |  2 – řazené kolekce členů (zdroj IP a cílová adresa IP), 3-n-tici (zdroj IP cílová adresa IP a port). Můžete změnit velikost nahoru nebo dolů na základě počtu virtuálních počítačích | Na základě souborů cookie spřažení<br>Směrování podle adresy URL |
| Stav sond | Výchozí: zkušební interval - 15 sekundy. Přijatá mimo otočení: 2 průběžné k chybám. Sond podporuje definované uživatelem | Interval nečinnosti zkušební 30 sekund. Považuje za 5 selhání po sobě jdoucí živou přenosy nebo selhání jednoho zkušební v nedělá režimu. Sond podporuje definované uživatelem | 
| Odstranění SSL | Nepodporovaná funkce | Podporované | 
  