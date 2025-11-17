# IT-Glue-API-PowerShell-Wrapper

Ce module PowerShell sert de wrapper pour l'API d'IT Glue.

---

## Introduction

L'API d'IT Glue offre la possibilité de lire, créer et mettre à jour une grande partie des données de la plateforme de documentation d'IT Glue. Cela inclut les organisations, les contacts, les éléments de configuration et plus encore. La documentation complète de l'API RESTful d'IT Glue peut être trouvée [ici](https://api.itglue.com/developer/).

Ce module vise à masquer les détails de l'interaction avec les points d'extrémité de l'API d'IT Glue d'une manière cohérente avec la terminologie de PowerShell. Cela donne aux administrateurs système et aux développeurs PowerShell un moyen pratique et familier d'utiliser l'API d'IT Glue pour créer des scripts de documentation, des automatisations et des intégrations.

## Nomenclature des fonctions

IT Glue dispose d'une API REST qui utilise des actions GET, POST, PATCH et DELETE communes à HTTP(s). Afin de respecter les meilleures pratiques de PowerShell, seuls les verbes approuvés sont utilisés. Ainsi, la correspondance suivante doit être utilisée :

- GET    -> Get-
- POST   -> New-
- PATCH  -> Set-
- DELETE -> Remove-

De plus, la nomenclature `verb-noun` de PowerShell est respectée. Chaque nom est préfixé par `ITGlue` afin d'éviter tout problème de nommage.

Par exemple, on peut accéder au point d'extrémité API /users/ en exécutant la commande PowerShell suivante avec les paramètres appropriés :

```powershell
Get-ITGlueUsers
```

## Installation

1. Téléchargez directement le module depuis [ce lien](https://github.com/nonBinaryGeek/powershellwrapper/archive/refs/heads/master.zip).

2. Extraire le fichier `*.zip` sous votre répertoire destiné aux modules PowerShell. Le répertoire par défaut ce trouve sous `C:\Program Files\WindowsPowerShell\Modules`. Pour identifié le chemin des autres répertoires, utilisez la commande PowerShell suivante:

```powershell
$env:PSModulePath
```

3. Finalement, chargez le module dans votre session PowerShell:

```powershell
Import-Module ITGlueAPI
```

## Configuration initiale

La première fois que vous utilisez ce module, vous devrez configurer l'URI de base et la clé API utilisées pour communiquer avec IT Glue. Pour ce faire, procédez comme suit :

1. Exécutez `Add-ITGlueBaseURI`. Par défaut, l'URI **api.itglue.com** d'IT Glue est entrée. Si vous avez votre propre passerelle ou proxy API, vous pouvez entrer votre propre URI personnalisée en spécifiant le paramètre `-base_uri`, comme suit : 

```powershell
Add-ITGlueBaseURI -base_uri http://myapi.gateway.example.com
```

2. Exécutez `Add-ITGlueAPIKey`. Une clé API vous sera demandée. (*veuillez vous référer à la [documentation d'IT Glue](https://api.itglue.com/developer/) pour générer une clé API*).

3. (OPTIONNEL) - Si vous souhaitez que le module IT Glue se souvienne de votre URI de base et de votre clé API, vous pouvez exécuter `Export-ITGlueModuleSettings`. Cela créera un fichier de configuration dans `%UserProfile%\ITGlueAPI` qui contiendra en toute sécurité ces informations. La prochaine fois que vous exécuterez `Import-Module`, cette configuration sera automatiquement chargée.

:warning: L'exportation des paramètres de module crypte votre clé API dans un format qui ne peut être déchiffré qu'avec votre compte Windows. Elle utilise le type `System.Security.SecureString` de PowerShell, qui utilise un chiffrement réversible lié à votre principal utilisateur. Cela signifie que vous ne pouvez pas copier votre fichier de configuration sur un autre ordinateur ou un autre compte utilisateur et vous attendre à ce qu'il fonctionne.

:warning: L'exportation et l'importation des paramètres de module nécessitent l'utilisation de la cmdlet `ConvertTo-SecureString`, qui n'est actuellement pas disponible dans les ports PowerShell Core de Linux et Mac. Jusqu'à ce que PS Core 6.0.0 soit disponible, cette fonctionnalité ne fonctionne que sur Windows.

## Utilisation

| API Resource             | Create                              | Read                                | Update                              | Delete                                 |
| ------------------------ | ----------------------------------- | ----------------------------------- | ----------------------------------- | -------------------------------------- |
| Attachments              | `New-ITGlueAttachments`             | -                                   | `Set-ITGlueAttachments`             | `Remove-ITGlueAttachments`             |
| Configuration Interfaces | `New-ITGlueConfigurationInterfaces` | `Get-ITGlueConfigurationInterfaces` | `Set-ITGlueConfigurationInterfaces` | `Remove-ITGlueConfigurationInterfaces` |                                    |
| Configuration Statuses   | `New-ITGlueConfigurationStatuses`   | `Get-ITGlueConfigurationStatuses`   | `Set-ITGlueConfigurationStatuses`   | -                                      |
| Configuration Types      | `New-ITGlueConfigurationTypes`      | `Get-ITGlueConfigurationTypes`      | `Set-ITGlueConfigurationTypes`      | -                                      |
| Configurations           | `New-ITGlueConfigurations`          | `Get-ITGlueConfigurations`          | `Set-ITGlueConfigurations`          | `Remove-ITGlueConfigurations`          |
| Contact Types            | `New-ITGlueContactTypes`            | `Get-ITGlueContactTypes`            | `Set-ITGlueContactTypes`            | -                                      |
| Contacts                 | `New-ITGlueContacts`                | `Get-ITGlueContacts`                | `Set-ITGlueContacts`                | `Remove-ITGlueContacts`                |
| Countries                | -                                   | `Get-ITGlueCountries`               | -                                   | -                                      |
| Documents                | -                                   | -                                   | `Set-ITGlueDocuments`               | -                                      |
| Domains                  | -                                   | `Get-ITGlueDomains`                 | -                                   | -                                      |
| Expirations              | -                                   | `Get-ITGlueExpirations`             | -                                   | -                                      |
| Flexible Asset Fields    | `New-ITGlueFlexibleAssetFields`     | `Get-ITGlueFlexibleAssetFields`     | `Set-ITGlueFlexibleAssetFields`     | `Remove-ITGlueFlexibleAssetFields`     |
| Flexible Asset Types     | `New-ITGlueFlexibleAssetTypes`      | `Get-ITGlueFlexibleAssetTypes`      | `Set-ITGlueFlexibleAssetTypes`      | -                                      |
| Flexible Assets          | `New-ITGlueFlexibleAssets`          | `Get-ITGlueFlexibleAssets`          | `Set-ITGlueFlexibleAssets`          | `Remove-ITGlueFlexibleAssets`          |
| Groups                   | -                                   | `Get-ITGlueGroups`                  | -                                   | -                                      |
| Locations                | `New-ITGlueLocations`               | `Get-ITGlueLocations`               | `Set-ITGlueLocations`               | `Remove-ITGlueLocations`               |
| Manufacturers            | `New-ITGlueManufacturers`           | `Get-ITGlueManufacturers`           | `Set-ITGlueManufacturers`           | -                                      |
| Models                   | `New-ITGlueModels`                  | `Get-ITGlueModels`                  | `Set-ITGlueModels`                  | -                                      |
| Operating Systems        | -                                   | `Get-ITGlueOperatingSystems`        | -                                   | -                                      |
| Organization Statuses    | `New-ITGlueOrganizationStatuses`    | `Get-ITGlueOrganizationStatuses`    | `Set-ITGlueOrganizationStatuses`    | -                                      |
| Organization Types       | `New-ITGlueOrganizationTypes`       | `Get-ITGlueOrganizationTypes`       | `Set-ITGlueOrganizationTypes`       | -                                      |
| Organizations            | `New-ITGlueOrganizations`           | `Get-ITGlueOrganizations`           | `Set-ITGlueOrganizations`           | `Remove-ITGlueOrganizations`           |
| Password Categories      | `New-ITGluePasswordCategories`      | `Get-ITGluePasswordCategories`      | `Set-ITGluePasswordCategories`      | -                                      |
| Passwords                | `New-ITGluePasswords`               | `Get-ITGluePasswords`               | `Set-ITGluePasswords`               | `Remove-ITGluePasswords`               |
| Platforms                | -                                   | `Get-ITGluePlatforms`               | -                                   | -                                      |
| Regions                  | -                                   | `Get-ITGlueRegions`                 | -                                   | -                                      |
| Related Items            | `New-ITGlueRelatedItems`            | -                                   | `Set-ITGlueRelatedItems`            | `Remove-ITGlueRelatedItems`            |
| User Metrics             | -                                   | `Get-ITGlueUserMetrics`             | -                                   | -                                      |
| Users                    | -                                   | `Get-ITGlueUsers`                   | `Set-ITGlueUsers`                   | -                                      |

Chaque fonction `Get-` répondra avec les données brutes fournies par l'API d'IT Glue. Habituellement, ces données ont au moins trois sous-sections :

- `data` - Les informations réellement demandées (ce qui intéresse la plupart des gens)
- `links` - Des liens vers des aspects spécifiques des données
- `meta` - Des informations sur le nombre de pages de résultats disponibles et d'autres métadonnées.

Chaque ressource permet l'utilisation de filtres et de paramètres pour spécifier la sortie désirée de l'API d'IT Glue. Consultez le [wiki sur l'utilisation des filtres et des paramètres](https://github.com/itglue/powershellwrapper/wiki/Using-Filters-and-Parameters).

Une liste complète des fonctions peut être obtenue en exécutant `Get-Command -Module ITGlueAPI`. Les informations d'aide et la liste des paramètres peuvent être trouvées en exécutant `Get-Help <nom de la commande>`.

```powershell
Get-Help Get-ITGlueUsers
```
