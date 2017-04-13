<properties
    pageTitle="Příklad infrastruktury návodu | Microsoft Azure"
    description="Informace o klíčových návrh a implementace pokyny pro nasazení infrastrukturu příklad v Azure."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="example-azure-infrastructure-walkthrough"></a>Příklad Azure infrastruktury návod

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Tento článek provede vytvoření infrastrukturu příklad aplikace. Jsme podrobností navrhování infrastrukturu pro jednoduché online úložiště, které spojují pokyny a rozhodnutí kolem konvence sady dostupnost, virtuálních sítí a vyrovnávání zatížení a skutečně nasazení virtuálních počítačích (VMs).


## <a name="example-workload"></a>Příklad pracovního vytížení

Adventure Works cyklů chce k vytváření online úložiště aplikace v Azure, která je tvořena:

- Dva IIS servery front-end ve vrstvě webový klient
- Dva servery služby IIS zpracování dat a objednávky v aplikační vrstvy
- Dvě instance skupinám AlwaysOn dostupnosti (dva servery SQL a monitorovací uzel většinou) pro ukládání údajů o produktech a objednávky ve vrstvě databáze Microsoft SQL Server
- Dva řadiče domény Active Directory pro zákazníky a dodavatele ve vrstvě ověřování
- Všechny servery jsou umístěny ve dvou podsítí:
    - front-end podsítě webových serverů 
    - back-end podsítě aplikační servery, clusteru SQL a řadiče domény

![Diagram různých úrovní pro infrastrukturu aplikace](./media/virtual-machines-common-infrastructure-service-guidelines/example-tiers.png)

Příchozí zabezpečeného webového provozu musí být vyrovnávání zatížení mezi webových serverů zákazníci procházení online úložiště. Pořadí zpracování přenosy ve formě HTTP žádosti z webu serverech musí být rovnoměrně mezi servery aplikace. Kromě toho infrastruktury musí určený pro dostupnost.

Musí mít výsledný návrhu:

- Účet a Azure předplatného
- Skupina jednoho zdroje
- Účty úložiště
- Virtuální sítě s dvěma podsítí
- Dostupnost nastaví pro VMs s podobným rolí
- Virtuálních počítačích

Všechny výše uvedené postupujte podle těchto názvů:

- Adventure Works cyklů používá **[IT pracovní zátěž]-[místa]-[Azure zdroje]** jako předpony
    - V tomto příkladu "**azos**" (Azure online úložiště přihlašovacích údajů) je název pracovního vytížení IT a je umístění. "**použít**" (východoasijských USA 2)
- Účty úložiště pomocí adventureazosusesa**[popis]**
    - "adventure" jsme přidali předponu k poskytování jedinečnosti a názvy účtů úložiště nepodporují využívání spojovníky.
- Virtuální sítě pomocí AZOS použití VN**[počet]**
- Dostupnost sady používat azos-použití-jako-**[role]**
- Virtuální názvy pomocí azos-použití-OM -**[vmname]**


## <a name="azure-subscriptions-and-accounts"></a>Účty a Azure předplatná

Adventure Works cyklů používá Enterprise předplatné, s názvem Adventure Works Enterprise předplatné poskytnout fakturace pro tento pracovní zátěž IT.


## <a name="storage-accounts"></a>Účty úložiště

Adventure Works cyklů určují, které nepotřebujete dva účty úložiště:

- **adventureazosusesawebapp** standardní úložištěm webových serverů, aplikace servery a řadiče domény a jejich disků data.
- **adventureazosusesasql** úložištěm Premium VMs SQL serveru a jejich disků data.


## <a name="virtual-network-and-subnets"></a>Virtuální sítě a podsítí

Protože virtuální sítě nemusí probíhající připojení k síti místní Adventure práce cyklů, rozhodli v síti virtuální jen cloudu.

Vytvořili jen cloudu virtuální sítě s tímto nastavením Azure portálu:

- Název: AZOS-použití VN01
- Umístění: Východoasijských USA 2
- Virtuální sítě adresní prostor: 10.0.0.0/8
- První podsítě:
    - Název: FrontEnd
    - Adresa místa: 10.0.1.0/24
- Druhá podsítě:
    - Název: back-end
    - Adresa místa: 10.0.2.0/24


## <a name="availability-sets"></a>Dostupnost sady

Abyste zachovali dostupnost ze všech čtyř úrovní online úložiště, Adventure Works cyklů rozhodne čtyři sady dostupnost:

- **azos použít jako webový** webových serverů
- **azos používejte jako aplikaci** pro servery aplikace
- **azos pomocí jako sql** serverů SQL Server
- **azos použití jako datacentrum** pro řadiče domény


## <a name="virtual-machines"></a>Virtuálních počítačích

Adventure Works cyklů rozhodne tyto názvy jejich Azure VMs:

- **azos použití OM web01** pro první webový server
- **azos použití OM web02** pro druhý webový server
- **azos použití OM app01** pro první aplikační server
- **azos použití OM app02** pro druhý aplikační server
- **azos použití OM sql01** prvního serveru SQL Server clusteru
- **azos použití OM sql02** druhý SQL serveru v clusteru
- **azos použití OM dc01** prvního řadiče domény
- **azos použití OM dc02** druhého řadiče domény

Tady je Výsledná konfigurace.

![Konečné aplikace infrastruktury nasazenou v Azure](./media/virtual-machines-common-infrastructure-service-guidelines/example-config.png)

Konfigurace zahrnuje:

- Jen cloudu virtuální sítě s dvěma podsítí (FrontEnd a back-end)
- Dva účty úložiště
- Čtyři sady dostupnost, jedno pro každé osy online úložiště
- Virtuálních počítačích čtyři úrovně
- Externí rozloženy nastavení pro webovou HTTPS umožnění datových přenosů z Internetu do webových serverů
- Vnitřní zatížení rovnováha nastavení pro přenos nešifrovaném web z webových serverů k serverům aplikace
- Skupina jednoho zdroje


## <a name="next-steps"></a>Další kroky

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 