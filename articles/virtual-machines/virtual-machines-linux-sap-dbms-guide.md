<properties
   pageTitle="SAP NetWeaver na Linux virtuálních počítačích (VMs) – Průvodce nasazením systému správy databáze | Microsoft Azure"
   description="SAP NetWeaver na Linux virtuálních počítačích (VMs) – Průvodce nasazením systému správy databáze"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# <a name="sap-netweaver-on-azure-virtual-machines-vms--dbms-deployment-guide"></a>SAP NetWeaver na Azure virtuálních počítačích (VMs) – Průvodce nasazením systému správy databáze

[767598]:https://service.sap.com/sap/support/notes/767598
[773830]:https://service.sap.com/sap/support/notes/773830
[826037]:https://service.sap.com/sap/support/notes/826037
[965908]:https://service.sap.com/sap/support/notes/965908
[1031096]:https://service.sap.com/sap/support/notes/1031096
[1139904]:https://service.sap.com/sap/support/notes/1139904
[1173395]:https://service.sap.com/sap/support/notes/1173395
[1245200]:https://service.sap.com/sap/support/notes/1245200
[1409604]:https://service.sap.com/sap/support/notes/1409604
[1558958]:https://service.sap.com/sap/support/notes/1558958
[1585981]:https://service.sap.com/sap/support/notes/1585981
[1588316]:https://service.sap.com/sap/support/notes/1588316
[1590719]:https://service.sap.com/sap/support/notes/1590719
[1597355]:https://service.sap.com/sap/support/notes/1597355
[1605680]:https://service.sap.com/sap/support/notes/1605680
[1619720]:https://service.sap.com/sap/support/notes/1619720
[1619726]:https://service.sap.com/sap/support/notes/1619726
[1619967]:https://service.sap.com/sap/support/notes/1619967
[1750510]:https://service.sap.com/sap/support/notes/1750510
[1752266]:https://service.sap.com/sap/support/notes/1752266
[1757924]:https://service.sap.com/sap/support/notes/1757924
[1757928]:https://service.sap.com/sap/support/notes/1757928
[1758182]:https://service.sap.com/sap/support/notes/1758182
[1758496]:https://service.sap.com/sap/support/notes/1758496
[1772688]:https://service.sap.com/sap/support/notes/1772688
[1814258]:https://service.sap.com/sap/support/notes/1814258
[1882376]:https://service.sap.com/sap/support/notes/1882376
[1909114]:https://service.sap.com/sap/support/notes/1909114
[1922555]:https://service.sap.com/sap/support/notes/1922555
[1928533]:https://service.sap.com/sap/support/notes/1928533
[1941500]:https://service.sap.com/sap/support/notes/1941500
[1956005]:https://service.sap.com/sap/support/notes/1956005
[1973241]:https://service.sap.com/sap/support/notes/1973241
[1984787]:https://service.sap.com/sap/support/notes/1984787
[1999351]:https://service.sap.com/sap/support/notes/1999351
[2002167]:https://service.sap.com/sap/support/notes/2002167
[2015553]:https://service.sap.com/sap/support/notes/2015553
[2039619]:https://service.sap.com/sap/support/notes/2039619
[2121797]:https://service.sap.com/sap/support/notes/2121797
[2134316]:https://service.sap.com/sap/support/notes/2134316
[2178632]:https://service.sap.com/sap/support/notes/2178632
[2191498]:https://service.sap.com/sap/support/notes/2191498
[2233094]:https://service.sap.com/sap/support/notes/2233094
[2243692]:https://service.sap.com/sap/support/notes/2243692

[azure-cli]:../xplat-cli-install.md
[azure-portal]:https://portal.azure.com
[azure-ps]:../powershell-install-configure.md
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../azure-subscription-service-limits.md#subscription

[dbms-guide]:virtual-machines-linux-sap-dbms-guide.md (SAP NetWeaver na Linux virtuálních počítačích (VMs) – Průvodce nasazením systému správy databáze) [dbms-guide-2.1]:virtual-machines-linux-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (ukládání do mezipaměti VMs a VHD) [dbms-guide-2.2]:virtual-machines-linux-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (Software RAID) [dbms-guide-2.3]:virtual-machines-linux-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (úložišti tabulek Microsoft Azure) [dbms-guide-2]:virtual-machines-linux-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (struktura nasazení RDBMS) [dbms-guide-3]:virtual-machines-linux-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (dostupnost a obnovení s Azure VMs) [dbms-guide-5.5.1]:virtual-machines-linux-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 a novější) [dbms-guide-5.5.2]:virtual-machines-linux-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 a Different releases) [dbms-guide-5.6]:virtual-machines-linux-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (pomocí serveru SQL Server obrázky z webu Microsoft Azure Marketplace) [dbms-guide-5.8]:virtual-machines-linux-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (Obecné serveru SQL Server pro SAP na souhrn Azure) [dbms-guide-5]:virtual-machines-linux-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (zvláštnosti pro SQL Server RDBMS) [dbms-guide-8.4.1]:virtual-machines-linux-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (konfigurace úložiště) [dbms-guide-8.4.2]:virtual-machines-linux-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (zálohování a obnovení) [dbms-guide-8.4.3]:virtual-machines-linux-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (důležité informace o výkonu pro zálohování a obnovení) [dbms-guide-8.4.4]:virtual-machines-linux-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (ostatní) [dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-linux-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-linux-sap-deployment-guide.md (SAP NetWeaver na Linux virtuálních počítačích (VMs) – Průvodce nasazením) [deployment-guide-2.2]:virtual-machines-linux-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP zdroje) [deployment-guide-3.1.2]:virtual-machines-linux-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (nasazení OM s vlastní obrázek) [deployment-guide-3.2]:virtual-machines-linux-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (scénář 1: nasazení OM z Azure Marketplace SAP) [deployment-guide-3.3]:virtual-machines-linux-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (scénář 2: nasazení OM s vlastního obrázku pro SAP) [deployment-guide-3.4]:virtual-machines-linux-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 () Scénář 3: Přesunutí virtuálního počítače z místních virtuálního pevného disku generalized Azure pomocí SAP) [deployment-guide-3]:virtual-machines-linux-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (nasazení scénáře z VMs pro SAP na Microsoft Azure) [deployment-guide-4.1]:virtual-machines-linux-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (rutiny prostředí PowerShell Azure nasazení) [deployment-guide-4.2]:virtual-machines-linux-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (stáhnout a Import SAP relevantní rutiny prostředí PowerShell) [deployment-guide-4.3]:virtual-machines-linux-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (OM připojení do místní domény – pouze Windows) [deployment-guide-4.4.2]:virtual-machines-linux-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [nasazení 4.4 Průvodce]: Virtual-Machines-Linux-SAP-Deployment-Guide.MD#c7cbb0dc-52a4-49DB-8E03-83e7edc2927d (ke stažení, instalace a povolit Azure OM Agent) [deployment-guide-4.5.1]:virtual-machines-linux-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure Powershellu) [deployment-guide-4.5.2]:virtual-machines-linux-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure rozhraní příkazového řádku) [deployment-guide-4.5]:virtual-machines-linux-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (konfigurace Azure rozšířeného sledování rozšíření SAP) [deployment-guide-5.1]:virtual-machines-linux-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Kontrola připravenosti pro Azure rozšířeného sledování SAP) [deployment-guide-5.2]:virtual-machines-linux-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Kontrola stavu Azure Sledování Konfigurace infrastruktury) [deployment-guide-5.3]:virtual-machines-linux-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (Další řešení potíží s Azure sledování infrastruktury SAP)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-linux-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-linux-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-linux-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-linux-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-linux-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-linux-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-linux-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-linux-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-linux-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-linux-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-linux-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-linux-sap-get-started.md
[getting-started-dbms]:virtual-machines-linux-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-linux-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-linux-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:virtual-machines-linux-sap-planning-guide.md (SAP NetWeaver na Linux virtuálních počítačích (VMs) – plánování a implementaci průvodce) [planning-guide-1.2]:virtual-machines-linux-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (zdroje) [planning-guide-11]:virtual-machines-linux-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (vysoké dostupnosti (HA) a havárie obnovení (DR) SAP NetWeaver spuštěný na virtuálních počítačích Azure) [planning-guide-11.4.1]:virtual-machines-linux-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (dostupnost pro servery aplikace SAP) [planning-guide-11.5]:virtual-machines-linux-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (pomocí automatické spuštění SAP instancí) [planning-guide-2.1]:virtual-machines-linux-sap-planning-guide.md# 1625df66-4CC6-4d60-9202-de8a0b77f803 (jen cloudu – nasazení virtuálního počítače do Azure bez závislostí na místní síti zákazníka) [planning-guide-2.2]:virtual-machines-linux-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (křížové místní – nasazení jeden nebo více VMs SAP do Azure s požadavkem je plně integrovaný v místní síti) [planning-guide-3.1]:virtual-machines-linux-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a (Azure oblasti) [planning-guide-3.2.1]:virtual-machines-linux-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (poruch domény) [planning-guide-3.2.2]:virtual-machines-linux-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (Upgrade domény) [planning-guide-3.2.3]:virtual-machines-linux-sap-planning-guide.md#18810088- F9BE - 4c 97-958a - 27996255c 665 (Azure dostupnost sady) [planning-guide-3.2]:virtual-machines-linux-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (Microsoft Azure virtuálního počítače pojem) [planning-guide-3.3.2]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (Azure Premium úložiště) [planning-guide-5.1.1]:virtual-machines-linux-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (přesunutí virtuálního počítače z místního na Azure s diskem generalized) [planning-guide-5.1.2]:virtual-machines-linux-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (nasazení OM s obrázkem určitého zákazníka) [planning-guide-5.2.1]:virtual-machines-linux-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (Příprava pro přechody OM z místního Azure s diskem generalized) [ Planning-Guide-5.2.2]:Virtual-Machines-Linux-SAP-Planning-Guide.MD#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (Příprava pro nasazení OM s obrázkem určitého zákazníka pro SAP) [planning-guide-5.2]:virtual-machines-linux-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (Příprava VMs služby Azure) [planning-guide-5.3.1]:virtual-machines-linux-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (rozdíl mezi Azure Disk a obrázek Azure) [planning-guide-5.3.2]:virtual-machines-linux-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (nahrávání virtuálního pevného disku z místního na Azure) [planning-guide-5.4.2]:virtual-machines-linux-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (kopírování disků mezi účty adresářové služby Azure úložiště) [planning-guide-5.5.1]:virtual-machines-linux-sap-planning-guide.md# 4efec401-91e0-40c0-8e64-f2dceadff646 (OM/virtuální pevný disk struktura SAP nasazení) [planning-guide-5.5.3]:virtual-machines-linux-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (nastavení automatického připojení pro připojených disků) [planning-guide-7.1]:virtual-machines-linux-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (jednoho OM s SAP NetWeaver ukázku/školení scénář) [planning-guide-7]:virtual-machines-linux-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (nasazení koncepty Cloud-Only instancí SAP) [planning-guide-9.1]:virtual-machines-linux-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure sledování řešení pro SAP) [planning-guide-azure-premium-storage]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (úložišti Premium Azure)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:virtual-machines-linux-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-linux-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../resource-group-overview.md
[resource-groups-networking]:../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../storage/storage-premium-storage.md
[storage-redundancy]:../storage/storage-redundancy.md
[storage-scalability-targets]:../storage/storage-scalability-targets.md
[storage-use-azcopy]:../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:virtual-machines-linux-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-linux-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes]:virtual-machines-linux-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasický nasazení modelu.

Tato příručka je součástí dokumentace o implementaci a nasazení software SAP na Microsoft Azure. Před čtení Tato příručka, přečtěte si prosím [plánování a implementaci Průvodce] [-příručka k plánování]. Tento dokument obsahuje nasazení různých relační databáze správy systémy (RDBMS) a související produkty v kombinaci s SAP na Microsoft Azure virtuálních počítačích (VMs) pomocí infrastruktury Azure jako možností služby (IaaS).

Papír doplňuje si přečtěte následující dokumentaci instalace SAP a SAP poznámky, které představují primární prostředky pro zařízení a nasazení softwaru SAP na danou platformách

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="general-considerations"></a>Obecné co byste měli zvážit
V této kapitoly co byste měli zvážit spuštění SAP související se systémy systému správy databáze v Azure VMs přináší. Existuje několik odkazy na určité systémy systému správy databáze v této kapitoly. Místo toho určité systémy systému správy databáze jsou zpracovány v rámci tohoto dokumentu po této kapitoly.

### <a name="definitions-upfront"></a>Definice předem
V celém dokumentu použijeme následující podmínky:

* IaaS: Infrastruktury služby.
* PaaS: Platformy jako služba.
* SaaS: Software jako služba.
* SAP součásti: jednotlivé SAP aplikace ATP ECC, pásma, správce řešení podnikovém portálu.  SAP součásti můžou být založená na tradiční ABAP nebo Java technologie nebo aplikace jiných NetWeaver na základě například organizační objektů.
* Prostředí SAP: jedna nebo více součástí SAP logicky seskupené provádět obchodní funkci ATP vývoj, QAS, školení, DR výroby.
* Orientace na šířku SAP: Toto nastavení se týká na celý majetek SAP v zákazníků IT orientace na šířku. Orientace na šířku SAP zahrnuje všechny výrobní a testovacím prostředí.
* SAP systém: Kombinací systému správy databáze a aplikace vrstva například SAP ERP vývoj systému, SAP pásma zkušebního systému, SAP CRM výrobního systému atd. V Azure nasazení není podporována dělit tyto dvě vrstvy mezi místním prostředím a Azure. To znamená, že je SAP systém používaný v místním nebo nasazenou v Azure. Však nástroje můžete nasazovat jiné systémy SAP na šířku v Azure nebo místní. Můžete třeba nasazení vývoj SAP CRM a testovat systémy v Azure, ale SAP CRM výrobní systém místní.
* Nasazení jen cloudu: nasazení, kde není Azure předplatné připojeného prostřednictvím webu na webu nebo ExpressRoute připojení k místní síťovou infrastrukturu. Společné Azure si přečtěte následující dokumentaci tyto typy nasazení jsou také popsány jako "Jen cloudu" nasazení. Virtuálních počítačích nasazený tímto způsobem se k nim získat přístup prostřednictvím Internetu a veřejné koncové body Internet přiřazená VMs v Azure. Místní služby Active Directory (AD) DNS není rozšířit Azure v těchto typech nasazení a. Proto VMs nejsou součástí na adresářová služba Active Directory. Poznámka: Nasazení jen cloudu v tomto dokumentu se definují takhle dokončení krajiny SAP, které jsou nastanou výhradně v Azure bez přípony služby Active Directory nebo překlad z místního veřejné cloudu. Konfigurace jen mraků nejsou podporovány konfigurace kde moduly SAP STM nebo jiných zdrojů na pracovišti nutné použít mezi systémům SAP hostitelem Azure a zdroje umístěných místní nebo systémům SAP výroby.
* Křížové místní: Popisuje scénáře, kde jsou VMs používaný k předplatnému Azure, který má na webu, více webů nebo ExpressRoute propojení mezi místním datacenter(s) a Azure. Společné Azure si přečtěte následující dokumentaci, tyto typy nasazení jsou také popsány jako místní křížově scénáře. Důvod, proč připojení je rozšíření místních domén, místní služby Active Directory a místní DNS do Azure. Orientace na šířku místní se rozšíří Azure majetku předplatné. S tuto linku, VMs může být součástí místní domény. Uživatelé domény místní domény můžete přistupovat k serverům a mohlo by umožnit spuštění služby na tyto VMs (třeba služby systému správy databáze). Komunikace a překladu mezi VMs nasazeném místní a VMs nasazenou v Azure je možné. Toto je nejběžnější scénář pro nasazení prostředky SAP na Azure očekávat. Viz [Tento] [ vpn-gateway-cross-premises-options] článek a [Tento] [ vpn-gateway-site-to-site-create] Další informace.

> [AZURE.NOTE] Mezi místním nasazení systémům SAP kde Azure virtuálních počítačích systémy SAP patří místní domény jsou podporovány u systémům SAP výroby. Konfigurace křížově místních poštovních jsou podporovány pro nasazení částí nebo dokončete SAP krajiny do Azure. I spuštění úplnou krajiny SAP v Azure vyžaduje s těmito VMs součást místní domény a služby Active Directory. V dřívější verze dokumentace jsme kontakt o hybridní IT scénáře, které se zobrazuje termín "Hybridní" tím, že je křížové místní připojení mezi místním prostředím a Azure. V tomto případě "Hybridní" také znamená, že VMs v Azure část na adresářová služba Active Directory.

Některé si přečtěte následující dokumentaci Microsoft popisuje scénáře křížové místních poštovních trochu jinak, zejména u konfigurací HA systému správy databáze. V případě SAP související dokumenty, více místní scénář jenom varu dolů na webu nebo soukromé připojení (ExpressRoute) a na to, že je mezi místním prostředím a Azure distribuovaná SAP orientace na šířku.

### <a name="resources"></a>Zdroje informací
Následující příručky jsou k dispozici v tématu nasazení SAP na Azure:

* [SAP NetWeaver na virtuálních počítačích Azure (VMs) – plánování a implementaci Průvodce] [-příručka k plánování]
* [SAP NetWeaver na Azure virtuálních počítačích (VMs) – Průvodce nasazením] [Průvodce nasazením]
* [SAP NetWeaver na Azure virtuálních počítačích (VMs) – Průvodce nasazením systému správy databáze (tohoto dokumentu)] [systému správy databáze příručka]

Následující poznámky SAP jsou související s tématem SAP na Azure:

| Číslo poznámky   | Název
|------------|--------
| [1928533] | SAP aplikací v Azure: typů podporovaných produktů a OM Azure
| [2015553] | SAP na Microsoft Azure: požadavky podpory
| [1999351] | Poradce při potížích s rozšířené Azure pro sledování SAP
| [2178632] | Klíč sledování metriky SAP na Microsoft Azure
| [1409604] | Virtualizace ve Windows: rozšířené sledování
| [2191498] | SAP na Linux s Azure: rozšířené sledování
| [2039619] | Aplikací SAP na Microsoft Azure pomocí databáze Oracle: podporované produkty a verze
| [2233094] | DB6: Aplikací SAP na Azure pomocí IBM DB2 Linux, UNIX a Windows – Další informace
| [2243692] | Linux na Microsoft Azure (IaaS) OM: SAP licenci problémy
| [1984787] | SUSE LINUX Enterprise Server 12: Poznámky k instalaci
| [2002167] | Červené klobouk Enterprise Linux 7.x: instalace a Upgrade

Přečtěte si taky [Oznámení změny stavu wikiwebu](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) , která obsahuje všechny poznámky SAP Linux.

Měli byste mít praxi o Microsoft Azure a jak nasazeném a spravovaný virtuálních počítačích Microsoft Azure. Další informace naleznete zde <https://azure.microsoft.com/documentation/>
 
> [AZURE.NOTE] Budeme **se zabývat platformu Microsoft Azure jako nabízené služby (PaaS) platformy Microsoft Azure** . Tento dokument je o spuštění systému správy databáze (systému správy databáze) v aplikaci Microsoft Azure virtuálních počítačích (IaaS) stejným způsobem jako systému správy databáze by byl spuštěn v místním prostředí. Funkce databáze funkcí mezi těmito dvěma nabídky jsou velmi liší a neměli smíšený mezi sebou. Viz taky: <https://azure.microsoft.com/services/sql-database/>

Protože budeme se zabývat IaaS, obecně Windows, Linux a systému správy databáze instalace a konfigurace jsou v podstatě shodný virtuálního počítače nebo úplné kovu počítače by instalace místní. Můžou ale nastat některých architekturu a systému správy implementaci rozhodnutí, která se budou lišit při využití IaaS. Tento dokument účel vysvětlit konkrétní architektonické a rozdíly systému správy, které se musí být počítat při použití IaaS.

Obecně celkové oblasti rozdílem, že tento dokument popisuje jsou:

* Plánování správné rozložení OM/virtuálního pevného disku systémům SAP ověřit, jestli že máte správná data soubor, rozložení a můžete dosáhnout dost procesorů pro vaše pracovní zátěž.
* Sítě co byste měli zvážit při použití IaaS.
* Funkce databáze používat chcete optimalizovat rozložení databáze.
* Zálohování a obnovení aspektech v IaaS.
* Použití různých typů obrázků nasazení.
* Dostupnost v Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Struktura RDBMS nasazení
V pořadí podle této kapitoly, je třeba porozumět tomu, jaký byl prezentovat [této] [nasazení příručka-3] kapitoly příručky [nasazení] [Průvodce nasazením]. Znalost různé OM řady a jejich rozdíly a Azure standardní a Premium úložiště rozdíly by mělo být chápat a známé před čtením této kapitoly.

Až březnem 2015 byly omezené až 127 GB velikosti VHD Azure, které obsahují operační systém. Toto omezení máte odvolat v březnem 2015 (pro další informace o zaškrtnutí <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/> ). Odtud na VHD obsahující operační systém nemůžou mít stejně velká jako jakékoli jiné virtuální pevný disk. Však jsme pořád raději strukturu nasazení, kde jsou nezávislá na databázové soubory operační systém, systému správy databáze a případné binární SAP. Proto Očekáváme, že v Azure virtuálních počítačích se systémy SAP bude mít základní OM (nebo virtuální pevný disk) s operačním systémem nainstalovanou databáze správy systému spustitelných a SAP spustitelných. Systému správy databáze dat a souborů protokolu budou uložené v úložišti Azure (standardní nebo Premium úložiště) v samostatných souborech virtuálního pevného disku a přiložený jako logické disků původní obrázek Azure operační systém OM. 

Závisí na nějaké využívání Azure standardní nebo Premium úložiště (například pomocí DS řady nebo GS řady VMs) jsou jiné kvóty Azure, což jsou popsány [tady][virtual-machines-sizes]. Při plánování VHD Azure, budete muset najít nejlepší zůstatek kvóty pro následující:

* Počet datových souborů.
* Počet VHD, které obsahují soubory.
* Kvóty procesorů jednoho virtuálního pevného disku.
* Výkon dat na virtuální pevný disk.
* Počet další možné VHD za OM velikost.
* Celkový výkon úložiště můžete poskytnout virtuálního počítače.
 
Azure vynutíte procesorů kvótu za jednotku virtuální pevný disk. Tyto kvóty se liší pro VHD hostitelem Azure standardní úložiště a Premium úložiště. Čekacích vstupu a výstupu dob budou příliš rozdíl mezi dva typy úložiště s úložištěm Premium předvádění faktory lepší čekacích dob vstupu a výstupu. Různé typy OM mají omezený počet VHD, které se chcete připojit. Další omezení je, že jenom určité typy OM můžete využít Azure Premium úložiště. To znamená, že rozhodnutí pro určitý typ OM nemusí pouze jsou závislé procesoru a paměti požadavků, ale také procesorů, latence a místo na disku výkon požadavky, které jsou obvykle měřítko s počtem VHD nebo typ disků Premium úložiště. Zejména v případě Premium úložiště velikost virtuálního pevného disku také může být dáno počet procesorů a výkon, kterou je potřeba dosáhnout tak, že každé virtuální pevný disk.

Tomu, že celkové rychlost procesorů počet VHD připojit a velikost OM je všechny stejným dohromady, může způsobit konfigurace systému SAP se liší od jeho místního nasazení služby Azure. Limity procesorů za logické jednotky jsou obvykle, která dokáže nahradit v místním nasazení. Vzhledem k tomu s úložištěm Azure jsou tyto limity pevné nebo jako Premium úložiště závisí na typu disku. Tak s v místním nasazení vidíme zákazníka konfigurace databáze servery, které používáte mnoho různých svazky zvláštní programů jiného, například SAP a systému správy databáze nebo jinak svazky pro dočasné databáze nebo tabulky mezery. Když přesunete místní systém Azure ho může vést k plýtvání potenciální šířky pásma procesorů promarnění virtuálního pevného disku spustitelné soubory nebo databáze, které neprovádějte některé nebo není hodně procesorů. V Azure VMs proto doporučujeme spustitelnými systému správy databáze a SAP nainstalované na disku OS Pokud je to možné.

Umístění databázové soubory a soubory protokolu a typ Azure použité úložiště, by měl být definované procesorů, zpoždění a požadavky na výkon. Pokud chcete mít dost procesorů transakční protokol, může vynucená využít více VHD souborů protokolu transakcí nebo použijte disk větší Premium úložiště. V tomto případě se jednoduše by sestavit softwarová RAID (jako je Windows úložiště fondu pro Windows nebo MDADM a LVM (Správce logických hlasitost) Linux) s VHD, která bude obsahovat transakční protokol.

___

> ![Windows][Logo_Windows] Windows
>
> Jednotka D:\ v Azure OM je jednotka není zachován, která zahrnuje některé místních discích na uzel Azure výpočetním. Protože je to není zachován, to znamená, že všechny změny provedené obsah na jednotce D:\ dojde ke ztrátě OM po restartování. Tak, že "změny" abychom nechtěli uložené soubory adresáře vytvořené, nainstalované aplikace, atd.
>
> ![Linux][Logo_Linux] Linux
>
> Linux Azure VMs jednotku v /mnt/resource, což je jednotka není zachován podporovaným místních discích na uzel Azure výpočetním automaticky připojit. Protože je to není zachován, to znamená, že všechny změny provedené na obsah v /mnt/resource dojde ke ztrátě OM po restartování. Všechny změny jsme nechtěli soubory uložené adresáře vytvořené, nainstalované aplikace, atd.

___

Závisí na Azure řady OM, místních discích na uzel výpočetním zobrazit různé výkonu, které se dá zařadit jako:

* A0 A7: Velmi omezený výkonu. Nelze použít něco za soubor modulu windows stránky
* A8 A11: Vlastnosti velmi dobrý výkon s některé procesorů 200 000 a > 1GB/sec výkon
* D-řada: Vlastnosti velmi dobrý výkon s některé procesorů 200 000 a > 1GB/sec výkon
* Pošta – řada: Vlastnosti velmi dobrý výkon s některé procesorů 200 000 a > 1GB/sec výkon
* G-řada: Vlastnosti velmi dobrý výkon s některé procesorů 200 000 a > 1GB/sec výkon
* GS-řada: Vlastnosti velmi dobrý výkon s některé procesorů 200 000 a > 1GB/sec výkon

Příkazy výše použít k těmto typům OM certifikované s SAP. Řada OM s vynikajícím procesorů a výkon nárok na využití tak, že některé funkce systému správy databáze, jako je tempdb nebo místo dočasné tabulky.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Ukládání do mezipaměti pro VMs a VHD
Při vytvoření těchto disků/VHD prostřednictvím portálu nebo při jsme připojit nahraný VHD VMs, jsme můžete zvolit, zda jsou v mezipaměti vstupu a výstupu komunikaci mezi OM a tyto VHD umístěn v Azure úložiště. Azure standardních a úložiště Premium použít dva různé technologie pro tento typ mezipaměti. V obou případech samotnou mezipamětí by být disku záložní na stejné jednotkách používaný dočasné disku (D:\ ve Windows) nebo /mnt/resource na Linux OM.
 
Standardní úložištěm Azure jsou možné mezipaměti typy:

* Bez ukládání do mezipaměti
* Přečtěte si ukládání do mezipaměti
* Čtení a zápis ukládání do mezipaměti

Abyste mohli získávat konzistentní a deterministický výkonu, je vhodné nastavit ukládání do mezipaměti Azure standardní úložný prostor pro všechny VHD obsahující **systému správy databáze související datové soubory, soubory protokolu a tabulky místo na "NONE"**. Ukládání do mezipaměti OM zůstat s výchozími.

Azure Premium úložištěm existují následující možnosti ukládání do mezipaměti:

* Bez ukládání do mezipaměti
* Přečtěte si ukládání do mezipaměti

Doporučení pro Azure Premium úložiště je můžete využít **Číst ukládání do mezipaměti pro datové soubory** databáze SAP a rozhodli, že **žádný ukládání do mezipaměti VHD(s) souborů protokolu**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>Software RAID
Co už uvedený, budete muset zůstatek počet procesorů potřebné pro soubory databáze přes počet VHD můžete nakonfigurovat a zadejte maximální procesorů Azure OM poskytne na disku virtuálního pevného disku nebo Premium úložiště. Nejjednodušší způsob, jak zacházet s načíst procesorů přes VHD je vytvářet software RAID přes různé VHD. Umístěte počet datových souborů systému správy databáze SAP na LOGICKÝM carved mimo software RAID. Závisí na požadavky, které že můžete chtít zvažte použití Premium úložiště a od dva ze tří různých discích Premium úložiště zadejte vyšší kvóty procesorů než VHD založené na standardní úložiště. Kromě významné lepší latence vstupu a výstupu poskytovanou Azure Premium úložiště. 

Totéž transakční protokol různých systémů systému správy databáze. S velkým množstvím je právě přidání další Tlog souborů nepomůže od systémy systému správy databáze zapisovat do jedné ze souborů najednou jenom. Případnému vyšší sazby procesorů než jeden Standard úložiště podle přináší virtuální pevný disk, můžete prokládanou přes více standardní VHD úložiště nebo můžete použít větší Premium disk typ úložiště, který po větších krocích procesorů také poskytuje faktory nižší latence pro zápis vstupně-výstupních do transakční protokol.
 
Situace došlo v Azure nasazení, které by kompresi pomocí softwaru RAID jsou:

* Transakční protokol/znovu protokol vyžadují více procesorů než Azure poskytuje jediné virtuálního pevného disku. Výše uvedené to bude možné vyřešit vytvořením logické jednotky přes více VHD pomocí softwaru RAID.
* Lichý vstupu a výstupu rozdělení pracovního vytížení přes různé datové soubory databáze SAP. V takovém případě jednu dojít zasažení kvóty raději často jeden datový soubor. Zatímco jiné datové soubory není slyšet i zavřít procesorů kvóty jednoho virtuálního pevného disku. V takovém případě Nejsnazším řešením je můžete vytvořit jednu logickou jednotku přes více VHD pomocí softwaru RAID. 
* Nevíte, co přesně vytížení vstupu a výstupu na datový soubor je a pouze zhruba ví, co je celková pracovní zátěž procesorů proti systému správy databáze. Nejjednodušší se má vytvořit jednu logickou jednotku pomocí softwaru RAID. Součet kvót více VHD za tuto logickou jednotku by pak splnění známé rychlost procesorů.

___

> ![Windows][Logo_Windows] Windows
>
> Použití systému Windows Server 2012 nebo vyšší mezery úložiště je konjunktivu, protože je výhodnější než Windows prokládání starší verzí systému Windows. Upozorňujeme, že může potřebujete k vytvoření fondů úložiště Windows a prostor úložiště příkazy Powershellu při použití jako operačního systému Windows Server 2012. Příkazy Powershellu najdete tady <https://technet.microsoft.com/library/jj851254.aspx>

> 
> ![Linux][Logo_Linux] Linux
>
> Pouze MDADM a LVM (Správce logických hlasitost) podporuje vytvářet software RAID na Linux. Další informace naleznete v následujících článcích:
>
> * [Konfigurace Software RAID na Linux] [ virtual-machines-linux-configure-raid] (pro MDADM)
> * [Konfigurace LVM na Linux OM v Azure][virtual-machines-linux-configure-lvm]


___

Platí pro využívání OM řad, které budou moct pracovat s úložištěm Premium Azure obvykle:

* Požadavky pro čekacích dob vstupu a výstupu, které se blíží předvádění SAN/NAS zařízení.
* Služba pro faktory lepší vstupu a výstupu latence než standardní úložišti Azure přináší.
* Vyšší procesorů za OM než co lze dosáhnout s více standardní VHD úložiště před určitý typ OM.

Protože základní úložiště Azure zreplikuje každý virtuální pevný disk na nejméně tři uzly úložiště jednoduché raid0 prokládání lze použít. Není potřeba k provedení RAID5 nebo RAID1.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Úložiště Microsoft Azure
Úložišti tabulek Microsoft Azure uloží základní OM (s operačním systémem) a VHD nebo objekty BLOB nejméně 3 samostatné úložiště uzlů. Při vytvoření účtu úložiště, bude s dotazem, jestli ochrany znázorněná na obrázku:

![GEO replikace povolené pro účet Azure úložiště][dbms-guide-figure-100]

Azure (místně nadbytečné) místní replikace úložiště poskytuje úrovně ochrany před ztrátou dat z důvodu selhání infrastruktury, které můžou dovolit několik zákazníci nasazení. Jak je znázorněno nad ní 4 různé možnosti s na pátý probíhá některou variantu některého z první tři. Hledání bližší jejich jsme můžete rozlišit:

* **Premium místně nadbytečné úložiště (LRS)**: úložišti Premium Azure poskytuje podpora výkonných maximum, minimum latence disku virtuálních počítačích spuštěný můžu/O-značnou úloh. Jsou 3 kopie dat v rámci stejné Azure datacentra Azure oblast. Kopie budou v různých poruchy a Upgrade domény (u koncepty najdete v článku, [to] [plánování: Příručka 3,2] kapitoly v [plánování Guide][planning-guide]). V případě otevřené data přejdete z provozu kvůli selhání uzlu úložiště nebo selhání disku se automaticky vytvoří nový otevřené.
* **Místně nadbytečné úložiště (LRS)**: V tomto případě jsou 3 kopie data v rámci stejné Azure datacentra Azure oblast. Kopie budou v různých poruchy a Upgrade domény (u koncepty najdete v článku, [to] [plánování: Příručka 3,2] kapitoly v [plánování Guide][planning-guide]). V případě otevřené data přejdete z provozu kvůli selhání uzlu úložiště nebo selhání disku se automaticky vytvoří nový otevřené. 
* **GEO nadbytečné úložiště (GRS)**: V tomto případě je asynchronní replikace, který bude kanálu další 3 kopie dat v jiné oblasti Azure, což je ve většině případů ve stejném zeměpisnou oblast (jako Evropa Severní a západní Evropa). Výsledkem bude 3 další repliky, takže nějaké jsou 6 kopie v součet. Změna tohoto doplňuje použití dat v oblasti replikovanou Azure geo čtení důvodů (přístup pro čtení Geo nadbytečné).
* **Zóny nadbytečné úložiště (ZRS)**: V tomto případě 3 kopie data zůstávají ve stejné oblasti Azure. Způsobem popsaným v tématu [to] [plánování: Příručka-3.1] kapitoly [Průvodce plánováním] [-příručka k plánování] Azure oblast může být celá řada datacentrech v blízkosti. V případě LRS by být replik průběhu různých datacentrech, díky kterým bude Azure oblastí.

Další informace najdete [tady][storage-redundancy].
 
> [AZURE.NOTE] Nasazení systému správy databáze se nedoporučuje použití Geo nadbytečné úložiště
>
> Azure Geo replikace úložiště je asynchronní. Replikace jednotlivé VHD připojených k jedné OM nejsou synchronizovány v kroku zámek. Proto není vhodné replikovat systému správy databáze soubory, které jsou průběhu různých VHD nebo nasadit proti softwarové RAID založeny na více VHD. Software systému správy databáze vyžaduje, aby trvalý úložištích se přesně synchronizuje v jiné logické a základní disků/VHD/vřetena. Software systému správy databáze používá různé mechanismy posloupnost vstupu a výstupu zápisu aktivity a systému správy databáze hlásí, že úložištích určené replikace je poškozený Pokud různá i za několik milisekund. Tedy pokud jedna skutečně požaduje konfigurace databáze s databází roztažen napříč několika VHD geo replikovat, tyto replikace je potřeba provést s znamená databáze a funkce. Jednu neměli závisí na Azure úložiště Geo replikace k provedení této úlohy. 
>
> Je to nejlepší popisují příklad systému. Předpokládejme, že máte systému SAP uloženy v Azure, který má 8 VHD obsahující datové soubory systému správy databáze plus jeden virtuální pevný disk obsahující souboru protokolu. Každý jednu z těchto 9 VHD bude mít dat, aby došlo k zápisu jejich v jednotný způsob podle předpisů rozhraní systému správy databáze, jestli data zapisuje protokoly dat nebo transakce.
>
> Kromě, aby se správně geo replikace data a udržovat obrázku konzistentní databáze obsahu všech devět VHD by mohl být geo replikovat v uvedeném pořadí operací byly spouštět oproti devět různých VHD. Azure úložiště geo replikace však neumožňuje deklarovat závislostí mezi VHD. To znamená, že geo replikace úložišti tabulek Microsoft Azure poslal, neví, o tom, že obsah v těchto devět různých VHD se vztahují k sobě a jestli změny dat konzistentní pouze v případě, že replikace v takovém pořadí, operací se stalo přes 9 VHD.
>
> Kromě šanci právě vysoké replikovat geo obrázky ve scénáři nenabízejí obrázku konzistentní databáze taky není snížení výkonu, který se zobrazuje s geo nadbytečné úložiště, který můžete značně mít vliv na výkon. V souhrnu nepoužívejte tento typ redundance úložiště pro typ úloh systému správy databáze.
 
#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Mapování VHD do účty úložiště služby Azure virtuálního počítače
Účet Azure úložiště je pouze pro správu konstrukce, ale také předmětem omezení. Vzhledem k tomu, omezení se liší v tom, jestli budeme Azure standardní úložiště účet nebo účet Azure Premium úložiště. Přesné funkce a omezení najdete [tady][storage-scalability-targets]
 
Aby standardní úložištěm Azure je důležité mít na paměti, je omezen na procesorů jednoho účtu úložiště (řádek obsahující "Celkové požádat o koeficient" v [článku][storage-scalability-targets]). Kromě toho je počáteční maximálně 100 úložiště účtů na Azure předplatného (od 2015 červenec). Proto doporučujeme zůstatek procesorů VMs mezi různými účty úložiště při použití Azure standardní úložiště. Zatímco jednoho OM v ideálním případě používá úložiště účtů pokud je to možné. Tak pokud budeme nasazení systému správy databáze kde může každý virtuálního pevného disku, který je hostovaný ve standardní úložišti Azure dosáhla limit kvóty, byste měli jenom nasadit 30-40 VHD za Azure úložiště účet, který používá Azure standardní úložiště. Na druhé straně pokud využití úložiště Azure Premium a chcete uložit databázi velkých objemů, nejspíš vás bude pořádku z hlediska procesorů. Ale účet Azure Premium úložiště způsobem omezenějším objemu dat než účet Azure standardní úložiště. V důsledku toho pouze nasazením omezený počet VHD v rámci účet Azure Premium úložiště před zasažení omezení objemu dat. Na konec myšlení účet Azure úložiště jako "Virtuální SAN", která obsahuje možnosti v procesorů a/nebo kapacity omezené. Úkol zůstane v důsledku toho jako v místním nasazení k definování rozložení VHD různých systémům SAP různých "imaginární SAN zařízení" nebo účty adresářové služby Azure úložiště.
 
Standardní úložištěm Azure nedoporučujeme prezentovat úložiště z různých úložiště účtů do jednoho OM Pokud je to možné.

Zatímco pomocí Pošta nebo GS řadu Azure VMs je možné připojení VHD z Azure standardní úložiště a Premium úložiště účtů. Případy použití psaní zálohy do standardní ukládání záložní VHD vzhledem systému správy databáze dat a protokoly úložný prostor Premium pocházet paměti, kde by mohly využít takové nesourodými úložiště. 

Na základě implementace zákazníků a testování asi 30 až 40 VHD obsahující datové soubory databáze a protokoly můžete zřízení v jednom Azure standardní úložiště účtu s přijatelného výkonu. Jak jsme zmínili dříve, bude pravděpodobně možné kapacita dat, které můžou obsahovat a ne procesorů omezení účet Azure Premium úložiště.

Jako s SAN zařízení místním sdílení vyžaduje některé sledování postupně nedetekuje problémových míst na účet Azure úložiště. Azure sledování rozšíření SAP a portálu Azure jsou nástroje, které lze použít ke zjištění zaneprázdněné účty adresářové úložiště služby Azure, může být předvádění nepřesnými ZVYŠUJÍ výkon.  Je-li této situaci zjištěn se doporučuje přesunout zaneprázdněné VMs na jiný účet Azure úložiště. Další informace o tom, jak aktivovat hostiteli SAP možnosti sledování získáte [Průvodce nasazením] [Průvodce nasazením].

Další článek vytváření souhrnů doporučené postupy kolem Azure standardní úložiště a standardní účty úložiště adresářové služby Azure najdete tady <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>
 
#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>Přesunutí nasazení systému správy databáze VMs z Azure standardní úložiště k úložišti Premium Azure
Jsme nastat úplně některé scénáře jako zákazník místo k přesunutí nasazeném OM z Azure standardní ukládání do úložiště Premium Azure. Toto není možné bez fyzicky přesouvat data. K dosažení cíle několika způsoby:

* Všechny VHD, základní virtuální pevný disk i VHD dat může jednoduše zkopírovat do nového účtu úložiště Premium Azure. Často jste se rozhodli počet VHD v úložišti standardní Azure není kvůli faktů potřeby hlasitost data. Ale potřebné tak velký počet VHD kvůli procesorů. Teď můžete přesunout k úložišti Premium Azure se může odejít způsob méně VHD dosáhnout některé procesorů výkon. Vzhledem k tomu, že v úložišti standardní Azure zaplatit používaná data a ne velikost nominal disku, počet VHD není důležité skutečnosti z hlediska nákladů. Však s úložištěm Premium Azure by platíte pro velikost nominal disku. Proto většina zákazníků pokusíte mějte počet Azure VHD Premium úložiště na čísle potřebné k dosažení výkon procesorů potřebné. Ano většina zákazníků rozhodněte, proti tak jednoduchou 1:1 kopírovat.
* Pokud ještě nejste připojili, připojíte jednoho virtuálního pevného disku, která může obsahovat zálohování databáze SAP databáze. Po vytvoření zálohy odpojit všechny VHD včetně virtuální pevný disk obsahující zálohu a zkopírujte základní virtuálního pevného disku a virtuální pevný disk s zálohování zohledňovala Azure Premium úložiště. By pak nasazení OM založené na základním virtuálního pevného disku a připojit virtuální pevný disk s zálohování. Nyní vytvořte další prázdné disků úložiště Premium pro OM, které slouží k obnovení databáze do. Tím se předpokládá, že systému správy databáze můžete změnit cest k souborům dat a protokolů jako součást procesu obnovit.
* Další možností je variant bývalého procesu, kde právě do úložiště Premium Azure zkopírujte zálohování virtuálního pevného disku a připojit proti OM, který nově nasazeném a nainstalovali.
* Čtvrtý možnost vyberte po kromě potřebují změnit počet datových souborů databáze. V takovém případě by provádět kopii SAP jednotné systému pomocí pro export a import. Umístění můžou být exportu souborů do virtuálního pevného disku, který zdroj zkopírována do účet Azure Premium úložiště a připojte ji k OM, které použijete ke spuštění procesu importu. Zákazníci využít tuto možnost, hlavně když chtějí snižte počet datových souborů.

### <a name="deployment-of-vms-for-sap-in-azure"></a>Nasazení VMs pro SAP v Azure
Microsoft Azure nabízí několik možností pro nasazení VMs a související disk. Tím je velmi důležité pochopit rozdíl od přípravy VMs se můžou lišit závislá na cestě nasazení. Obecně se podíváme do popsaných v následujících kapitol scénářích.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Nasazení OM z Azure Marketplace
Líbí se vám umožní společnosti Microsoft nebo 3 strany podle obrázek z webu Azure Marketplace nasazení vaší OM. Po zavedení OM v Azure sledujete stejné pokyny a nástroje pro instalaci softwaru SAP uvnitř vaší OM, jako je popsaný v místním prostředí. Pro instalaci softwaru SAP uvnitř OM Azure, SAP a Microsoft doporučujeme uložit a uložte instalační médium SAP Azure VHD ani vytvářet Azure OM práce jako "souborový server", která obsahuje všechny potřebné SAP instalačního média.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Nasazení OM s obrázkem konkrétní generalized na zákazníka
Z důvodu požadavky konkrétní opravy týkající vaší verzí operačního systému nebo systému správy databáze uvedl obrázky v Azure Marketplace nemusí vašim potřebám. Proto je potřeba vytvořit OM vlastní soukromé"OS/systému správy databáze OM obrázek, který může být nasazené několikrát později. Příprava "Soukromé" obrázek duplikace, musíte operační systém generalized na OM místní. Další informace o tom, jak generalize virtuálního počítače získáte [Průvodce nasazením] [Průvodce nasazením].

Pokud jste již nainstalovali SAP obsahu ve vaší místní OM (zvlášť pro systémy úroveň 2), můžete upravit nastavení systému SAP po nasazení OM Azure pomocí instance přejmenovat postup nepodporuje SAP Software zřizování správce (Poznámka SAP [1619720]). Jinak můžete instalace softwaru SAP později po nasazení OM Azure.
 
K databázi obsahu používané aplikací SAP obsahu mohou generovat čerstvě instalací SAP nebo obsahu můžete importovat do Azure pomocí virtuálního pevného disku s zálohy databáze systému správy databáze nebo využití možností systému správy databáze zálohování přímo do úložišti tabulek Microsoft Azure. V tomto případě může taky připravit VHD se systému správy databáze dat a protokolů soubory místní a pak můžou být jako disků naimportujte Azure. Ale přenosu systému správy databáze data, která se zobrazuje načte z místního na Azure vhodná přes virtuální pevný disk disků, které je potřeba ještě počítat místní.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>S diskem generalized přesouvání virtuálního počítače z místního Azure
Plánujete přesunete určitý systém SAP z místního Azure (výtah a shift). Tím se teď dá uložením virtuálního pevného disku, který obsahuje OS binární SAP a případné binární soubory systému správy databáze plus VHD se dat a protokolů soubory systému správy databáze Azure. V opačném scénář #2 výše si Poznámkový blok hostname, ID SAP zabezpečení a uživatelských účtů SAP OM Azure byly nakonfigurovaná v místním prostředí. Proto proces generalizace obrázek není nutné. Tomto případě použije převážně křížově místní scénářích kde součástí orientace na šířku SAP spuštěna místním prostředím a částí na Azure.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Dostupnost a obnovení s Azure VMs
Azure nabízí tyto funkce Vysoký dostupnosti (HA) a obnovení havárie (DR), které se vztahují na jiné součásti, které by používáme SAP a systému správy databáze nasazení

### <a name="vms-deployed-on-azure-nodes"></a>VMs nasazený uzlech Azure
Platformu Azure nenabízí funkce, jako jsou Live migrace pro nasazeném VMs. To znamená, že pokud na serveru obrázku, na kterém je nasazený virtuálního počítače je nezbytné údržby, je potřeba OM získat zastavení a opětovném spuštění. Udržování v Azure se provádí pomocí tak s názvem Upgrade domén v rámci clusterů serverů. Pouze jeden Upgrade domény v čase trvá. Během takové restartování bude přerušení poskytování služeb a vypnout OM, provést údržbu OM restartovat. Většina dodavatelé systému správy databáze však poskytují dostupnost a obnovení funkce, které rychle služby systému správy databáze na jiném uzlu Pokud restartuje primární uzel není k dispozici. Platformu Azure nabízí funkce vysílat VMs, úložiště a další služby Azure mezi Upgrade doménami zajistit, že plánovaná údržba nebo infrastruktury selhání by pouze mít vliv na malou VMs nebo služeb.  Při plánování pozor, je možné k dosažení dostupnost úrovně srovnatelná s místním infrastruktury.

Microsoft Azure dostupnost sady jsou logické seskupení VMs nebo služby, které zajistí VMs a dalších služeb se úměrně různých poruch a Upgrade domén v rámci clusteru tak, že by existovat pouze jeden uzel vypnutí v kterémkoli bodě v čase (Číst [Toto] [ virtual-machines-manage-availability] článku Další podrobnosti).

Je potřeba nakonfigurovat tak, že účel při zavádění VMs jak je vidět tady:

![Definice nastavte dostupnost pro konfigurace HA systému správy databáze][dbms-guide-figure-200]

Pokud chcete vytvořit vysoce dostupné konfigurace nasazení systému správy databáze (nezávisle na jednotlivé HA systému správy databáze funkci používat), by potřeba VMs systému správy databáze:

* Přidání VMs do stejné síti Azure virtuální (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* VMs konfigurace HA by měly být také ve stejném podsítě. Překlad mezi různých podsítí není možné v cloudu – jen nasazeních, nebudou funkční pouze překlad IP. Použití webu webu nebo ExpressRoute připojení mezi místním nasazení se v síti s alespoň jeden podsítě už zřídí. Překlad provede podle předpisů rozhraní místní AD zásady a síťové infrastruktury. 
[Komentář]: <>  (Test úkol MSSedusch true pořád v ARM)

#### <a name="ip-addresses"></a>IP adresy
Důrazně doporučujeme nastavit VMs HA konfigurace pružné způsobem. Může na adresy IP adrese partnerů HA v konfiguraci HA není spolehlivé v Azure, pokud se používají statické IP adres. Existují dvě koncepce "Vypnutí" v Azure:

* Vypnutí prostřednictvím portálu Azure nebo Azure PowerShell rutina zastavit AzureRmVM: V tomto případě virtuální počítač získá vypnutí a zrušte přidělit. Váš účet Azure už strhne příslušná tento OM tak, aby byly jediné poplatky, které je možné za pro použité úložiště. Pokud soukromé IP adresu rozhraní sítě statické, vydání IP adresa a není zaručit, že rozhraní sítě získá původní IP adresa přiřazené znovu po restartování OM. Nenahrávají velké množství vypnout dolů portálu Azure tak, že zavoláte zastavit AzureRmVM automaticky způsobí rušení rozdělení. Pokud nechcete deallocat počítači pomocí zastavit AzureRmVM - StayProvisioned 
* Vypnutí OM z úrovně OS OM získá vypnout a není zrušte přiřazený. Však v tomto případě účet Azure pořád strhne příslušná OM, přestože je vypnout. V takovém případě přiřazení IP adresu přestal OM zůstane nedotčený. Vypnutí OM z v rámci nebude vynutit automaticky rušení rozdělení.

I pro místní křížově scénáře ve výchozím nastavení vypnutí a rušení přidělení bude znamenat, že rušení přiřazení IP adres z OM, i v případě místních zásad v dialogovém okně Nastavení DHCP se liší. 

* Výjimky je-li jeden přiřadí statickou IP adresu síťové jako popsaných [v tomto poli][virtual-networks-reserved-private-ip].
* V takovém případě zůstane IP adresu pevné, dokud neodstraníte rozhraní sítě.

> [AZURE.IMPORTANT] Pokud chcete zachovat celé nasazení jednoduchý a spravovatelné, vymazat doporučujeme nastavit VMs partnerství systému správy databáze HA nebo web DR konfiguraci v rámci Azure tak, že je funkční překlad mezi různých VMs souvisejících.
 
## <a name="deployment-of-host-monitoring"></a>Nasazení hostitele sledování
Produktivní využití aplikací SAP v Azure virtuálních počítačích a SAP vyžaduje možnost hostitel monitorování dat z fyzické hosts systém virtuálních počítačích Azure. Určité úrovni oprava SAP HostAgent, budete vyzváni, který umožňuje tuto možnost v SAPOSCOL a SAP HostAgent. Přesné oprav jsou popsány v SAP poznámku [1409604].

Další informace o nasazení součástí splnit host data s SAPOSCOL a SAPHostAgent a správa životního cyklu součástí těchto naleznete [Průvodce nasazením] [Průvodce nasazením]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Zvláštnosti Microsoft SQL Server

### <a name="sql-server-iaas"></a>SQL Server IaaS
Začnete s Microsoft Azure, můžete snadno migrovat existující SQL Server aplikace integrované platformy Windows Server na virtuálních počítačích Azure. SQL Server ve virtuálních počítače umožňuje snadno migraci těchto aplikací na Microsoft Azure snížit celkové náklady na vlastnictví nasazení, Správa a údržba podnikových aplikací šířka. Se serverem SQL Server ve Azure virtuálního počítače správci a vývojáři můžete dál používat stejný vývoj a nástroje pro správu, které jsou k dispozici místního. 

> [AZURE.IMPORTANT] Upozorňujeme, že jsme neprojednáváte databázi Microsoft Azure SQL, který je platformu jako nabídku služby Azure platformy Microsoft. Diskuse v tomto dokumentu se zabývá systémem SQL serveru, jako je známo, místní nasazení ve virtuálních počítačích Azure, využívání infrastruktury jako možností služby Azure. Funkce databáze funkcí mezi těmito dvěma nabídky se liší a neměli smíšený mezi sebou. Viz taky: <https://azure.microsoft.com/services/sql-database/>
 
Důrazně doporučujeme ke kontrole [tohoto] [ virtual-machines-sql-server-infrastructure-services] si přečtěte následující dokumentaci před pokračováním.

V následujících částech použitelné části dokumentace podle výše uvedeného odkazu agregované a uvedené. Zvláštnosti kolem SAP zmiňují i a některé koncepty se podrobněji píše. Ale doporučujeme s přípravou dokumentace nad první před serveru SQL Server konkrétní si přečtěte následující dokumentaci pro čtení.

Je v IaaS konkrétních informací, které byste měli vědět před pokračováním některé SQL serveru:

* **Virtuální počítač SLA**: existuje SLA pro virtuálních počítačích spuštěné v Azure, které tady najdete: <https://azure.microsoft.com/support/legal/sla/>  
* **Podpora verze SQL**: SAP zákazníci podporujeme SQL Server 2008 R2 nebo novější na Microsoft Azure virtuálního počítače. Starší verze nejsou podporované. Přečtěte tato Obecné [Podpory údajů](https://support.microsoft.com/kb/956893) další podrobnosti. Dejte pozor, aby obecně SQL Server 2008 je podporované společností Microsoft taky. Ale kvůli významné funkce SAP, která byla zavedená SQL Server 2008 R2, SQL Server 2008 R2 je minimální verze pro SAP. Mějte na paměti, SQL Server 2012 a 2014 máte rozšířeného integrací hlouběji do scénář IaaS (třeba zálohování přímo proti úložišti Azure). Proto jsme omezit tento dokument SQL Server 2012 a 2014 s jeho nejnovější úroveň oprav pro Azure.
* **Podpora funkcí SQL**: nejčastěji SQL Server jsou však podporovány ve virtuálních počítačích Microsoft Azure s určitými výjimkami. **SQL Server převzetí clusterů pomocí sdílené disků nepodporuje**.  Distribuované technologie jako odrážející strukturu databáze, skupiny dostupnosti AlwaysOn, replikace, protokolu a zprostředkovatel služeb jsou podporované v rámci jedné oblasti Azure. SQL Server AlwaysOn je podporuje i mezi různými oblastmi Azure jak je uvedeno tady: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Přečtěte si [Prohlášení podpory](https://support.microsoft.com/kb/956893) další podrobnosti. V [tomto] je znázorněn příklad o nasazení AlwaysOn konfigurace[ virtual-machines-workload-template-sql-alwayson] článek. Také podívejte se osvědčené postupy dokumentovaného [tady][virtual-machines-sql-server-infrastructure-services] 
* **Výkon SQL**: můžeme si jistí, že se může lišit ve srovnání s jiné veřejné pro virtualizaci Cloud, ale jednotlivé výsledky provede dobře hostovanou virtuálních počítačích Microsoft Azure. Podívejte se na [Tento] [ virtual-machines-sql-server-performance-best-practices] článek.
* **Použití snímků z Azure Marketplace**: nasazení nové OM Microsoft Azure nejrychleji použít obrázek z webu Azure Marketplace. Existuje obrázky v Azure Marketplace, které obsahují SQL serveru. Obrázky, které je už nainstalovaný SQL Server nelze použít ihned SAP NetWeaver aplikací. Důvod, proč je že výchozí SQL Server řazení se instaluje v rámci těmito obrázky a ne řazení vyžadované SAP NetWeaver systémy. Aby bylo možné použít tyto obrázky, zkontrolujte, zda kroků popsaných v kapitola [pomocí serveru SQL Server obrázky z webu Microsoft Azure Marketplace] [systému správy databáze 5.6 Průvodce]. 
* Další informace, podívejte se na [Ceny podrobnosti](https://azure.microsoft.com/pricing/) . [SQL Server 2012 licencování Průvodce](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) a [SQL Server 2014 licencování Guide](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) jsou také důležité zdroje.
 
### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>Pokyny ke konfiguraci serveru SQL Server pro SAP související s instalací SQL serveru ve Azure VMs

#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Doporučení ohledně toho OM/virtuální pevný disk strukturu pro SAP související nasazení serveru SQL
V souladu s obecný popis spustitelných SQL Server nachází nebo nainstalovaný na systémovou jednotku základní virtuální pevný disk OM (jednotku C:\).  Obvykle většina databáze systému SQL Server nejsou využít na vysoké úrovni tak, že pracovní zátěž SAP NetWeaver. Proto databáze systému SQL Server (předlohy msdb a model) zůstat na jednotce C:\. Výjimky může být tempdb, který v případě některé ERP SAP a všechny úlohami pásma, může vyžadovat větší rozsah dat nebo operace objem vstupu a výstupu, který nelze zapadají do celkového konceptu původní OM. Pro tyto systémy by měl provést následující kroky:

* Přesunete soubory primární tempdb dat na stejné logické jednotku jako primární datové soubory databáze SAP.
* Pro každý další logické jednotky obsahující datový soubor databáze uživatelských SAP přidáte všechny další tempdb datové soubory.
* Přidání souboru protokolu tempdb logické jednotku, která obsahuje soubor protokolu databázi uživatelů.
* **Výhradně typy OM, který použijete místní disky SSD** výpočetním uzel tempdb dat a protokolů soubory, může proběhnout na disku D:\. Však může být vhodné používat více tempdb datové soubory. Mějte na paměti, že svazky jednotek D:\ se liší v závislosti na typu OM.
 
Tyto konfigurace povolit tempdb využívat více místa než systémovou jednotku je mohli dát. K určení tempdb správné velikosti, jednu můžete zkontrolovat velikost tempdb na existující systémy, které pracují místní. Kromě toho taková konfigurace umožní procesorů čísla proti tempdb, který nelze součástí systémovou jednotku. Znovu počítačích se systémem místní slouží ke sledování pracovní zátěž vstupu a výstupu proti tempdb tak, aby odvodit procesorů čísla, který očekáváte viděli na vaší tempdb.

Konfigurace OM kterém běží server SQL Server s databází SAP a umístění tempdb dat a soubor protokolu tempdb na disku D:\ vypadat:
 
![Přehled konfigurace Azure OM IaaS pro SAP][dbms-guide-figure-300]

Mějte na paměti, že má jednotka D:\ různě závisí na typu OM. Závisí na velikosti požadavku tempdb se může být vynucená pár tempdb dat a souborů protokolu s SAP dat a protokolů soubory databáze v případech, kdy je malý D:\ jednotky.

#### <a name="formatting-the-vhds"></a>Formátování VHD
Pro systém SQL Server NTFS blokovat velikost pro VHD obsahující data SQL serveru a souborů protokolu by měl být 64 kB. Není potřeba k formátování D:\ jednotky. Tato jednotka je předformátované.

Před uskutečněním jistotu, že vytvoření databáze nebo obnovení není inicializace datové soubory tak, že který pak zmizí obsah souborů, jednu byste zkontrolovat, že kontext uživatele služby SQL Server běží v některých oprávnění. Obvykle uživatele do skupiny správců Windows mít tato oprávnění. Pokud je v kontextu uživatele Windows uživatel není správcem služby SQL Server, budete muset danému uživateli přiřadit právo "Hlasitost údržbu úkoly".  Naleznete v tomto článku znalostní báze Microsoft Knowledge Base: <https://support.microsoft.com/kb/2574695>
 
#### <a name="impact-of-database-compression"></a>Dopad komprese databáze
V konfigurací vstupu a výstupu šířky pásma, kde může být omezující faktor každé míra, která omezuje procesorů vám mohou pomoci roztáhněte pracovní zátěž, které jednu mohlo by umožnit spuštění ve scénáři IaaS jako Azure. Proto pokud ještě nejste v době, použití SQL Server PAGE komprese důrazně doporučujeme SAP a Microsoft před nahráváním existující SAP databáze Azure.
 
Doporučení k provedení databáze komprese před odesláním Azure je uveden ze dvou důvodů:

* Částka data, která chcete nahrát, je menší.
* Doba trvání spuštění komprese je kratší za předpokladu, že jeden pomocí dialogového okna silnější hardware s více procesorů nebo větší šířku pásma vstupu a výstupu nebo méně vstupu a výstupu latence místní.
* Menší velikosti databáze může vést k menší nákladů na disku rozdělení

Komprese databáze funguje taky virtuálních počítačích Azure stejně jako místní. Další informace o tom, jak komprimovat existující SAP Server SQL databáze zkontrolujte tady: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>
  
### <a name="sql-server-2014--storing-database-files-directly-on-azure-blog-storage"></a>SQL Server 2014 – ukládání souborů přímo v Azure blogu úložiště databáze
SQL Server 2014 otevře možnost ukládat soubory databáze přímo v úložišti objektů Blob Azure bez "Obálka" virtuálního pevného disku okolo nich. Zejména v případě použití standardní úložišti Azure nebo menší typy OM díky scénáře, kde můžete vyřešit limity procesorů, která by nevynucují tak, že omezený počet VHD, které lze připojit k některé menší OM. To funguje pro uživatele databáze ale ne pro databáze systému SQL serveru. Funguje taky pro data a souborů protokolu SQL serveru. Pokud byste chtěli nasazení databáze SQL serveru SAP tímto způsobem místo "obtékání" do VHD, přejděte prosím vezměte v úvahu následující myslet:

* Účet úložiště používá potřeb ve stejné oblasti Azure jako té, která se používá k nasazení serveru SQL Server OM běží.
* Důležité informace o uvedených výše v vzhledem k distribuci VHD nad jiné účty adresářové služby Azure úložiště použít pro tuto metodu i nasazení. Znamená, že vstupu a výstupu operace se započítávají limity úložiště účet Azure.
[Komentář]: <>  (MSSedusch úkol, ale bude použit šířka pásma sítě a ne úložiště šířka pásma, není to?)

Podrobnosti o tomto typu nasazení, najdete tady: <https://msdn.microsoft.com/library/dn385720.aspx>
 
Aby bylo možné uložit SQL Server datových souborů přímo v úložišti Premium Azure, musíte mít minimální verze oprava 2014 serveru SQL, které jsou zde uvedeny: <https://support.microsoft.com/kb/3063054>. Ukládání souborů dat systému SQL Server Azure standardní úložný prostor funguje s vydanou verzi SQL Server 2014. Velmi stejné opravy však obsahovat další řada problémů, které spolehlivost přímé využití úložiště objektů Blob Azure SQL Server datové soubory a zálohování. Proto doporučujeme používat tyto opravy obecně.

### <a name="sql-server-2014-buffer-pool-extension"></a>SQL Server 2014 vyrovnávací paměť fondu rozšíření
SQL Server 2014 zavádí nové funkce, která je označená jako vyrovnávací paměť fondu rozšíření. Tato funkce slouží k rozšíření fondu vyrovnávací paměť SQL serveru, který bude k dispozici v paměti s druhé úrovně mezipaměti, které je podporovaným místní disky SSD serveru nebo OM. Díky mějte větší pracovní množiny dat "paměti". Ve srovnání s přístup k úložišti standardní Azure přístup do rozšíření fondu vyrovnávací paměť, který je uložený na místním disky SSD Azure OM je spousta faktorů rychlejší.  Využívání místní disk D:\ typů OM, které mají pracovníků procesorů a výkon proto může být velmi rozumné způsob, jak omezit načíst procesorů proti Azure úložiště a výrazně zvýšit doby odezvy dotaz. Toto nastavení se projeví hlavně když nepoužíváte Premium úložiště. V případě Premium úložiště a použití te000126961 mezipaměť Premium Azure pro čtení na uzel výpočetním doporučené pro datové soubory, žádné velké rozdíly očekává. Důvodem je, obě mezipaměti (SQL Server vyrovnávací paměť fondu linky a Premium úložiště pro čtení mezipaměti) používáte místní disk výpočetním uzlů.
Podrobné informace o této funkci, zkontrolujte toto si přečtěte následující dokumentaci: <https://msdn.microsoft.com/library/dn133176.aspx> 

### <a name="backuprecovery-considerations-for-sql-server"></a>Zálohování a obnovení aspektech pro systém SQL Server
Při nasazení serveru SQL Server do Azure musí být zkontrolována metodologie vašeho zálohování. I když systému produktivní systém, databázi SAP hostované pomocí serveru SQL Server je nutné zálohovat pravidelně. Od úložišti Azure pořád tři obrázky, je teď méně důležité z hlediska kompenzace chybu úložiště zálohy. Priority (priorita) důvod, proč správy správnou zálohování a obnovení plánu je informace, které můžete vyrovnat chyb logické/ruční zadáním bodu v možnosti v době obnovení. Aby cílem je buď použití zálohování obnovení databáze zpátky na určitém bodě v čase nebo použít zálohy v Azure pro jiný systém zkopírováním stávající databázi. Například může přenesete z konfigurace SAP 2 osy nastavení osy 3 systému stejného systému obnovením zálohy.

K zálohování SQL serveru k základnímu úložišti Azure třemi různými způsoby:
 
1. SQL Server 2012 CU4 a vyšší nativně záložní databáze se dají na adresu URL. Toto je uvedené v blogu [nové funkce v SQL Server 2014 – část 5 – zálohování a obnovení vylepšení](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Viz kapitola [SQL Server 2012 SP1 CU4 a v novějších verzích] [systému správy databáze 5.5.1 Průvodce].
1. Uvolnění systému SQL Server před SQL 2012 CU4 pomocí funkce přesměrování zálohování virtuálního pevného disku a v podstatě přechod proudu zápisu k úložiště Azure, nakonfigurovaný. Viz kapitola [SQL Server 2012 SP1 CU3 a starších verzích] [systému správy databáze 5.5.2 Průvodce].
1. Metoda konečný je zálohování běžných SQL serveru k příkazu disk do diskové zařízení virtuální pevný disk.  To je stejný vzorek v místním nasazení a není popisované podrobně v tomto dokumentu.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 a v novějších verzích
Tato funkce umožňuje přímo zálohování k úložišti objektů BLOB Azure. Bez tato metoda musí zálohovat do jiných VHD Azure, které byste měli používat virtuálního pevného disku a procesorů kapacity. Cílem je v podstatě takto:
 
 ![Použití zálohování SQL Server 2012 k úložišti objektů BLOB Microsoft Azure][dbms-guide-figure-400]

Tu výhodu, v tomto případě, že jeden nevyžaduje věnovat VHD ukládání SQL Server na. To je potřeba méně VHD přidělit a celé šířku pásma virtuální pevný disk procesorů se dá použít pro data a souborů protokolu. Dejte pozor, aby maximální velikosti doručovaných zálohy se omezí na do velikosti 1 TB, jak je uvedeno v části "Omezení" v tomto článku: <https://msdn.microsoft.com/library/dn435916.aspx#limitations>. Pokud záložní velikost, bez ohledu na pomocí komprese SQL Server zálohování překročí 1 TB v jeho velikost, funkce popsané v kapitola [SQL Server 2012 SP1 CU3 a starších verzích] [systému správy databáze příručka-5.5.2] v tomto dokumentu, musí se nemusí používat.

Doporučujeme, abyste [si přečtěte následující dokumentaci související](https://msdn.microsoft.com/library/dn449492.aspx) s popisem obnovení databáze ze zálohy proti úložiště objektů Blob Azure není možné obnovit přímo z úložiště objektů BLOB Azure, pokud je zálohy > 25 GB. Doporučení v tomto článku je jednoduše na základě důležité informace o výkonu a ne z důvodu omezení funkční. Proto různých podmínek uplatnit na základě případ.

Si přečtěte následující dokumentaci toho, jak nastavit a využít tento typ zálohování najdete v [tomto](https://msdn.microsoft.com/library/dn466438.aspx) kurzu
 
Příklad posloupnost kroků můžete přečíst [v tomto poli](https://msdn.microsoft.com/library/dn435916.aspx).

Automatizace zálohy, je nejvyšší důležité, abyste měli jistotu, že jsou objekty BLOB pro každou zálohu jiný název. V opačném případě bude přepsání a obnovení řetězec se přeruší.
 
Aby se kombinovat nahoru věci mezi 3 různými typy zálohy je vhodné vytvořit různé kontejnery pod úložiště účet použili záloh. Kontejnery může být OM pouze nebo podle typu OM a zálohování. Schéma může vypadat:
 
 ![Použití SQL Server 2012 zálohování Microsoft Azure úložiště objektů BLOB – různých kontejnerů účtem samostatné úložiště][dbms-guide-figure-500]

Ve výše uvedeném příkladu by zálohy provést do jednoho účtu úložiště kde VMs používají. Bude nový účet úložiště konkrétně záloh. V rámci účtů úložiště bude různých kontejnerů vytvořili s maticí typu zálohování a název OM. Tyto segmentace budou snadněji spravovat zálohy různých VMs.

Objekty BLOB jednu přímo zapíše zálohy, Komu nejsou přidání počet VHD virtuálního počítače. Proto jednu může maximalizovat maximálně VHD připojit z konkrétní SKU OM pro data a transakce souboru protokolu a pořád spustit zálohy proti kontejneru úložiště. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 a starší verze
Cílem prvního kroku je nutné provést za účelem dosažení zálohy přímo proti Azure úložiště bude stáhnout Instalační službu msi, který je propojený na [Tento](https://www.microsoft.com/download/details.aspx?id=40740) článek KBA.
 
Stažení x64 instalační soubor a dokumentaci. Soubor bude nainstalujte: "Microsoft SQL Server zálohování Microsoft Azure Tool". Důkladně naleznete v dokumentaci produktu.  Nástroj pracuje v podstatě následujícím způsobem:

* Na straně serveru SQL Server je definován umístění na disku pro zálohování serveru SQL Server (nepoužívejte jednotku D:\ pro tuto).
* Nástroj vám umožní definujte pravidla, které mohou sloužit k přímé různé typy zálohování do různých kontejnerů Azure úložiště.
* Jakmile pravidla jsou na místě, nástroj vás přesměruje proudu zápisu zálohy na jeden z VHD/disků do úložiště Azure, která byla dříve definované.
* Nástroj ponechá soubor malé několik velikosti KB na virtuální pevný disk či disketa, která byla definovaná pro systém SQL Server zálohování. **Tento soubor má zůstat v úložišti vzhledem k tomu, že je potřeba k obnovení znovu z Azure úložiště.**
    * Pokud jste ztratili zástupný soubor (například přes ztráty médium, který obsahoval základní soubor) a jste zvolili možnost zálohování k úložišti tabulek Microsoft Azure účtu, může obnovit zástupný soubor prostřednictvím úložišti tabulek Microsoft Azure tak, že si ji stáhnete z úložiště kontejneru, ve kterém je umístěn. Základní soubor by měl umístěte do jiné složky v místním počítači kde nástroj nakonfigurovaný tak, aby zjišťování a nahrajte do stejného kontejneru s stejné heslo šifrování Pokud šifrování používal původní pravidlo. 

To znamená, že schéma ve výše uvedeném novější verzí systému SQL Server mohou být umístěny na místě i v případě verze serveru SQL Server, které nejsou umožňující přímý adresu úložiště Azure.
 
Tento způsob není určená k použití s novější verzí systému SQL Server, které podporují zálohování nahoru nativně proti Azure úložiště. Výjimky jsou, kde omezení nativního backup do Azure blokují nativního backup spuštění do Azure.

#### <a name="other-possibilities-to-backup-sql-server-databases"></a>Další možnosti pro zálohování databáze systému SQL Server
Další možnosti pro zálohování databáze je připojení dalších VHD OM, které používáte pro ukládání záloh na. V takovém případě musíte Ujistěte se, že VHD se nepoužívá úplné. Pokud to je případ, by potřebujete odpojte virtuálního pevného disku a tak na speak '"ho archivovat a nahradit je novou prázdnou virtuální pevný disk. Přejděte dolů dané cestě, chcete-li zabránit v těchto VHD v samostatném účty adresářové služby Azure úložiště těch, které VHD se soubory databáze.

Druhá možnost, je použít velké OM, která může obsahovat více VHD připojené. Například D14 s 32VHDs. Pomocí úložiště mezery vytvářet flexibilní prostředí, kde může vytvořit sdílené složky, které pak slouží jako záložní cíle u jiných serverů systému správy databáze.
 
Máte popsány některé osvědčené postupy [tady](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) taky. 

#### <a name="performance-considerations-for-backupsrestores"></a>Informace o výkonu při zálohování a obnovení
Jako čistý počítač nasazení jsou závislé na kolik svazky může číst paralelně a co může být výkon svazky zálohování a obnovení výkonu. Kromě toho může procesoru spotřeba tak, že zálohování komprese přehrát významné role v VMs s jenom až 8 podprocesů procesoru. Proto může mít jednu:

* Méně počet VHD využít k ukládání dat soubory, menší celkový výkon v režimu čtení.
* Čím menší že počet procesoru podprocesy v OM přísnější dopad komprese zálohování.
* Méně cíle (objektů BLOB nebo VHD) k zápisu zálohování, nižší výkon.
* Menší OM velikost, čím menší výkon kvóty úložiště psaní a čtením Azure úložiště. Nezávisle na tom, jestli zálohy přímo v ukládají objektů Blob Azure nebo zda jsou uloženy v VHD znovu uložených v objektů BLOB Azure.

Pokud chcete použít jako cíl zálohy v novějších verzích objektů BLOB Microsoft Azure úložiště, jste s omezeným přístupem k určení jedinou cílovou adresu URL pro každý konkrétní zálohu.
 
Ale pokud chcete použít "Microsoft SQL Server zálohování Microsoft Azure nástroji" ve starších verzích, můžete definovat více než jeden soubor cíl. S více než jeden cíl zálohování můžete změnit velikost a výkon zálohování je vyšší. Výsledkem by pak více souborů i v úložišti Azure účtu. V našem testování pomocí více míst soubor jasné jednu dosáhnete výkon, které jednu dosáhnout s záložní přípony implementovaná v z SQL Server 2012 SP1 CU4 na. Můžete taky se nebudou blokovat tak, že limit 1 TB jako nativního backup do Azure.

Ale, mějte na paměti, výkon také závisí na umístění účtu úložiště Azure použitý pro zálohování. Přehled může být vyhledejte úložiště účtu v jiné oblasti, než VMs běží v. Například Chcete spustit konfigurace OM v Evropě západní, ale umístění úložiště účet, který použijete k obecnějším nahoru proti v Evropě Severní. Určitě bude mít vliv na výkon zálohování a není vzniká výkon 150MB/sec, jak to vypadá možné v případech, kdy cílového úložiště a VMs běží ve stejném regionálního datacentra.

#### <a name="managing-backup-blobs"></a>Správa záložní objekty BLOB
Je povinné pro správu zálohování na vlastní. Protože očekává se, že počet objektů BLOB vytvoří se spuštěním zálohy protokolu časté transakce správy tyto objekty BLOB snadno můžete přetížit portálu Azure. Je tedy recommendable pomocí Průzkumníka úložišť Azure. Existuje několik dobré z nich dostupné které pomáhají spravovat účet Azure úložiště

* Microsoft Visual Studio s Azure SDK nainstalovaný (<https://azure.microsoft.com/downloads/>)
* Microsoft Azure úložiště Explorer (<https://azure.microsoft.com/downloads/>)
* 3 nástroje stran

[Komentář]: <>  (Není podporován v cloudu)
[Komentář]: <>  (### Azure OM zálohování)
[Komentář]: <>  (VMs sady SAP zálohovat lze pomocí funkce Azure virtuálního počítače zálohování. Azure virtuálního počítače záložní máte zavádí začátku roku 2015 a mezitím je standardní metoda zálohování dokončení OM v Azure. Azure zálohování ukládá zálohy Azure a umožňuje obnovení virtuálního počítače znovu.) 
[Poznámka]: <> (VMs, které spuštění databáze lze zálohovat způsobem konzistentní i v případě systems systému správy databáze podporuje VSS systému Windows (hlasitost stín služby kopie - <https://msdn.microsoft.com/library/windows/desktop/bb968832.aspx>) stejně jako například SQL serveru. Aby zálohování Azure OM může být způsob, jak získat restorable záložní kopii databáze SAP. Ale mějte na paměti, založené na Azure OM zálohy, kterou v okamžiku obnoví databází není možné. Proto doporučujeme zálohování databáze s funkcemi systému správy databáze namísto Azure OM zálohování.) [Poznámka]: <> (kde se seznámíte s Azure virtuálního počítače zálohování začněte tady <https://azure.microsoft.com/documentation/services/backup/>)

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Použití systému SQL Server obrázků z webu Microsoft Azure Marketplace
Společnost Microsoft nabízí VMs v Azure Marketplace, které již obsahují verzí systému SQL Server. SAP zákazníkům, kteří vyžadují licence pro SQL Server a Windows může to být příležitosti v podstatě pokrýval nutnosti licencí podle otáčející nahoru VMs se serverem SQL Server už nainstalovaný. Aby bylo možné použít tyto obrázky pro SAP, následující skutečnosti potřebovat provést:

* Zkušební verze systému SQL Server získat vyšší náklady než jenom "Pouze pro systém Windows" OM nasazeny z Azure Marketplace. Zkontrolujte najdete v těchto článcích a umožňuje porovnávat ceny: <https://azure.microsoft.com/pricing/details/virtual-machines/> a <https://azure.microsoft.com/pricing/details/virtual-machines/#Sql>. 
* Můžete použít jenom verze SQL serveru, které jsou podporovány SAP, třeba SQL Server 2012.
* Řazení instanci systému SQL Server, ve které je nainstalován VMs nabízená v Azure Marketplace není řazení SAP NetWeaver vyžaduje instanci systému SQL Server spustit. Když se podle pokynů v následující části můžete změnit řazení.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Změna řazení serveru SQL serveru Microsoft Windows/SQL OM
Protože na serveru SQL Server některých obrázcích v Azure Marketplace nejsou nastavit tak, aby pomocí řazení, který je potřeba SAP NetWeaver aplikací, ho potřebujete-li změnit ihned po nasazení. Pro SQL Server 2012 to lze provést pomocí následujícího postupu hned, jak byly nasazeny OM a správce je možné k přihlášení do publikovaném OM:

* Otevřete okno příkazového řádku Windows "jako správce".
* Přejděte v adresáři C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* Provedení příkazu: Setup.exe/quiet nezobrazí /ACTION = REBUILDDATABASE InstanceName = MSSQLSERVER /SQLSYSADMINACCOUNTS =`<local_admin_account_name`> /SQLCOLLATION = SQL_Latin1_General_Cp850_BIN2   
    * `<local_admin_account_name`> účet, která byla definována jako účet správce při instalaci OM poprvé v galerii.

Má jenom trvat několik minut. Abyste mohli zkontrolujte, jestli v kroku ukončí s správné výsledky, proveďte následující kroky:

* Otevřete SQL Server Management Studio.
* Otevřete okno s dotazem.
* Provedení příkazu sp_helpsort v hlavní databázi SQL serveru.

Očekávaný výsledek by měl vypadat:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Pokud není výsledkem, zastavit nasazení SAP a zjistěte, proč příkazu setup nefunguje očekávaným způsobem. Nasazení aplikace SAP NetWeaver do instance serveru SQL Server pomocí různých neurčují SQL Server než tu uvedené výše **není** podporována.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server vysoká dostupnost pro SAP v Azure
Jak jsme zmínili dříve v tomto dokumentu, není možné vytvořit sdílené úložiště, která je potřebná pro použití dostupnost funkcí nejstarší SQL serveru. Tato funkce by nainstalovat nejméně dvě instance serveru SQL Server ve Windows Server překlopení obrázku (WSFC) používat sdílený disk uživatelských databází (a nakonec tempdb). Toto je standardní dostupnost metoda dlouhou dobu, kterou také nepodporuje SAP. Protože Azure, které nejsou podporovány sdílené úložiště, nemůžete realizují dostupnost konfigurace systému SQL Server s konfigurací clusteru sdílené disku. Jiné metody dostupnost se však možné, že a popsaných v následujících částech.

[Komentář]: <>  (Článek odkazuje stále na ASM)
[Komentář]: <>  (Před čtení použitelné pro systém SQL Server v Azure, jiné konkrétní dostupnost technologie je vynikajícím dokument, který poskytuje další podrobnosti a ukazatele [here][virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions])

#### <a name="sql-server-log-shipping"></a>Námořní protokolu SQL serveru
Jednou z metody vysoké dostupnosti (HA) je dodací protokolu SQL serveru. Pokud VMs účast v konfiguraci HA práce překlad, došlo k potížím žádné a nastavení v Azure nebude lišit od nastavení, která probíhá místní. Nedoporučujeme spolehnout pouze překlad IP. Nastavení protokolu dodací a zásady kolem protokolu týkající zkontrolujte toto si přečtěte následující dokumentaci:

<https://technet.microsoft.com/library/ms187103.aspx>
 
Dosáhnout tak skutečně vysokou dostupnost, třeba nasazení VMs, které jsou v rámci taková protokolu konfigurace být v rámci stejné Azure dostupnost sadu.

#### <a name="database-mirroring"></a>Odrážející strukturu databáze
Zrcadlení databáze jako podporovaný SAP (viz poznámka SAP [965908]) vychází definování partnerem převzetí v připojovacím řetězci SAP. Pro názvy případů křížově místních poštovních předpokladu, že, dvě VMs jsou tu samou doménu, uživatelské jméno serveru SQL Server jsou spuštěné dvě instance v části i uživatelé domény a dostatečná oprávnění ve dvou souvisejících instance serveru SQL Server. Nastavení databáze odrážející strukturu v Azure proto neliší mezi typické místního nastavení a konfiguraci.

K jen cloudu nasazení najdete nejjednodušší způsob je jiné nastavení domény v Azure mít tyto VMs systému správy databáze (i v ideálním případě vyhrazené VMs SAP) v rámci jedné domény.

Pokud doménu není možné, jednu slouží také certifikáty pro databázi odrážející strukturu koncové body podle níže uvedeného popisu: <https://technet.microsoft.com/library/ms191477.aspx>

Kurzu, který nastavuje databáze odrážející strukturu v Azure najdete tady: <https://technet.microsoft.com/library/ms189852.aspx> 

#### <a name="alwayson"></a>AlwaysOn
Podporované AlwaysOn pro SAP místních (viz poznámka SAP [1772688]), je podporována pro použití v kombinaci s SAP v Azure. Skutečnosti, abyste se nemuseli moct vytvářet sdílených disků v Azure neznamená, jednu nelze vytvořit konfigurace AlwaysOn systému Windows Server překlopení obrázku (WSFC) mezi různých VMs. Pouze znamená to, nemají možnost používat sdílený disk jako hlasování v konfiguraci clusteru. Můžete tedy AlwaysOn WSFC konfiguraci v Azure sestavení a jednoduše není vyberte požadovaný typ kvora využívá sdílený disk. Azure prostředí tyto VMs jsou nasazenou v by měl vyřešit VMs podle názvu a VMs by měl být na tu samou doménu. To platí jenom Azure a mezi místním nasazení. Existují některé důležité kolem nasazení SQL Server dostupnost skupiny posluchače (ne zaměňovat s sadu dostupnost Azure) od Azure v daném okamžiku neumožňuje jednoduše objekt AD/DNS vytvořte je to možné místního. Některé kroky jiné instalace jsou proto muset vyřešit konkrétní chování Azure.

Je několik tipů pomocí posluchače dostupnost skupiny:

* Pomocí posluchače dostupnost skupina je možné pouze v systému Windows Server 2012 nebo Windows Server 2012 R2 jako Host OS OM. Pro Windows Server 2012, budete muset zkontrolujte, zda je použit opravy: <https://support.microsoft.com/kb/2854082> 
* Pro Windows Server 2008 R2 opravu neexistuje a AlwaysOn by bylo nutné použít stejným způsobem jako odrážející strukturu databáze zadáním partnerem převzetí v řetězci připojení (Hotovo serverem SAP default.pfl parametr dbs/mss / – najdete v článku SAP poznámku [965908]).
* Při použití posluchače skupiny dostupnost, musíte být připojeni k vyhrazené Vyrovnávání zatížení VMs databáze. Překlad v cloudu – jen nasazeních buď vyžadují všechny VMs systému SAP (aplikace servery systému správy databáze a () SCS server) jsou ve stejné síti virtuální nebo vyžadují z vrstvy aplikace SAP zachování souboru etc\host abyste mohli získávat OM názvy VMs SQL Server vyřešit. A zabránit tak, že Azure přiřazení nové IP adresy v případech, kdy obou VMs náhodně vypnutí, jednu měli přiřadit statické IP adresy rozhraní sítě můžou být VMs v konfiguraci AlwaysOn (definování statické IP adresy je popsaná v [této] [ virtual-networks-reserved-private-ip] článek) [Komentář]: <> (starý blogy) [Komentář]: <> (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx> <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Existuje zvláštní kroky nutné v případě vytváření konfigurace clusteru WSFC kde clusteru vyžaduje speciální IP adresa přiřazen, protože Azure s jeho aktuálním funkce by přiřadit název clusteru stejnou IP adresu jako uzel clusteru je vytvořen na. To znamená, že je nutné provést ruční kroku přiřadit různé IP adresu clusteru.
* Skupina posluchače dostupnost bude vytvořené v Azure pomocí TCP/IP koncové body, které jsou přiřazené VMs systém primárních a sekundárních kopie skupiny dostupnosti.
* Může být potřeba zabezpečené tyto koncové body s ACL.

[Komentář]: <>  (Úkol staré blogu)
[Komentář]: <>  (Podrobný postup a životní potřeby instalace AlwaysOn konfigurace na Azure jsou nejlépe využijete při rozbor výuková dostupné [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[Komentář]: <>  (Automaticky předem nakonfigurovaná AlwaysOn instalace prostřednictvím Galerie Azure < https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[Komentář]: <>  (Vytváření posluchače dostupnost skupina je nejlepší podle [this][virtual-machines-windows-classic-ps-sql-int-listener] kurz)
[Komentář]: <>  (Zabezpečení koncové body sítě s ACL jsou vysvětleny nejlépe tady:)
[Komentář]: <>  (* < https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[Komentář]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>)
[Komentář]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[Komentář]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Je možné nasadit skupinu SQL Server AlwaysOn dostupnost přes různé regiony Azure a taky. Tato funkce bude využít připojení Azure VNet Vnet ([Podrobnosti][virtual-networks-configure-vnet-to-vnet-connection]).
[Poznámka]: <> (úkol staré blog) [Komentář]: <> (zde popsané instalaci systému SQL Server AlwaysOn dostupnost skupiny v tomto případě: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>Souhrn pro SQL Server dostupnost Azure
Vzhledem k tomu, že úložišti Azure ochrana obsahu, je jedna méně důvod trvat na obrázku za běhu úsporném režimu. To znamená, že nefunguje dostupnost potřebuje jenom chránit před následujících případech:

* Nedostupnost OM jako celek kvůli údržby clusteru server Azure nebo dalších důvodů
* Problémy se softwarem v instanci systému SQL Server
* Ochrana před ruční chyba místo, kam se data odstraněna a není potřeba v okamžiku obnovení

Prohlížíte odpovídající technologiích jednu můžete uvádějí, že prvních dvou případech můžete vztahovat odrážející strukturu databáze nebo AlwaysOn, že třetího případu pouze můžete vztahovat protokolu dodávky.

Je třeba vyvážení komplexnější nastavení AlwaysOn ve srovnání s odrážející strukturu databáze, s výhody AlwaysOn. Tyto výhody může být vedená jako:

*   Čitelné sekundární repliky.
*   Zálohování z sekundární repliky.
*   Lepší škálovatelnost.
*   Více než jednu vedlejší repliky.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Obecné SQL Server pro spolupráci prostřednictvím SAP na Azure souhrn
V této příručce mnoho doporučeními a doporučujeme, abyste že si přečíst ho více než jednou před plánování Azure nasazení. Obecně ale nezapomeňte ke sledování horních deseti obecné systému správy databáze na Azure určité body:

[Komentář]: <>  (2.3 vyšší výkon než co? Než jeden virtuální pevný disk?)
1. Použijte nejnovější verzi systému správy databáze, jako je 2014 SQL serveru, s většina výhody v Azure. Pro systém SQL Server je SQL Server 2012 SP1 CU4, který by obsahoval funkci zálohování s desktopovým Azure úložiště. Však ve spojení s SAP doporučujeme aspoň SQL Server 2014 SP1 CU1 nebo SQL Server 2012 SP2 a nejnovější CU.
1. Naplánujte vaší SAP systém orientace na šířku v Azure vyrovnání rozložení soubor dat a Azure omezení:
    * Nechcete mít moc VHD, ale dost zajistěte, aby že se dostanete požadované procesorů.
    * Mějte na paměti procesorů jsou také omezené každý účet Azure úložiště a že účty úložiště jsou omezené v rámci každé Azure předplatného ([Podrobnosti][azure-subscription-service-limits]). 
    * Pouze pruh přes VHD v případě potřeby dosažení vyšší výkon.
1. Nikdy instalace softwaru nebo umístění všechny soubory, které vyžadují trvalé na disku D:\, jako je není permanent a všechno, co na disku budou ztraceny při restartování Windows.
1. Nepoužívejte virtuální pevný disk Azure ukládání do mezipaměti pro standardní úložišti Azure.
1. Nepoužívejte Azure replikovat geo úložiště účty.  Použití místně nadbytečné pro úloh systému správy databáze.
1. Pomocí řešení HA/DR dodavatele systému správy databáze replikovat dat z databáze.
1. Vždy používat překlad, není spolehnout IP adres.
1. Použití nejvyšší možná komprese databáze. Pro systém SQL Server je komprese stránky.
1. Dejte pozor, pomocí serveru SQL Server obrázky z Azure Marketplace. Pokud používáte jednu SQL serveru, musíte změnit řazení instance před instalací žádný SAP NetWeaver systém v něm.
1. Nainstalujte a nakonfigurujte sledování SAP hostitele pro Azure podle popisu v [Průvodce nasazením] [Průvodce nasazením].

## <a name="specifics-to-sap-ase-on-windows"></a>Zvláštnosti pomocného mechanismu řízení SAP v systému Windows
Začnete s Microsoft Azure, můžete snadno migrovat existující aplikace SAP pomocného mechanismu řízení na virtuálních počítačích Azure. SAP pomocného mechanismu řízení ve počítače virtuální umožňuje zmenšit snadno migraci těchto aplikací na Microsoft Azure celkové náklady na vlastnictví nasazení, Správa a údržba podnikových aplikací šířka. S SAP pomocný v Azure virtuálního počítače správci a vývojáři můžete dál používat stejný vývoj a nástroje pro správu, které jsou k dispozici místního.

Existuje SLA pro virtuálních počítačích Azure které naleznete zde: <https://azure.microsoft.com/support/legal/sla>

Jsme si jistí, že ve srovnání s jiné veřejné pro virtualizaci Cloud, ale jednotlivé výsledky provede dobře hostovanou virtuálních počítačích Microsoft Azure se může lišit. SAP změny velikosti protokoly SAP počet různých SAP certifikované že OM SKU se dozvíte v samostatném SAP poznámku [1928533].

Příkazy a doporučení týkající se používání Azure úložiště, VMs SAP nasazení nebo sledování SAP platí pro nasazení pomocného mechanismu řízení SAP ve spojení s aplikací SAP, jak je uvedeno v celém první čtyři kapitoly v tomto dokumentu.

### <a name="sap-ase-version-support"></a>Podpora pomocného mechanismu řízení verze SAP 
SAP aktuálně podporuje SAP pomocného mechanismu řízení verze 16.0 pro použití s produkty SAP Business Suite. Všechny aktualizace pro pomocného mechanismu řízení SAP server nebo JDBC a ODBC ovladačů pro použití s produkty SAP Business Suite jsou k dispozici pouze prostřednictvím služby Marketplace SAP na: <https://support.sap.com/swdc>.

Pokud jde o instalaci na pracovišti není ke stažení aktualizací pro pomocného mechanismu řízení SAP server nebo JDBC nebo ODBC ovladače přímo z webů Sybase. Podrobné informace o oprav, které jsou podporovány pro použití s SAP Business Suite na místní produkty a v Azure virtuálních počítačích najdete v článku následující SAP poznámky:

* [1590719]
* [1973241]
 
Obecné informace o spuštění SAP Business Suite na pomocného mechanismu řízení SAP můžete najít v [oznámení změny stavu](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Pokyny ke konfiguraci pomocného mechanismu řízení SAP pro SAP související SAP pomocného mechanismu řízení zařízení Azure VMs

#### <a name="structure-of-the-sap-ase-deployment"></a>Struktura nasazení pomocného mechanismu řízení SAP
V souladu s obecný popis pomocného mechanismu řízení SAP spustitelných nachází nebo nainstalovaný na systémovou jednotku základní virtuální pevný disk OM (jednotku c:\). Obvykle Většina pomocného mechanismu řízení SAP systému a nástroje databáze nejsou využít skutečně pevně tak, že pracovní zátěž SAP NetWeaver. Proto systém a nástroje databáze (předlohy, model, saptools, sybmgmtdb, sybsystemdb) zůstat na C:\drive stejně. 

Výjimky může být dočasný databázi obsahující všechny tabulky práce a dočasné tabulky vytvořené funkcí SAP pomocný který v případě některé ERP SAP a všechny úlohami pásma může vyžadovat větší rozsah dat nebo operace objem vstupu a výstupu, který nelze zapadají do celkového konceptu původní OM základní virtuálního pevného disku (jednotku c:\).
 
V závislosti na verzi SAPInst/SWPM použili k instalaci systému může obsahovat databáze:

* Jeden tempdb pomocného mechanismu řízení SAP, která je vytvořená při instalaci pomocného mechanismu řízení SAP
* SAP pomocného mechanismu řízení tempdb vytvořil instalaci SAP pomocného mechanismu řízení a další saptempdb vytvořil SAP instalačního programu
* SAP pomocného mechanismu řízení tempdb vytvořil instalaci SAP pomocného mechanismu řízení a další tempdb, který byl vytvořen ručně (například následující SAP poznámku [1752266]) požadavkům konkrétní tempdb ERP/pásma

V případě konkrétní ERP nebo všechny úlohami pásma má smysl týkajících se výkonu, abyste mohli tempdb zařízení navíc vytvořené tempdb (SWPM nebo ručně) jednotku než C:\. pokud existuje žádné další tempdb se doporučuje vytvořit (Poznámka SAP [1752266]).

Pro tyto systémy má provést následující kroky pro navíc vytvořené tempdb:

* Přesunutí na prvním zařízení tempdb první zařízení databázi SAP
* Přidání tempdb zařízení pro každý VHD obsahující zařízení databázi SAP

Toto nastavení umožňuje tempdb buď využívat více místa než systémovou jednotku je mohli dát. Jako odkaz na jednu můžete zkontrolovat velikost zařízení tempdb na existující systémy, které pracují místní. Nebo taková konfigurace umožní procesorů čísla proti tempdb, který nelze součástí systémovou jednotku. Znovu počítačích se systémem místní slouží ke sledování pracovní zátěž vstupu a výstupu proti tempdb.

Nikdy umístit všechna zařízení pomocného mechanismu řízení SAP D:\ jednotka OM. To se týká rovněž k tempdb, i když objektů v tempdb k dispozici pouze dočasné.

#### <a name="impact-of-database-compression"></a>Dopad komprese databáze
V konfigurací vstupu a výstupu šířky pásma, kde může být omezující faktor každé míra, která omezuje procesorů vám mohou pomoci roztáhněte pracovní zátěž, které jednu mohlo by umožnit spuštění ve scénáři IaaS jako Azure. Proto doporučujeme abyste měli jistotu, že pomocného mechanismu řízení SAP komprese před nahráním existující databáze SAP Azure.

Doporučení k provedení komprese před nahráním na Azure, pokud již není implementovaná je uveden z několika důvodů:

* Částka data, která chcete nahrát Azure je menší
* Za předpokladu, že jeden pomocí dialogového okna silnější hardware s více procesorů nebo větší šířku pásma vstupu a výstupu nebo méně vstupu a výstupu latence místní je kratší doba trvání provádění komprese
* Menší velikosti databáze může vést k menší nákladů na disku rozdělení

Komprese dat a LOB pracovat v OM hostované ve virtuálních počítačích Azure stejně jako místní. Další informace o tom, jak zkontrolovat, zda komprese je už se používá v existující databázi pomocného mechanismu řízení SAP zkontrolujte, zda SAP poznámku [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Používání DBACockpit ke sledování instance databáze
K systémům SAP, které používají pomocného mechanismu řízení SAP jako platformu databáze DBACockpit přístupný jako vložený prohlížeče windows transakce DBACockpit nebo Webdynpro. Ale plnou funkčnost pro monitorování a správu databáze je dostupná v Webdynpro provádění DBACockpit pouze.

Jako s místní systémy několika kroky nutné povolit všem funkcím SAP NetWeaver používaný Webdynpro provádění DBACockpit. Postupujte podle SAP poznámku [1245200] povolit použití webdynpros a generovat požadované z nich. Při pokynů uvedených v výše poznámky se taky nakonfigurovat komunikace správce sítě Internet (korekce barev) spolu s porty, které chcete použít pro připojení http a https. Ve výchozím nastavení http vypadat takto:

> korekce barev/server_port_0 = PROT = HTTP, PORT = 8000 PROCTIMEOUT = 600, časový limit = 600
>
> korekce barev/server_port_1 = PROT = HTTPS, PORT 443$, PROCTIMEOUT = = 600, časový limit = 600

a odkazy generované transakce DBACockpit bude vypadat nějak takto:

> https://`<fullyqualifiedhostname`>: 44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/sap/bc/webdynpro/sap/dba_cockpit

V závislosti na jestli a jak virtuálního počítače Azure hostitelem systému SAP je připojený prostřednictvím webu na webu, více webu nebo ExpressRoute (křížové místní nasazení) musíte zajistit, že korekce barev používá plně kvalifikovaný název hostitele, který lze převést na počítači místo, kam se pokusíte otevřít DBACockpit z. Viz poznámka SAP [773830] pochopit, jak korekce barev Určuje úplný název hostitele v závislosti na profil parametry a nastavte parametr korekce barev/host_name_full explicitně případné potíže.

Pokud jste nainstalovali OM ve scénáři jen cloudu bez křížové místní připojení mezi místním prostředím a Azure, budete muset definovat veřejnou IP adresu a domainlabel. Formát názvu veřejné DNS OM bude pak vypadat takhle:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Další informace týkající se název DNS najdete [tady][virtual-machines-azurerm-versus-azuresm].

Nastavení parametrů profilu SAP korekce barev/host_name_full DNS název OM Azure odkaz může vypadat podobně jako:

> https://mydomainlabel.westeurope.cloudapp.NET:44300/sap/bc/webdynpro/sap/dba_cockpit

> http://mydomainlabel.westeurope.cloudapp.NET:8000/sap/bc/webdynpro/sap/dba_cockpit

V tomto případě je potřeba zkontrolujte, že:

* Přidání příchozí pravidla do skupiny zabezpečení sítě Azure portálu pro porty TCP/IP používané komunikovat s korekce barev
* Přidání příchozí pravidla konfigurace brány Windows Firewall pro porty TCP/IP používané komunikovat s korekce barev

Pro automatizovaný importovat všechny oprav k dispozici doporučuje se pravidelně použít kolekci oprav SAP poznámky k verzi SAP:

* [1558958]
* [1619967]
* [1882376]

Další informace o obchodní řídicí panel pro pomocného mechanismu řízení SAP najdete v následujících SAP poznámek:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Zálohování a obnovení aspektech pro pomocného mechanismu řízení SAP
Při nasazení pomocného mechanismu řízení SAP do Azure musí být zkontrolována metodologie vašeho zálohování. I když systému produktivní systém, databázi SAP hostované pomocným SAP je nutné zálohovat pravidelně. Od úložišti Azure pořád tři obrázky, je teď méně důležité z hlediska kompenzace chybu úložiště zálohy. Primární důvod, proč správy správnou zálohování a obnovení plánu je informace, které můžete vyrovnat chyb logické/ruční zadáním bodu v možnosti v době obnovení. Aby cílem je buď použití zálohování obnovení databáze zpátky na určitém bodě v čase nebo použít zálohy v Azure pro jiný systém zkopírováním stávající databázi. Například může přenesete z konfigurace SAP 2 osy nastavení osy 3 systému stejného systému obnovením zálohy.

Zálohování a obnovování databází v Azure pracuje stejným způsobem jako místní. Najdete v tématu SAP poznámek:

* [1588316]
* [1585981]

Podrobné informace o vytváření výpisu konfigurace a plánování zálohy. Podle strategie a požadavky, které můžete konfigurovat databáze a protokol vypíše na disk na jednu z existující VHD nebo přidejte další virtuální pevný disk pro zálohování.  Zmenšit nebezpečí ztrátou dat v případě chyby se doporučuje používat virtuálního pevného disku, kde je uložena žádné databáze zařízení.

Kromě dat a LOB komprese pomocného mechanismu řízení SAP také nabízí komprese zálohování. Zaplnit méně místa s výpisy databáze a přihlaste se doporučuje používat komprese zálohování. Přečtěte si téma SAP poznámku [1588316] Další informace. Komprese zálohování je také důležité snížit množství dat, přenesou Pokud budete chtít stáhnout zálohy nebo VHD obsahující záložní výpisy z Azure virtuálního počítače do místního nasazení.

Nepoužívejte jednotka D:\ jako cíle výpis databáze nebo protokolu.

#### <a name="performance-considerations-for-backupsrestores"></a>Informace o výkonu při zálohování a obnovení
Jako čistý počítač nasazení jsou závislé na kolik svazky může číst paralelně a co může být výkon svazky zálohování a obnovení výkonu. Kromě toho může procesoru spotřeba tak, že zálohování komprese přehrát významné role v VMs s jenom až 8 podprocesů procesoru. Proto může mít jednu:

* Méně počet VHD využít k ukládání zařízení databáze Čím menší celkový výkon v režimu čtení
* Čím menší že počet procesoru podprocesy v OM přísnější dopad komprese zálohování
* Méně cílů (prokládání adresáře, VHD) k zápisu zálohování, nižší výkon

Zvýšit počet cílů zapisovat do, zda že jsou dvě možnosti, což může být použita/kombinované podle vašich potřeb:

* Prokládání cílového svazku zálohy přes více připojené VHD Pokud chcete zlepšit výkon procesorů na tomto prokládané svazku
* Vytvoření konfigurace výpisu na úrovni pomocného mechanismu řízení SAP, který využívá víc než jeden cílový adresář psát výpisu do

Prokládání svazku přes více připojené VHD má popsané výše v této příručce. Další informace o použití více adresářů v pomocného mechanismu řízení SAP Vypsání konfigurace naleznete v dokumentaci na sp_config_dump uložená procedura, která slouží k vytvoření Vypsání konfigurace na [Informační středisko Sybase](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Obnovení havárie s Azure VMs

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replikace dat serverem replikace Sybase SAP
S pomocného mechanismu řízení SAP SAP Sybase serveru SRS (Replication) řešením je teplé úsporném asynchronní přenášet databázové transakce vzdálené umístění. 

Instalace a fungování SRS funguje taky funkčně v OM použitý ve počítače virtuální služby Azure stejně jako místní.

Pomocného mechanismu řízení HADR prostřednictvím SAP Server replikace plánuje s budoucí verzi. Bude testováno s a vydání pro platformy Microsoft Azure hned, jak je k dispozici.

## <a name="specifics-to-sap-ase-on-linux"></a>Zvláštnosti pomocného mechanismu řízení SAP na Linux

Začnete s Microsoft Azure, můžete snadno migrovat existující aplikace SAP pomocného mechanismu řízení na virtuálních počítačích Azure. SAP pomocného mechanismu řízení ve počítače virtuální umožňuje zmenšit snadno migraci těchto aplikací na Microsoft Azure celkové náklady na vlastnictví nasazení, Správa a údržba podnikových aplikací šířka. S SAP pomocný v Azure virtuálního počítače správci a vývojáři můžete dál používat stejný vývoj a nástroje pro správu, které jsou k dispozici místního.

Pro nasazení Azure VMs je důležité vědět oficiální rozsahu, které tady najdete: <https://azure.microsoft.com/support/legal/sla>

Informace pro změnu velikosti SAP a seznam SAP certifikované OM SKU bude součástí SAP poznámku [1928533]. Další SAP pro změnu velikosti dokumentů pro Azure virtuálních počítačích najdete tady <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> a tady se dozvíte <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Příkazy a doporučení týkající se používání Azure úložiště, VMs SAP nasazení nebo sledování SAP platí pro nasazení pomocného mechanismu řízení SAP ve spojení s aplikací SAP, jak je uvedeno v celém první čtyři kapitoly v tomto dokumentu.

Následující dvě poznámky SAP patří obecné informace o pomocného mechanismu řízení na Linux a pomocného mechanismu řízení v cloudu:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>Podpora pomocného mechanismu řízení verze SAP 
SAP aktuálně podporuje SAP pomocného mechanismu řízení verze 16.0 pro použití s produkty SAP Business Suite. Všechny aktualizace pro pomocného mechanismu řízení SAP server nebo JDBC a ODBC ovladačů pro použití s produkty SAP Business Suite jsou k dispozici pouze prostřednictvím služby Marketplace SAP na: <https://support.sap.com/swdc>.

Pokud jde o instalaci na pracovišti není ke stažení aktualizací pro pomocného mechanismu řízení SAP server nebo JDBC nebo ODBC ovladače přímo z webů Sybase. Podrobné informace o oprav, které jsou podporovány pro použití s SAP Business Suite na místní produkty a v Azure virtuálních počítačích najdete v článku následující SAP poznámky:

* [1590719]
* [1973241]
 
Obecné informace o spuštění SAP Business Suite na pomocného mechanismu řízení SAP můžete najít v [oznámení změny stavu](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Pokyny ke konfiguraci pomocného mechanismu řízení SAP pro SAP související SAP pomocného mechanismu řízení zařízení Azure VMs

#### <a name="structure-of-the-sap-ase-deployment"></a>Struktura nasazení pomocného mechanismu řízení SAP
V souladu s obecný popis by měly být pomocného mechanismu řízení SAP spustitelných nachází nebo nainstalovaný do systému souborů kořenové OM (/sybase). Obvykle Většina pomocného mechanismu řízení SAP systému a nástroje databáze nejsou využít skutečně pevně tak, že pracovní zátěž SAP NetWeaver. Proto systém a nástroje databáze (předlohy, model, saptools, sybmgmtdb, sybsystemdb) zůstat v systému souborů kořenové stejně. 

Výjimky může být dočasný databázi obsahující všechny tabulky práce a dočasné tabulky vytvořené funkcí SAP pomocný který v případě některé ERP SAP a všechny úlohami pásma může vyžadovat větší rozsah dat nebo operace objem vstupu a výstupu, který nelze zapadají do celkového konceptu původní OM OS disku.
 
V závislosti na verzi SAPInst/SWPM použili k instalaci systému může obsahovat databáze:

* Jeden tempdb pomocného mechanismu řízení SAP, která je vytvořená při instalaci pomocného mechanismu řízení SAP
* SAP pomocného mechanismu řízení tempdb vytvořil instalaci SAP pomocného mechanismu řízení a další saptempdb vytvořil SAP instalačního programu
* SAP pomocného mechanismu řízení tempdb vytvořil instalaci SAP pomocného mechanismu řízení a další tempdb, který byl vytvořen ručně (například následující SAP poznámku [1752266]) požadavkům konkrétní tempdb ERP/pásma

V případě konkrétní ERP nebo všechny úlohami pásma má smysl týkajících se výkonu, abyste mohli tempdb zařízení navíc vytvořené tempdb (SWPM nebo ručně) samostatného souboru systému, který lze znázornit tak, že disk jednu datovou Azure nebo Linux RAID zahrnující více disků Azure data. Pokud existuje žádné další tempdb se doporučuje vytvořit (Poznámka SAP [1752266]).

Pro tyto systémy má provést následující kroky pro navíc vytvořené tempdb:

* Přesunutí adresáři první tempdb první systému souborů databáze SAP
* Přidání tempdb adresářů pro každý VHD obsahující systému souborů databáze SAP

Toto nastavení umožňuje tempdb buď využívat více místa než systémovou jednotku je mohli dát. Jako odkaz na jednu můžete zkontrolovat velikost adresáře tempdb na existující systémy, které pracují místní. Nebo taková konfigurace umožní procesorů čísla proti tempdb, který nelze součástí systémovou jednotku. Znovu počítačích se systémem místní slouží ke sledování pracovní zátěž vstupu a výstupu proti tempdb.

Nikdy umístit všechny adresáře pomocného mechanismu řízení SAP /mnt nebo /mnt/resource OM. To se týká rovněž k tempdb, i když objektů k dispozici v tempdb jsou pouze dočasné, protože /mnt nebo /mnt/resource je temp prostor výchozí Azure OM, který není trvalý. Další informace o prostoru temp Azure OM najdete v [tomto článku][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Dopad komprese databáze
V konfigurací vstupu a výstupu šířky pásma, kde může být omezující faktor každé míra, která omezuje procesorů vám mohou pomoci roztáhněte pracovní zátěž, které jednu mohlo by umožnit spuštění ve scénáři IaaS jako Azure. Proto doporučujeme abyste měli jistotu, že pomocného mechanismu řízení SAP komprese před nahráním existující databáze SAP Azure.

Doporučení k provedení komprese před nahráním na Azure, pokud již není implementovaná je uveden z několika důvodů:

* Částka data, která chcete nahrát Azure je menší
* Za předpokladu, že jeden pomocí dialogového okna silnější hardware s více procesorů nebo větší šířku pásma vstupu a výstupu nebo méně vstupu a výstupu latence místní je kratší doba trvání provádění komprese
* Menší velikosti databáze může vést k menší nákladů na disku rozdělení

Komprese dat a LOB pracovat v OM hostované ve virtuálních počítačích Azure stejně jako místní. Další informace o tom, jak zkontrolovat, zda komprese je už se používá v existující databázi pomocného mechanismu řízení SAP zkontrolujte, zda SAP poznámku [1750510]. Viz také SAP poznámku [2121797] Další informace týkající se komprese databáze.

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Používání DBACockpit ke sledování instance databáze
K systémům SAP, které používají pomocného mechanismu řízení SAP jako platformu databáze DBACockpit přístupný jako vložený prohlížeče windows transakce DBACockpit nebo Webdynpro. Ale plnou funkčnost pro monitorování a správu databáze je dostupná v Webdynpro provádění DBACockpit pouze.

Jako s místní systémy několika kroky nutné povolit všem funkcím SAP NetWeaver používaný Webdynpro provádění DBACockpit. Postupujte podle SAP poznámku [1245200] povolit použití webdynpros a generovat požadované z nich. Při pokynů uvedených v výše poznámky se taky nakonfigurovat komunikace správce sítě Internet (korekce barev) spolu s porty, které chcete použít pro připojení http a https. Ve výchozím nastavení http vypadat takto:

> korekce barev/server_port_0 = PROT = HTTP, PORT = 8000 PROCTIMEOUT = 600, časový limit = 600
>
> korekce barev/server_port_1 = PROT = HTTPS, PORT 443$, PROCTIMEOUT = = 600, časový limit = 600

a odkazy generované transakce DBACockpit bude vypadat nějak takto:

> https://`<fullyqualifiedhostname`>: 44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: 8000/sap/bc/webdynpro/sap/dba_cockpit

V závislosti na jestli a jak virtuálního počítače Azure hostitelem systému SAP je připojený prostřednictvím webu na webu, více webu nebo ExpressRoute (křížové místní nasazení) musíte zajistit, že korekce barev používá plně kvalifikovaný název hostitele, který lze převést na počítači místo, kam se pokusíte otevřít DBACockpit z. Viz poznámka SAP [773830] pochopit, jak korekce barev Určuje úplný název hostitele v závislosti na profil parametry a nastavte parametr korekce barev/host_name_full explicitně případné potíže.

Pokud jste nainstalovali OM ve scénáři jen cloudu bez křížové místní připojení mezi místním prostředím a Azure, budete muset definovat veřejnou IP adresu a domainlabel. Formát názvu veřejné DNS OM bude pak vypadat takhle:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Další informace týkající se název DNS najdete [tady][virtual-machines-azurerm-versus-azuresm].

Nastavení parametrů profilu SAP korekce barev/host_name_full DNS název OM Azure odkaz může vypadat podobně jako:

> https://mydomainlabel.westeurope.cloudapp.NET:44300/sap/bc/webdynpro/sap/dba_cockpit
> 
> http://mydomainlabel.westeurope.cloudapp.NET:8000/sap/bc/webdynpro/sap/dba_cockpit

V tomto případě je potřeba zkontrolujte, že:

* Přidání příchozí pravidla do skupiny zabezpečení sítě Azure portálu pro porty TCP/IP používané komunikovat s korekce barev
* Přidání příchozí pravidla konfigurace brány Windows Firewall pro porty TCP/IP používané komunikovat s korekce barev

Pro automatizovaný importovat všechny oprav k dispozici doporučuje se pravidelně použít kolekci oprav SAP poznámky k verzi SAP:

* [1558958]
* [1619967]
* [1882376]

Další informace o obchodní řídicí panel pro pomocného mechanismu řízení SAP najdete v následujících SAP poznámek:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Zálohování a obnovení aspektech pro pomocného mechanismu řízení SAP
Při nasazení pomocného mechanismu řízení SAP do Azure musí být zkontrolována metodologie vašeho zálohování. I když systému produktivní systém, databázi SAP hostované pomocným SAP je nutné zálohovat pravidelně. Od úložišti Azure pořád tři obrázky, je teď méně důležité z hlediska kompenzace chybu úložiště zálohy. Primární důvod, proč správy správnou zálohování a obnovení plánu je informace, které můžete vyrovnat chyb logické/ruční zadáním bodu v možnosti v době obnovení. Aby cílem je buď použití zálohování obnovení databáze zpátky na určitém bodě v čase nebo použít zálohy v Azure pro jiný systém zkopírováním stávající databázi. Například může přenesete z konfigurace SAP 2 osy nastavení osy 3 systému stejného systému obnovením zálohy.

Zálohování a obnovování databází v Azure pracuje stejným způsobem jako místní. Najdete v tématu SAP poznámek:

* [1588316]
* [1585981]

Podrobné informace o vytváření výpisu konfigurace a plánování zálohy. Podle strategie a požadavky, které můžete konfigurovat databáze a protokol vypíše na disk na jednu z existující VHD nebo přidejte další virtuální pevný disk pro zálohování.  Zmenšit nebezpečí ztrátou dat v případě chyby se doporučuje používat virtuálního pevného disku, kde je umístěný žádné adresáře/soubor databáze.

Kromě dat a LOB komprese pomocného mechanismu řízení SAP také nabízí komprese zálohování. Zaplnit méně místa s výpisy databáze a přihlaste se doporučuje používat komprese zálohování. Přečtěte si téma SAP poznámku [1588316] Další informace. Komprese zálohování je také důležité snížit množství dat, přenesou Pokud budete chtít stáhnout zálohy nebo VHD obsahující záložní výpisy z Azure virtuálního počítače do místního nasazení.

Nepoužívejte Azure OM temp místo /mnt nebo /mnt/resource jako cíle výpis databáze nebo protokolu.

#### <a name="performance-considerations-for-backupsrestores"></a>Informace o výkonu při zálohování a obnovení
Jako čistý počítač nasazení jsou závislé na kolik svazky může číst paralelně a co může být výkon svazky zálohování a obnovení výkonu. Kromě toho může procesoru spotřeba tak, že zálohování komprese přehrát významné role v VMs s jenom až 8 podprocesů procesoru. Proto může mít jednu:

* Méně počet VHD využít k ukládání zařízení databáze Čím menší celkový výkon v režimu čtení
* Čím menší že počet procesoru podprocesy v OM přísnější dopad komprese zálohování
* Méně cílů (Linux software RAID, VHD) k zápisu zálohování, nižší výkon

Zvýšit počet cílů zapisovat do, zda že jsou dvě možnosti, což může být použita/kombinované podle vašich potřeb:

* Prokládání cílového svazku zálohy přes více připojené VHD Pokud chcete zlepšit výkon procesorů na tomto prokládané svazku
* Vytvoření konfigurace výpisu na úrovni pomocného mechanismu řízení SAP, který využívá víc než jeden cílový adresář psát výpisu do

Prokládání svazku přes více připojené VHD má popsané výše v této příručce. Další informace o použití více adresářů v pomocného mechanismu řízení SAP Vypsání konfigurace naleznete v dokumentaci na sp_config_dump uložená procedura, která slouží k vytvoření Vypsání konfigurace na [Informační středisko Sybase](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Obnovení havárie s Azure VMs

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Replikace dat serverem replikace Sybase SAP
S pomocného mechanismu řízení SAP SAP Sybase serveru SRS (Replication) řešením je teplé úsporném asynchronní přenášet databázové transakce vzdálené umístění. 

Instalace a fungování SRS funguje taky funkčně v OM použitý ve počítače virtuální služby Azure stejně jako místní.

Pomocného mechanismu řízení HADR prostřednictvím SAP Server replikace není podporována v daném okamžiku. Může být testováno s a vydání pro platformy Microsoft Azure v budoucnu.

## <a name="specifics-to-oracle-database-on-windows"></a>Zvláštnosti k databázi Oracle v systému Windows
Od midyear 2013 Oracle software nepodporuje Oracle ke spuštění v aplikaci Microsoft Windows Hyper-V a Azure. Přečtěte si tento článek získat další informace o obecné podpory Windows Hyper-V a Azure tak, že Oracle: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Následující obecné podpory konkrétním scénáři aplikací SAP využívání databáze Oracle podporované taky. Podrobnosti o jsou uvedeny v této části dokumentu.

### <a name="oracle-version-support"></a>Podpora verzi Oracle
Všechny informace o Oracle verzí a odpovídající verzí operačního systému, které podporují výpočetnímu SAP Oracle na virtuálních počítačích Azure najdete v následujících SAP Poznámka [2039619]

Obecné informace o tom, jak SAP Business Suite na Oracle můžete najít na oznámení změny stavu: <https://scn.sap.com/community/oracle>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Pokyny ke konfiguraci Oracle pro instalaci SAP v Azure VMs

#### <a name="storage-configuration"></a>Konfigurace úložiště
Jenom jedna instance podporovaný Oracle NTFS formátované discích. Všechny soubory databáze musí být uloženy v systému souborů NTFS podle disků virtuální pevný disk. Tyto VHD jsou připojeny angličtině Azure a jsou založené na úložiště objektů BLOB Azure stránky (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Jakýkoli druh síťové jednotky nebo vzdálené akcie jako služby Azure souborů:
 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>
 
**nejsou** podporovány pro soubory databáze Oracle!

Použití Azure VHD podle úložiště objektů BLOB Azure stránky, prohlášení v tomto dokumentu v kapitola [ukládání do mezipaměti VMs a VHD] [systému správy databáze Průvodce 2.1] a [úložiště Microsoft Azure] [systému správy databáze 2.3 Průvodce] platí pro nasazení s databázi Oracle.

Jak je popsáno dříve v hlavní část dokumentu, existují kvóty procesorů výkon pro VHD Azure. Přesné kvóty se v závislosti na typu OM používají. Seznam typů OM s jejich kvóty najdete [tady][virtual-machines-sizes]

Jak identifikovat podporované typy Azure OM, SAP poznámku [1928533] získáte

Dokud aktuální kvóty procesorů na disku splňuje požadavky, je možné uložit všechny soubory databáze na jeden jednoho připojené Azure virtuální pevný disk. 

Pokud podporují více procesorů, důrazně doporučujeme používat okno úložiště fondů (pouze k dispozici v systému Windows Server 2012 nebo novější) nebo prokládání systému Windows pro systém Windows 2008 R2 vytvářet jeden velký logického zařízení více připojené disků virtuální pevný disk. Další informace najdete v článku kapitola – Software RAID [systému správy databáze 2.2 Průvodce] v tomto dokumentu. Tento přístup zjednodušuje režijních správu ke správě místa na disku a zabrání úsilí o ruční distribuci soubory napříč několika připojené VHD.

#### <a name="backup--restore"></a>Zálohování a obnovení
Zálohování a obnovení funkcí SAP BR * nástroje pro Oracle podporují stejným způsobem jako na standardní serverové operační systémy Windows a Hyper-V. Správce obnovení dat Oracle (RMAN) podporuje i záloh na disk a obnovit z disku.

#### <a name="high-availability"></a>Dostupnost
[Komentář]: <>  (odkaz odkazuje na ASM)
Ochrana dat Oracle je podporovaný pro účely obnovení havárie a dostupnost. Podrobnosti najdete v [tomto] [ virtual-machines-windows-classic-configure-oracle-data-guard] si přečtěte následující dokumentaci.

#### <a name="other"></a>Další
Všechny ostatní obecná témata jako sady dostupnost Azure nebo SAP sledování použití podle popisu v první tři kapitoly v tomto dokumentu nasazení VMs s databázi Oracle.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>Zvláštnosti pro databázi MaxDB SAP v systému Windows

### <a name="sap-maxdb-version-support"></a>Podpora MaxDB verze SAP
SAP aktuálně podporuje SAP MaxDB verze 7.9 pro použití s produkty založené na SAP NetWeaver v Azure. Všechny aktualizace pro SAP MaxDB server nebo JDBC a ODBC ovladačů pro použití s produkty založené na SAP NetWeaver jsou k dispozici pouze prostřednictvím služby Marketplace SAP na <https://support.sap.com/swdc>.
Obecné informace o spuštění SAP NetWeaver na SAP MaxDB můžete najít na <https://scn.sap.com/community/maxdb>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Zobrazení podporovaných typů verze systému Microsoft Windows a Azure OM SAP MaxDB systému správy databáze
Podporované verze systému Microsoft Windows pro SAP MaxDB systému správy databáze na Azure najdete v tématu:

* [SAP produktu dostupnost matice (PAM)][sap-pam]
* Poznámka: SAP [1928533]

Důrazně doporučujeme používat nejnovější verze operačního systému Microsoft Windows, což je Microsoft Windows 2012 R2.

### <a name="available-sap-maxdb-documentation"></a>Si přečtěte následující dokumentaci MaxDB dostupné SAP
Aktualizovaný seznam si přečtěte následující dokumentaci SAP MaxDB najdete v následujících SAP Poznámka [767598]
    
### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Pokyny ke konfiguraci MaxDB SAP pro instalaci SAP v Azure VMs

#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Konfigurace úložiště
Azure úložiště doporučené postupy pro SAP MaxDB doporučeními obecné podle kapitoly [strukturu RDBMS nasazení] [systému správy databáze příručka-2].

> [AZURE.IMPORTANT] Stejně jako další databáze SAP MaxDB taky má dat a souborů protokolu. SAP MaxDB terminologie správný termín je však "hlasitost" (ne "soubor"). Například existuje SAP MaxDB dat a svazky protokolu. Nezaměňujte s operačním systémem diskové svazky. 

Stručně řečeno potřebujete:

* Nastavte účet Azure úložiště obsahující **Místní nadbytečné úložiště (LRS)** SAP MaxDB dat a protokolů svazky (tedy soubory) podle kapitoly [úložiště Microsoft Azure] [systému správy databáze 2.3 Průvodce].
* Cesta ke vstupu a výstupu pro objemů dat SAP MaxDB (tedy soubory) oddělte od vstupu a výstupu cestu k protokolu svazky (tedy soubory). To znamená, že objemů dat SAP MaxDB (tedy soubory) s nainstalovat na jeden logické jednotce SAP MaxDB protokolu svazky (tedy soubory) muset nainstalovat na jiný logické jednotky.
* Nastavení správném souboru ukládání do mezipaměti pro každý blob Azure, podle toho, jestli se používá pro SAP MaxDB dat nebo protokolu svazky (tedy soubory) a jestli používáte Azure standardní nebo Azure Premium úložiště, podle popisu v kapitola [VMs ukládání do mezipaměti] [systému správy databáze Průvodce 2.1].
* Dokud aktuální kvóty procesorů na disku splňuje požadavky, je možné ukládat všechny objemů dat v jedné připojené Azure virtuálního pevného disku a také ukládat všechny svazky protokolu databáze na jiné jednoho připojené Azure virtuální pevný disk.
* Jsou-li více procesorů a/nebo místo povinná, velmi doporučujeme používat Microsoft okno úložiště fondů (pouze k dispozici v aplikaci Microsoft Windows Server 2012 nebo novější) nebo Microsoft Windows prokládání pro Microsoft Windows 2008 R2 vytvářet jeden velký logického zařízení více připojené disků virtuální pevný disk. Další informace najdete v článku kapitola – Software RAID [systému správy databáze 2.2 Průvodce] v tomto dokumentu. Tento přístup zjednodušuje režijních správu ke správě místa na disku a zabrání úsilí o ruční distribuci napříč několika VHD připojené soubory.
* Nejvyšší procesorů požadavky můžete Azure Premium úložiště, která je dostupná na řadu Pošta a VMs GS řady.

![Přehled konfigurace Azure OM IaaS pro SAP MaxDB systému správy databáze][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Zálohování a obnovení
Když nasadíte SAP MaxDB do Azure, je třeba zkontrolovat metodologie vašeho zálohování. I když systému produktivní systém, databázi SAP hostitelem SAP MaxDB je nutné zálohovat pravidelně. Protože úložišti Azure pořád tři obrázky, zálohy je teď méně důležité z hlediska nastavení ochrany proti selhání úložiště a důležitější selhání provozní nebo správce systému. Primární důvod, proč správy správnou zálohování a obnovení plánu je tak, aby lze vyrovnávat logické nebo ruční chyby tak, že nabízí možnosti obnovení v okamžiku. Cílem tedy buď použít zálohování, obnovení databáze na určitém bodě v čase nebo použití zálohy v Azure pro jiný systém zkopírováním stávající databázi. Například může přenesete z konfigurace SAP 2 osy nastavení osy 3 systému stejného systému obnovením zálohy.

Zálohování a obnovování databází v Azure funguje stejně, stejně jako pro místní systémy, takže můžete použít standardní SAP MaxDB zálohování a obnovení nástroje, které jsou popsané v jednom z dokumentů si přečtěte následující dokumentaci SAP MaxDB uvedené v SAP poznámku [767598]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Informace o výkonu při zálohování a obnovení
Jako čistý počítač nasazení jsou závislé na kolik svazky můžete přečíst v paralelní a výkon svazky zálohování a obnovení výkonu. Kromě toho můžete procesoru spotřeba tak, že zálohování komprese přehrát důležitou roli na VMs s vlákny procesoru až 8. Proto může mít jednu:

* Méně počet VHD využít k ukládání databáze zařízení nižší celkový výkon čtení
* Čím menší že počet procesoru podprocesy v OM přísnější dopad komprese zálohování
* Méně cílů (prokládání adresáře, VHD) zapisovat zálohování, nižší výkon

Zvětšíte počet cílů zapisovat do dvěma způsoby, které můžete použít, případně v kombinaci, v závislosti na vašim potřebám:

* Přidělíte samostatné svazky pro zálohování
* Prokládání cílového svazku zálohy přes více připojené VHD Pokud chcete zlepšit výkon procesorů na tomto prokládaných disků svazku
* S samostatný vyhrazené logické disk zařízení pro:
    * SAP MaxDB záložní svazky (tedy souborů)
    * SAP objemů dat MaxDB (tedy souborů)
    * SAP MaxDB protokolu svazky (tedy souborů)

Prokládání svazku přes více připojené VHD má popisované dříve v kapitola – Software RAID [systému správy databáze 2.2 Průvodce] v tomto dokumentu. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Další
Všechny ostatní obecná témata například Azure dostupnost sady nebo SAP sledování také použít podle popisu v první tři kapitoly v tomto dokumentu nasazení VMs s databází SAP MaxDB.
Další SAP MaxDB nastavení jsou průhledné Azure VMs a jsou popsané v různé dokumenty uvedené v SAP poznámku [767598] a tyto poznámky SAP:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Zvláštnosti SAP liveCache v systému Windows

### <a name="sap-livecache-version-support"></a>SAP liveCache podporu verze
Minimální verze SAP liveCache podporovaná v Azure virtuálních počítačích je **SAP LC/LCAPPS 10.0 SP 25** včetně **liveCache 7.9.08.31** a **25 LCA sestavení**uvolnění **EhP** 2 pro SAP Správce služeb 7.0 nebo novější.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Zobrazení podporovaných typů verze systému Microsoft Windows a Azure OM SAP liveCache systému správy databáze
Podporované verze systému Microsoft Windows pro liveCache SAP na Azure najdete v tématu:

* [SAP produktu dostupnost matice (PAM)][sap-pam]
* Poznámka: SAP [1928533]

Důrazně doporučujeme používat nejnovější verze operačního systému Microsoft Windows, což je Microsoft Windows 2012 R2. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache pokyny ke konfiguraci pro instalaci SAP v Azure VMs

#### <a name="recommended-azure-vm-types"></a>Doporučeno typy Azure OM
SAP liveCache je aplikace, která provádí velké výpočty, množství a rychlosti procesoru a paměti RAM má hlavní vliv na výkon liveCache SAP. 

Pro typy Azure OM nepodporuje SAP (Poznámka SAP [1928533]) jsou všechny virtuální prostředky procesoru přidělit OM získáváte vyhrazené fyzické procesoru zdroje hypervisor. Žádné overprovisioning (a tedy žádné soutěže procesoru zdrojů) probíhá.

Podobně u všech Azure OM instance typů podporovaných SAP paměti OM je 100 % namapované fyzické paměti – overprovisioning (navýšení potvrzení), například nepoužívá.

Z této perspektivy důrazně doporučujeme použít nový typ D řady nebo OM Azure DS řady (v kombinaci s úložištěm Premium Azure), musí být 60 % rychlejší procesorů než A řadu. Pro nejvyšší RAM a procesoru zatížení můžete použít řady G a VMs GS řady (v kombinaci s úložištěm Premium Azure) s nejnovějšími Intel Xeon® E5 v3 procesorů, jejichž dvakrát paměti a čtyřikrát plné stavu jednotka úložiště (disky SSD) série D/DS.

#### <a name="storage-configuration"></a>Konfigurace úložiště
Jak SAP liveCache je založený na technologii SAP MaxDB, Azure úložiště vhodné praktické cvičení doporučení pro SAP MaxDB podle kapitoly [Konfigurace úložiště] [systému správy databáze 8.4.1 Průvodce] jsou platná pro SAP liveCache. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Vyhrazené OM Azure pro liveCache
SAP liveCache intenzivně používá výpočetní power, pro produktivní spotřebu doporučujeme pro nasazení na vyhrazené virtuálního počítače Azure. 
 
![Vyhrazené Azure OM pro liveCache pro případ použití produktivní][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Zálohování a obnovení
Zálohování a obnovení, včetně důležité informace o výkonu, už jsou popsané v příslušné SAP MaxDB kapitoly [zálohování a obnovení] [systému správy databáze 8.4.2 Průvodce] a [důležité informace o výkonu pro zálohování a obnovení] [systému správy databáze 8.4.3 Průvodce]. 

#### <a name="other"></a>Další
Všechny ostatní obecná témata jsou popsaných v příslušné kapitoly SAP MaxDB [to] [systému správy databáze 8.4.4 Průvodce]. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>Zvláštnosti SAP obsahu serveru v systému Windows
SAP Server obsah je součástí samostatné, serverová ukládat obsah, jako je elektronické dokumentů v různých formátů. SAP Server obsahu poskytuje společnost vývoj technologie a má být použité mezi aplikacemi pro všechny aplikace SAP. Hned po instalaci používat v samostatném systému. Typické obsah je školicí materiály a si přečtěte následující dokumentaci v Knowledge sklad nebo technických výkresů pocházející z mySAP PLM systému správy dokumentů. 

### <a name="sap-content-server-version-support"></a>SAP Server obsahu verze podpory
SAP v současné době podporuje:

* **SAP Server obsahu** s verze **6.50 (a novější)**
* **SAP MaxDB verze 7.9**
* **Microsoft IIS (serverem IIS) verze 8.0 (a novější)**

Důrazně doporučujeme používat nejnovější verze obsahu Server SAP, který při psaní tento dokument je **6.50 SP4**a v nejnovější verzi aplikace **Microsoft IIS 8.5**. 

Kontrola podporované verze SAP obsahu serveru a Microsoft IIS v [SAP produktu dostupnost matice PAM][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Podporované typy Microsoft Windows a Azure OM pro obsah serveru SAP
Vysvětlení podporovanou verzi systému Windows pro SAP Server obsahu v Azure, najdete v tématu:

* [SAP produktu dostupnost matice (PAM)][sap-pam]
* Poznámka: SAP [1928533]

Důrazně doporučujeme používat nejnovější verzi systému Microsoft Windows, který při psaní tento dokument je **Windows serveru 2012 R2**.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP pokyny ke konfiguraci obsahu serveru pro instalaci SAP v Azure VMs

#### <a name="storage-configuration"></a>Konfigurace úložiště 
Pokud konfigurace SAP obsahu serveru pro ukládání souborů v databázi SAP MaxDB všechny Azure úložiště osvědčené postupy doporučení pro SAP MaxDB podle kapitoly – úložiště konfigurace [systému správy databáze 8.4.1 Průvodce] platí taky pro scénář SAP obsahu serveru. 

Pokud jste nakonfigurovali SAP obsahu serveru pro ukládání souborů v systému souborů, doporučujeme použít jednotku vyhrazené logické. Použití úložiště mezer umožňuje taky zvětšit logického disku velikost a výkon procesorů podle v kapitoly – Software RAID [systému správy databáze 2.2 Průvodce]. 

#### <a name="sap-content-server-location"></a>SAP Server obsahu umístění
SAP Server obsahu musí být nasazené ve stejné Azure oblasti, kde je používaný systém SAP VNET Azure. Můžete se rozhodnout, jestli chcete nasazení obsahu SAP Server součástí vyhrazené OM Azure nebo na stejné OM kde SAP systému neobsahují. 
 
![Vyhrazené Azure OM pro obsah serveru SAP][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>Umístění serveru mezipaměti SAP
SAP Server mezipaměti je další serverová komponentu zajistit přístup k dokumentům (režim cached) místně. SAP Server mezipaměti ukládá dokumenty SAP Server obsahu. Toto je optimalizaci provozu v síti, pokud dokumentů do více než jednou načtená z různých míst. Obecně platí, že SAP Server mezipaměti musí být fyzicky zavřít klienta, který má přístup k SAP Server mezipaměti. 

Tady máte dvě možnosti:

1. **Klient je systém SAP back-end** Pokud je systém SAP back-end nakonfigurován pro přístup k obsahu serveru SAP, je systému SAP klient. Jak systém SAP a SAP Server obsahu jsou nasazeny ve stejné oblasti Azure – ve stejném Azure datacentru – jedná se o fyzicky blízko vedle sebe. Proto je potřeba mít vyhrazené SAP Server mezipaměti. Uživatelské rozhraní SAP klientů (grafické SAP nebo webový prohlížeč) přímý přístup k systému SAP a SAP systém načte dokumenty z obsahu SAP Server.
1. **Klient je místní webového prohlížeče** SAP Server obsahu mohou být nakonfigurované pro přistupovat přímo pomocí webového prohlížeče. V tomto případě webového prohlížeče s místním – je klient SAP Server obsahu. Místní datacentra a Azure datacentra jsou umístěná na různých místech fyzické (v ideálním případě blízko vedle sebe). Místní datacentra je připojen k Azure přes VPN Azure na webu nebo ExpressRoute. I když obou možnostech nabízet zabezpečené připojení sítě VPN Azure, připojení k síti na webu nenabízí SLA síťové šířky pásma a latence mezi místním datacentra a Azure datacentra. K dosažení vyššího přístup k dokumentům, můžete udělat jednu z následujících akcí:
    1. SAP Server mezipaměti místní instalaci, zavřít místní webovém prohlížeči (možnost na obrázek [this][dbms-guide-900-sap-cache-server-on-premises])
    1. Konfigurace Azure ExpressRoute, který nabízí vysokorychlostní a nízkou latencí vyhrazené síťového připojení mezi místním datacentra a Azure datacentra.
 
![Možnost instalovat SAP Server mezipaměti místní][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Zálohování a obnovení
Pokud konfigurace SAP obsahu na serveru uložit soubory databáze SAP MaxDB aspektech postup a výkonu zálohování a obnovení už jsou popsané v SAP MaxDB kapitola [zálohování a obnovení] [systému správy databáze 8.4.2 Průvodce] a kapitoly [důležité informace o výkonu pro zálohování a obnovení] [systému správy databáze 8.4.3 Průvodce]. 

Pokud jste nakonfigurovali SAP Server obsahu pro ukládání souborů v systému souborů, jednou z možností je spustit ruční zálohování a obnovení struktury celého souboru ve se nacházíte dokumenty. Podobně jako SAP MaxDB zálohování a obnovení se doporučuje mít svazku vyhrazené disku pro záložní účel. 

#### <a name="other"></a>Další
Další zvláštní nastavení obsahu SAP Server jsou průhlednou Azure VMs a jsou popsané v různých dokumentů a SAP poznámek:

* <https://Service.SAP.com/contentserver> 
* Poznámka: SAP [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>Zvláštnosti IBM DB2 pro LUW v systému Windows
S Microsoft Azure můžete snadno migrovat SAP aplikace spuštěna IBM DB2 Linux, UNIX a systému Windows (LUW) na Azure virtuálních počítačích. S SAP na IBM DB2 LUW správci a vývojáři můžete dál používat stejnou vývoj a nástroje pro správu, které jsou k dispozici místního.
Obecné informace o tom, jak SAP Business Suite na IBM DB2 LUW můžete najít v SAP komunity síti (oznámení změny stavu) na <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Další informace a aktualizace týkající se SAP na DB2 LUW na Azure najdete v tématu SAP poznámku [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>Databáze IBM DB2 Linux, UNIX a podporu verze systému Windows
SAP na IBM DB2 LUW na Microsoft Azure virtuálního počítače Services podporovaná DB2 verzi 10.5.

Informace o podporovaných SAP produkty a typy Azure OM získáte SAP poznámku [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Databáze IBM DB2 Linux, UNIX a pokyny ke konfiguraci systému Windows pro instalaci SAP v Azure VMs

#### <a name="storage-configuration"></a>Konfigurace úložiště
Všechny soubory databáze musí být uloženy v systému souborů NTFS podle disků virtuální pevný disk. Tyto VHD jsou připojeny angličtině Azure a jsou založeny v úložišti objektů BLOB Azure stránky (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Jakýkoli druh síťové jednotky nebo vzdálené sdílené položky, jako jsou následující služby Azure soubor **není** podporována pro soubory databáze: 

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>
 
Pokud používáte Azure VHD podle úložiště objektů BLOB Azure stránky, údaji v tomto dokumentu v kapitola [strukturu RDBMS nasazení] [systému správy databáze příručka-2] také platí pro nasazení s IBM DB2 LUW databáze. 

Jak je popsáno dříve v hlavní část dokumentu, existují kvóty procesorů výkon pro VHD Azure. Přesné kvóty závisí na typu OM použít. Seznam typů OM s jejich kvóty najdete [tady][virtual-machines-sizes].

Dokud aktuální kvóty procesorů na disku dostatečné, je možné uložit všechny soubory databáze na jeden jednoho připojené Azure virtuální pevný disk. 

Výkon aspektech také v příručce kapitola "Data zabezpečení a výkonu aspektech pro databázi adresářů" SAP instalace vodítka.

Můžete taky můžete Windows úložiště fondů (pouze k dispozici v systému Windows Server 2012 nebo novější) nebo prokládání systému Windows pro systém Windows 2008 R2 podle kapitoly – Software RAID [systému správy databáze 2.2 Průvodce] Tento dokument vytvářet jeden velký logického zařízení více připojené disků virtuální pevný disk.
Pro disků obsahujících cest DB2 úložiště pro sapdata a saptmp adresáře je nutné zadat velikost sector fyzický disk 512 kB. Pokud používáte Windows úložiště fondů, musíte vytvořit fondů úložiště ručně pomocí rozhraní příkazového řádku pomocí parametru "-LogicalSectorSizeDefault". Podrobnosti najdete v článku <https://technet.microsoft.com/library/hh848689.aspx>.

#### <a name="backuprestore"></a>Zálohování a obnovení
Funkci zálohování a obnovení databáze IBM DB2 pro LUW je podporována stejným způsobem jako na standardní serverové operační systémy Windows a Hyper-V.

Musíte mít strategii zálohování platné databáze na místě. 

Jako čistý počítač nasazení zálohování a obnovení výkon závisí na kolik svazky může číst paralelně a co může být výkon svazky. Kromě toho může procesoru spotřeba tak, že zálohování komprese přehrát významné role v VMs s jenom až 8 podprocesů procesoru. Proto může mít jednu:

* Méně počet VHD využít k ukládání zařízení databáze Čím menší celkový výkon v režimu čtení
* Čím menší že počet procesoru podprocesy v OM přísnější dopad komprese zálohování
* Méně cílů (prokládání adresáře, VHD) zapisovat zálohování, nižší výkon

Ke zvýšení počtu cílových k zápisu, může být dvě možnosti použít/kombinované podle vašich potřeb:

* Prokládání cílového svazku zálohy přes více připojené VHD Pokud chcete zlepšit výkon procesorů na tomto prokládané svazku
* Použití více cílový adresář psát zálohování

#### <a name="high-availability-and-disaster-recovery"></a>Dostupnost a obnovení
Microsoft Cluster Server (MSCS) není podporované.

Je podporován DB2 dostupnost havárie zotavení (HADR). Pokud virtuálních počítačích konfigurace HA práce překlad, nebude nastavení v Azure lišit od nastavení, která probíhá v místním. Nedoporučujeme spolehnout pouze překlad IP.

Nepoužívejte Azure úložiště Geo-replikace. Další informace najdete v příručce kapitola [úložiště Microsoft Azure] [systému správy databáze 2.3 Průvodce] a kapitoly [dostupnost a obnovení s Azure VMs] [systému správy databáze příručka-3].

#### <a name="other"></a>Další
Všechny ostatní obecná témata jako sady dostupnost Azure nebo SAP sledování použití podle popisu v první tři kapitoly v tomto dokumentu nasazení VMs s IBM DB2 LUW stejně. 

Také odkazovat na kapitoly [Obecné SQL Server pro spolupráci prostřednictvím SAP na Azure souhrn] [systému správy databáze 5.8 Průvodce].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>Zvláštnosti IBM DB2 pro LUW na Linux
S Microsoft Azure můžete snadno migrovat SAP aplikace spuštěna IBM DB2 Linux, UNIX a systému Windows (LUW) na Azure virtuálních počítačích. S SAP na IBM DB2 LUW správci a vývojáři můžete dál používat stejnou vývoj a nástroje pro správu, které jsou k dispozici místního. Obecné informace o tom, jak SAP Business Suite na IBM DB2 LUW můžete najít v SAP komunity síti (oznámení změny stavu) na <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Další informace a aktualizace týkající se SAP na DB2 LUW na Azure najdete v tématu SAP poznámku [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>Databáze IBM DB2 Linux, UNIX a podporu verze systému Windows
SAP na IBM DB2 LUW na Microsoft Azure virtuálního počítače Services podporovaná DB2 verzi 10.5.

Informace o podporovaných SAP produkty a typy Azure OM získáte SAP poznámku [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Databáze IBM DB2 Linux, UNIX a pokyny ke konfiguraci systému Windows pro instalaci SAP v Azure VMs

#### <a name="storage-configuration"></a>Konfigurace úložiště
Všechny soubory databáze musí být uloženy v systému souborů založené na discích virtuální pevný disk. Tyto VHD jsou připojeny angličtině Azure a jsou založeny v úložišti objektů BLOB Azure stránky (<https://msdn.microsoft.com/library/azure/ee691964.aspx>).
Jakýkoli druh síťové jednotky nebo vzdálené sdílené položky, jako jsou následující služby Azure soubor **není** podporována pro soubory databáze:

* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/Introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-Connections-to-Microsoft-Azure-Files.aspx>

Pokud používáte Azure VHD podle úložiště objektů BLOB Azure stránky, údaji v tomto dokumentu v kapitola [strukturu RDBMS nasazení] [systému správy databáze příručka-2] také platí pro nasazení s IBM DB2 LUW databáze.

Jak je popsáno dříve v hlavní část dokumentu, existují kvóty procesorů výkon pro VHD Azure. Přesné kvóty závisí na typu OM použít. Seznam typů OM s jejich kvóty najdete [tady][virtual-machines-sizes].

Dokud aktuální kvóty procesorů na disku dostatečné, je možné uložit všechny soubory databáze na jeden jednoho připojené Azure virtuální pevný disk.

Výkon aspektech také v příručce kapitola "Data zabezpečení a výkonu aspektech pro databázi adresářů" SAP instalace vodítka.

Můžete taky můžete LVM (Správce logických hlasitost) nebo MDADM podle kapitoly – Software RAID [systému správy databáze 2.2 Průvodce] Tento dokument vytvářet jeden velký logického zařízení více připojené disků virtuální pevný disk.
Pro disků obsahujících cest DB2 úložiště pro sapdata a saptmp adresáře je nutné zadat velikost sector fyzický disk 512 kB.

#### <a name="backuprestore"></a>Zálohování a obnovení
Funkci zálohování a obnovení databáze IBM DB2 pro LUW je podporována stejným způsobem jako standardní Linux instalace na místní.

Musíte mít strategii zálohování platné databáze na místě.

Jako čistý počítač nasazení zálohování a obnovení výkon závisí na kolik svazky může číst paralelně a co může být výkon svazky. Kromě toho může procesoru spotřeba tak, že zálohování komprese přehrát významné role v VMs s jenom až 8 podprocesů procesoru. Proto může mít jednu:

* Méně počet VHD využít k ukládání zařízení databáze Čím menší celkový výkon v režimu čtení
* Čím menší že počet procesoru podprocesy v OM přísnější dopad komprese zálohování
* Méně cílů (prokládání adresáře, VHD) zapisovat zálohování, nižší výkon

Ke zvýšení počtu cílových k zápisu, může být dvě možnosti použít/kombinované podle vašich potřeb:

* Prokládání cílového svazku zálohy přes více připojené VHD Pokud chcete zlepšit výkon procesorů na tomto prokládané svazku
* Použití více cílový adresář psát zálohování

#### <a name="high-availability-and-disaster-recovery"></a>Dostupnost a obnovení
Je podporován DB2 dostupnost havárie obnovení (HADR). Pokud virtuálních počítačích konfigurace HA práce překlad, nebude nastavení v Azure lišit od nastavení, která probíhá v místním. Nedoporučujeme spolehnout pouze překlad IP.

Nepoužívejte Azure úložiště Geo-replikace. Další informace najdete v příručce kapitola [úložiště Microsoft Azure] [systému správy databáze 2.3 Průvodce] a kapitoly [dostupnost a obnovení s Azure VMs] [systému správy databáze příručka-3].

#### <a name="other"></a>Další
Všechny ostatní obecná témata jako sady dostupnost Azure nebo SAP sledování použití podle popisu v první tři kapitoly v tomto dokumentu nasazení VMs s IBM DB2 LUW stejně.

Také odkazovat na kapitoly [Obecné SQL Server pro spolupráci prostřednictvím SAP na Azure souhrn] [systému správy databáze 5.8 Průvodce].