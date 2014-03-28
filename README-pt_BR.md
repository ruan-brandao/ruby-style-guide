# Prelúdio

> Modelos a seguir são importantes. <br/>
> -- Oficial Alex J. Murphy / RoboCop

Uma coisa sempre me incomodou como desenvolvedor Ruby - Desenvolvedores Python têm uma ótima referência de estilo de programação ([PEP-8](http://www.python.org/dev/peps/pep-0008/)) e nós nunca tivemos um guia oficial, documentando as melhores práticas para codificar Ruby. E eu acredito que este estilo é importante. Eu também acredito que uma grande comunidade hacker, como a que o Ruby possui, deve ser bastante capaz de produzir o cobiçado documento.

Este guia nasceu como nossas orientações internas da empresa para codificar Ruby (feitas por este que vos escreve). Em algum ponto eu decidi que o trabalho que estava fazendo poderia ser interessante para membros em geral da comunidade Ruby e que o mundo precisava muito pouco de outro guia interno de uma empresa. Mas o mundo certamente se beneficiaria de um conjunto de práticas, expressões e roteiros de estilo para programar em Ruby voltado para a comunidade e aprovado por ela.

Desde a origem do guia eu recebi bastante feedback de membros da excepcional comunidade Ruby ao redor do mundo. Obrigado por todas as sugestões e todo o suporte! Juntos nós podemos criar um recurso útil para todo desenvolvedor Ruby.

A propósito, se você gosta de Rails, você pode querer conferir o complementar [Ruby on Rails 3 & 4 Style Guide](https://github.com/bbatsov/rails-style-guide).

# O Guia de Estilo de Ruby

Este guia de estilo de Ruby recomenda as melhores práticas para que programadores Ruby do mundo real possam escrever código que possa ser mantido por outros programadores Rubt do mundo real. Um guia de estilo que reflete o uso do mundo real é utilizado, já um guia de estilo que se prende a um ideal que foi rejeitado pelas pessoas que ele deveria ajudar corre o risco de nunca ser utilizado &ndash; não importa o quão bom ele seja.

Este guia é separado em diversas seções de regras relacionadas. Eu tentei adicionar a base lógica por trás das regras, se está omitida eu presumi que é bastante óbvia).

Eu não criei essas regras do nada - elas são principalmente baseadas na minha longa carreira como engenheiro de software profissional, feedback e sugestões de membros da comunidade Ruby e vários recursos bem conceituados de programação em Ruby, como ["Programming Ruby 1.9"](http://pragprog.com/book/ruby4/programming-ruby-1-9-2-0) e ["The Ruby Programming Language"](http://www.amazon.com/Ruby-Programming-Language-David-Flanagan/dp/0596516177).

Há algumas áreas nas quais não há um consenso claro na comunidade Ruby em relação a um determinado estilo (como aspas em literais de strings, espaçamento em literais de hash, posição do ponto em encadeamento de métodos em múltiplas linhas, etc.). Em tais cenários todos os estilos populares são reconhecidos e cabe a você escolher um e aplicá-lo consistentemente.

O guia ainda é um trabalho em progresso - algumas regras não têm exemplos, algumas regras não possuem exemplos que as ilustrem suficientemente bem. No tempo certo estes problemas serão abordados - apenas os mantenha em mente por enquanto.

Você pode gerar uma cópia em PDF ou HTML deste guia usando o [Transmuter](https://github.com/TechnoGate/transmuter).

[RuboCop](https://github.com/bbatsov/rubocop) é um analisador de código, baseado neste guia.

Traduções do guia estão  disponíveis nos seguintes idiomas:

* [Inglês (versão original)](https://github.com/bbatsov/ruby-style-guide)
* [Chinês Simplificado](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhCN.md)
* [Chinês Tradicional](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhTW.md)
* [Francês](https://github.com/porecreat/ruby-style-guide/blob/master/README-frFR.md)
* [Japonês](https://github.com/fortissimo1997/ruby-style-guide/blob/japanese/README.ja.md)
* [Espanhol](https://github.com/alemohamad/ruby-style-guide/blob/master/README-esLA.md)
* [Vietnamita](https://github.com/scrum2b/ruby-style-guide/blob/master/README-viVN.md)

## Índice

* [Layout do Código](#source-code-layout)
* [Sintaxe](#syntax)
* [Nomeando](#naming)
* [Comentários](#comments)
  * [Anotações em Comentários](#comment-annotations)
* [Classes](#classes--modules)
* [Exceções](#exceptions)
* [Coleções](#collections)
* [Strings](#strings)
* [Expressões Regulares](#regular-expressions)
* [Literais com Porcentagem](#percent-literals)
* [Metaprogramação](#metaprogramming)
* [Misc](#misc)
* [Ferramentas](#tools)

## Layout do código

> Quase todo mundo está convencido de que todo estilo exceto o
> seu próprio é feio e ilegível. Tire o "exceto o seu próprio"
> e eles provavelmente estarão certos... <br/>
> -- Jerry Coffin (sobre indentação)

* Use 'UTF-8' como codificação do arquivo fonte
* Use dois **espaços** por nível de indentação (soft tabs). Não utilize hard tabs.

  ```Ruby
  # ruim - quatro espaços
  def some_method
      do_something
  end

  # bom
  def some_method
    do_something
  end
  ```

* Use quebra de linha Unix-style. (*Usuários de BSD/Solaris/Linux/Os X já fazem isso por padrão. Usuários Windows devem ser extra cuidadosos.)
  * Se você está usando Git pode querer adicionar a seguinte configuração para proteger seu projeto de quebras de linha Windows.
  
    ```bash
    $ git config --global core.autocrlf true
    ```

* Não use ';' para separar declarações e expressões. Como conclusão - use uma expressão por linha.

  ```Ruby
  # ruim
  puts 'foobar'; # ponto-e-vírgula desnecessário

  puts 'foo'; puts 'bar' # duas expressões na mesma linha

  # bom
  puts 'foobar'

  puts 'foo'
  puts 'bar'

  puts 'foo', 'bar' # isso se aplica para o puts em particular
  ```

* Prefira o formato de uma linha para definir classes vazias

  ```Ruby
  # ruim
  class FooError < StandardError
  end

  # mais ou menos
  class FooError < StandardError; end

  # bom
  FooError = Class.new(StandardError)
  ```
  
* Evite métodos de uma única linha. Apesar de eles serem um pouco populares, há algumas peculiaridades sobre a sintaxe de sua definição que os torna indesejáveis. De qualquer maneira, não deve haver mais que uma expressão em um método de uma linha

  ```Ruby
  # ruim
  def too_much; something; something_else; end

  # mais ou menos - perceba que o primeiro ; é obrigatório
  def no_braces_method; body end

  # mais ou menos - perceba que o segundo ; é opcional
  def no_braces_method; body; end

  # mais ou menos - sintaxe válida, mas nenhum ; o faz meio difícil de ler
  def some_method() body end

  # bom
  def some_method
    body
  end
  ```

  Uma exceção a regra são os métodos vazios.

  ```Ruby
  # bom
  def no_op; end
  ```

* Use espaços antes e depois de operadores, depois de vírgulas, dois pontos e ponto-e-vírgula, antes e depois de '{' e antes de '}'. Espaços embranco são (quase sempre) irrelevantes para o interpretador Ruby, mas seu uso adequado é a chave para escrever código com boa legibilidade.

  ```Ruby
  sum = 1 + 2
  a, b = 1, 2
  1 > 2 ? true : false; puts 'Hi'
  [1, 2, 3].each { |e| puts e }
  ```
  
  A única exceção, em relação a operadores, é o operador de exponenciação:
  
  ```Ruby
  # ruim
  e = M * c ** 2

  # bom
  e = M * c**2
  ```
  
  '{' e '}' merecem um pouco de esclarecimento, por serem usados em blocos e hashes, assim como em expressões embutidas em strings. Para hashes os dois estilos são aceitáveis.
  
  ```Ruby
  # bom - espaço depois { e antes }
  { one: 1, two: 2 }

  # bom - sem espaço depois { e antes }
  {one: 1, two: 2}
  ```
  
  A primeira variante é levemente mais legível (e indiscutivelmente mais popular na comunidade Ruby em geral). A segunda variante tem a vantagem de adicionar uma diferença visual entre blocos e a sintaxe literal de hashes. Qualquer uma que você escolher - aplique-a consistentemente.
  
  Quanto a expressões embutidas, também há duas opções aceitáveis:
  
  ```Ruby
  # bom - sem espaços
  "string#{expr}"

  # ok - Indiscutivelmente mais legível
  "string#{ expr }"
  ```
  
  O primeiro estilo é extremamente mais popular e você é geralmente aconselhado a se manter nele. O segundo, por outro lado, é (sem dúvida) um pouco mais legível. Assim como com hashes - escolha um estilo e aplique-o consistentemente.
  
* Não utilize espaços depois de '(', '[' ou antes de ']', ')'.

  ```Ruby
  some(arg).other
  [1, 2, 3].size
  ```
  
* Não use espaço após '!'

  ```Ruby
  # ruim
  ! something

  # bom
  !something
  ```
