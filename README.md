# ![Logo du portfolio](https://raw.githubusercontent.com/thierry-laval/archives/master/images/logo-portfolio.png)

## Auteur

👤 &nbsp; **Thierry LAVAL** [🇫🇷 Contactez moi 🇬🇧](mailto:contact@thierrylaval.dev)

- GitHub: [@Thierry Laval](https://github.com/thierry-laval)
- LinkedIn: [@Thierry Laval](https://www.linkedin.com/in/thierry-laval)
- Visitez ==> 🏠 [Site Web](https://thierrylaval.dev)

## 📎 Projet Fork - PrestaShift — Module de migration PrestaShop

<img src="prestashift/logo.png" alt="Logo du portfolio" width="100">

_`Début du projet le 12/04/2021`_ - _`Fork créé le 17/09/2026`_

> Fork basé sur le travail de [marcingajewski.pl](https://marcingajewski.pl) / [GajewskiMarcin/prestashift](https://github.com/GajewskiMarcin/prestashift).

Ce fork est une adaptation locale du projet original. Il peut inclure des correctifs, des compatibilités spécifiques ou des modifications destinées à un environnement de production particulier. Il sert de base de travail personnelle et de maintenance continue, sans remplacer le dépôt upstream officiel.

Outil professionnel de migration de données pour PrestaShop. Migrez les données de votre boutique entre PrestaShop 1.7, 8 et 9 en toute simplicité.

### ✨ Description du projet

Ce dépôt reprend le module PrestaShift et l’adapte selon les besoins de son environnement de développement et de mise en production. Les modifications peuvent évoluer au fil du temps en fonction des cas d’usage rencontrés, des compatibilités PrestaShop et des corrections nécessaires sur des versions spécifiques.

Le module permet notamment de migrer les produits, catégories, clients, commandes, images, transporteurs, CMS et bien plus encore, avec un système de vérification et de contrôle avant l’exécution de la migration.

Il est conçu pour travailler en deux modes : via un connecteur bridge ou par connexion directe à la base de données. Les migrations peuvent être traitées par lots, avec synchronisation incrémentale, vérification des versions source/cible, validations PHP/MySQL, gestion des redirections, mappage des statuts et génération de journaux détaillés.

### 🚀 Fonctionnalités

- **Pipeline de migration en 39 étapes** — produits, catégories, clients, commandes, images, transporteurs, CMS et bien plus encore
- **Deux modes de connexion** — Connecteur Bridge (fonctionne entre serveurs) ou base de données directe (plus rapide, même serveur)
- **Compatible avec les versions** — détection automatique des versions source/cible avec avertissements de compatibilité
- **Transformations PS 1.7 → 8/9** — `redirect_type`, `id_type_redirected` convertis automatiquement
- **Synchronisation incrémentale** — ne migrer que les modifications depuis la dernière synchronisation
- **Traitement par lots** — tailles de lots configurables avec prise en charge de pause/reprise
- **Transfert d’images** — télécharge les images et génère toutes les tailles de vignettes
- **Support multiboutique** — choisissez l’ID de la boutique cible
- **Vérifications avant migration** — valide les limites PHP, l’espace disque, cURL avant le démarrage
- **Tâches post-migration** — reconstruit automatiquement l’index de recherche, l’arbre des catégories, vide le cache
- **Aperçu en mode test** — voir le nombre d’enregistrements avant la migration
- **Journalisation des fichiers** — journaux détaillés dans `var/logs/prestashift.log`
- **Plan de redirection** — génère un fichier de redirection 301 pour préserver le référencement
- **Mappage des statuts** — mappe les statuts de commande entre la source et la cible
- **Configuration sélective** — migre les paramètres sécurisés de la boutique (nom, SEO, livraison, etc.)
- **Multi-langue** — Anglais + Polonais (traductible via le back office PrestaShop)

### 📊 Ce qui est migré

| Zone | Données |
| ------ | --------- |
| **Catalogue** | Produits, catégories, attributs, fonctionnalités, stock, packs, produits virtuels, champs de personnalisation, étiquettes |
| **Tarification** | Prix spécifiques, règles de prix catalogue |
| **Médias** | Images produits (avec vignettes), pièces jointes, logos fabricants |
| **Clients** | Clients, groupes, adresses, listes de souhaits |
| **Commandes** | Commandes, détails de commande, historique, paiements, factures, avoirs, paniers |
| **Marques** | Fabricants, fournisseurs, liens produit-fournisseur |
| **Livraison** | Transporteurs, tranches, zones de livraison, frais |
| **Contenu** | Pages CMS & catégories, méta/SEO, contacts, magasins physiques |
| **Localisation** | Pays, États/régions, zones, devises, langues, règles de taxes |
| **Administration** | Employés, profils, règles de panier (avec conditions), configuration de la boutique |
| **Stock** | Stock disponible, mouvements de stock |

### ⚙️ Installation

#### Sur la boutique CIBLE (nouvelle boutique)

1. Téléversez le dossier `prestashift/` dans `/modules/`
2. Installez via le back office → Modules → "PrestaShift Migration"

#### Sur la boutique SOURCE (ancienne boutique)

1. Téléversez le dossier `psconnector/` dans `/modules/`
2. Installez via le back office → Modules → "PrestaShift Connector"
3. Copiez le jeton sécurisé généré depuis la page de configuration du module

#### Lancer la migration

1. Ouvrez PrestaShift sur la boutique cible
2. Saisissez l’URL de la boutique source + le jeton
3. Sélectionnez la portée des données
4. Configurez les options (taille des lots, nettoyage de la cible, etc.)
5. Lancez la migration

## 🏗️ Architecture

```
Boutique source (PS 1.7/8)          Boutique cible (PS 8/9)
┌─────────────────┐            ┌──────────────────┐
│   PSConnector    │◄──HTTP──► │   PrestaShift     │
│   (pont API      │   Bridge  │   (moteur de      │
│    en lecture)   │           │    migration)     │
└─────────────────┘            └──────────────────┘
```

PSConnector expose un point d’entrée API sécurisé en lecture seule. PrestaShift s’y connecte via HTTP, récupère les données par lots, les transforme pour assurer la compatibilité des versions, puis les importe dans la base de données cible.

Alternative : connexion directe à la base de données (PDO) pour les migrations sur le même serveur — plus rapide, sans pont nécessaire.

## 🔐 Sécurité

- Authentification basée sur un jeton (64 caractères hexadécimaux)
- Connecteur en lecture seule — les opérations d’écriture sont bloquées
- Accès aux fichiers limité aux répertoires `img/`, `download/`, `upload/`
- Protection contre les traversées de chemin avec validation via `realpath()`
- Comparaison de jetons sécurisée contre les attaques temporelles (`hash_equals`)

## 📌 Rapports d’erreurs (optionnels)

Le module peut envoyer un rapport d’échec par e-mail à l’auteur afin de diagnostiquer les problèmes de migration.
**Il est désactivé par défaut** — vous l’activez avec le commutateur *Envoyer les rapports d’erreur au développeur* dans l’étape Options.

Quand il est activé, une migration en échec envoie : le message d’erreur et la charge utile de débogage, les versions de PrestaShop et PHP, `memory_limit` / `max_execution_time`, l’URL de la boutique source et l’adresse e-mail de l’employé connecté. Rien n’est envoyé lorsque le commutateur est désactivé, et aucune donnée client ou commande n’est jamais transmise. Voir [TelemetryService.php](prestashift/src/Service/TelemetryService.php) — c’est le seul chemin de code qui envoie quoi que ce soit à l’auteur. L’autre connexion sortante du module (`ConnectorClient`) ne communique qu’avec l’URL de la boutique source que vous configurez vous-même.

## 🧩 Support du fork

- Signaler les problèmes directement dans ce dépôt GitHub si vous utilisez cette version forkée.
- Suivre les correctifs et les changements spécifiques à cette adaptation.
- Les demandes de support doivent rester centrées sur cette version locale et non sur le dépôt original.

> Si vous utilisez ce fork, veuillez signaler les problèmes directement dans ce dépôt et non dans le dépôt d’origine, sauf indication contraire.

### 📦  &nbsp; Utilisé dans ce projet

| Langages        | et Applications    |
| :-------------: |:--------------:    |
| HTML5           | Visual Studio Code |
| CSS3            | Git/GitHub         |
| Javascript      | PHP + PrestaShop   |

## 📄 Licence

[Academic Free License 3.0 (AFL-3.0)](LICENSE) — la licence standard des modules PrestaShop.

**[⬆ Retour en haut](#auteur)**
