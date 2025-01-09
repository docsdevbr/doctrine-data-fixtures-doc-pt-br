---
source_url: https://github.com/doctrine/data-fixtures/blob/2.1.x/docs/en/index.rst
revision: fea0550b9426845214957a1aad8c266891181124
status: ready

title: Doctrine Data Fixtures
---

Doctrine Data Fixtures
======================

Esta extensão fornece uma maneira de carregar dados arbitrários em seu banco de
dados a partir de classes PHP especiais chamadas "fixtures".
Ela pode ser útil para fins de teste ou para semear um banco de dados com dados
iniciais.

Recursos
--------

* suporte para ORM e ambos os ODMs (PHPCR, MongoDB);
* objetos podem ser compartilhados entre [fixtures]{lang="en"};
* especificação da ordem em que os [fixtures]{lang="en"} são carregados;
* especificação de dependências entre [fixtures]{lang="en"}.

Instalação
----------

O cenário mais provável é que você precisará desta biblioteca para fins de
teste::

    $ composer require --dev doctrine/data-fixtures

Obtendo ajuda
-------------

* converse conosco no `Slack <https://www.doctrine-project.org/slack>`_;
* faça uma pergunta no `StackOverflow <https://stackoverflow.com/questions/tagged/doctrine>`_;
* relate uma falha no `GitHub <https://github.com/doctrine/data-fixtures/issues>`_.
