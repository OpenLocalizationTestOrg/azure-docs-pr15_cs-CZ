<properties 
   pageTitle="Ladění Azure Cloudovým službám | Microsoft Azure"
   description="Ladění služby Azure Cloud Services"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-cloud-services"></a>Ladění cloudovým službám

Můžete provádět způsobů ladění aplikace Azure pomocí nástroje Azure pro Microsoft Visual Studio a Azure SDK:

- Azure aplikace Visual Studio můžete ladění při vyvíjíte, stejně jako jakékoli aplikace Visual Basic nebo Visual Basic. Další informace najdete v tématu [ladění cloudové služby na místním počítači](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

- Diagnostika Azure umožňuje podrobné informace Zaprotokolují kód spuštěný v rámci role, zda role běží ve vývojovém prostředí nebo v Azure. Další informace najdete v tématu [Operace shromažďování protokolování dat pomocí diagnostických nástrojů Azure](http://go.microsoft.com/fwlink/p/?LinkId=400450).

- Pokud používáte Visual Studio Enterprise psát role určené pro .NET Framework 4 nebo .NET Framework 4.5, můžete povolit IntelliTrace v době nasazení služby Azure cloudu z aplikace Visual Studio. IntelliTrace poskytuje protokol, který vám pomohou s Visual Studio ladění aplikace, jako kdyby ho používali v Azure. Další informace najdete v tématu [ladění publikované Cloudová služba s IntelliTrace a Visual Studia]( http://go.microsoft.com/fwlink/p/?LinkId=623016).

- Můžete povolit vzdálené ladění cloudovým službám v době, kdy nasadíte cloudovou službu z aplikace Visual Studio. Pokud se rozhodnete povolení vzdáleného ladění nasazení, vzdálené ladění nainstalovaných na virtuálních počítačích, na kterých běží pokaždé role. Tyto služby, jako je msvsmon.exe, nevytvářejte mít vliv na výkon a mít za následek navíc náklady. Další informace najdete v tématu [ladění do cloudové služby Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).



