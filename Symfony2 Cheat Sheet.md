Symfony2 Cheat Sheet
====================

Application
-----------

### Structure

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

### Console Commands

- créer la base de données

        php app/console doctrine:database:create

- supprimer la base de données

        php app/console doctrine:database:drop --force

- valider les annotations Doctrine et les comparer avec le schéma de la base de données

        php app/console doctrine:schema:validate

- mettre à jour le schéma de la base de données

        php app/console doctrine:schema:update --force

### Annotations

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



Tests unitaires
---------------

Répliquer la structure du Bundle dans Tests/

Classe de test

    class ExempleTest extends \PHPUnit_Framework_TestCase
    {
        public function setUp() {

        }

        public function testMethode() {
            $this->assert*();
        }

    }

Tests fonctionnels
------------------

= test d'un contrôleur, généralement

### Classe de test

    use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

    class ExempleTest extends WebTestCase { }

### Exécuter une requête

    $crawler = $client->request('GET', '/signup')

### Réponse

    $response = $client->getResponse();

### Traverser le DOM (HTML/XML)

    $crawler->filter()

### Suivre un lien

    $link = $crawler->selectLink('Link text')->link();
    
    $client->click($link);

### Remplir un formulaire et le soumettre

    $form = $crawler->selectButton('Button text')->form();

    $client->submit($form);

Validation
----------

Validation implicite des formulaires
Validation explicite d'objets, tableaux et variables en utilisant le service de validation

Notion de contrainte

validation peut porter sur
- propriété d'une classe
- getter (is*(), get*())
- classe

### Annotations

@Assert\[ConstraintName]

    use Symfony\Component\Validator\Constraints as Assert;

    class BlogArticle
    {
        /**
         * @Assert\NotBlank()
         */
        private $subject;

        /**
         * @Assert\Length(
         *      min = 3,
         *      minMessage = "C'est trop court !"
         * )
         */
        private $content;
    }

Validation explicite

    $validator = $this->get('validator');
    $errors = $validator->validate($blogArticle);

    if (count($errors) > 0) { }

Validation group sequence

Validation group sequence provider

Validation de variables simples

    use \Constraint;

    $validator = $this->validateValue($value, new Constraint());

Routing
-------

contact:
    path:     /blog/article/{articleId}
    defaults: { _controller: GredinDemoBundle:Blog:read }
    requirements: { articleId: \d+ }
    
Exemple synthétique

    article_show:
        path:     /{_locale}/article/{year}/{title}.{_format}
        defaults: { _controller: GredinDemoBundle:Article:show, _locale: en, _format: html }
        methods:  [GET]
        condition: "request.headers.get('User-Agent') matches '/firefox/i'"
        requirements:
            _locale:  en|fr
            _format:  html|json|rss
            year:     \d+

Syntaxe du contrôleur
    [bundle]:[contrôleur]:[action]

    GredinDemoBundle:Article:show
    <=>
    Gredin\DemoBundle\Controller\ArticleController::showAction

Tous les paramètres sont disponibles comme argument du contrôleur, dans n'importe quel ordre.

    showAction($year, $_format, $title)
    showAction($title)

Importation des routes d'un bundle dans app/config/routing.yml
(avec ajout d'un préfixe à toutes ces routes)

    gredin_cantata:
        resource: "@GredinDemoBundle/Resources/config/routing.yml"
        prefix:   /demo

Débug

    app/console router:debug

    app/console router:debug [nom de la route]

    app/console router:match /blog/article/bla-bla-bla

Génération d'URL

    $this->get('router')->...()

Template

    <a href="{{ path('blog_show', {'slug': 'my-blog-post'}) }}">Read article</a>