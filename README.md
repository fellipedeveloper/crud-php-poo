CRUD PHP com Programação orientada a objetos, PDO e MVC.

A conexão com o banco de dados (Source\Core\Connect) segue o padrão de projeto SINGLETON, que permite garantir que classe tenha apenas uma instância, ao mesmo tempo em que fornecesse um ponto de acesso global para essa instância.

instância de conexão com o banco de dados:

Deve se chamar a classe Connect e o método estático getInstance(), sem a necessidade de instânciar um objeto.
ex:

$stmt = Connect::getInstance();

Persistência no banco de dados

A classe Source\Core\Model() é uma classe base, responsável pela persistência, execução das transações e conexão com o banco de dados e serve a todos os modelos, uma classe layer supertype. Implementa também o active record, com métodos específicos para cada etapa do CRUD.

A classe Source\Models\User() é responsável pela regra de negócio referente ao cadastro dos usuários na entidade users do banco de dados.

Fazendo Buscas

Para se fazer alguma busca, deve-se instânciar um objeto da classe Source\Models\User() com o método find().

A assinatura do método find() possue os seguintes parâmetros:

string $terms - Informar o termos referente a sua busca, ex: "id = :id AND email = :email".
string $params - Informar os parâmetros, sem espaços e utilizando o caractere "&" para separá-los. ex: "id=1&email=user@mail.com".
string $columns - Por padrão o parâmetro tem o valor "*", que retorna todas as colunas. Se você quiser o resultado com alguma columa específica, basta atribuir o valor ao parâmetro. ex: "id".
Exemplos:

Busca de usuário por id
$user = (new \Source\Models\User())->find("id = :id", "id=1");

Busca de usuário por id e email
$user = (new \Source\Models\User())->find("id = :id AND email = :email", "id=1&email=user@mail.com");

Se houver algum resultado, o retorno será um objeto User(), se não, o retorno setá null.

Listagem de todos os usuários cadastrado, com limit e offset.

Para listar todos os usuários cadastros deve-se instanciar a classe \Source\Models\User() e chamar o método all(). Os parâmetos do método all() são = $limit, $offset e $columns. Por padrão o limit é 10, offset é 0 e columns é "*".

ex1:

$list = (new User())->all();

ex2:

$list = (new User())->all(5, 5, "id");