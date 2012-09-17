# Princípios para Escrever JavaScript de forma Consistente e Idiomática

Este documento é um guia de estilos para ser utilizado para projetos da MobVox.

## Índice

 * [Espaço em branco](#whitespace)
 * [Sintaxe bonita](#spacing)
 * [Exemplo](#practical)
 * [Nomenclatura](#naming)
 * [Comentários](#comments)
 * [Código em apenas um idioma](#language)

------------------------------------------------

## Manifesto de estilo idiomático


1. <a name="whitespace">Espaço em branco</a>
  - Nunca misture espaços e tabs.
  - Quando começar um projeto, antes de escrever qualquer código, escolha entre indentação suave (espaços) ou tabulação real (tabs), considere isso como **lei**.

2. <a name="spacing">Sintaxe bonita</a>

  A. Parênteses, chaves e quebras de linhas

  ```javascript

  // if/else/for/while/try sempre tem espaços, chaves e ocorrem em múltiplas linhas
  // isso facilita a legibilidade

  // 2.A.1.1
  // Exemplos de código pouco claro/bagunçado

  if(condicao) facaAlgo();

  while(condicao) iteracao++;

  for(var i=0;i<100;i++) algumaIteracao();


  // 2.A.1.1
  // Use espaço em branco para facilitar a leitura

  if (condicao) {
    // instruções
  }

  while (condicao) {
    // instruções
  }

  for (var i = 0; i < 100; i++) {
    // instruções
  }

  // Melhor ainda:

  var i,
    length = 100;

  for (i = 0; i < length; i++) {
    // instruções
  }

  // Ou...

  var i = 0,
    length = 100;

  for (; i < length; i++) {
    // instruções
  }

  var prop;

  for (prop in object) {
    // instruções
  }


  if (true) {
    // instruções
  } else {
    // instruções
  }
  ```

  B. Atribuições, declarações, funções (nomenclatura, expressão, construtor)

  ```javascript

  // 2.B.1.1
  // Variáveis
  var foo = "bar",
    num = 1,
    undef;

  // Notações literais:
  var array = [],
    object = {};


  // 2.B.1.2
  // Utilizando apenas um `var` por escopo (função) promove legibilidade
  // e mantém a sua lista de declaração livre de desordem (além de evitar algumas tecladas)

  // Ruim
  var foo = "";
  var bar = "";
  var qux;

  // Bom
  var foo = "",
    bar = "",
    quux;

  // ou..
  var // comentário aqui
  foo = "",
  bar = "",
  quux;


  // 2.B.1.3
  // declarações de variáveis devem sempre estar no início de seu respectivo escopo (função)
  // O mesmo deve acontecer para declarações de `const` e `let` do ECMAScript 6.

  // Ruim
  function foo() {

    // algumas instruções aqui

    var bar = "",
      qux;
  }

  // Bom
  function foo() {
    var bar = "",
      qux;

    // algumas instruções depois das declarações de variáveis
  }

  ```

  ```javascript

  // 2.B.2.1
  // Declaração de função nomeada
  function foo(arg1, argN) {

  }

  // Utilização
  foo(arg1, argN);


  // 2.B.2.2
  // Declaração de função nomeada
  function square(number) {
    return number * number;
  }

  // Utilização
  square(10);

  // Estilo de passagem artificialmente contínua
  function square(number, callback) {
    callback(number * number);
  }

  square(10, function(square) {
    // instruções de callback
  });


  // 2.B.2.3
  // Expressão de função
  var square = function(number) {
    // Retorna algo de valor e relevante
    return number * number;
  };

  // Expressão de função com identificador
  // Esse formato preferencial tem o valor adicional de permitir
  // chamar a si mesmo e ter uma identidade na pilha de comandos:
  var factorial = function factorial(number) {
    if (number < 2) {
      return 1;
    }

    return number * factorial(number - 1);
  };


  // 2.B.2.4
  // Declaração de construtor
  function FooBar(options) {

    this.options = options;
  }

  // Utilização
  var fooBar = new FooBar({ a: "alpha" });

  fooBar.options;
  // { a: "alpha" }

  ```


  C. Exceções, pequenos desvios

  ```javascript

  // 2.C.1.1
  // Funções com callbacks
  foo(function() {
    // Veja que não há espaço extra entre os parênteses
    // da chamada de função e a palavra "function"
  });

  // Função recebendo uma array, sem espaço
  foo(["alpha", "beta"]);

  // 2.C.1.2
  // Função recebendo um objeto, sem espaço
  foo({
    a: "alpha",
    b: "beta"
  });

  // String literal como argumento único, sem espaço
  foo("bar");

  // Parênteses internos de agrupamento, sem espaço
  if (!("foo" in obj)) {

  }

  ```

  D. Finais de linha e linhas vazias

  Espaços em branco podem arruinar diffs e fazer com que _changesets_ sejam impossíveis de se ler. Considere incorporar um gancho de pre-commit que remova espaços em branco ao final das linhas e espaços em branco em linhas vazias automaticamente.

3. <a name="practical">Exemplo</a>

  ```javascript

  // 3.1.1
  // Um módulo prático

  (function(global) {
    var Module = (function() {

      var data = "segredo";

      return {
        // Essa é uma propriedade booleana
        bool: true,
        // Algum valor de string
        string: "uma string",
        // Uma propriedade em array
        array: [1, 2, 3, 4],
        // Uma propriedade em objeto
        object: {
          lang: "pt-BR"
        },
        getData: function() {
          // pega o valor atual de `data`
          return data;
        },
        setData: function(value) {
          // atribui o valor a data que é retornado
          return (data = value);
        }
      };
    })();

    // Outras coisas que também podem acontecer aqui

    // Expor seu módulo ao objeto global
    global.Module = Module;

  })(this);

  ```

  ```javascript

  // 3.2.1
  // Um construtor prático

  (function(global) {
    function Ctor(foo) {
      this.foo = foo;
      return this;
    }

    Ctor.prototype.getFoo = function() {
      return this.foo;
    };

    Ctor.prototype.setFoo = function(val) {
      return (this.foo = val);
    };

    // Para chamar um construtor sem o `new`, você pode fazer assim:
    var ctor = function(foo) {
      return new Ctor(foo);
    };

    // exponha nosso construtor ao objeto global
    global.ctor = ctor;

  })(this);

  ```

4. <a name="naming">Nomenclatura</a>

  A. Se você não é um compilador humano ou compactador de código, não tente ser um.

  O código a seguir é um exemplo de nomenclatura ruim:

  ```javascript

  // 4.A.1.1
  // Exemplo de código com nomenclaturas fracas

  function q(s) {
    return document.querySelectorAll(s);
  }
  var i,a=[],els=q("#foo");
  for(i=0;i<els.length;i++){a.push(els[i]);}
  ```

  Sem dúvida, você já deve ter escrito código assim - provavelmente isso acaba hoje.

  Aqui temos o mesmo trecho lógico, porém com uma nomenclatura simpática e mais inteligente (e uma estrutura legível):

  ```javascript

  // 4.A.2.1
  // Exemplo de código com nomenclatura melhorada

  function query(selector) {
    return document.querySelectorAll(selector);
  }

  var idx = 0,
    elements = [],
    matches = query("#foo"),
    length = matches.length;

  for(; idx < length; idx++){
    elements.push(matches[idx]);
  }

  ```

  Algumas indicações adicionais de nomenclaturas

  ```javascript

  // 4.A.3.1
  // Nomes de strings

  `dog` é uma string

  // 4.A.3.2
  // Nomes de arrays

  `dogs` é uma array de strings `dog`


  // 4.A.3.3
  // Nomes de funções, objetos, instancias, etc

  // funções e declarações de variáveis
  camelCase;


  // 4.A.3.4
  // Nomes de construtores, protótipos, etc

  // função construtora
  PascalCase;


  // 4.A.3.5
  // Nomes de expressões regulares

  rDesc = //;


  // 4.A.3.6
  // Do Guia de Estilos da Biblioteca do Google Closure

  funcoesNomeadasAssim;
  variaveisNomeadasAssim;
  ConstrutoresNomeadosAssim;
  EnumNomeadosAssim;
  metodosNomeadosAssim;
  CONSTANTES_SIMBOLICAS_ASSIM;

  // nota da tradução: não havia tradução no Google Closure, o original é o seguinte:

  functionNamesLikeThis;
  variableNamesLikeThis;
  ConstructorNamesLikeThis;
  EnumNamesLikeThis;
  methodNamesLikeThis;
  SYMBOLIC_CONSTANTS_LIKE_THIS;

  ```

5. <a name="comments">Comentários</a>

  * Uma linha única acima do código que é comentado
  * Multiplas linhas é bom
  * Comentários ao final da linha são proibidos!
  * O estilo do JSDoc é bom, porém requer um investimento de tempo significante 


6. <a name="language">Código em apenas um idioma</a>

  Programas devem ser escritos em um único idioma, não importe o idioma que seja, a ser definido por quem o mantém.


### Qualidade de código: ferramentas, recursos e referências 

 * [JavaScript Plugin](http://docs.codehaus.org/display/SONAR/JavaScript+Plugin) for [Sonar](http://www.sonarsource.org/)
 * [jsPerf](http://jsperf.com/)
 * [jsFiddle](http://jsfiddle.net/)
 * [jsbin](http://jsbin.com/)
 * [JavaScript Lint (JSL)](http://javascriptlint.com/)
 * [jshint](http://jshint.com/)
 * [jslint](http://jslint.org/)