**Doctrine**
===========

Les **extensions Doctrine** permettent de par son rôle d'ORM, de mapper des objets puis de lui définir des états via l'entity manager(data mapper et unit of work). L'avantage de ce systéme est que les objets sont complétements indépendants du systéme de stockage. Pour faire plus simple, l'entity manager ne va pas mettre à jour la base de données à chaque changement.
A l'aide de fonction (**find**, **flush** et **persist**) l'entity manager va faire le lien avec la base de donnée.
Exemple avec le codage du mot de passe d'un utilisateur:

        <?php

        namespace App\EventSubscriber;

        use App\Entity\User;
        use Doctrine\Common\EventSubscriber;
        use Doctrine\Common\Persistence\Event\LifecycleEventArgs;
        use Doctrine\ORM\Events;
        use Symfony\Component\Security\Core\Encoder\UserPasswordEncoderInterface;

        class PasswordEncoderSubscriber implements EventSubscriber
        {
            private $encoder;

            public function __construct(UserPasswordEncoderInterface $encoder)
            {
                $this->encoder = $encoder;
            }

            public function getSubscribedEvents()
            {
                return [
                    Events::prePersist
                ];
            }

            public function prePersist(LifecycleEventArgs $args)
            {
                $user = $args->getObject();

                if ($user instanceof User) {

                    $user->setPassword($this->encoder->encodePassword(
                        $user,
                        $user->getPassword()
                    ));
                    
                }
            }
        }



**Doctrine et Api Platform**
============================

Api platform permet d'utiliser les extensions de Doctrine à condition d'appeler les interfaces concernés (à l'aide des **"use"**). Il permet également de modifier des collections ainsi que les requêtes d'item.
Il suffit de créer la classe de l'extension et de créer un service pour cette classe. Il ne faut surtout pas oublier d'appeler les interfaces sans lesquels cela ne marchera pas.

**L'intérêt des interfaces**
============================

Le rôle d'une interface est de décrire un comportement à notre objet.
L'un des intérêts principales de celui-ci va être de fournir un plan général qu'il suffira juste d'implémeneter à notre code. Il faudra donc créer des classes dérivées à partir de ces interfaces pour pouvoir utiliser ses éléments. Une interface ne peut contenir ques des signatures de méthode (comme dit plus haut, c'est un plan). On peut aussi dans une seul classe implémenter plusieurs interfaces ce qui est trés intéressant lorsque l'on veut manipuler plusieurs éléments à la fois.

**Le polymorphisme**
====================

C'est l'utilisation de fonctionnalités différentes dans des classes tout en partageant une interface commune.On utilise le même nom/méthode mais on obtient un résultat avec un comportement différent à chaque fois.


