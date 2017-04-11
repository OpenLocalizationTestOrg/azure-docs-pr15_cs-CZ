<properties
    pageTitle="Konfigurace DHCPv6 pro Linux VMs | Microsoft Azure"
    description="Jak nastavit DHCPv6 pro Linux VMs."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="protokol IPv6 Vyrovnávání zatížení azure, duální zásobníku, veřejnou ip, nativní protokol ipv6, mobilní telefon, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="configuring-dhcpv6-for-linux-vms"></a>Konfigurace DHCPv6 pro Linux VMs

Na některých obrázcích virtuálního počítače Linux v Azure Marketplace nemají ve výchozím nastavení nakonfigurována DHCPv6. Podporuje protokol IPv6, musí být nakonfigurováno v DHCPv6 v distribuční Linux OS, které používáte. Různé distribuce Linux mají různé způsoby konfigurace DHCPv6, protože používají různé balíčků.

>[AZURE.NOTE] Poslední SUSE Linux a jádro operačního systému obrázky v Azure Marketplace byly předkonfigurovaná s DHCPv6. Žádná další změny, které jsou potřeba při použití těmito obrázky.

Tento dokument popisuje, jak povolit DHCPv6 tak, aby virtuálního počítače Linux získá adresy IPv6.

>[AZURE.WARNING] Nesprávně upravovat soubory konfigurace sítě mohou způsobit ztratíte přístup k síti vaší v angličtině. Doporučujeme otestovat provedené změny konfigurace systémech testovacím. Pokyny v tomto článku jste otestovali na nejnovější verze Linux obrázky v Azure Marketplace. V dokumentaci pro konkrétní verzi Linux podrobnější pokyny.

## <a name="ubuntu"></a>Se systémem Ubuntu

1. Upravte soubor `/etc/dhcp/dhclient6.conf` a přidejte následující řádek:

        timeout 10;

2. Úprava konfigurace sítě pro rozhraní eth0 s nastavením těchto věcí:

    * V **systémem Ubuntu 12.04 a 14.04**upravte soubor`/etc/network/interfaces.d/eth0.cfg`
    * Na **systémem Ubuntu 16.04**upravte tento soubor`/etc/network/interfaces.d/50-cloud-init.cfg`

    ```bash
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. Obnovení IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Upravte soubor `/etc/dhcp/dhclient6.conf` a přidejte následující řádek:

        timeout 10;

2. Upravte soubor `/etc/network/interfaces` a přidejte následující konfigurace:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Obnovení IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Upravte soubor `/etc/sysconfig/network` a přidejte následující parametr:

        NETWORKING_IPV6=yes

2. Upravte soubor `/etc/sysconfig/network-scripts/ifcfg-eth0` a přidejte následující dva parametry:

        IPV6INIT=yes
        DHCPV6C=yes

3. Obnovení IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Poslední SLES a openSUSE obrázky v Azure byly předkonfigurovaná s DHCPv6. Žádná další změny, které jsou potřeba při použití těmito obrázky. Pokud máte OM podle starší nebo vlastní SUSE obrázku, použijte následující kroky:

1. Instalace `dhcp-client` balíček, v případě potřeby:

    ```bash
    # sudo zypper install dhcp-client
    ```

2. Upravte soubor `/etc/sysconfig/network/ifcfg-eth0` a přidejte následující parametr:

        DHCLIENT6_MODE='managed'

3. Obnovení IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 a openSUSE přestupných

Poslední SLES a openSUSE obrázky v Azure byly předkonfigurovaná s DHCPv6. Žádná další změny, které jsou potřeba při použití těmito obrázky. Pokud máte OM podle starší nebo vlastní SUSE obrázku, použijte následující kroky:

1. Upravte soubor `/etc/sysconfig/network/ifcfg-eth0` a nahradit tento parametr

        #BOOTPROTO='dhcp4'

    pomocí následující hodnoty:

        BOOTPROTO='dhcp'

2. Přidejte následující parametr `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Obnovení IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>Jádro operačního systému

Poslední jádro operačního systému obrázky v Azure byly předkonfigurovaná s DHCPv6. Žádná další změny, které jsou potřeba při použití těmito obrázky. Pokud máte OM podle starší nebo vlastní obrázek jádro operačního systému, použijte následující kroky:

1. Upravit tento soubor`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Obnovení IPv6 address:

    ```bash
    # sudo systemctl restart systemd-networkd
    ```
