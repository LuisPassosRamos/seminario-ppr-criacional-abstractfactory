# Abstract Factory

## Intenção
Permite a criação de famílias de objetos relacionados ou dependentes sem especificar suas classes concretas.

## Também conhecido como
- Kit
- Fábrica de fábricas

## Motivação
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


## Estrutura

```plantuml

    class WidgetFactory {
        +CreateScrollBar()
        +CreateWindow()
    }
    class MotifWidgetFactory extends WidgetFactory  {
        +CreateScrollBar()
        +CreateWindow()
    }
    class PMWidgetFactory  extends WidgetFactory{
        +CreateScrollBar()
        +CreateWindow()
    }
    class Client {
        +operation()
    }
    class ScrollBar
    class MotifScrollBar extends ScrollBar
    class PMScrollBar extends ScrollBar
    class Window
    class MotifWindow extends Window
    class PMWindow  extends Window

    Client --> Window
    Client --> ScrollBar
    MotifWidgetFactory --> MotifScrollBar
    MotifWidgetFactory --> MotifWindow
    PMWidgetFactory --> PMScrollBar
    PMWidgetFactory --> PMWindow
```

## Participantes

### WidgetFactory (Fábrica Abstrata)
- Define uma interface para criar famílias de objetos relacionados, como `CreateScrollBar()` e `CreateWindow()`.

### MotifWidgetFactory e PMWidgetFactory (Fábricas Concretas)
- Implementam a interface `WidgetFactory` para criar produtos específicos.
  - **MotifWidgetFactory**: Cria instâncias de `MotifScrollBar` e `MotifWindow`.
  - **PMWidgetFactory**: Cria instâncias de `PMScrollBar` e `PMWindow`.

### ScrollBar e Window (Produtos Abstratos)
- Representam interfaces ou classes abstratas para os tipos de produtos que serão criados.
  - **ScrollBar**: Interface para barras de rolagem.
  - **Window**: Interface para janelas.

### MotifScrollBar, PMScrollBar, MotifWindow e PMWindow (Produtos Concretos)
- Implementam os produtos abstratos definidos por `ScrollBar` e `Window`.
  - **MotifScrollBar** e **PMScrollBar**: Implementações concretas do produto `ScrollBar`.
  - **MotifWindow** e **PMWindow**: Implementações concretas do produto `Window`.

### Client
- Utiliza apenas as interfaces fornecidas por `WidgetFactory`, `ScrollBar` e `Window` para criar e usar os objetos. 


## Outro exemplo

```plantuml

title Fábrica de Marcas

class Actor

interface IDeviceFactory {
    +createPhones()
    +createWatch()
}

class AndroidFactory {
    +createPhones(): AndroidPhone
    +createWatch(): AndroidWatch
}

class AppleFactory {
    +createPhones(): IPhone
    +createWatch(): IWatch
}

class Phone {
    +getDetails(): string
}

class Watch {
    +getDetails(): string
}

class AndroidPhone {
    +getDetails(): string
}

class IPhone {
    +getDetails(): string
}

class AndroidWatch {
    +getDetails(): string
}

class IWatch {
    +getDetails(): string
}

Actor --> IDeviceFactory

IDeviceFactory <|-- AndroidFactory
IDeviceFactory <|-- AppleFactory

AndroidFactory --> AndroidPhone
AndroidFactory --> AndroidWatch

AppleFactory --> IPhone
AppleFactory --> IWatch

AndroidPhone --|> Phone
IPhone --|> Phone

AndroidWatch --|> Watch
IWatch --|> Watch
```

## Participantes

### IDeviceFactory (Fábrica Abstrata)
- Define a interface para a criação de famílias de produtos relacionados, como `createPhones()` e `createWatch()`.

### AndroidFactory e AppleFactory (Fábricas Concretas)
- Implementam a interface `IDeviceFactory` para criar produtos específicos.
  - **AndroidFactory**: Cria instâncias de `AndroidPhone` e `AndroidWatch`.
  - **AppleFactory**: Cria instâncias de `IPhone` e `IWatch`.

### Phone e Watch (Produtos Abstratos)
- São classes ou interfaces que definem os tipos de produtos criados pelas fábricas.
  - **IPhone**: Interface base para os diferentes tipos de telefones.
  - **IWatch**: Interface base para os diferentes tipos de relógios.

### AndroidPhone, IPhone, AndroidWatch e IWatch (Produtos Concretos)
- Implementam as interfaces ou classes abstratas definidas por `Phone` e `Watch`.
  - **AndroidPhone**: Implementação concreta do produto `Phone` para dispositivos Android.
  - **ApplePhone**: Implementação concreta do produto `Phone` para dispositivos Apple.
  - **AndroidWatch**: Implementação concreta do produto `Watch` para dispositivos Android.
  - **AppleWatch**: Implementação concreta do produto `Watch` para dispositivos Apple.


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

## Conclusão
O padrão **Abstract Factory** é uma solução poderosa para criar famílias de objetos relacionados, mantendo a consistência e a flexibilidade do código. Ele é ideal para sistemas que precisam suportar múltiplas variações de produtos, desde que a complexidade adicional seja gerenciável.

## Referências

- GAMMA, Erich et al. Padrões de projeto [recurso eletrônico]: soluções reutilizáveis de software orientado a objetos. Tradução de Luiz A. Meirelles Salgado. Porto Alegre: Bookman, 2007.
- SHVETS, Alexander. Mergulho no agulho nos padrões de projeto. v.2021-1.16. 2021.
- ORACLE. Data Access Object (DAO). Disponível em: https://www.oracle.com/java/technologies/dataaccessobject.html. Acesso em: 5 fev. 2025.