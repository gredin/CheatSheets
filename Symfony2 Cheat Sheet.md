Symfony2 Cheat Sheet
====================

Application
-----------

Structure

    app/
        config/
            parameters.yml
            routing.yml
        AppKernel.php

    src/
        AccountBundle/

    vendor/
        composer/
        doctrine/
        symfony/
        twig/

    web/
        config.php
        app.php
        app_dev.php

Formats de configuration : YML, XML et PHP
+Annotations

Bundle
------

Structure

    src/Gredin/AccountBundle/

        Controller/
            WelcomeController.php

        DependencyInjection/
        
        Entity/
        
        Resources/
            config/
                routing.yml
            public/
                css/
                images/
                js/
            views/
                Welcome/
                    index.html.twig
                base.html.twig

Création d'un bundle

    php app/console generate:bundle --namespace=Gredin/AccountBundle --format=yml

Quand créer un bundle ?
- une même fonctionnalité
- réutilisable
- ...


Controller
----------




Routing
-------





Doctrine
--------

Console Commands

- créer la base de données

        php app/console doctrine:database:create

- supprimer la base de données

        php app/console doctrine:database:drop --force

- valider les annotations Doctrine et les comparer avec le schéma de la base de données

        php app/console doctrine:schema:validate

- mettre à jour le schéma de la base de données

        php app/console doctrine:schema:update --force

Annotations

    use <namespace\Mapping> as ORM;

    @ORM\Entity(repositoryClass="Gredin\AccountBundle\Entity\UserRepository")
    @ORM\Table(name)

    @ORM\Column(type="integer", nullable=false, unique=true)


Sécurité
--------

Protection anti-CSRF automatique

Formulaires
-----------

concept : objet <=> form

il faut donc définir le mapping entre les deux
pour la réutilisation, ce mapping peut être placé en dehors du contrôleur, dans une classe distincte

cas des unmapped fields

cas où on ne veut pas une association object <=> form, mais simplement un tableau de données

Doctrine
$em = $this->getDoctrine()->getManager();

Tests
-----

Répliquer la structure du Bundle dans Tests/

Tests unitaires

    extends \PHPUnit_Framework_TestCase

    $this->assert*();

Tests fonctionnels
    = test d'un contrôleur, généralement

    extends TestWebCase (?)
    $client
    $crawler