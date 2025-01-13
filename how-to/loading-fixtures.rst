---
source_url: https://github.com/doctrine/data-fixtures/blob/2.0.x/docs/en/how-to/loading-fixtures.rst
revision: 0bc3f4cd58648d6454452f242374573722003d6c
status: ready

title: Carregando fixtures
---

Carregando [fixtures]{lang="en"}
================================

Vamos supor que você tenha um projeto existente com um modelo ``User``.
Para criar um [fixture]{lang="en"} para esse modelo, há três etapas:

#. criar uma classe de [fixture]{lang="en"}.
#. carregar esse [fixture]{lang="en"} com um carregador.
#. executar o [fixture]{lang="en"} com um executor.

Criando uma classe de fixture
-----------------------------

As classes de fixture têm dois requisitos:

* Elas devem implementar ``Doctrine\Common\DataFixtures\FixtureInterface``.
* Se elas tiverem um construtor, esse construtor deve ser invocável sem
  argumentos.

.. code-block:: php

    <?php

    namespace MyDataFixtures;

    use Doctrine\Common\DataFixtures\FixtureInterface;
    use Doctrine\Persistence\ObjectManager;

    class UserDataLoader implements FixtureInterface
    {
        public function load(ObjectManager $manager): void
        {
            $user = new User();
            $user->setUsername('jwage');
            $user->setPassword('test');

            $manager->persist($user);
            $manager->flush();
        }
    }

.. note::

    ``FixtureInterface`` está no namespace ``Common``, porque já esteve no
    pacote ``doctrine/common``, que foi dividido em vários pacotes.
    O namespace foi mantido para compatibilidade com versões anteriores.

Carregando [fixtures]{lang="en"}
--------------------------------

Para carregar um [fixture]{lang="en"}, você pode chamar
``Loader::addFixture()``:

.. code-block:: php

    <?php

    use Doctrine\Common\DataFixtures\Loader;
    use MyDataFixtures\UserDataLoader;

    $loader = new Loader();
    $loader->addFixture(new UserDataLoader());

Também é possível carregar um [fixture]{lang="en"} fornecendo seu caminho:

.. code-block:: php

    <?php
    $loader->loadFromFile('/caminho/para/MyDataFixtures/MyFixture1.php');

Se você tiver muitos [fixtures]{lang="en"}, isso pode ficar cansativo bem
rápido, e você pode querer carregar um diretório inteiro de
[fixtures]{lang="en"} em vez de fazer uma chamada por [fixture]{lang="en"}.

.. code-block:: php

    <?php
    $loader->loadFromDirectory('/caminho/para/MyDataFixtures');

Você pode obter os [fixtures]{lang="en"} adicionados usando o método
``getFixtures()``:

.. code-block:: php

    <?php
    $fixtures = $loader->getFixtures();

Executando [fixtures]{lang="en"}
--------------------------------

Para carregar os [fixtures]{lang="en"} no seu armazenamento de dados, você
precisa executá-los.
É quando você precisa escolher classes diferentes dependendo do tipo de
armazenamento que você está usando.
Por exemplo, se você estiver usando ORM, você deve fazer o seguinte:

.. code-block:: php

    <?php
    use Doctrine\Common\DataFixtures\Executor\ORMExecutor;
    use Doctrine\Common\DataFixtures\Purger\ORMPurger;

    $executor = new ORMExecutor($entityManager, new ORMPurger());
    $executor->execute($loader->getFixtures());

.. note::

    Cada classe executora fornecida por este pacote vem com uma classe de
    exclusão que será usada para esvaziar seu banco de dados, a menos que você a
    desabilite explicitamente.

Se você quiser anexar os [fixtures]{lang="en"} em vez de excluir os dados antes
de carregar, passe ``append: true`` para o método ``execute()``:

.. code-block:: php

    <?php
    $executor->execute($loader->getFixtures(), append: true);

Por padrão, o ``ORMExecutor`` encapsulará a exclusão e o carregamento dos
[fixtures]{lang="en"} em uma única transação, que é a maneira recomendada, mas
em alguns casos (por exemplo, se o carregamento de seus [fixtures]{lang="en"}
for muito lento e causar esgotamento de tempo), você pode querer encapsular a
exclusão dos dados e a carga de cada [fixture]{lang="en"} em sua própria
transação.
Para fazer isso, você pode usar ``MultipleTransactionORMExecutor``.

.. code-block:: php

    <?php
    $executor = new MultipleTransactionORMExecutor($entityManager, new ORMPurger());
