---
uid: webhooks/diagnostics/debugging
title: WebHooks de ASP.NET depuración | Microsoft Docs
author: rick-anderson
description: Cómo depurar WebHooks de ASP.NET.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 25fa47202dd31d6ebd7a0e179075333108515274
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801993"
---
# <a name="aspnet-webhooks-debugging"></a>Depuración de WebHooks de ASP.NET  

## <a name="debugging-in-azure"></a>Depuración en Azure

Para depurar la aplicación Web mientras se ejecuta en Azure, consulte el tutorial [solucionar problemas de una aplicación web en Azure App Service mediante Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Depuración con símbolos y código fuente

Además de su propio código de depuración, es posible depurar directamente en Microsoft ASP.NET WebHooks y, en realidad todo de. NET. Esto funciona independientemente de si se depura localmente o remotamente. En primer lugar, configurar Visual Studio para ver el código fuente y símbolos, vaya a **depurar** y, a continuación, **opciones y configuración**. Establecer las opciones similar al siguiente:

![Opciones y configuración](_static/SourceSymbols.png)

A continuación, agregue un vínculo a [symbolsource.org](http://symbolsource.org) para descargar el código fuente y símbolos. Vaya a la **símbolos** ficha del menú anterior y agregue lo siguiente como una ubicación de símbolos:

```
http://srv.symbolsource.org/pdb/Public
```

Además, asegúrese de que el directorio de caché tiene un nombre corto; en caso contrario, los nombres de archivo pueden obtener demasiados lo que hará que los símbolos que no se cargue. Es una ruta de acceso de ejemplo:

```
C:\SymCache
```

La configuración debe ser similar al siguiente:

![Ejemplo de ubicación de archivo de símbolos de opciones](_static/SymSource.png)
