# TOS_ApiGateway_Ocelot
Ce README fournit des instructions pour mettre en place et configurer un projet d'API Gateway en utilisant Ocelot, une solution populaire pour les passerelles API en .NET.
[]()
## Introduction

Ocelot est une API Gateway sur .NET, permettant de router les requêtes à travers plusieurs microservices. Elle offre des fonctionnalités comme par exemple l'équilibrage de charge, l'authentification ou bien la mise en cache.

## Prérequis

- [.NET 5.0 SDK](https://dotnet.microsoft.com/download/dotnet/5.0) à minima
- [Visual Studio](https://visualstudio.microsoft.com/) (de préférence)
- Connaissances de base en C# et ASP.NET Core

## Installation

### Créer le Projet

1. Ouvrez Visual Studio et créez un nouveau projet.
2. Sélectionnez "Application Web ASP.NET Core".
3. Nommez votre projet et choisissez son emplacement.
4. Sélectionnez de préférence pour .NET 7.0 où à minima comme énnoncer dans les pré-requis .NET 5.0 comme framework.

### Intégration de Ocelot

Dans le terminal ou la console de commande :

```bash
cd chemin/vers/votre/projet
dotnet add package Ocelot
```

## Configuration

### Fichier `ocelot.json`

A la racine du projet fraichement créez un fichier `ocelot.json`. Ce fichier contiendra la configuration de vos routes. 

Voici un exemple de configuration pour les routes d'un microservice  :

```json
{
    "Routes": [
        {
            "DownstreamPathTemplate": "/api/bars",
            "DownstreamScheme": "http",
            "DownstreamHostAndPorts": [
                {
                    "Host": "authentication-svc"
                }
            ],
            "UpstreamPathTemplate": "/api/bars",
            "UpstreamHttpMethod": [ "Get", "Post", "Put" ]
        },
        {
            "DownstreamPathTemplate": "/api/bars/{id}",
            "DownstreamScheme": "http",
            "DownstreamHostAndPorts": [
                {
                    "Host": "authentication-svc"
                }
            ],
            "UpstreamPathTemplate": "/api/bars/{id}",
            "UpstreamHttpMethod": [ "Get", "Delete" ]
        }
    ],
    "GlobalConfiguration": {
        "BaseUrl": "https://ocelot.local"

    }
}

```
### Mise à jour de `Program.cs`

Après avoir configuré votre fichier `ocelot.json`, la prochaine étape est d'intégrer Ocelot dans votre projet ASP.NET Core. Cela se fait en modifiant le fichier `Program.cs` de votre projet. 
Le but est d'ajouter des services au conteneur de l'application. Vous devez y ajouter Ocelot. Insérez le code suivant :

   ```csharp
   builder.Configuration.SetBasePath(builder.Environment.ContentRootPath)
    .AddJsonFile("ocelot.json", optional: false);
   builder.Services.AddOcelot();
   app.UseOcelot();
  ```

Et voila, votre API Gateway est prête à être testée puis déployée dans votre environnement de dev.
