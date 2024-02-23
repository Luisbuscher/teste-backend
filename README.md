# DESAFIO BACKEND

## Configuração do Ambiente

### Requisitos
- Instalar o _PHP >= 8.0_ e [extensões](https://www.php.net/manual/pt_BR/extensions.php) (**não esquecer de instalar as seguintes extensões: _pdo_, _pdo_sqlite_ e _sqlite3_**);
- Instalar o [_SQLite_](https://www.sqlite.org/index.html);
- Instalar o [_Composer_](https://getcomposer.org/).

### Instalação
- Instalar dependências pelo _composer_ com `composer install` na raiz do projeto;
- Servir a pasta _public_ do projeto através de algum servidor.
  (_Sugestão [PHP Built in Server](https://www.php.net/manual/en/features.commandline.webserver.)_. Exemplo para servir a pasta public: `php -S localhost:8000 -t public`)

## Sobre o Projeto

- O cliente XPTO Ltda. contratou seu serviço para realizar alguns ajustes em seu sistema de cadastro de produtos;
- O sistema permite o cadastro, edição e remoção de _produtos_ e _categorias de produtos_ para uma _empresa_;
- Para que sejam possíveis os cadastros, alterações e remoções é necessário um usuário administrador;
- O sistema possui categorias padrão que pertencem a todas as empresas, bem como categorias personalizadas dedicadas a uma dada empresa. As categorias padrão são: (`clothing`, `phone`, `computer` e `house`) e **devem** aparecer para todas as _empresas_;
- O sistema tem um relatório de dados dedicado ao cliente.

## Sobre a API
As rotas estão divididas em:
  -  _CRUD_ de _categorias_;
  - _CRUD_ de _produtos_;
  - Rota de busca de um _relatório_ que retorna um _html_.

E podem ser acessadas através do uso do Insomnia, Postman ou qualquer ferramenta de sua preferência.

**Atenção**, é bem importante que se adicione o _header_ `admin_user_id` com o id do usuário desejado ao acessar as rotas para simular o uso de um usuário no sistema.

A documentação da API se encontra na pasta `docs/api-docs.pdf`
  - A documentação assume que a url base é `localhost:8000` mas você pode usar qualquer outra url ao configurar o servidor;
  - O _header_ `admin_user_id` na documentação está indicado com valor `1` mas pode ser usado o id de qualquer outro usuário caso deseje (_pesquisando no banco de dados é possível ver os outros id's de usuários_).
  
Caso opte por usar o [Insomnia](https://insomnia.rest/) o arquivo para importação se encontra em `docs/insomnia-api.json`.
Caso opte por usar o [Postman](https://www.postman.com/) o arquivo para importação se encontra em `docs/postman-api.json`.

## Sobre o Banco de Dados
- O banco de dados é um _sqlite_ simples e já vem com dados preenchidos por padrão no projeto;
- O banco tem um arquivo de backup em `db/db-backup.sqlite` com o estado inicial do projeto caso precise ser "resetado".

## Demandas
Abaixo, as solicitações do cliente:

### Categorias
- [X] A categoria está vindo errada na listagem de produtos para alguns casos
  (_exemplo: produto `blue trouser` está vindo na categoria `phone` e deveria ser `clothing`_);
- [X] Alguns produtos estão vindo com a categoria `null` ao serem pesquisados individualmente (_exemplo: produto `iphone 8`_);
- [X] Cadastrei o produto `king size bed` em mais de uma categoria, mas ele aparece **apenas** na categoria `furniture` na busca individual do produto.

### Filtros e Ordenamento
Para a listagem de produtos:
- [X] Gostaria de poder filtrar os produtos ativos e inativos;
- [X] Gostaria de poder filtrar os produtos por categoria;
- [X] Gostaria de poder ordenar os produtos por data de cadastro.

### Relatório
- [X] O relatório não está mostrando a coluna de logs corretamente, se possível, gostaria de trazer no seguinte formato:
  (Nome do usuário, Tipo de alteração e Data),
  (Nome do usuário, Tipo de alteração e Data),
  (Nome do usuário, Tipo de alteração e Data)
  Exemplo:
  (John Doe, Criação, 01/12/2023 12:50:30),
  (Jane Doe, Atualização, 11/12/2023 13:51:40),
  (Joe Doe, Remoção, 21/12/2023 14:52:50)

### Logs
- [X] Gostaria de saber qual usuário mudou o preço do produto `iphone 8` por último.

### Extra
- [ ] Aqui fica um desafio extra **opcional**: _criar um ambiente com_ Docker _para a api_.

**Seu trabalho é atender às 7 demandas solicitadas pelo cliente.**

Caso julgue necessário, podem ser adicionadas ou modificadas as rotas da api. Caso altere, por favor, explique o porquê e indique as alterações nesse `README`.

Sinta-se a vontade para refatorar o que achar pertinente, considerando questões como arquitetura, padrões de código, padrões restful, _segurança_ e quaisquer outras boas práticas. Levaremos em conta essas mudanças.

Boa sorte! :)

## Suas Respostas, Duvidas e Observações
[Adicione  aqui suas respostas, dúvidas e observações]

  Corrigido a função getOne, onde estava retornando um erro através do echo, pelo fato da categoria estar sendo passada mesmo constando como "null";

  Corrigido o retorno null dos produtos, estava retornando null, pois a tabela "company" tinha estava com a coluna null para algumas categorias;

  Corrigido os produtos individuais que tinham 2 categorias, eu atualizei a função getOne de ProductController para buscsar todas as categorias associadas a um produto e adicionr ao objeto do produto, ao inves de buscar apenas uma categoria. Isso vai garantir que tdas as categorias vao ser exibidas corretamente;

  Corrigido a listagem de produtos, onde o "Blue Trouser" estava vindo como "phone" ao invés de vir como "clothing". Tudo que eu fiz foi, na funcao getAll do arquivo ProductController, foi substituir na consulta do banco de dados o pc.id = c.id por pc.cat_id = c.id.

  Adicionado a filtragem de itens por ativos e inativos. criei uma função especifica chamada "getAllFilter" que tem como propósito filtrar somente por ativos e inativos de acordo com os valores nos parametros ex: localhost:8000/products/filter/1. Dessa forma ira exibir somente os produtos no qual o filtro for 1, ou seja ativo, para inativo apenas mude o 1 para 0;

  Adicionado a filtragem de itens por CATEGORIAS. criei uma função especifica chamada "getCategory" que tem como propósito filtrar os itens por categoria. Dessa forma ira exibir somente os produtos no qual o id da categoria for igual o id passado. Ex: http://localhost:8000/products/category/2 vai selecionar somente os produtos da categoria "PHONE";

  Adicionei na funcao um getAll, do productService um ORDER BY, para assim por padrão ao pesquisar os produtos, eles irão vir filtrados por data de criação, o parametro é o mesmo "/products".

  O usuario que mudou o preco do Iphone 8 por ultimo foi "corin.tucker@xpto.com|corin" no qual tem a log mais recente para o id do Iphone.