---
layout: post
title:  "Darkmira 2015 - Résumé"
date:   2015-06-22 18:31:21
categories: PHP Zend Darkmira
summary: Résumé de la première édition du Darkmira Tour
comments: True
---

# DARKMIRA 2015

Retour sur cet après-midi ayant eu lieu le 18 juin 2015 à l'ESGI – Ecole Supérieure de Génie Informatique.
Cette première édition était surtout orientée Zend et PHP 7, avec - personnellement - comme gros sujet la présentation du futur Zend Framework 3.
Les conférences étaient au nombre de 6 :

* 60 minutes en continue par Frédéric Dewinne
* Développer des applications sécurisées avec ZF2 par Cyrille Grandval et Julien Charpentier
* Travailler plus efficacement avec Magento par Pierre Fay
* Boostez vos déploiements web avec PostGre et POMM par Grégoire Hubert
* Présentation de la roadmap de ZF3 par Julien Guittard
* Apigility : Créez vos API en quelques clics, également par Julien Guittard

Les sponsors, continuousPHP et FHM Solutions étaient présents.

## Keynote d'ouverture

Comme toute session de conférence, celle-ci a débuté par une brève keynote d'ouverture, qui a permis de nous rendre compte que cette après-midi a intéressé beaucoup de monde au vu du nombre de personnes n'ayant pas pu avoir de place assise. La plupart étaient des étudiants de l'ESGI.

Les deux premières conférences ont ensuite débuté dans des salles différentes, je me suis donc dirigé vers celle concernant la sécurisation de ses application avec ZF2.

---

## Sécuriser mes applications avec ZF2

* Julien Charpentier - [@jcharpentier_](https://twitter.com/jcharpentier_) - jcharpentier@darmira.fr
* Cyrille Grandval - [@CyrilleGranval](https://twitter.com/CyrilleGranval) - cgrandval@darkmira.fr

S'il fallait retenir une phrase sur ces 45 minutes, ce serait celle-ci : **Ne vous fiez à personne**.
Cette conférence a été l'occasion de voir et revoir quelques éléments que Zend Framework 2 propose pour améliorer la sécurité de ses applications, que ce soit côté server ou côté client.

### Composants de sécurité

Voici quelques exemples de composants ZF2 :

* Zend\Authentification
* Zend\Db
* Zend\Capcha
* ...

La sécurité d'une application n'est pas à prendre à la légère. Une petite faille, qu'elle vous paraisse insignifiante ou non, est une faille de trop. Il faut penser à vraiment tout valider et ne pas utiliser telle quelle tout information.
Il ne faut pas penser "*les hackers ne se soucient pas de mon site*". Des PME se sont faites attaquer via de petites applications, dont les failles ont permis de remonter jusqu'à elles.

Solution proposée : Filtrer et valider les données entrantes, encoder les données sortantes.

Voici quelques exemples de sécurisation :

* S'assurer de la provenance des données (Zend\Captcha)
* Filtrer les données utilisateur
* Valider les données utilisateur
* Etablir une stratégie de filtrage (Zend\Input)
* ...

**Problématique : Comment filtrer, valider et échapper les données d'une application ?**

### Données entrantes

Voici quelques exemples de gestion des données entrantes.

#### Zend\Captcha\Image

Le captcha est un test de défi-réponse utilisé en informatique qui s'assure qu'une réponse n'est pas générée par un robot.

#### Zend\Filter

Ceci permet d'appliquer un ou plusieurs traitement sur une donnée.

Un filtre est un transformateur : il retourne une donnée après l'avoir nettoyée, tronquée, cryptée, compressée voire même traduite.
Exemple : Zend\Filter\StripTags nettoie les données en empêchant d'exploiter la faille XSS.

Il est possible de :

* Appeler un filtre de manière statique
* Définir un filtre avec une fonction de callback
* Créer ses filtres personnels

Ces trois possibilités sont valables pour la plupart des composants Zend.

#### Zend\Validator

Le validator valide le fait qu'une donnée reçue en entrée respecte certains critères prédéfinis.
Chaque validateur contient une méthode isValid().
Exemple de validateurs :

* **Zend\Validator\Digits :** vérifie que le format est numérique
* **Zend\Validator\EmailAddress :** vérifie que la donnée est une adresse email

#### Zend\InputFilter

Un filtre d'entrée permet de définir une collection de filtres et de validateurs applicables sur n'importe quel type de données d'entrées (formulaire, paramètres GET, COOKIE, entête HTTP, etc.) dont le nom est connu.

### Données sortantes

Ne faire confiance à aucune donnée entrante c'est bien, mais il faut aussi contrôler les données sortantes.

#### Zend\Escaper

Il permet de transformer une donnée destinée à l'affichage afin que celle-ci ne soit pas interprétable par le navigateur (faille XSS).
Un encodeur contient plusieurs méthodes permettant l'encodage suivant le context d'affichage.

---

Plusieurs failles de sécurité possibles existent. Les plus communes apparaissent dans le top 10 **OWASP** (Open Web Application Security Project).

### Top 10 OWASP 2013

Les 10 failles les plus courantes, sans ordre d'utilisation, sont les suivantes :

* A1 Injection
* A2 Violation de gestion d'authentification et de session
* A3 Cross-Site Scripting (xss) : la plus importante, car attaque les utilisateurs
* A4 Références directes non sécurisées à un objet
* A5 Mauvaise configuration sécurité
* A6 Exposition de données sensibles
* A7 Manque de contrôle  d'accès au niveau fonctionnel
* A8 Falsification de requête intersite (CSRF)
* A9 Utilisation de composants avec des vulnérabilités connues
* A10 Redirections et renvois non validés

Voici, pour 8 d'entre elles, comment s'en prémunir vec ZF2.

#### A1 Injection

**Explication :** Une donnée entrante brute est insérée dans une commande liée à une interprétation.
Parmi les injections les plus connues, il existe les injections SQL mais également bien d'autres : LDAP, Xpath, etc.

**Solution :**

* Valider les données entrantes
* Utiliser des requêtes paramétrées (préparées, procédures stockées)

#### A2 Violation de gestion d'authentification et de session

**Explication :** Il s'agit d'une usurpation d'identité. L'attaquant tente d'obtenir des droits auxquels qu'il ne possède pas.

**Vulnérabilités les plus communes pour ce type d'attaque :**

* Mot de passes utilisateurs incorrectement stockés
* Divination des mots de passe utilisateurs et des identifiants de session
* Permettre l'utilisation des identifiants de session dans l'URL
* Pas de timeout de session
* Pas de renouvellement d'identifiant de session après changement de droits
* Utilisation de connexions sécurisées
* Mauvaise configuration

#### A3 Cross-Site Scripting (XSS)

Cette faille est la plus répandue. Elle implique l'inclusion directe de code script fourni par l'utilisateur dans la réponse au navigateur. Il existe plusieurs type d'attaque par cette faille :

* **XSS réfléchi**, par exemple l'utilisation directe d'une donnée fournie par un champs texte pour une phrase telle que "Bienvenue Mickaël"
* **XSS Stocké**, Stocké en base, par exemple dans un commentaire, qui lance le code à chaque fois qu'il est affiché par un navigateur
* **XSS basé sur DOM**, qui correspond à la modification manuelle par l'attaquant du DOM, par exemple avec les outils de développement présents par défaut sur les navigateurs récents

#### A4 Référence directe non sécurisée à un objet

Cette faille est l'utilisation d'objet interne faisant directement référence à une ressource (fichier, répertoire, enregistrement en base de données, etc.) sans contrôle d'accès spécifique.

**Exemples :**

* Modification d'un id dans l'url pour accéder à une autre ressource
* Mettre /admin après l'url, qui peut fonctionner si seule la page d'index vérifie que l'utilisateur est authentifié
* ...

Afin de bien gérer les droits, il faut vérifier **systématiquement** si un utilisateur a bien accès à un type de ressource défini

Les failles possibles sont la modification du html ou du JS.

#### A5 Mauvaise configuration sécurité

La configuration en terme de sécurité doit être définie, mise en oeuvre et maintenue au jour le jour
Parmi les principes de sécurité basiques, il est possible de :

* Ne pas afficher les messages d'erreurs / stacktrace
* Modifier les logins et mot de passes par défaut des applications utilisées
* Mettre à jour sa version de PHP
* Mettre à jour ses bibliothèques tierces
* Bloquer tous les ports inutilisés par les applications

#### A6 Exposition de données sensibles

Pour se protéger de cette faille, il s'agit de crypter les données sensibles telles que des mots de passe, des données concernant des patients, des cartes bleues, etc. avec des algorithmes forts.

   **ATTENTION :**
    /!\ md5 ou sha1 avec ou sans grain de sel n'est plus suffisamment sécurisé.
    Aujourd'hui, il vaut mieux utiliser d'autres méthodes de cryptographie telles que bcrypt, etc.

#### A8 CSRF

Cette faile consiste à faire envoyer une requête par le navigateur d'un utilisateur, à son insu, pour effectuer une action.
Ce qu'il faut savoir, c'est que les navigateurs envoient automatiquement les informations d'authentification tels que les cookies de session.

Une utilisation connue, qui explique pourquoi il faut toujours vérifier les liens sur lesquels nous cliquons, est par exemple l'exécution d'actions comme le `like` d'une page lors du click sur un lien, qui s'active automatiquement si l'utilisateur est connecté à son compte Facebook sur un autre onglet.

Pour s'en protéger, il est possible par exemple d'inclure un jeton unique dans un champ caché (pour un formulaire), dans une url (pour les liens), etc.

#### A9 Utilisation de composants avec des vulnérabilités connues

Comme précisé en introduction de la présentation de cette conférence, il faut toujours faire attention aux biliothèques tierces, car  elles peuvent avoir des failles.

ZF2 en est un exemple, la liste des failles connues est d'ailleurs accessible à l'url suivante : [http://framework.zend.com/security/advisories](http://framework.zend.com/security/advisories)

Retenons cette phrase de Billy Wilder citée par Julien Charpentier concernant cette faille : "*Il vaut mieux que les erreurs soient les vôtres plutôt que celles de quelqu'un d'autre*".

----

Suite à cette conférence, j'ai eu le choix entre rester à ma place pour assister à la conférence "Travailler plus efficacement avec Magento 2" ou "Boostez vos développements web avec PostGre et POMM". Sachant que la première allait tourner autour de l'utilisation de Docker, je suis rester pour y assister.

## Travailler plus efficacement avec Magento 2

* Pierre Fay - [@pierrefay](https://twitter.com/pierrefay) - [github.com/pierrefay/magento2-azure-demo](github.com/pierrefay/magento2-azure-demo)

Conférence orientée Magento dans le titre, elle était surtout orientée utilisation de Docker en pratique, allant de l'installation à la configuration de base pour une utilisation avec Magento.
Cette conférence était plutôt un TP avec du live coding pour démontrer en pratique la théorie expliquée dans les différentes slides.

### Qu'est-ce que Docker ?

C'est, pour simplifier, une VM allégée basée sur un système d'exploitation permettant l'installation de ce qui est nécessaire uniquement dans une image qui est exécutable indépendamment, et à laquelle il est possible de se connecter en SSH.

### Ce qui a été fait pendant la démonstration

L'objectif de cette conférence était de mettre en place une application Magento 2 sur une image en utilisant Docker, puis de la rendre accessible sur la plateforme Microsoft Azure.
Les étapes étaient :

1. Installation de Docker
2. Installation de MySql
3. Installation de ZendServer
4. Installation de Magento2
5. Mise en place sur Microsoft Azure

Le processus de création d'une image Docker est simple : il suffit de créer un fichier Dockerfile qui s'occupe de la créer. Il faut ensuite lancer l'image (`run`) avec un container.


### Commandes de base

Les commandes de base pour Docker sont les suivantes :

* Installation : wget -qO- https://get.docker.com/ | sh
* Build : docker build -t <nom_image>
* Run : docker run -d -p 3306:3306 -v /data/mysql:/var/lib/mysql -- name <nom_image> <nom_du_container>
* Stop : docker stop <nom_du_container>
* rm : docker rm <nom_du_container>
* processus qui tournent : docker ps

**A savoir :**
    Une fois un container Docker lancé, il faut au moins un processus pour le laisser actif, sinon il s'arrête.

### Création d'un Dockerfile

La création d'un Dockerfile est simple. En voici un exemple, qui installe Apache, PHP et Mysql :

FROM komljen/apache
MAINTAINER Alen Komljen <alen.komljen@live.com>

    RUN \
      apt-get update && \
      apt-get -y install \
              libapache2-mod-php5 \
              php5-mysql \
              mysql-client-5.5 && \
      rm -rf /var/lib/apt/lists/*

Source : [https://github.com/komljen/dockerfile-examples/blob/master/php-apache/Dockerfile](https://github.com/komljen/dockerfile-examples/blob/master/php-apache/Dockerfile).
Le Gihub duquel est tiré cet exemple liste de nombreux Dockerfile différents.

### Questions à se poser

Avant de créer son Dockerfile, il faut se poser quelques questions :

* Sur quel système d'exploitation je me base ?
* Quels appli j'installe ?
* Quels fichiers j'ajoute ?
* Quels ports je vais ouvrir ?
* Quels dossier de mon container ont être partagés ?

Je rentrerais plus en détail sur la mise en place d'une application avec Docker dans un futur article.

---

La conférence suivante était celle que j'attendais le plus : Présentation de la roadmap ZF3 par Julien Guittard. Pourquoi une telle impatience ? Parce qu'en fin d'année dernière j'avais été à un Meetup présentant "ZF3", qui avait fini sur la conclusion qu'il n'y aurait pas de version 3 du framework avant longtemps mais plutôt une version 2 ++. Pour plus d'informations sur ce Meetup, je vous invite à lire mon article à ce sujet : [Premier Meetup Zend Framework](http://blog.mickaeleuranie.com/php/meetup/zend/framework/zend-framework/2014/11/19/meetup-zend-framework/).

## Présentation de la roadmap ZF3

* Julien Guittard - [@julienguittard](https://twitter.com/julienguittard) - [https://apigility.org](https://apigility.org)

### ZF3 en quelques mots

ZF3 va mettre l'accent sur les composants et va se focaliser sur HTTP et les middlewares via PSR-7 (voir [HTTP message interfaces](http://www.php-fig.org/psr/psr-7/)).
Cette nouvelle version sera optimisée pour PHP 7, mais aura un full support des versions PHP 5.5+.
La première version stable sortira en octobre 2015.

### Composants

Il est intéressant de voir comment l'utilisation des composants a évolué en fonction des différentes version, pour comprendre les choix faits lors du développement de cette troisième.

#### ZF1 : le passé

A l'époque, les composants étaient développé au sein même du repository du framework, et n'étaient stable qu'avec le framework en entier.


#### ZF2 : aujourd'hui

Les composants sont développés au sein même du repository du framework et sont installés individuellement (via Composer, git submodules).

#### ZF4 : Le futur

Le gros changement annoncé est le suivant : l'**atomicité des composants**.
Chaque composant aura son propre repository et pourra être utilisé individuellement. Le framework en lui-même dépendra de ces composants, il sera un méta-package, chacun pourra faire de ZF son propre framework, on choisissant les composants qu'il souhaite et uniquement ceux là.

Cette logique d'utilisation des composants est celle qui devait être appliquée pour ZF1 à la base.

#### Pourquoi ce retour ?

**Maintenance simplifiée :**

* Permet plus d'équipe avec des droits bien définis sur les commits
* Permet un destin déterminé des répositories

**Evolution sélective :**

* Le framework pourra mettre à jour ses composants de façon spécifique, sélective
* Faire évoluer les versions : les versions se feront par composants

**Avoir une structure correspondant à ses propres besoins :**

* L'application ne sera qu'un webservice ?
* Besoin de performance ?
* Besoin de quoique ce soit ?
* Il y aura une structure pour ça !

Comme dit plus haut, il sera possible d'avoir exactement les composants dont nous avons besoin et uniquement ceux-là. Plusieurs structures types devraient voir le jour et être proposées par/pour la communauté.

### HTTP, PSR-7 et middlewares

Ce qu'il faut retenir, c'est que **pour contribuer à ZF il faut que le code soit PSR-2 compliant**.
PSR-2 défini comment bien écrire du PHP (logique de camelCase, classes qui commencent par une majuscule, etc.)

#### HTTP

HTTP est la fondation du web. Un client envoie une requête, le serveur renvoie une réponse.

Exemple d'un message HTTP :

    request
    GET /path HTTP/1.1
    Host: example.com
    Accept: application/json

Le message est le même pour chaque framework, mais chacun a sa propre méthode pour le récupérer, comme par exemple pour récupérer le type de requête :

    $method = $request->getMethod();
    $method = $request->getRequestMethod();
    $method = $request->method();

#### PSR-7

C'est une collection d'interfaces permettant de définir des interfaces HTTP. Pour plus d'informations, voir [http://www.php-fig.org/](http://www.php-fig.org/).

S'il y a une remarque à retenir de ce point, c'est ceci :

    Il faut développer à l'aide d'interfaces, pas d'héritage

Exemple d'interfaces :

    REQUEST :
    $method : $request->getMethod();
    $accept : $request->getHeader('Accept');

    RESPONSE :
    $response->getBody()->write('Hello world');


#### PSR-7 et le MVC ZF

Cette partie est encore en discussion, mais il est possible de consommer PSR-7 directement.


### Middleware

Le Middleware est situé entre la requête et la réponse. Il prend un objet de requête, un objet de réponse, prend la requête, fait son boulot et retourne une réponse.

Il existe plusieurs types de middleware.

#### Stacks ou pipelines

Ce type de middleware permet de n'exécuter un middleware que pour un path particulier, par exemple :

    $pipeline->pipe($middleware);
    $pipeline->pipe('/path', $middleware2');
    $pipeline->pipe('/different/path', $middleware3');
    $pipeline->$pipe->run();

#### Onion style

Celui-ci permet d'utiliser un middleware dans un middleware[[, dans un middleware], etc.].

#### Lambda style

Ce type permet d'invoquer un middleware via un callback.

#### Consommer un middleware

ZF permettra l'utilisation du middleware suivant :

    /**
     * @return Response
     */
    public function (Request $request, Response $response);

Et les controller deviendront des middlewares.


#### Utiliser les middlewares comme un runtime alternatif

**Pourquoi ?**

* Performance
* Expérience des développeurs
* Réutilisabilité entre frameworks

Il n'y aura que le middleware voulu qui sera changé.

Exemple :

    $app - new Middleware();$app->pipe('/', $homepage); // Static
    $app->pipe('/customer', $zf2Middleware); // ZF2
    $app->pipe('/products', $zf1Middleware); // ZF1
    $app->pipe('/api', $Apigility); // Apigility
    $app->pipe('/user', $userMiddleware);
    $server->listen($app);

Pour résumer : les chemins définissent les chemins des middlewares dont l'utilisation et l'interopérabilité est optimale.

### PHP 7 et PHP 5.5

#### PHP 5.5

Cette version a apporté son lot de fonctionnalités telles que :

* Traits
* Nouvelle syntaxe pour les tableaux
* Type hint
* Finnaly
* constantes de classe ::class magique
Generators
* Plus rapide, plus sécurisé

#### PHP 7

Cette version de PHP se démarque par l'impressionnante amélioration des performances, suite à une nouvelle gestion de la structure des données dans le moteur PHP et l'apparition des nouvelles fonctionnalités, tel que le type de retour.

#### ZF3 optimisé pour PHP ?

Cette nouvelle monture du framework permet :
* L'exécution de tests en mode strict ou coercitif
* De corriger des bugs pour PHP 5
* ...
* Le profiling en PHP 7

#### Pourquoi mettre à jour PHP ?

Comme pour toute mise à jour, se maintenir à la dernière version permet de profiter d'une sécurité améliorée, ainsi que les performances. Elle permet également d'améliorer le framework en lui-même.

---

Ce fut le moment de la pause, l'occasion d'essayer de gagner quelques éléphpants ! Malgré le [défi lancé sur Twitter](https://twitter.com/Darkmira1/status/611194334633984001) par [@Darkmira1](https://twitter.com/Darkmira1) je n'ai pas réussi à crocheter un des cadenas mis à disposition. J'aurais dû jouer un peu plus aux jeux vidéos pour m'entraîner ..!
Autre tentative : le stand de [@continuousphp](https://twitter.com/continuousphp) qui proposait de créer un élephpant en Lego. Celui dont la photo aura été le plus retweeté gagné une peluche. Mon manque de temps m'a forcé à faire une tentative un peu désespérée (faut bien trouver des excuses !) : ![](https://pbs.twimg.com/media/CHywq2zWgAAexfl.jpg)

Après avoir discuté avec les représentants des sponsors dont certains visages étaient familiers, je suis retourné dans la salle pour la dernière - et non des moindres - conférences de l'après-midi.

----

Après la présentation de Zend Framework 3, Julien Guittard nous a proposé une présentation et démonstration live d'Apigility.

## Apigility

* Julien Guittard - [@julienguittard](https://twitter.com/julienguittard) - [https://apigility.org](https://apigility.org)

Cet outil permet de créer très simplement des APIs (Application Programming Interface).
De nos jours, les APIs les plus connues sont des APIs web (Google, Amazon, etc.) mais ... Windows est également une API !

Il existe 2 types d'API : RPC et REST.

### RPC

RPC (Remote Procedure Call) est l'ancêtre de la notion d'API telle que nous la connaissons, présenté pour la première fois en 196 (RFC 707).
Son principe est simple : à partir d'une URI, il est possible d'effectuer plusieurs opérations, uniquement à partir d'appels en POST.
Formats existant : XML-RPC, SOAP.

#### RPC, les mauvais côtés

* Impossible de connaître le nombre de ressources disponibles
* Manque de cache HTTP
* Impossible d'utiliser d'autres méthodes que POST
* Manque de code réponse HTTP : elle sera toujours 200, sauf en cas d'erreur 500
* Gestion des erreurs

### REST

Les spécifications de REST (Representational State Transfert) ont été publiées en 2000 par Roy Thomas Fielding. Je vous encourage vivement à aller lire ce document : [https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm).

Son architecture est construite autour des spécifications HTTP. Il permet l'utilisation des verbes pour les opérations sur les ressources (GET, PUT, etc.) et le rendu dans différents formats (JSON, XML, etc.).
La notion d'HATEOAS est importante (Hypermedia as the Engine of Application State : [https://en.wikipedia.org/wiki/HATEOAS](https://en.wikipedia.org/wiki/HATEOAS)).
Le modèle de maturité de Richardson (Richardson Maturity Model) est un modèle qui décompose l’approche REST en trois étapes qui introduisent progressivement les principaux éléments de REST (Ressources ; Verbes et Codes retours HTTP ; Contrôles hypermédia) pour passer d’un modèle RPC sur HTTP à un modèle RESTFul. Je vous invite également à lire cet article qui décrit ce modèle : [http://blog.xebia.fr/2010/06/25/rest-richardson-maturity-model/](http://blog.xebia.fr/2010/06/25/rest-richardson-maturity-model/).

#### REST, les mauvais côtés

* Quels sont les formats de représentation exposés ?
* Rapport d'erreur
* Quelles sont les méthodes HTTP autorisées ?
* Authentification
* Liens hypermédia
* Documentation

Comment gérer tous ces points ? Apigility est là pour ça.

### APIGILITY

#### History

Anecdote amusante concernant le pourquoi le développement de cet outil à commencé : ZF2 venait de sortir, et l'équipe s'est posé la question "Maintenant, on fait quoi ?".
La version Beta a été présentée à la ZendCon de 2013, la version 1.0 est sortie le 7 mai 2014 et la version 1.1 en avril 2015, qui apporte nouveau design et nouvelles fonctionnalités.

#### Tour d'Apigility

Julien nous a ensuite fait une démonstration de l'outil, de l'installation jusqu'aux premières fonctionnalités.
Plusieurs méthodes sont possibles pour l'installer : Tarball, Composer, Git ou Zend Studio.
Apigility est une application basée sur ZF1 qui permet de créer ses propres API à travers une interface d'administration et vient avec 15 modules installés par défaut.
Plus d'informations concernant l'utilisation et l'installation de cet outil sur le site officiel [https://www.apigility.org/](https://www.apigility.org/).

#### Apigility en résumé

Pour résumer, Apigility permet entre autre de :

* Se concentrer sur la partie principale et non sur tout ce qui entour la création d'API
* Définir des règles de validation des données entrante
* Générer et compléter une documentation
* Intégrer la gestion via Grant
* Packager ses APIs pour le déploiement

#### Apigility dans le futur

L'outil est en constante évolution, il permettra le support de ZF3, utilisera PSR-7, des middleware, des micro services.
Concernant l'authentification au webservice, il est prévue d'améliorer les accès : utilisation de RBAC (Roll Based Access Control) qui permet de gérer oauth2, mais tout le monde a accès à tout. Il n'y a pas encore de gestion de rôles ayant accès à tel ou tel groupe, telle ou telle permission (cela est dû au fait que la configuration se fait par fichier plat, donc impossible de gérer les tableaux de droit pour le moment, mais c'est en cours d'amélioration).

---

## Keynote de fermeture

Après une après-midi bien remplie, l'heure était venue de remercier les différents acteurs de cet après-midi riche en informations. Les gagnants des élephpants ont été récompensés (je n'en faisais malheureusement pas partie !).

Merci encore aux organisateurs et acteurs de cette première édition du Darkmira Tour ! J'attends avec impatience de voir ce qu'il va advenir du ZF3 et de le voir en action, tellement les promesses sont nombreuses.