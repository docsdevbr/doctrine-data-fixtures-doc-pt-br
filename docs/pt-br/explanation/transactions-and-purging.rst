:source_url: https://github.com/doctrine/data-fixtures/blob/3.0.x/docs/en/explanation/transactions-and-purging.rst
:revision: 71ba52a81dd8c1e0c47830789b0a5662416daf6c
:status: ready

:title: Transações e expurgo

Transações e expurgo
====================

Este pacote fornece executores para ``doctrine/orm``, ``doctrine/mongodb-odm`` e
``doctrine/phpcr-odm``.

Os executores expurgam o banco de dados e, em seguida, carregam os fixtures.
A implementação do ORM encapsula essas duas etapas em uma transação de banco de
dados, que fornece uma propriedade adicional interessante: atomicidade.
Por causa dessa transação, o carregamento é bem-sucedido ou falha de forma
limpa, o que significa que nada é realmente alterado no banco de dados se o
carregamento falhar.
Ele delega o expurgo para uma classe separada que pode ser configurada para usar
uma instrução ``TRUNCATE`` ou ``DELETE`` para esvaziar tabelas.

Nem todos os RDBMS têm a capacidade de permitir instruções ``TRUNCATE`` dentro
de transações.
Notavelmente, o MySQL produzirá a infame mensagem "Não há transação ativa"
quando tentamos fechar uma transação que já foi `implicitamente fechada`_.

.. _implicitamente fechada: https://www.doctrine-project.org/projects/doctrine-migrations/en/stable/explanation/implicit-commits
