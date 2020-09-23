# EJB com Jakarta EE: API Rest com o servidor Wildfly

## Do Java EE ao Jakarta

•	Houve uma transição do Java EE para o Jakarta EE
•	Precisamos de um servidor de aplicação para deployar nossa aplicação.


 
## Apresentando EJB’s e o projeto de agendamento de e-mails

•	Em uma aplicação corporativa, utilizamos diversos recursos de infraestrutura, tais como transação, pool de conexões, filas entre outros.
•	Para que o desenvolvedor foque nas regras negociais da aplicação e menos com questões de infraestrutura, criou-se um recurso, que é uma especificação do Jakarta EE, chamado EJB (Enterprise Java Beans).
•	O EJB faz com que haja a inversão de controle, passando a responsabilidade de instanciar objetos e fornecimento de serviços para o servidor de aplicação. Para criar um EJB basta anotar uma classe com @Stateless


#### Diferença entre as anotações @EJB e @Inject

Podemos fazer o uso da injeção de dependências, usando a anotação @Inject, para obter uma instância de um EJB. Dessa maneira não precisamos nos preocupar em instanciar um objeto “na mão”, ou seja, utilizando a palavra reservada new e o resultado disto é que temos um menor acoplamento entre as classes. Além da anotação @Inject, nós temos a anotação @EJB, que entrega o mesmo resultado citado acima. A diferença é que a anotação @EJB só funcionará se você estiver trabalhando com um EJB, caso contrário ocorrerá uma exceção. Já a anotação @Inject funcionará para EJB e, também, para outros recursos habilitados para injeção de dependência.


## Persistência com JPA

•	Usando o JPA com EJB, vimos que o servidor de aplicação é responsável por fornece uma instância do EntityManager.
•	Temos que informar, para o nosso servidor de aplicação, qual banco de dados iremos acessar e essa configuração é feita em um recurso chamado DataSource.
•	Uma vez configurado, podemos fazer as operações com o banco de dados, como inserts, deletes, entre outros.


## Fazendo requisição ao nosso EJB com JAX-RS

•	Para fazer requisições para nossa aplicação, utiliza-se o protocolo HTTP.
•	Para abstrair a complexidade do protocolo, o Jakarta EE nos provê um recurso chamado JAX-RS.
•	O JAX-RS fornecerá um conjunto de classes, interfaces e anotações.
•	Com pequenas configurações, podemos definir o método da requisição, o tipo de formato que a aplicação irá consumir e/ou produzir, qual status a resposta da requisição irá retornar, entre outras facilidades.


## Agendamento com TimerService

•	Com a anotação @Schedule, do EJB, conseguimos facilmente criar um agendamento de execuções de um determinado serviço.
•	Temos que entender o cenário proposto e verificar se aquela atividade permite concorrência ou não.
•	Caso o cenário proposto não permita, temos a opção de criar uma única instância do objeto e compartilhá-la para todo o sistema. Isso é feito através do componente Singleton do EJB e para utilizá-lo, basta que anotemos nossa classe com @Singleton.


#### Tipos de EJB

Vimos dois componentes do EJB, o Singleton e o Stateless. Ambos são componentes Session Beans, que geralmente são usados para escrita das regras negociais do sistema. Além destes, existe o Stateful. Para entender melhor, vamos conhecer cada um.
Stateless: Ocorre para cenários em que a requisição não precisa guardar estado. São invocados, processados e retornam o resultado para o cliente que o requisitou. Stateful: Ocorre para cenários em que a requisição deve guardar estado. A invocação é feita e deve durar mais que uma chamada. Um exemplo seria o carrinho de compras de sites e-commerce, onde o usuário pode colocar itens no carrinho e voltar depois para finalizar a compra. Singleton: A instância é criada apenas uma vez e disponibilizada para todo o sistema. Qualquer alteração feita em um componente Singleton estará visível para todos os usuários da aplicação.
Além dos componentes citados acima, temos, também, o MDB (Message-Driven Beans). Não está ligado diretamente ao cliente, ou seja, um cliente não faz uma requisição diretamente para um MDB. Eles seguem o paradigma de comunicação assíncrona, em que a interação se dá através de troca de mensagens. A requisição será feita por um provedor (ou broker) que fará o envio de mensagens e o MDB deverá processá-las. Um provedor é uma aplicação que cria, gerencia e transmite mensagens através do JMS (Java Message Service) que é uma especificação do Jakarte EE.
Por fim temos o Entity Beans, que é um componente usado para persistência de dados. De forma resumida, as entidades do sistema são dados que serão armazenados em uma estrutura de dados, como um banco de dados. Cada instância do bean equivale a um registro nessa estrutura. A vantagem de se trabalhar com entidades, é que não precisamos nos preocupar com a manipulação dos dados diretamente no banco de dados. Outra vantagem é o controle dos serviços por parte do servidor de aplicação.


## Mensageria com JMS

•	Podemos fazer o envio de mensagens de forma assíncrona.
•	Para o correto funcionamento dessa abordagem, precisamos configurar um recurso, no nosso servidor de aplicação ou em um repositório externo, que se chama Fila JMS.
•	JMS é uma especificação do Jakarta EE e significa Jakarta Message Service.
•	É necessário configurarmos um producer, classe responsável pelo envio das mensagens para a fila.
•	Uma vez persistida as mensagens na fila, podemos criar um consumer, classe responsável por consumir e processar as mensagens.


#### Topic x Queue

Uma fila do tipo Queue para receber as nossas mensagens. Uma característica dessa abordagem é que a mensagem será recebida, exatamente, por um consumidor.Além deste tipo, existe o Topic. Ele implementa uma semântica de publicação e assinatura. Quando a mensagem é publicada, ela vai para todos os assinantes que estão interessados, logo, zero a n assinantes poderão receber a mensagem.
Material para apoio: https://activemq.apache.org/how-does-a-queue-compare-to-a-topic

