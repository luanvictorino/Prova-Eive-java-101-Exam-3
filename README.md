1) No Maven o arquivo `pom.xml` é o principal arquivo de configuração de dependencias. Sabendo disso leia as afirmativas abaixo.

I - A dependências podem ser adicionadas diretamente na pasta lib do projeto e este após compilar continua reconhecendo.
II - A dependências seguem um padrão de organização, sempre devem ser adicionadas na sessão `<dependencies>` do arquivo.
III - Sempre que uma nova depenência é adicionada no projeto o `Mavem` identifica e baixa de forma altomatica sem que o desenvolvedor precise se preocupar.

Assinale a alternativa que corresponte apenas as informações verdadeiras.

- [ ] III, apenas
- [ ] I e II
- [ ] I e III 
- [ ] II e III
- [x] I, II e III


2) No `Maven` existem alguns comandos que são conhecidos como `goal`, com base no seu conhecimento, leia as afirmativas abaixo e assinale a alternativa correta

I - O comando `mvn test` tem como finalidade executar os testes do projeto
II - O comando `mvn deploy production` é usado quando precisa gerar o artefado para o ambiente de produção
III - `mvn install` é o goal que realiza a instalação do projeto no diretório local

- [ ] I, apenas
- [ ] II apenas
- [ ] I e II apenas
- [x] I e III apenas
- [ ] II e III, apenas

 
3) Supondo que existe um banco de dados onde neste banco de dados existe uma tabela com o nome `Produto` e nesta tabela temos dois registros: 
 
 ID = 1
 NOME = TELEVISÃO
 DESCRICAO = TV 52"

 ID = 2
 NOME = RÁRIO
 DESCRICAO = RÁDIO AM/FM

 Dado o seguinte código, qual o resultado é impresso no console?
```java
  
  Statement stmt = connection.createStatement();
  stmt.execute("Select id, nome, descricao From Produto");
  ResultSet rst = stmt.getResultSet();

  while(rst.next()){
      System.out.print(rst.getInt("id"));
      System.out.print(rst.getString("nome"));
      System.out.print(rst.getString("descricao"));
      System.out.println("");
  }
  connection.close();
  
```
  * Resposta:
```text
1TELEVISÃOTV 52"

2RÁDIORÁDIO AM/FM
```

4) Veja o comando abaixo, como ele poderia ser adaptado para imprimir o código gerado na inclusão do registro?
```java
  Statement stmt = connection.createStatement();
  stmt.execute("INSERT INTO Produto VALUES ('Cadeira', 'Cadeira 4 pernas')");  
```

```java
  Statement stmt = connection.createStatement();
  stmt.execute("INSERT INTO Produto (NOME, DESCRICAO) VALUES ('Cadeira', 'Cadeira 4 pernas')",
    Statement.RETURN_GENARETED_KEYS);

  ResultSet rst = stmt.getGeneratedKeys();
  while(rst.next()){
    Integer id = rst.getInt(1);
    System.out.println("ID gerado: " + id);
  }  
```

5) Oque é um pool de conexão e qual sua vantagem? Explique.
  * O pool mantém um número definido de conexões abertas, quando o cliente abre a conexão através do pool, ele pega uma dessas conexões que já estão abertas em vez de abrir uma nova conexão.
  Em casos em que diversos usuários podem acessar uma mesma aplicação a abordagem mais recomendada é o pool de conexões, pois assim abrimos várias conexões e reaproveitamos.

6) De um exemplo de uso de pool de conexão usando a biblioteca c3p0
  * Uma das formas mais simples de usar a biblioteca c3p0 é baixando a biblioteca(.jar) e adicionando no projeto.
  * Dentro da sua classe de conexão, você pode criar o pool de conexões da seguinte forma:
  ```java
    public class ConnectionFactory{
      public Datasource datasource;

      //construtor da minha classe
      public ConnectionFactory(){
        //Instânciando o a classe para configurar o pool de conexões
        ComboPooledDataSource comboPooledDs = new ComboPooledDataSource();
        comboPooledDs.setJdbcUrl({sua_string_se_sonexao});
        comboPooledDs.setUser("{seu_usuario}");
        comboPooledDs.setPassword("{sua_senha}");
        //setando quantas conexões serão carregadas no meu pool de conexões
        comboPooledDs.SetMaxPoolSize(10);

        this.datasource = comboPooledDs; 
      }

      //método que retorna minha conexão, pode lançar a exception SQLException
      public Connection recuperarConexao() throws SQLException {
        //essa conexão já vai estar aberta no meu pool de conexões
        return this.datasource.getConnection();
      }
    }

    //Classe para testar o pool
    public class TestaPoolDeConexoes{
      public static void main(String[] args){
        //Instanciando a classe que vai criar meu pool de conexões
        ConnectionFactory connectionFactory = new ConnectionFactory();
        try {
          //Recuperando minha conexão do pool
          Connection connection = connectionFactory.recuperarConexao();
          //Fechando a conexão
          connection.close();
        } catch(SQLException e){
          e.printStackTrace();
        }
      }
    }
    
  ```
  * Obs* Enchi de comentários por ser uma prova e para fins explicativos, no dia a dia eu não faria isso e sei que polui o código.

7) Quais as desvantagens de usar o JDBC em uma aplicação?
  * Em relação ao JPA, o JDBC é bem mais verboso, precisamos de mais linhas de códigos para fazermos operações simples e o seu alto acomplamento com o banco de dados também é um problema, se tiver alguma alteração na estrutura da tabela no banco, precisaremos alterar a classe DAO também.

8) Explique de forma suscinta como funciona o ciclo de vida de uma aplicação JPA.
  * Quando instânciamos uma entidade JPA, por ser uma entidade nova que nunca foi persistida ela inicia no estado "Transient". Esse estado muda para "Managed" quando ela passa a ser gerenciada pela JPA através do método .persist(), esse é o estado mais comum de uma entidade JPA, enquanto a entidade estiver no estado Managed podemos chamar o método .commit() para commitar as alterações no banco ou o método .flush() que sincroniza os dados com o banco de dados mas não commita. Se chamarmos o método .close() que fecha o gerenciador da entidade ou o método .clear() que limpa as entidades gerenciadas, a nossa entidade passa para o estado "Detached", ou seja, para um estado em que a entidade foi criada mas não está mais sendo gerenciada.

9) Sobre JPA explique a diferença entre o uso da importação de `javax.persistence.Entity` e `org.hibernate.annotations.Entity`.
  * `javax.persistence.Entity`: Anotação da entidade do JPA. Você vai programar voltado a especificação e não a implementação.
  * `org.hibernate.annotations.Entity`: Anotação do Hibernate. Com ela você vai programar voltado a implementação, quando precisa trocar a implementação você precisa alterar as classes e entidades.

10) Dado a estrutura de uma tabela `Carros`, escreva como seria o mapeamento em JAVA usando o JPA 

```SQL
   Create Table Cores(
      ID_COR INT auto_increment,
      NOME VARCHAR(50),
      PRIMARY KEY(ID_COR)
   );

   Create Table Carros(
      ID_CARRO INT auto_increment,
      MARCA VARCHAR(50),      
      MODELO VARCHAR(50),
      PLACA VARCHAR(10),
      ID_COR INT,
      PRIMARY KEY(ID_CARRO)
   );

   ALTER TABLE Carros ADD FOREIGN KEY(ID_COR) REFERENCES Cores (ID_COR);
```

* Classe Carro:
```JAVA
  @Entity
  @Table(name = "Carros")
  public class Carro {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Integer id_carro;
    private String marca;
    private String modelo;
    private String placa;
    private Integer id_cor;

    @ManyToOne
    private Cores cores;

    public Integer getId_carro() {
      return id_carro;
    }

    public void setId_carro(Integer id_carro){
      this.id_carro = id_carro;
    }

    public String getMarca() {
      return marca;
    }

    public void setMarca(String marca){
      this.marca = marca;
    }

    public String getModelo() {
      return modelo;
    }

    public void setModelo(String modelo){
      this.modelo = modelo;
    }

    public String getPlaca() {
      return placa;
    }

    public void setPlaca(Integer placa){
      this.placa = placa;
    }

    public Integer getId_cor() {
      return id_cor;
    }

    public void setId_cor(Integer id_cor){
      this.id_cor = id_cor;
    }
  }
```

* Classe Cores:
```JAVA
  @Entity
  @Table(name = "Cores")
  public class Cor {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id_cor;
    private String nome;

    public Integer getId_cor() {
      return id_cor;
    }

    public void setId_cor(Integer id_cor){
      this.id_cor = id_cor;
    }

    public String getNome() {
      return nome;
    }

    public void setNome(String nome){
      this.nome = nome;
    }
  }
```

11) Sobre o ciclo de vida de uma Entidade, Explique cada um dos estados `TRANSIENT`, `MANAGED` e `DETACHED`.
  * `TRANSIENT`: Quando instanciamos uma entidade JPA ela ainda não é reconhecida porque nunca passou pelo gerenciador de entidades(EntityManager). O JPA enxerga o objeto como qualquer outro objeto do Java que não tenha nada a ver com persistência. Esse é o primeiro estado de uma entidade e se você não informá-la pro gerenciador, nada será salvo no banco após o commit.
  * `MANAGED`: Após informar a entidade pro gerenciador através do método .persist() da classe EntityManager, ele passa do estado Transient pro estado Managed, esse é o estado mais comum de uma entidade. Nesse estado a entidade está sendo gerenciada pela JPA.
  * `DETACHED`: Quando você fecha o EntityManager através do método .close() a entidade passa do estado Managed pro estado Detached. Qualquer alteração que você fizer na entidade enquanto ela estiver nesse estado não será persistido no banco de dados. 

12) Qual a finalidade do método `merge()` no JPA. Escreva um exemplo simples para mostrar seu uso.
  * Se for chamado quando o estado da entidade for Detached ela volta pro estado Managed. Se chamado enquanto a entidade estiver no estado Managed nada acontece.

13) Usando a sintaxe JPQL como poderia ser adaptado o código abaixo para listar todos os dados da tabela `Carros`

```java
public Lista<Carro> findAll(){
    return null;
}
```

```java

  public List<Carro> findAll(){
    String consulta = "SELECT p FROM Carro p";
    return entityManager.createQuery(consulta, Carro.class).getResultList();
  }
```
   
14) Usando a sintaxe JPQL avalie o código abaixo e sinalize quais pontos estão errados no código

```java
  public Lista<Pessoa> findById(Long id) {
      String consulta = "Select * from Pessoas P Where P.ID_PESSOA = ?";
      entityManager.createStatement(consulta);
      return entityManager.createQuery(consulta, Pessoa.class)
         .setParameter(1, 1)
         .getResultList(); 
  } 
```
  * No lugar de "Lista" seria "List".
  * No select da consulta, no lugar de * deve ser informado o alias da tabela (P).
  * Se usarmos o parâmetro "?" na consulta, devemos indicar qual parâmetro ele se refere, exemplo: "?1".
  * No setParameter da forma como está irá funcionar, mas não é recomendado passar fixo como está no código, e sim o parâmetro que será passado na chamada do método.

15) Qual a função do `mappedBy` e onde ele deve ser usado?
  * É Usado para indicar que se trata de um relacionamento bidirecional.
  Quando você tem um relacionamento bidirecional, você deve usar o mappedby do lado do @OneToMany para evitar que seja gerada uma nova tabela.

16) Escreva um exemplo de como fazer um `JOIN` no JPQL?
  Supondo que tenho as seguintes tabelas no banco:
  ```SQL
   Create Table Cores(
      ID_COR INT auto_increment,
      NOME VARCHAR(50),
      PRIMARY KEY(ID_COR)
   );

   Create Table Carros(
      ID_CARRO INT auto_increment,
      MARCA VARCHAR(50),      
      MODELO VARCHAR(50),
      PLACA VARCHAR(10),
      ID_COR INT,
      PRIMARY KEY(ID_CARRO)
   );

   ALTER TABLE Carros ADD FOREIGN KEY(ID_COR) REFERENCES Cores (ID_COR);
  ```
  O meu `JOIN` no JPQL ficaria da seguinte forma:
  ```Java
    public List<Object[]> ExemploJoinModeloCarro(){
      String jpql = "SELECT car.modelo, " +
                    "       cor.nome " +
                    "  FROM carros car" +
                    "  JOIN car.cor cor ";
      return entityManager.createQuery(jpql, Object[].class).getResultList();
    }
  ```

17) Qual a função do `select new` no JPA?
  * Bastante utilizado quando precisamos fazer uma consulta para algum relatório.
  * Supondo que tenho uma classe(que nesse caso não é uma entidade) com os atributos do relatório que preciso, não podemos dar select nessa classe por não se tratar de uma entidade. Como não podemos dar um select nessa classe, podemos usar o `select new` que cria uma instância da classe e vai passar os valores retornados na consulta pro construtor dessa classe.

18) Considerando que no seu projeto tem a seguinte estrutura de pacotes:
```java
  br.com.curso.modelo
         Carro.java
         Cores.java
  br.com.curso.dao
         CarroDao.java
         CoresDao.java
```

  Escreva um método que exemplifique uma consulta usando `select new` do JPA, descreva se precisa de novas classes e se for preciso onde cada uma delas deve ser organizada no projeto seguindo as boas praticas.

```java
  public List<RelatorioCarroVo> relatorioCarro() {
    String jpql = "SELECT new br.com.curso.vo.RelatorioCarroVo( " +
                  "       carro.marca, " +
                  "       carro.modelo, " +
                  "       carro.placa, " +
                  "       cor.nome) " +
                  "  FROM Carro carro" +
                  "  JOIN carro.cor cor ";
    return entityManager.CreateQuery(jpql, RelatorioCarroVo.class).getResultList();
  }
                      
```

19) Sobre a configuração de conexão com banco de dados no Spring leia as afirmativas abaixo:

I. Para conexão com o banco de dados é necessário apenas adicionar as dependências no arquivo `pom.xml`
II. As configurações de acesso ao banco de dados devem ficar no arquivo `application.propriedades`
III. A definição em `spring.jpa.hibernate.ddl-auto=update` é usada para dar atualizar as informações do banco de dados

Assinale apenas as alternativas que correspondem a afirmações verdadeiras

- [ ] I, apenas
- [ ] I e II, apenas
- [ ] II e III, apenas
- [ ] I, II e III
- [x] nenhuma das afirmativas

20) Escreva um exemplo de implementação e uso de um `CrudRepository` para a classe da entidade `Cores.java`.
Classe CorRepository:
```Java
    //...Imports

    @Repository
    public interface CorRepository extends CrudRepository<Cor, Integer> {

    }
```

Classe CrudCorService:
```Java
    //...Imports

    @Service
    public class CrudCorService {
      private final CorRepository corRepository;

      public CrudCorService(CorRepository corRepository){
        this.corRepository = corRepository;
      }

      private void visualizar() {
        Iterable<Cor> cores = coresRepository.findAll();
        cores.forEach(cor -> System.out.println(cor));
      }

      private void deletar(Integer id){
        corRepository.deleteById(id);
      }

      private void salvar(Cor cor){
        corRepository.save(cor);
      }
    }
```

21) Explique como funciona as `Derived Query` e de exemplos de métodos com o uso das opções: `E` , `OU` e `MAIOR QUE`.

Derived Query permite fazer consultas no banco sem utilizar comandos sql.
```Java
    //...Imports

    @Repository
    public interface CorRepository extends CrudRepository<Cor, Integer> {
      List<Cor> findById_corAndCor(Integer id_cor, String nome);
      List<Cor> findById_corOrCor(Intreger id_cor, String nome);
      List<Cor> findById_corGreaterThan(Integer id_cor);
    }
```

22) Explique qual a aplicabilidade ideal para os Repository `CrudRepository`, `PaginAndSortingRepository` e `JpaRepository`. Escreva a forma de como declarar a assinatura de cada um deles.
  * CrudRepository: Usado para realizar métodos CRUD sem precisar criar objetos do JPA.
  * Assinatura:
  ```Java
      public interface CarroRepository extends CrudRepository<Carro, Integer> {

      }
  ```
  * Sendo o "CarroRepository" o nome da minha interface, o parâmetro "Carro" é a entidade que representa a tabela que quero manipular e o "Integer" o tipo do ID informado na minha entidade. 


  * PaginAndSortingRepository: Usado para paginar as consultas de dados da entidade.
  * Assinatura:
  ```Java
      public interface CarroRepository extends PagingAndSortingRepository<Carro, Integer> {

      }
  ```
  * Assinatura bastante similar a assinatura de um CrudRepository, a única diferença é que extende PagingAndSortingRepository.


  * JpaRepository: Extende crudRepository e PaginAndSortingRepository além de conter alguns métodos extras como o método flush().
  * Assinatura:
  ```Java
      public interface CarroRepository extends JpaRepository<Carro, Integer> {

      }
  ```
  * Assinatura bastante similar as outras duas anteriores, a diferença é que extende de JpaRepository.

23) Dado o código abaixo:
```SQL
   Create Table Cores(
      ID_COR INT auto_increment,
      NOME VARCHAR(50),
      PRIMARY KEY(ID_COR)
   );

   Create Table Carros(
      ID_CARRO INT auto_increment,
      MARCA VARCHAR(50),      
      MODELO VARCHAR(50),
      PLACA VARCHAR(10),
      ID_COR INT,
      PRIMARY KEY(ID_CARRO)
   );

   ALTER TABLE Carros ADD FOREIGN KEY(ID_COR) REFERENCES Cores (ID_COR);
```
  Escreva um exemplo de método que use uma `projeção` que exiba os dados dos carros com cor azul, os campos exibidos devem ser:
     Marca, Modelo, Placa e nome da cor

```Java
    //...Imports

    public interface CarroProjecao{
      String getMarca();
      String getModelo();
      String getPlaca();
      String getCor();
    }
```

```Java
    //...Imports
    @Repository
    public interface CarroRepository extends CrudRepository<Carro, Integer> {
      @Query(value = "SELECT ca.marca, " +
                     "       ca.modelo, " + 
                     "       ca.placa, " +
                     "       cor.nome " +
                     "  FROM carros ca " + 
                     "  JOIN cores cor " +
                     "    ON cor.id_cor = ca.id_cor " +
                     " WHERE cor.nome = :cor",
             nativeQuery = true)
      List<CarroProjecao> findCarrosPorCor(String cor);
    }
```

24) Explique para que servem as anotações `@Repository`, `@Service`, `@Controller`, `@SpringBootApplication`.
  * `@Repository`: Essa anotação serve para informar que a classe irá atuar como um repositório de banco de dados.
  * `@Service`: Usado para informar que a classe pertence a camada de serviço da aplicação.
  * `@Controller`: Usado para informar que é uma classe do Spring MVC e usado para redirecionar views.
  * `@SpringBootApplication`: Contém as anotações @Configuration, @EnableAutoConfiguration e @ComponentScan.

25) Escreva uma classe que contenha um método REST que retorna uma lista fixa de Carros, utilize o padrão DTO.
  ```Java
  //Imports
    @RestController
    @RequestMapping("/carros")
    public class CarroController {
      
      @getMapping
      public List<CarroDto> getLista() {
        List<CarroDto> carros = new ArrayList<CarroDto>();
        Carro saveiro = new Carro("volksWagen", "caminhonete", "AVA5747");
        carros.add(saveiro);
        Carro fusca = new Carro("volksWagen", "convensional", "BTO8972");
        carros.add(fusca);
        
        return carros;
      }
    }
  ```

26) Adapte seu código da atividade anterior para um modelo que usa o padrão `Repository` e retorne uma lista de Carros. 
  ```Java
  //Imports
    @RestController
    @RequestMapping("/carros")
    public class CarroController {

      @Autowired
      private CarroRepository carroRepository;

      @GetMapping
      public List<CarroDto> getLista() {
        List<Carro> carros = carroRepository.findAll();
        return CarroDto.converter(carros);
      }
    }
  ```

27) Qual a finalidade do `Bean Validation`, Explique e de um exemplo do seu uso em código com a annotation `@Valid`
  * A finalidade do Bean Validation é uma especificação do java para tratar validações de dados de classes da camada de modelo.

  ```Java
  //Imports
    public class CarroForm {
      @NotNull @NotEmpty @Length(min = 5)
      private String nome;
      @NotNull @NotEmpty @Length(min = 5)
      private String modelo;
      @NotNull @NotEmpty @Length(min = 8)
      private String placa;

      public String getNome(){
        return nome;
      }

      public void SetNome(String nome){
        this.nome = nome;
      }

      public String getModelo(){
        return modelo;
      }

      public void SetModelo(String modelo){
        this.modelo = modelo;
      }

      public String getPlaca(){
        return placa;
      }

      public void SetPlaca(String placa){
        this.placa = placa;
      }

      public Carro converter(CarroRepository carroRepository){
        return new Carro(this.marca, this.modelo, this.placa);
      }
    }
  ```

  ```Java
  //Imports
    @RestController
    @RequestMapping("/carros")
    public class CarroController {
  
      @Autowired
      private CarroRepository carroRepository;
    
      @PostMapping
      public ResponseEntity<CarroDto> cadastrar(@RequestBody @Valid CarroForm form, UriComponentsBuilder uriBuilder{
        Carro carro = carroForm.converter(carroRepository);
        carroRepository.save(carro);
      
        URI uri = uriBuilder.path("/topicos/{id}").buildAndExpand(topico.getId()).toUri();
        return ResponseEntity.created(uri).body(new CarroDto(carro));
      }

}
  ```

28) Explique para que é usado a annotation `@ExceotionHandler` e de um exemplo de método que utiliza ela.
  * Essa annotation serve para informar que o método que a usa será executado quando essa exceção for lançada. 
  Classe ErroDeValidacaoHandler:
  ```Java
  @RestControllerAdvice
  public class ErroDeValidacaoHandler {
    @ResponseStatude(code = HttpStatus.BAD_REQUEST)
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public List<ErroDeFormularioDto> handle(MethodArgumentNotValidException exception){
      List<ErroDeFormularioDto> dto = new ArrayList<>();

      List<FieldError> fieldErrors = exception.getBindingResult().getFieldErrors();
      fieldErrors.forEach(e -> {
        String mensagem = messageSource.getMessage(e, LocaleContextHolder.getLocale());
        ErroDeFormularioDto erro = new ErroDeFormularioDto(e.getField(), mensagem);
        dto.add(erro);
      });

      return dto;
    }
  }
  ```
  Classe ErroDeFormularioDto:
  ```Java
  public class ErroDeFormularioDto {
    private String campo;
    private String erro;

    public ErroDeFormularioDto(String campo, String erro){
      this.campo = campo;
      this.erro = erro;
    }

    public String getCampo(){
      return campo;
    }

    public String getErro(){
      return erro;
    }
  }
  ```
