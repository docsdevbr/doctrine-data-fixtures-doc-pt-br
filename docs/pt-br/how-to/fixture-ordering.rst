:source_url: https://github.com/doctrine/data-fixtures/blob/1.6.x/docs/en/how-to/fixture-ordering.rst
:revision: 0bc3f4cd58648d6454452f242374573722003d6c
:status: ready

:title: Ordenação de fixtures

Ordenação de fixtures
=====================

Há duas interfaces que você pode implementar em seus fixtures para controlar em
qual ordem eles serão carregados.

* Ao implementar ``OrderedFixtureInterface``, você poderá especificar
  manualmente uma prioridade para cada fixture.
* Ao implementar ``DependencyFixtureInterface``, você poderá declarar qual
  classe deve ser carregada após quais classes (observe o plural) e deixar o
  pacote descobrir a ordem para você.

.. note::
    Você pode implementar uma interface em um fixture, e outra interface em
    outro fixture, e até mesmo nenhuma interface (além de ``FixtureInterface``)
    em um terceiro.
    Implementar ambas no mesmo fixture é um erro.

Opção 1: Controlando a ordem manualmente
----------------------------------------

.. code-block:: php

    <?php

    namespace MyDataFixtures;

    use Doctrine\Common\DataFixtures\AbstractFixture;
    use Doctrine\Common\DataFixtures\OrderedFixtureInterface;
    use Doctrine\Persistence\ObjectManager;

    final class MyFixture extends AbstractFixture implements OrderedFixtureInterface
    {
        public function load(ObjectManager $manager): void
        {
            // …
        }

        public function getOrder(): int
        {
            return 10; // menor significa antes
        }
    }

.. note::
    Embora estender ``AbstractFixture`` não seja necessário, é provável que você
    precise, já que as pessoas geralmente precisam que os fixtures sejam
    carregados em uma ordem específica por causa de referências de um fixture
    para o outro.

Opção 2: Declarando dependências
--------------------------------

Se você tem muitos modelos e um projeto que evolui, pode haver várias ordens
corretas.
Usar ``OrderedFixtureInterface`` pode se tornar impraticável caso você precise
inserir um novo fixture em uma posição onde não haja lacuna na ordem.
Em vez de sempre renumerar os fixtures ou ter cuidado para deixar grandes
lacunas, você pode declarar que seu fixture deve ser carregado após alguns
outros fixtures e deixar o pacote descobrir o que fazer.

.. code-block:: php

    <?php
    namespace MyDataFixtures;

    use Doctrine\Common\DataFixtures\AbstractFixture;
    use Doctrine\Common\DataFixtures\DependentFixtureInterface;
    use Doctrine\Persistence\ObjectManager;

    class MyFixture extends AbstractFixture implements DependentFixtureInterface
    {
        public function load(ObjectManager $manager): void
        {
        }

        /**
         * @return list<class-string<FixtureInterface>>
         */
        public function getDependencies(): array
        {
            return [MyOtherFixture::class];
        }
    }

    class MyOtherFixture extends AbstractFixture
    {
        public function load(ObjectManager $manager): void
        {}
    }
