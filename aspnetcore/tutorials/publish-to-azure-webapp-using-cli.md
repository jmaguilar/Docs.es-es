---
title: "Publicación de una aplicación de ASP.NET Core en Azure con las herramientas de línea de comandos | Microsoft Docs"
description: "Obtenga información sobre cómo compilar e implementar una aplicación de Microsoft Azure con ASP.NET Core y el cliente de línea de comandos de Git."
services: multiple
keywords: "ASP.NET Core, Azure, App Service, Git, línea de comandos"
author: camsoper
ms.author: casoper
manager: wpickett
ms.date: 11/03/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
ms.custom: mvc
ms.devlang: dotnet
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 0bcff4f79356b960f663dcebb1d79a108417dbd2
ms.sourcegitcommit: f017f940a164dbaf84307410c78eb14e0f3ac811
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a><span data-ttu-id="bec03-104">Implementación de una aplicación de ASP.NET Core en Azure App Service desde la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="bec03-104">Deploy an ASP.NET Core application to Azure App Service from the command line</span></span>

<span data-ttu-id="bec03-105">Por [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="bec03-105">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="bec03-106">En este tutorial se explica cómo compilar e implementar una aplicación de ASP.NET Core en Microsoft Azure App Service con las herramientas de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="bec03-106">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="bec03-107">Cuando termine, tendrá una aplicación web integrada en ASP.NET MVC Core hospedada como una aplicación web de Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="bec03-107">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="bec03-108">Este tutorial se escribe con herramientas de línea de comandos de Windows, pero también puede aplicarse a entornos de macOS y Linux.</span><span class="sxs-lookup"><span data-stu-id="bec03-108">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="bec03-109">En este tutorial aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="bec03-109">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bec03-110">Crear un sitio web en Azure App Service con la CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="bec03-110">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="bec03-111">Implementar una aplicación de ASP.NET Core en Azure App Service con la herramienta de línea de comandos de Git</span><span class="sxs-lookup"><span data-stu-id="bec03-111">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bec03-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="bec03-112">Prerequisites</span></span>

<span data-ttu-id="bec03-113">Para completar este tutorial, necesita:</span><span class="sxs-lookup"><span data-stu-id="bec03-113">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="bec03-114">Una [suscripción de Microsoft Azure](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="bec03-114">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [<span data-ttu-id="bec03-115">Núcleo de .NET</span><span class="sxs-lookup"><span data-stu-id="bec03-115">.NET Core</span></span>](https://www.microsoft.com/net/download/core)
* <span data-ttu-id="bec03-116">Un cliente de línea de comandos de [Git](https://www.git-scm.com/)</span><span class="sxs-lookup"><span data-stu-id="bec03-116">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="bec03-117">Creación de una aplicación web</span><span class="sxs-lookup"><span data-stu-id="bec03-117">Create a web application</span></span>

<span data-ttu-id="bec03-118">Cree un directorio para la aplicación web, cree una aplicación de ASP.NET Core MVC y luego ejecute el sitio web de forma local.</span><span class="sxs-lookup"><span data-stu-id="bec03-118">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bec03-119">Windows</span><span class="sxs-lookup"><span data-stu-id="bec03-119">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="bec03-120">Otros problemas</span><span class="sxs-lookup"><span data-stu-id="bec03-120">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![Salida de la línea de comandos](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="bec03-122">Navegue a http://localhost:5000 para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bec03-122">Test the application by browsing to http://localhost:5000.</span></span>

![Sitio web en ejecución en el entorno local](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="bec03-124">Creación de la instancia de Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bec03-124">Create the Azure App Service instance</span></span>

<span data-ttu-id="bec03-125">Con [Azure Cloud Shell](/azure/cloud-shell/quickstart), cree un grupo de recursos, un plan de App Service y una aplicación web de App Service.</span><span class="sxs-lookup"><span data-stu-id="bec03-125">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

<span data-ttu-id="bec03-126">Antes de la implementación, defina las credenciales de implementación a nivel de cuenta con el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="bec03-126">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="bec03-127">Se necesita una dirección URL de implementación para implementar la aplicación con Git.</span><span class="sxs-lookup"><span data-stu-id="bec03-127">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="bec03-128">Recupere una dirección URL como esta.</span><span class="sxs-lookup"><span data-stu-id="bec03-128">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="bec03-129">Anote la dirección URL mostrada que termina en `.git`.</span><span class="sxs-lookup"><span data-stu-id="bec03-129">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="bec03-130">Se utiliza en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="bec03-130">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="bec03-131">Implementación de la aplicación con Git</span><span class="sxs-lookup"><span data-stu-id="bec03-131">Deploy the application using Git</span></span>

<span data-ttu-id="bec03-132">Está listo para implementar desde el equipo local mediante Git.</span><span class="sxs-lookup"><span data-stu-id="bec03-132">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="bec03-133">Es seguro pasar por alto las advertencias de Git sobre los finales de línea.</span><span class="sxs-lookup"><span data-stu-id="bec03-133">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bec03-134">Windows</span><span class="sxs-lookup"><span data-stu-id="bec03-134">Windows</span></span>](#tab/windows)
```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[<span data-ttu-id="bec03-135">Otros problemas</span><span class="sxs-lookup"><span data-stu-id="bec03-135">Other</span></span>](#tab/other)
```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```
---

<span data-ttu-id="bec03-136">Git solicitará las credenciales de implementación establecidas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="bec03-136">Git will prompt for the deployment credentials that were set earlier.</span></span>  <span data-ttu-id="bec03-137">Tras la autenticación, la aplicación se inserta en la ubicación remota, se compila y se implementa.</span><span class="sxs-lookup"><span data-stu-id="bec03-137">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Resultado de la implementación de Git](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="bec03-139">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="bec03-139">Test the application</span></span>

<span data-ttu-id="bec03-140">Navegue a `https://<web app name>.azurewebsites.net` para probar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bec03-140">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="bec03-141">Para mostrar la dirección en Cloud Shell o en la CLI de Azure, se usa lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bec03-141">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![La aplicación en ejecución en Azure](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="bec03-143">Limpieza</span><span class="sxs-lookup"><span data-stu-id="bec03-143">Clean up</span></span>

<span data-ttu-id="bec03-144">Cuando termine de probar la aplicación y de inspeccionar el código y los recursos, elimine la aplicación web y el plan con la eliminación del grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="bec03-144">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="bec03-145">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="bec03-145">Next steps</span></span>

<span data-ttu-id="bec03-146">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="bec03-146">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bec03-147">Crear un sitio web en Azure App Service con la CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="bec03-147">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="bec03-148">Implementar una aplicación de ASP.NET Core en Azure App Service con la herramienta de línea de comandos de Git</span><span class="sxs-lookup"><span data-stu-id="bec03-148">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="bec03-149">A continuación, aprenda a usar la línea de comandos para implementar una aplicación web existente que usa Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="bec03-149">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bec03-150">Implementación en Azure desde la línea de comandos con .NET Core</span><span class="sxs-lookup"><span data-stu-id="bec03-150">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)