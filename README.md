# Abstract Factory

## Intenção
Permite a criação de famílias de objetos relacionados ou dependentes sem especificar suas classes concretas.

## Também conhecido como
- Kit
- Fábrica de fábricas

## Motivação

Pense em criar um sistema que precisa gerenciar diversas famílias de objetos relacionados, tais como componentes gráficos em diversas plataformas ou dispositivos de diversos fabricantes. Inicialmente, é possível criar objetos com estruturas condicionais, determinando qual tipo de produto deve ser instanciado. Contudo, à medida que o sistema se expande e novas categorias de produtos são incorporadas, essa estratégia se torna inflexível e difícil de manter. A solução encontrada pelo padrão Abstract Factory é proporcionar uma interface central para a criação de objetos relacionados, assegurando que cada agrupamento de produtos seja criado de forma consistente e desacoplado. Isso não só torna a manutenção do código mais simples, como também possibilita o crescimento do sistema sem alterar a lógica já estabelecida, favorecendo a flexibilidade e a organização.

O código a seguir representa um problema clássico de alto acoplamento e dificuldade de manutenção. 

@import "devicesExample/badCode/src/service/DeviceFactory.ts"

O uso de estruturas como if ou switch para determinar o tipo de dispositivo e suas variantes gera as seguintes limitações:
1. **Complexidade do Cliente**: A lógica para determinar o dispositivo e seu tipo estão embutidas na classe DeviceFactory, tornando-a mais difícil de manter a longo prazo e testar.
2. **Dificuldade para Adicionar Novos Produtos**: Sempre que um novo tipo de dispositivo (ou variante) é introduzido, é necessário modificar o método createDevice, violando o princípio aberto/fechado (Open/Closed Principle).

   
`💡 Um design mais modular e flexível pode ser alcançado encapsulando a criação dos dispositivos em fábricas específicas e criando assim um nível de abstração, eliminando a necessidade de lógica condicional dentro do cliente.`


## Aplicabilidade
Use o padrão Abstract Factory quando:
- Um sistema precisa ser independente, gerando uma solução desacoplada para criar produtos relacionados.
- Um sistema precisa ser configurado com uma dentre várias famílias de produtos.
- Desejar garantir que objetos de uma mesma família sejam usados em conjunto.
- Desejar fornecer uma biblioteca de classes de produtos sem alterar o código do cliente e sem expor suas interfaces e implementação.

## Exemplo Aplicado

```plantuml

title Fábrica de Marcas

class Actor

interface IDeviceFactory {
    +createPhones(): IPhone
    +createWatch(): IWatch
}

class AndroidFactory {
    +createPhones(): IPhone
    +createWatch(): IWatch
}

class AppleFactory {
    +createPhones(): IPhone
    +createWatch(): IWatch
}

interface IPhone {
    +getDetails(): string
}

interface IWatch {
    +getDetails(): string
}

class AndroidPhone {
    +getDetails(): string
}

class ApplePhone {
    +getDetails(): string
}

class AndroidWatch {
    +getDetails(): string
}

class AppleWatch {
    +getDetails(): string
}

Actor --> IDeviceFactory

IDeviceFactory --|> AndroidFactory
IDeviceFactory --|> AppleFactory

AndroidFactory --|> IWatch
AppleFactory --|> IWatch
AndroidFactory --|> IPhone
AppleFactory --|> IPhone

IWatch --> AppleWatch 
IWatch --> AndroidWatch

IPhone --> ApplePhone
IPhone --> AndroidPhone
```
.
## Estrutura GOF
![Image](https://github.com/user-attachments/assets/baa8c39d-3066-4327-8eeb-8deea6e96266)

## Participantes

### AbstractFactory (IDeviceFactory)
- Define a interface para a criação de famílias de produtos relacionados, como `createPhones()` e `createWatch()`.

### Concrete Factory (AndroidFactory, AppleFactory)
- Implementam a interface `IDeviceFactory` para criar produtos específicos.
  - **AndroidFactory**: Cria instâncias de `AndroidPhone` e `AndroidWatch`.
  - **AppleFactory**: Cria instâncias de `ApplePhone ` e `AppleWatch`.

### AbstractProduct (Phone e Watch)
- São interfaces que definem as operações básicas dos produtos.
  - **IPhone**: Método `getDetails()`:Retorna informações sobre o telefone.
  - **IWatch**: Método `getDetails()`:Retorna informações sobre o relógio.

### ConcreteProduct (AndroidPhone, ApplePhone, AndroidWatch e AppleWatch)
- Implementam as interfaces `IPhone` e `IWatch`, representando produtos específicos.
  - **AndroidPhone**: Implementação concreta do produto `IPhone` para dispositivos Android.
  - **ApplePhone**: Implementação concreta do produto `IPhone` para dispositivos Apple.
  - **AndroidWatch**: Implementação concreta do produto `IWatch` para dispositivos Android.
  - **AppleWatch**: Implementação concreta do produto `IWatch` para dispositivos Apple.
  
### Client (Actor)
- Usa somente interfaces declaradas pelas classes Abstract Factory.


## Colaborações

- **Cliente e Fábrica Abstrata**: O cliente interage com a interface da Fábrica Abstrata (`AbstractFactory`) para criar famílias de produtos relacionados, sem conhecer as classes concretas.
- **Fábrica Concreta e Produtos Concretos**: Cada Fábrica Concreta (`ConcreteFactory`) cria uma família específica de produtos concretos.
- **Produtos Abstratos e Produtos Concretos**: Os produtos concretos implementam interfaces ou classes abstratas, garantindo que as fábricas concretas possam ser substituídas sem impacto no cliente.

O cliente utiliza a Fábrica Abstrata para criar os objetos, e as Fábricas Concretas instanciam os produtos concretos necessários.


## Consequências

### Benefícios

1. **Consistência entre produtos**: Garante que os produtos criados por uma fábrica pertencem à mesma família e funcionam bem juntos.
    > Exemplo: Um sistema gráfico com botões e barras de rolagem consistentes em estilo.
2. **Isolamento da implementação**: O cliente interage apenas com interfaces ou classes abstratas, deixando o código mais flexível e desacoplado.

3. **Facilidade para introduzir novas famílias de produtos**: Adicionar uma nova família requer apenas criar uma nova Fábrica Concreta e seus produtos concretos.

4. **Organização por famílias**: Estrutura sistemas que precisam criar objetos agrupados logicamente.


### Desvantagens

1. **Aumento da complexidade**: Implementar uma Fábrica Abstrata pode gerar muitas classes (Fábricas Concretas e Produtos Concretos).

2. **Dificuldade em adicionar novos produtos**: Alterar a Fábrica Abstrata para incluir um novo produto afeta todas as Fábricas Concretas existentes.


## Implementação

1. **Definir a Fábrica Abstrata**: Declare métodos para criar cada tipo de produto relacionado.

@import "devicesExample/goodCode/src/services/contracts/IDeviceFactory.ts"

2. **Implementar as Fábricas Concretas**: Implemente a interface da Fábrica Abstrata, criando objetos específicos de uma família.

@import "devicesExample/goodCode/src/services/factorys/AndroidFactory.ts"

@import "devicesExample/goodCode/src/services/factorys/AppleFactory.ts"

3. **Definir os Produtos Abstratos**: Crie interfaces ou classes abstratas para os produtos.

@import "devicesExample/goodCode/src/core/contracts/IPhone.ts"

@import "devicesExample/goodCode/src/core/contracts/IWatch.ts"

4. **Implementar os Produtos Concretos**: Implemente os produtos abstratos.

@import "devicesExample/goodCode/src/core/models/android/AndroidPhone.ts"

@import "devicesExample/goodCode/src/core/models/android/AndroidWatch.ts"

@import "devicesExample/goodCode/src/core/models/ios/ApplePhone.ts"

@import "devicesExample/goodCode/src/core/models/ios/AppleWatch.ts"

5. **Usar o Padrão**: O cliente cria a fábrica concreta desejada e utiliza para criar os produtos.

@import "devicesExample/goodCode/src/main.ts"

## Usos Conhecidos

- O InterViews utiliza o padrão Abstract Factory para a geração de componentes de interface gráfica. Suas abstrações-chave são WidgetKit e DialogKit, que criam objetos específicos para interação, e LayoutKit, que compõe objetos de acordo com o layout desejado. O ET++ [WGM88] também emprega Abstract Factory para garantir portabilidade entre sistemas de janelas. Sua classe base WindowSystem define uma interface para criação de recursos gráficos, como janelas e fontes, enquanto subclasses concretas implementam essas operações para diferentes sistemas. Em tempo de execução, uma instância de WindowSystem é criada dinamicamente para fornecer os objetos apropriados.

- O padrão Abstract Factory da Oracle gerencia o acesso a dados em aplicações J2EE, garantindo flexibilidade e portabilidade entre diferentes fontes de armazenamento. Suas abstrações-chave são DAOFactory e DataAccessObject. Diferentes fontes de dados são implementadas como subclasses de DAOFactory, permitindo que a aplicação selecione dinamicamente a implementação adequada, como OracleDAOFactory ou CloudscapeDAOFactory. O DataAccessObject abstrai o acesso ao banco de dados e delega operações a objetos específicos, como CustomerDAO e AccountDAO. Essa abordagem desacopla a camada de negócios da persistência, facilitando a migração entre bancos de dados e promovendo reutilização e manutenção eficiente.
[Oracle: Padrões principais do J2EE - Objeto de acesso a dados](https://www.oracle.com/java/technologies/dataaccessobject.html)

## Padrão relacionados
**Factory Method**: O padrão AbstractFactory é implementado com frequência com o factory method

**Prototype**: O padrão AbstractFactory também pode ser implementado utilizando prototype

**Singleton**: Uma fábrica concreta é freqüentemente um singleton

## Exemplos complementares

### UML

```plantuml

interface DatabaseConnection {
    + connect()
}

class MySQLConnection {
    + connect()
}
class MongoDBConnection {
    + connect()
}

DatabaseConnection <|-- MySQLConnection
DatabaseConnection <|-- MongoDBConnection

interface DataRepository {
    + save(String data)
    + find(int id)
}

class MySQLRepository {
    + save(String data)
    + find(int id)
}
class MongoDBRepository {
    + save(String data)
    + find(int id)
}

DataRepository <|-- MySQLRepository
DataRepository <|-- MongoDBRepository

interface DatabaseFactory {
    + createConnection(): DatabaseConnection
    + createRepository(): DataRepository
}

class MySQLFactory {
    + createConnection()
    + createRepository()
}
class MongoDBFactory {
    + createConnection()
    + createRepository()
}

DatabaseFactory <|-- MySQLFactory
DatabaseFactory <|-- MongoDBFactory

class Application {
    - DatabaseConnection connection
    - DataRepository repository
    + Application(DatabaseFactory factory)
    + run()
}

Application --> DatabaseFactory
DatabaseFactory --> DatabaseConnection
DatabaseFactory --> DataRepository

```

```java
// Interface para a conexão com o banco de dados
interface DatabaseConnection {
    void connect();
}

// Implementações para diferentes bancos
class MySQLConnection implements DatabaseConnection {
    public void connect() {
        System.out.println("Conectando ao MySQL...");
    }
}

class MongoDBConnection implements DatabaseConnection {
    public void connect() {
        System.out.println("Conectando ao MongoDB...");
    }
}

// Interface para operações de repositório (CRUD)
interface DataRepository {
    void save(String data);
    String find(int id);
}

// Implementações concretas para cada banco de dados
class MySQLRepository implements DataRepository {
    public void save(String data) {
        System.out.println("Salvando no MySQL: " + data);
    }

    public String find(int id) {
        return "Dados do MySQL com ID " + id;
    }
}

class MongoDBRepository implements DataRepository {
    public void save(String data) {
        System.out.println("Salvando no MongoDB: " + data);
    }

    public String find(int id) {
        return "Dados do MongoDB com ID " + id;
    }
}

// Abstract Factory para criar conexões e repositórios
interface DatabaseFactory {
    DatabaseConnection createConnection();
    DataRepository createRepository();
}

// Implementações concretas da fábrica para MySQL
class MySQLFactory implements DatabaseFactory {
    public DatabaseConnection createConnection() {
        return new MySQLConnection();
    }

    public DataRepository createRepository() {
        return new MySQLRepository();
    }
}

// Implementações concretas da fábrica para MongoDB
class MongoDBFactory implements DatabaseFactory {
    public DatabaseConnection createConnection() {
        return new MongoDBConnection();
    }

    public DataRepository createRepository() {
        return new MongoDBRepository();
    }
}

// Aplicação que usa a fábrica abstrata
class Application {
    private DatabaseConnection connection;
    private DataRepository repository;

    public Application(DatabaseFactory factory) {
        this.connection = factory.createConnection();
        this.repository = factory.createRepository();
    }

    public void run() {
        connection.connect();
        repository.save("Dado de teste");
        System.out.println(repository.find(1));
    }
}

// Teste da implementação
public class Main {
    public static void main(String[] args) {
        DatabaseFactory mysqlFactory = new MySQLFactory();
        Application mysqlApp = new Application(mysqlFactory);
        System.out.println("Sistema com MySQL:");
        mysqlApp.run();

        System.out.println("\n---------------------\n");

        DatabaseFactory mongoFactory = new MongoDBFactory();
        Application mongoApp = new Application(mongoFactory);
        System.out.println("Sistema com MongoDB:");
        mongoApp.run();
    }
}
```

## Conclusão
O padrão **Abstract Factory** é uma solução poderosa para criar famílias de objetos relacionados, mantendo a consistência e a flexibilidade do código. Ele é ideal para sistemas que precisam suportar múltiplas variações de produtos, desde que a complexidade adicional seja gerenciável.

## Referências

- GAMMA, Erich et al. Padrões de projeto [recurso eletrônico]: soluções reutilizáveis de software orientado a objetos. Tradução de Luiz A. Meirelles Salgado. Porto Alegre: Bookman, 2007.
- SHVETS, Alexander. Mergulho no agulho nos padrões de projeto. v.2021-1.16. 2021.
- ORACLE. Data Access Object (DAO). Disponível em: https://www.oracle.com/java/technologies/dataaccessobject.html. Acesso em: 5 fev. 2025.
