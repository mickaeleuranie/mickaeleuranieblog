---
layout: post
title:  "Forum PHP 2014 - Premier jour"
date:   2014-10-25 13:18:09
categories: PHP ForumPHP
summary: Résumé du Forum PHP du 23 au 24 octobre 2014
comments: True
---
# ForumPHP 2014
Premier jour – 23/10/2014


Introduction

1. Tests
2. Clean code
3. Automatisation
    1. Exemple de Lexpress
    2. Exemple de Diffbot
    3. Conclusion
4. API
    1. Exemple de KlubUp
    2. Exemple de ARTE
    3. Exemple de Lexpress
    4. Conclusion
5. Logs
    1. Exemple de BlaBlaCar
    2. Exemple de KlubUp

6. Applications/outils cités (et j’en oublie)
7. Liens utiles


## Introduction
Le jeudi 23 octobre 2014 avait lieu le premier jour du Forum PHP 2014 se déroulant au béffroi de Montrouge. Pour plus d’informations, suivez le hashta… Pardon, mot-dièse `forumphp` sur Twitter ou suivez moi : [@mickaeleuranie](http://twitter.com/mickaeleuranie).
Le thème cette année est _Du concept à la production : PHP premier à l'arrivée_ avec des conférences s'orientant pour la plupart autour des sujets suivants :

* Retour d'expérience sur l'utilisation de PHP à grande vitesse
* Gagner du temps avec l'écosystème open source PHP
* Agile et DevOps PHP
* Les solutions du développeur PHP rapide

Je ne vais pas vous faire un résumé conférence par conférence de cette journée riche en informations ni ne vais faire la présentation en détail des différentes applications ou différents logiciels cités (Google est là pour ça) mais plutôt tenter de vous retranscrire les idées et applications qui en sont resorties à travers les différents exemples.

Tel un nuage de mots (ou citations) voici ceux qui, de mon point de vue, résument bien la journée :

* Tests
* Bugs
* DevOps
* API
* Microservices
* Automatiser
* Help yourself
* Un bon développeur est un développeur fainéant
* AWS
* Symfony
* Logs
* Monitoring
* Throttling

Pour plus de détails concernant la seconde journée, allez jeter un oeil à l'article de Julien Benoit ( [@_JulienBenoit](http://twitter.com/_JulienBenoit) ) : [http://blog.julienbenoit.net/2014/11/10/forum-php-2014.html](http://blog.julienbenoit.net/2014/11/10/forum-php-2014.html).

## 1 Tests

S’il est un point commun dans ces conférences, ce sont bien les tests. Que ce soit lors de la mise en place d’automatisation ou simplement pour le développement de son application, ils sont nécessaires.
Je n’ai pas eu la chance d’aller à celle parlant spécifiquement de ça mais certaines informations utiles ont été présentées.

Sebastian Bergmann nous a donné un aperçu de `PHPUnit` dont il est le créateur pendant sa conférence _A state of mind_ dans laquelle nous avons pu voir trois différentes façon de visualiser le retour des tests via son outil. Il a également parlé de l’utilisation de générateurs de code, qui facilitent les tests: le code généré est également … Testé (dans l’exemple qu’il a donné, avec `Symfony`, qui a transformé 20 lignes de configuration `YAML` en plusieurs miliers de lignes `PHP` propres et testées).

Des tests `JavaScript` ont été énoncés par François Dume chez ARTE. Ils n’y utilisent pas de framework de tests unitaires mais utilisent des tests fonctionnels avec `casper-js` et `frisby.js`.

Kitchen, cité par Nicolas Silberman et Sebastian Angele, permet de faire des tests unitaires et fonctionnels sur leurs recettes. Il est compatibles avec beaucoup de provisionner dont Puppet et Chef et bootstraper (Amazon EC2).

Les tests sont au coeur des développements car ce que l’on veut tous éviter est le bug en production.

## 2 Clean code

Comme l’a très justement dit Sebastian Bergmann ( [@s_bergmann](http://twitter.com/s_bergmann) ) lors de son talk "A state of mind", il est très facile d’écrire du code compréhensible par une machine, mais c’est beaucoup plus difficile d’écrire un code compréhensible par vos collègues.

Le développement d’applications de plus en plus complexes implique d’autres contraintes que l’automatisation. Il faut également penser à la maintenabilité de son code. Lorsqu’un bug survient, il faut pouvoir identifier rapidement sa provenance pour le corriger au plus vite. Son code doit également pouvoir être compris par ses collègues au cas où l’on ne serait pas/plus là ou sur autre chose lorsqu’il survient. Car un bug surviendra quoi qu’il arrive comme on nous l’a souvent rappelé.

Le respect d’une norme, par exemple `PSR-3`, dans toute son application est un premier pas vers un code propre.

Pour présenter une méthode propre d’écriture de code, Sebastian a utilisé l’exemple d’une porte, qui peut avoir 3 état et à laquelle on ne peut faire que certaines actions en fonction de son état :

* Opened
    * peut être fermée
    * ne peut pas être ouverte
    * ne peut pas être fermée à clé
    * ne peut pas être unlock
* Closed
    * ne peut pas être fermée
    * peut être ouverte
    * peut être fermée à clé
    * ne peut pas être unlock
* Locked
    * ne peut pas être fermée
    * ne peut pas être ouverte
    * ne peut pas être fermée à clé
    * peut être unlock

L’utilisation d’une interface, d’une classe abstraite et d’exceptions permettent de rapidement identifier les erreurs d’utilisation à l’aide des exceptions. Je n’entre pas dans les détails, le code est dans ses slides (voir les liens plus bas).

Une bonne maintenance passe également par une bonne représentation et documentation qui peuvent permettre de connaître l’utilisation et les liens entre différentes fonctions.

Représentations :

* XML
* Schémas
* Code
* Liste (cf juste au dessus)
* PHPUnit result
* …

Visualisation (Pour documentation) : `Graphthis`

La gestion des erreurs et leur localisation est grandement aidée par les logs.

## 3 Automatisation

L'automatisation du déploiement, des tests, de la création des différents environnements, de la récupération/du tris des logs (cf partie 1.4 Logs) est également un des mots d'ordre de cette journée. Le but est de gagner du temps, que le développeur passe plus de temps à … Développer qu’à construire ses environnements de dev.
L'exemple donné lors du talk _Industrialisation des environnements de dev avec `Puppet` et Amazon_ permet de s'en rendre compte.

### 3.1 Exemple de Lexpress


Le retour d'expérience de Nicolas Silberman ( [@nsilberman](http://twitter.com/nsilberman) ) et Sebastien Angele ( [@sangele](http://twitter.com/sangele) ) est un exemple parmi d’autre de la nécessité, au bout d’un moment dans la vie d’une application, de mettre en place des automatisations.

Les chiffres qu’ils ont donné parlent d'eux-mêmes :

* 2005
    * 1 application
    * 1 base de donnée
    * 1 serveur
* 2014
    * 6 applications `PHP`
    * 15 bdds métier sous `MySql`, `MongoDb` et `SqlServer`
    * `Redis`
    * `RabbitMQ`

Ce qu’ils ont essayé et qui n’a pas fonctionné :

* Reproduire un bout de l’environnement : trop de dépendances
* Le faire à la main : long et toujours quelque chose qui manque
* Espérer que ça fonctionne en prod

Leur objectif étant de reconstruire la prod entière pour avoir le même environnement pour pratiquer leurs tests, ils sont passés par plusieurs étapes.

* Gestion du middleware
    * Applicatifs
    * Bdds
    * Fichiers (images, pdf, …)
* Industrialisation du code : `Git`
* Expérience du déploiement en continue : `Capistrano`
* Compétences en `DevOps`

Pour la configuration, leur choix s’est d’abord porté sur `Puppet` (`Ruby`), puis `Chef` (`Erlang`).

**Pourquoi avoir choisi `Puppet` ?**

* Précédentes expériences sur des stacks plus petites dans l’équipe
* Expérience significative de Nicolas Siberman chez Mediapart sur les problématiques d’environnement de dev
* Volonté de rapidement démarrer le projet

**Retour d’expérience sur Puppet :**

* Pour démarrer, utiliser Foreman directement
* Attention : configuration de type hiérarchie (fichiers `YAML`, …) non compatibles avec Foreman
* Complexité de l’arbre des dépendances
* Il n’est pas rare de se poser la question "Cette action doit-elle être réalisée avant celle là ?"
* Exemple de dépendances :
    * install_apache
    * Créer un `Vhost`
    * restart_apache
    * `Puppet` compile l’ensemble des recettes, réalise un arbre de dépendance et joue les modifications à faire sur le système dans l’ordre choisi
    * A partir d’une 20 aine de niveau de dépendance, ça devient compliqué

**Pourquoi être passé à Chef ?**

* Tutoriaux de la communauté et documentation fournie par `AWS` faisant uniquement mention de Chef (même si `Puppet` arrive doucement sur `AWS`)
* Choix de Chef par l’hébergeur de la prod
* Recettes plus simples à écrire

**Recettes middleware :**

* Au début: images de base (AMI) des distributions linux
* Maintenance à l’aide de `Packer.io` pour construire des images avec :
    * `nginx` compilé (gestion des ssi)
    * autres briques : `MySql`, `Redis`, `MemCache`, `RabbitMQ`, `Apache`, `PHP`

**Recettes applicatives :**

* Tableau de variable pour :
    * Créer des Vhost nginx
    * Cloner le projet
    * Exécuter `composer install`
    * ....

**Gestion des bdds :**

* A remonter de la prod tous les jours :
    * 10 bases de données `MySql`
    * 5 bases `MongoDb`
    * 1 dump Redis
    * 1 base Sql Server
* 30h pour tout remonter, ce qui pose problème ! Solutions proposées :
        * Arbitrer la fraicheur des données (quelles sont les données qu’il faut absolument récupérer ?)
        * Optimiser les bases de données (split entre les données dynamiques et les données statiques)

**Gestion des fichiers :**

* Pas de solutions trouvées pour le moment
* `Rsync`

**Kitchen :**

* Permet de faire des tests unitaires et fonctionnels sur les recettes
* Compatible avec beaucoup de provisionner (`Puppet`, `Chef`), bootstraper (Amazon EC2)
* Concept de l’_infrastructure as code_

**Pilotage :**

* Faire le moins de choses à la main
* Gérer son parc
* Ne pas être `DevOps` dépendant

Une des remarques les plus importantes qui est ressortie des questions est que les développeurs ne peuvent pas, dans cet exemple, développer sur leur machine en local. Il leur faut toutes les données à disposition pour ne pas avoir de bug. Ils travaillent sur des instances.


## 3.2 Exemple de Diffbot

Un autre exemple d’automatisation a été donné lors de la conférence _Automate your Workflow_ par Bruno Skvorc ( [@bitfall](http://twitter.com/bitfall) ) avec la présentation de `Diffbot`, un robot permettant l’extraction de données de pages web et qui apprend de ses erreurs. A partir de ces données, libre à chacun d’en faire ce dont il a besoin.
Après une obligatoire configuration, le robot s’occupe de récupérer les informations voulues sur un site donné, de les afficher et beaucoup plus encore. Je vous encourage à aller voir le site [https://www.diffbot.com/](https://www.diffbot.com/) qui présente le produit.
Le mot d’ordre de cette conférence était _Help yourself_, s’aider de systèmes automatisés pour se simplifier la vie.


### 3.3 Conclusion

L’investissement pour la mise en place d’un système automatisé est lourd au début mais fini toujours par être payant. Il est conseillé de se mettre en mode projet avec une équipe dédié, de travailler en agile et lean et surtout de finir à 100% son projet (et non pas en commencer plusieurs qu’on ne fini jamais).



## 4. API

L’utilisation d’`APIs` / `microservices` a été particulièrement cité aujourd’hui, avec des exemples chez Arte, KlubUp et Lexpress.
Pourquoi choisir de mettre en place des `microservices` et quels sont les points à prendre en compte lors de leur mise en place / leur utilisation ? Cette réponse a bien été apportée par la conférence _Architecture d'une application full API orienté `microservice`_ à travers l'exemple du site KlubUp.

### 4.1 Exemple de KlubUp

D’après Xavier Gorse ( [@xgorse](http://twitter.com/xgorse) ) et Yves Heitz qui présentaient ce talk, l’avènement du mobile les a incité à passer d’une application monolithique composée de 60 bundle à une application basée sur des microservice. L’utilisation de microservices permet de rendre son application compatible avec n’importe quelle plateforme.
Concernant le choix de la technologie il ont pris en compte plusieurs critères :

* Les compétences
* La productivité
* La performance
* L'évolution de la technologie

Ils ont également tenu compte d’autres facteurs.

**La communication :**

* HTTP (universel)
* API REST JSON (universel, simple)
* Consommation directe et encapsulée via un service

**La gestion des erreurs**

* Accepter l’idée qu’à un moment donné il va y avoir des erreurs (par exemple avec HTTP : dns, https, connexion, …)
* La criticité : erreur impactante ? Peut-elle être ignorée ?
* Front : ne pas bloquer l’application s’il y a des briques qui buggent
* Back : un peu pareil, sauf qu’il ne faut pas de timeout. Si une erreur survient ils la catchent, puis essayent de la répéter un peu plus tard

**La sécurité**

* `oAuth 2`
* `Hawk`
* Réseaux privés
* `HTTPS` : penser à le mettre tout de suite, pour le front et le back, pour éviter les problèmes lors d’une mise en place tardive

**Le transactionnel**

* Pas de gestion de transaction cross services, uniquement au sein d’un service
* Accepter l’erreur pour éviter les transactions
* Le monitoring/reporting
* Uniquement basé sur les logs avec Logentries
* `RequestId` pour regrouper les logs
* `UserAgent` des applications et des services

**La scalabilité**

* Isolation des services
* Séparation des API en lecture/écriture
* Concurrency des `workers`
* Scalabilité humaine : 1 team / 1 service

**La performance**

* Rendre la main le plus vite possible au client qui a fait la requête (au front)
* Limiter les rebonds entre les services
* Prendre en compte la latence `HTTP`
* Faire des traitements en asynchrone

**Traitements asynchrones**

* `Iron.io` SAAS : `IronMQ` & `IronWorker`
* Callback `HTTP`

**Le cache**

Dans leur cas, ils ont fait le choix de ne pas utiliser de cache au niveau application et de séparer la partie publique de la partie privée :
* public (/public/*) : cache HTTP via Varnish
* private (/private/*) : pas de cache pour le moment

**Infrastructure**

* Provisionning avec Chef `OpsCode`
* VM via `OpenVz`
* Automatisée : obligatoire pour ce type d’architecture

**Evolutions à venir**

* Mise en place de Docker
* Choix en fonction des résultats suite à de fortes charges

### 4.2 Exemple de ARTE

Le retour d’expérience de François Dume ( [@_franek_](http://twitter.com/_franek_) ) chez ARTE était très instructif. L’objectif de l’utilisation d’API pour ARTE est différente de KlubUp :

* Mettre à disposition tout le contenu d’ARTE (aller vers de l’Open data ? La question est posée)
* Avoir un suivi de l’usage (throttling) : autoriser par exemple un simple utilisateur à faire 1000 requêtes/heure mais un partenaire avec un contrat premium pourrait en faire plus

Pour comparer avec KlubUp, voici quelques informations sur leur utilisation de `microservices`.

**Architecture technique**

* `Symfony 2` / `MongoDB`
* `RabbitMQ` : Synchronisation des données via des messages asynchrones
* Utilisation du standard `{json:api}`
* Découplage en composants autonomes (authentification, open program `API` (OPA), générateur de player ARTE (iframe/oEmbed), statistiques

**Sécurisation des API et throttling**

* Authentification via `oAuth 2.0`
* Reverse proxy identifiant (Openresty, distribution nginx avec support de Lua)
=> toutes les API "sécurisées" sont protégées par ce reverse proxy
* Développement de scripts Lua chargés dans la conf nginx permettant de valider le token oAuth
* Reverse proxy authentifiant également en charge du throttling (exemple : 1000 requêtes/min)

**Standard {json:api}**

Ce standard est utilisé pour construire une API. Il décrit la manière d’inclure des sous-documents (réduction du nombre de requêtes client/server) et comment limiter les attributs retournés. Il permet donc également la création de requêtes complexes.
Son implémentation dans Symfony 2 a été effectuée en plusieurs étapes :
* Surcharge du bundle BazingaHateoasBundle
* Gestion des inclusions avec l’utilisation d’un serialize.post_serialize
* Limitation des attributs retournés via la mise en place d’une classe ExclusionStrategy développée par [@damienalexandre](http://twitter.com/damienalexandre) de JoliCode

**Problèmes rencontrés : solutions**

* Performances du JMSSerialize : création d’un bundle permettant de mettre en cache les "visitors"
* Performances de BazingaHateoasBundle : création d’un bundle permettant de mettre en cache le mécanisme de sérialisation

**Evolutions à venir**

* Monitoring de l’usage : script Lua pour envoyer  des métriques à StatsD
* Mise à disposition de `SDK` pour faciliter l’utilisation de l’`API` par des partenaires externes (travail en cours : module `Drupal`)
* Ouvrir le code du serveur de l’`API` (en prenant l’exemple de The Guardian)
* Ouvrir l’API à des dev externes (Open Data ?)
* Intégration d’une stratégie d’invalidation du cache `Varnish` (bundle `FOSHttpCacheBundle`)
* `HHVM`


### 4.3 Exemple de Lexpress

Nicolas Silberman ( [@nsilberman](http://twitter.com/nsilberman) ) et Sébastien Angele ( [@sangele](http://twitter.com/sangele) ) ont évoqué, lors de leur Talk _Industrialisation des environnements de développement avec `Puppet` et `Amazon`_, l’utilisation de l’`API` d’Amazon. Les critères ayant déterminés leur choix sont :

* La (très bonne) documentation
* La disponibilité et le temps de réponse
* Le `SDK` fourni pour `PHP` : se base sur `Guzzle` pour envoyer les requêtes et clients distincts pour chaque service
* La facile intégration du `SDK`
Mais il y a quelques points négatifs tout de même :
* Beaucoup (trop) de doc à parcourir
* il est nécessaire de savoir précisément ce que l’on souhaite faire pour ne pas prendre une mauvaise direction.

Une question leur a été posé sur le choix. Pourquoi Amazon et non pas Google ? Leur réponse a été simple : parce que l’offre de Google est moins riche et que Amazon est le leader dans le domaine.


### 4.4 Conclusion

Ce que l’on peut conclure sur l’utilisation d’`API` est qu’il faut avoir une approche orientée composant, métier et produit. Il faut décentraliser la gouvernance (chaque service est plus ou moins autonome) et les données (coût non négligeable mais atouts importants).
Il faut également mettre en place une infrastructure automatisée, ce qui devient obligatoire pour ce type d’architecture, sans oublier de gérer les erreurs.
Ce qui remonte également est qu'il ne faut pas craindre de faire des erreurs. Il ne faut surtout pas commencer un développement en s'estimant trop doué pour créer des bugs. Ce sont les bugs qui permettent d'améliorer son approche des problèmes et leur gestion.

## 5. Logs

Les logs permettent de connaître les détails d’une erreur qui survient, mais également de simplement savoir, par exemple, toutes les actions effectuées par un utilisateur. Pour ça il existe plusieurs niveaux de logs qui peuvent être récupérés et décryptés pour simplifier le debug, avoir des statistiques, …
La première idée qui nous vient à l’esprit pour accéder aux logs est de se connecter au serveur en question en `SSH`, mais il existe des solutions beaucoup plus lisibles et automatisées pour faire la même chose.
Nous avons eu le droit à plusieurs exemples d’utilisation des logs en ce premier jour.

Olivier Dolbeau ( [@odolbeau](http://twitter.com/odolbeau) ) a présenté la gestion des logs faite chez BlaBlaCar et quelques outils utilisés pour ça.

### 5.1 Exemple de BlaBlaCar

Chez BlaBlaCar, l’utilisation de la suite EKL associé à Monolog semble bien fonctionner, et il leur l’a prouvé. `EKL` ? C’est :

* `ElasticSearch`
* `Kibana`
* `Logstash`

Cette suite fonctionne de la façon suivante :

* `Monolog` s’occupe d’envoyer les logs à `LogStash`
* `Logstash` les renvoi dans le backend de stockage `ElasticSearch`
* `Kibana` interroge `ElasticSearch` pour récupérer les données et les afficher

Avec cette solution, les développeurs peuvent en quelques clics lister les erreurs, les filtrer, les identifier et, du coup, les corriger rapidement. De fait, ils ne gardent les logs que 15/20 jours, les bugs doivent donc être corrigés dans ce laps de temps.

Outil de visualisation utilisé : `Newrelic Dashboard`.

### 5.2 Exemple de KlubUp

Un autre exemple d’utilisation des logs a été rapidement énoncé lors de la présentation de KlubUp et la mise en place des `APIs`.
Les logs sont utilisés pour servir de monitoring/reporting, à l’aide de `Logsentries` et `RequestId`.

Pour éviter au maximum les logs d’erreur, il est nécessaire d’effectuer des tests en amont.

## 6. Applications/outils cités (et j’en oublie)

* Symfony
* PHPUnit
* Graphthis
* MySql
* MongoDB
* Sql Server
* Redis
* RabbitMQ
* Instance EC2 chez Amazon
* Cloud Formation
* Amazon
* AWS
* Puppet
* Chef
* Foreman
* Packer.io
* Nginx
* Apache
* PHP
* CloudFormation
* Kitchen
* PostgreSql
* ElasticSearch
* Kibana
* Logstash
* Monolog
* Heka
* syslog
* Newrelic Dashboard
* Diffbot
* Guzzle
* C3 (c3.js)
* D3 (d3.js)
* HTTP
* REST
* JSON
* oAuth 2
* Logsentries
* RequestId
* NodeJs
* Iron.io
* IronMQ
* IronWorker
* Varnish
* OpenVz
* Docker
* {json:api}
* casper-js
* frisby.js
* noxlogic/ratelimit-bundle
* JMSSerializer
* BazingaHateoarsBundle
* Drupal
* Ruby
* Go
* Java
* Vagrant
* Newrelic
* Jira

## 7. Liens utiles

Liste des conférences : [http://joind.in/event/view/2091](http://joind.in/event/view/2091)

**A state of mind :**
[http://github.com/sebastianbergmann/state](http://github.com/sebastianbergmann/state)
[http://thephp.cc/dates](http://thephp.cc/dateshttp://thephp.cc/dates)
[sebastian@thePGP.cc](sebastian@thePGP.cc)
Sebastian Bergmann : [http://twitter.com/@s_bergmann](http://twitter.com/@s_bergmann)

**Industrialisation des environnements de dev avec Puppet et Amazon :**
Nicolas Silberman : [http://twitter.com/@nsilberman](http://twitter.com/@nsilberman)
Sebastien Angele : [http://twitter.com/@sangele](http://twitter.com/@sangele)

**Laisse pas trainer ton log :**
Olivier Dolbeau : [http://twitter.com/@odolbeau](http://twitter.com/@odolbeau)
[http://joind.in/11942](http://joind.in/11942)
[http://speakerdeck.com/odolbeau/laisse-pas-trainer-ton-log](http://speakerdeck.com/odolbeau/laisse-pas-trainer-ton-log)

**Automate your workflow : Removing tedium in everyday work :**
Bruno Skvorc : [http://twitter.com/@bitfalls](http://twitter.com/@bitfalls)
[bruno.skvorc@sitepoint.com](bruno.skvorc@sitepoint.com)
[http://twitter.com/@diffbot](http://twitter.com/@diffbot)

**Architecture d’une application full API orientée micro service :**
Xavier Gorse : [http://twitter.com/@xgorse](http://twitter.com/@xgorse)
Yves Heitz :

**Retour d’expérience ARTE EGIE : développement API :**
François Dume : [http://twitter.com/@_franek_](http://twitter.com/@_franek_)
[http://joind.in/11962](http://joind.in/11962)

**Résumé de la seconde journée :**
[http://blog.julienbenoit.net/2014/11/10/forum-php-2014.html](http://blog.julienbenoit.net/2014/11/10/forum-php-2014.html) par Julien Benoit ( [@_JulienBenoit](http://twitter.com/_JulienBenoit) )
