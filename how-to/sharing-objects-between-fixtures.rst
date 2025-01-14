:source_url: https://github.com/doctrine/data-fixtures/blob/1.5.x/docs/en/how-to/sharing-objects-between-fixtures.rst
:revision: fea0550b9426845214957a1aad8c266891181124
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

E o fixture de carregamento de dados de ``User``:

Note que, como o último fixture depende do primeiro, a ordem em que os fixtures
estão sendo carregados se torna importante.
Você pode aprender mais sobre como gerenciar a ordem de carregamento no
:doc:`guia dedicado <fixture-ordering>`.
