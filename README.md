# Initiation Symfony
Pour Licence MMI @moshifr < moshi@moshi.fr >

----------
>Ce tutoriel a pour but de se familiariser pendant 25 heures à Symfony.
Nous n'irons pas dans le détail, vous aurez d'autres cours en S4 pour continuer / approfondir.
[Dernière correction]

#Introduction
Nous allons voir pendant ce cours [Symfony] et [Twig].
Symfony étant le framework PHP incluant le moteur de template Twig maintenus par la même entité : SensioLabs. 
Symfony est un framework dit "full stack" soit un framework pouvant répondre à tous les besoins pour un site à moyenne ou forte audience ou technicité. Symfony est modulaire et a comme intérêt que tous ses modules peuvent être utilisés séparément ou via d'autre framework. 
L'intérêt d'un framework est d'avoir une méthode de travail qui soit la même pour toute une équipe et donc de pouvoir comprendre le code par n'importe qui maîtrisant le framework.

#Pré-requis
* Serveur Web
* PHP 5.3.3 minimum
* Mod_rewrite à activer
* Module pdo 
* Console (cmd.exe, gitbash, term, xterm ...)
* date.timezone à changer dans php.ini
* Mod intl à activer
* Composer (on y reviendra)

#Installation
Aller sur [Symfony] , Download, First install Symfony Installer

Window (terminal) en se placant sur le répertoire web (/wamp/www par exemple): 
> c:\> php -r "file_put_contents('symfony', file_get_contents('http://symfony.com/installer'));"

Linux / Mac (term) : 
> sudo curl -LsS http://symfony.com/installer -o /usr/local/bin/symfony

> sudo chmod a+x /usr/local/bin/symfony


Une fois installé nous allons récupérer une application demo de Symfony pour pouvoir prendre un projet déjà en cours.
Se placer dans le répertoire web (/wamp/www)
> symfony demo
> ou
> php symfony demo

Aller ensuite sur votre navigateur :
> http://localhost/symfony_demo/web/check.php pour vérifier sa configuration
> http://localhost/symfony_demo/web pour le site de démo

Découvrir le site démo, présentation des répertoires.

#Arborescence Symfony
*  **app** contiendra toute votre configuration et des fichiers utiles à votre application
 * **config** contient les fichiers de configuration de l'application
 * **Resources** contient toutes les vues, templates TWIG
* **src** code métier, votre application
* **vendor** applications tierces, à ne pas modifier
* **web** répertoire exposé au service web

#Définitions
* **bundle** : Module ou application 
* **Envirionnement** : Symfony vous propose 2 environnements natifs : dev et prod, chacun peut avoir une configuration spécifique et un comportement unique en fonction des fichiers de paramètres et du fichier d'appel (app.php ou app_dev.php)
* **Route** : Système permettant à Symfony de savoir pour chaque url quel controller appeler
* **template** : Système simplifié pour gérer la modification du design, les logiques complexes ne peuvent être réalisées, on pourra seulement utiliser des conditions et des boucles. On utilisera Twig.
* **controler** : Permet des logiques complexes et de lier les vues (templates) aux modèles 
* **ORM** : Système permettant de lier une table ou un élément de la base de données à une classe. On utilisera Doctrine2.

#Profiler
A compléter

#Exo 1 
* Parcourir les pages 
* Regarder le code 

#Exo 2
* Modifier le footer en remplacant "The Symfony Project" par "MMI"
* Créer une nouvelle page qui affichera seulement "Bonjour" avec comme url : http://localhost/symfony_test/web/app.php/fr/hello
 * On copiera le fichier src\AppBundle\Controller\BlogController.php pour faire src\AppBundle\Controller\HelloController.php en ne gardant que indexAction
 * On utilisera la classe Symfony\Component\HttpFoundation\Response pour afficher "HELLO"

#Routes
Le fichier app/config/routing.yml permet de structurer une url, de la nommer et de la lier à une méthode d'un controller.
Ce fichier est dans une stucturation de données de type YAML qui se distingue en n'utilisant que des espaces pour structurer les informations. On utilisera donc des espaces plus ou moins grand pour mettre des informations au même niveau.
On pourra mettre rajouter dans app/config.routing.yml par exemple (remplacer les étoiles par des espaces) :
>  app:
    resource: @AppBundle/Controller/
    type:     annotation
    prefix:   /{_locale}
    requirements: 
        _locale: %app_locales%
    defaults:
        _locale: %locale%

La route se nomme "app", elle est de type annotation ce la signifie que les sous cheminements se font directement dans les controller du répertoire AppBundle/Controller/* via des annotations de type :
> /**
     * @Route("/", name="blog_index")
     */
     
Ici la route se nomme blog_index et se réfère à l'url "/" 

# Exo 3 
* Intégration simple d'une template Twig
* Créer la template dans app/Resources/views/hello/index.html.twig
* Au lieu de return new Response on fera return $this->render('hello/index.html.twig');

#Twig
Aller sur [Twig] pour voir la documentation. On utilisera des logiques simples tels que {if} {for} {foreach}. On affecte les variables dans le controller et on les affiche dans la template via {{ $mavariable }}

#Exo 4
* Créer une nouvelle page dans HelloController qui va reprendre un code couleur depuis l'url pour changer le fond de la page
* http://localhost/symfony_demo/web/app.php/fr/hello/switch_bg/bleu
* Créer une nouvelle méthode de controller avec une annotation qui reprendra des arguments.
* Et afficher cette variable dans la template twig pour en changer la couleur.

#Exo 4.1
* Faire que hello/index.html.twig hérite de base.html.twig

#Controller et route
Vous pouvez récupérer des variables de l'url en rajoutant :
> 
     /**
     * @Route("/switch_bg/{color}", name="hello_switch", defaults={"color"="red"})
     */
    public function switchAction($color)
    {
        return $this->render('hello/index.html.twig', array('color' => $color));
    }

Dans  l'annotation si on rajoute des accolades {valeur} on peut récupérer cette valeur dans la méthode et ainsi faire un traitement spécifique. Ensuite on le fourni à la vue via le deuxième argument de render qui est un tableau. On a mis par défaut la couleur à rouge on peut également faire des conditions de vérification par exemple si la valeur de couleur ne contient pas de nombre par exemple.

#Exo 5
* Modifier BlogController::indexAction pour n'afficher que les articles avec le mot "Curabitur"
* Modifier PostRepository pour ajouter une fonction de recherche sur le titre
* Rajouter une page http://localhost/symfony_demo/web/app_dev.php/fr/blog/filter/cura/ qui affichera les articles avec "cura" dans le titre

#Repository
Une repository est l'extension d'une entité, quant une entité se contente de lister ses propriétés et les getter / setter de celles ci la repository va effectuer toutes les recherches, fonctions annexes propre à une entité, à un modèle.
Elle se trouve en général dans un répertoire Repository ou dans Entity
ON va pouvoir créer des requetes via
> $this->getEntityManager()
            ->createQuery('select * ...')

#Exo 6 
* Créer une nouvelle entité AppBundle:Category :
$ php app/console doctrine:generate:entity 
* ajouter title et slug tous les deux champs en varchar
* Mettre à jour la base de données 
$ php app/console doctrine:schema:update --force
* Créer un backoffice Category (reprendre tout le controller admin/BlogController.php) et copier admin/BlogCategoryController
* Modifier toutes les instances Post par Category, $posts par $categories et $post en $category ...
* Mettre en place les templates admin/category/*
* Attention les variables sont $category et $categories et il y a des champs en moins (author) par exemple
* Tester http://localhost/symfony_demo/web/app_dev.php/fr/admin/category/  
* Ajouter le formulaire Form/CategoryType.php
* Tester sur http://localhost/symfony_demo/web/app_dev.php/fr/admin/category/new

# Entité
Pour créer une Entité on utilise la console 
$ php app/console doctrine:generate:entity
On renseigne les différents champs.
Puis on met à jour la base de données :
$ php app/console doctrine:schema:update --force

Pour modifier une entité on modifie la classe Entite.php (Post.php par exemple) en rajoutant des propriétés var $title;
Ensuite on mets à jour l'entité :
$ php app/console doctrine:generate:entities AppBundle:Category
puis on remet à jour la base de données
$ php app/console doctrine:schema:update --force

# Exo 7 - Relations  
* On va utiliser une relation OneToMany (One Category to Many Post) et ManyToOne (Many Posts to One Category)
* On modifie les entités
* On modifie les forms pour rajouter les liaisons
* On rajoute ensuite des pages catégories qui vont filtrer par catégories les posts

http://www.moshi.fr/symfony2/Symfony2-LP-Entites.pdf
http://www.moshi.fr/symfony2/Symfony2-LP-FORM.pdf
http://www.moshi.fr/symfony2/Symfony2-LP-TWIG.pdf
http://www.moshi.fr/symfony2/Symfony2-LP-TDTP1.pdf

# Exo 8 - Recherche multicritère 
* Faire une page de recherche multicritère
* Utiliser une class Form/RechercheType.php



 
       
[Symfony]: <http://www.symfony.com>
[Twig]: <http://twig.sensiolabs.org/>
[Dernière correction]: <https://github.com/moshifr/initiation_symfony#exo-5>