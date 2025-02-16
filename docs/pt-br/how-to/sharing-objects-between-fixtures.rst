:source_url: https://github.com/doctrine/data-fixtures/blob/3.0.x/docs/en/how-to/sharing-objects-between-fixtures.rst
:revision: 0bc3f4cd58648d6454452f242374573722003d6c
:status: ready

:title: Compartilhando objetos entre fixtures

Compartilhando objetos entre fixtures
=====================================

É provável que seus modelos tenham relacionamentos entre si.
Por isso, pode ser interessante criar e persistir um objeto em um fixture e,
então, referenciá-lo em outro fixture.

Assumindo que você tenha um modelo ``User`` e um modelo ``Role``, aqui está um
exemplo mostrando como usar a classe ``AbstractFixture`` para fazer isso.

.. note::

    ``AbstractFixture`` implementa ``FixtureInterface``, o que significa que o
    requisito mencionado no
    :doc:`guia de instruções anterior <loading-fixtures>` é satisfeito se você
    estender ``AbstractFixture``.

.. code-block:: php

    <?php
    namespace MyDataFixtures;

    use Doctrine\Common\DataFixtures\AbstractFixture;
    use Doctrine\Persistence\ObjectManager;

    class UserRoleDataLoader extends AbstractFixture
    {
        public function load(ObjectManager $manager): void
        {
            $adminRole = new Role();
            $adminRole->setName('admin');

            $anonymousRole = new Role();
            $anonymousRole->setName('anonymous');

            $manager->persist($adminRole);
            $manager->persist($anonymousRole);
            $manager->flush();

            // armazena a referência admin-role para ser usada na relação de
            // User com Role
            $this->addReference('admin-role', $adminRole);
        }
    }

E o fixture de carregamento de dados de ``User``:

.. code-block:: php

    <?php

    namespace MyDataFixtures;

    use Doctrine\Common\DataFixtures\AbstractFixture;
    use Doctrine\Persistence\ObjectManager;

    class UserDataLoader extends AbstractFixture
    {
        public function load(ObjectManager $manager): void
        {
            $user = new User();
            $user->setUsername('jwage');
            $user->setPassword('test');
            $user->setRole(
                $this->getReference('admin-role', Role::class) // carrega a referência armazenada
            );

            $manager->persist($user);
            $manager->flush();

            // armazena a referência admin-user para outros Fixtures
            $this->addReference('admin-user', $user);
        }
    }

Note que, como o último fixture depende do primeiro, a ordem em que os fixtures
estão sendo carregados se torna importante.
Você pode aprender mais sobre como gerenciar a ordem de carregamento no
:doc:`guia dedicado <fixture-ordering>`.
