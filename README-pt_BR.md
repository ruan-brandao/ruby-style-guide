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

* Use `UTF-8` como codificação do arquivo fonte
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

* Não use `;` para separar declarações e expressões. Como conclusão - use uma expressão por linha.

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

* Use espaços antes e depois de operadores, depois de vírgulas, dois pontos e ponto-e-vírgula, antes e depois de `{` e antes de `}`. Espaços embranco são (quase sempre) irrelevantes para o interpretador Ruby, mas seu uso adequado é a chave para escrever código com boa legibilidade.

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
  
  `{` e `}` merecem um pouco de esclarecimento, por serem usados em blocos e hashes, assim como em expressões embutidas em strings. Para hashes os dois estilos são aceitáveis.
  
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
  
* Não utilize espaços depois de `(`, `[` ou antes de `]`, `)`.

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

* Indente `when` no mesmo nível de `case`. Eu sei que muitos discordarão deste item, mas é o estilo estabelecido tando em "The Ruby Programming Language" quanto em "Programming Ruby".

  ```Ruby
  # ruim
  case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
  end

  # bom
  case
  when song.name == 'Misty'
    puts 'Not again!'
  when song.duration > 120
    puts 'Too long!'
  when Time.now.hour > 21
    puts "It's too late"
  else
    song.play
  end
  ```
  
* Quando for atribuir o resultado de uma condicional a uma variável, preserve o alinhamento comum de suas ramificações.

  ```Ruby
  # ruim - muito confuso
  kind = case year
  when 1850..1889 then 'Blues'
  when 1890..1909 then 'Ragtime'
  when 1910..1929 then 'New Orleans Jazz'
  when 1930..1939 then 'Swing'
  when 1940..1950 then 'Bebop'
  else 'Jazz'
  end

  result = if some_cond
    calc_something
  else
    calc_something_else
  end

  # bom - é aparente o que está acontecendo
  kind = case year
         when 1850..1889 then 'Blues'
         when 1890..1909 then 'Ragtime'
         when 1910..1929 then 'New Orleans Jazz'
         when 1930..1939 then 'Swing'
         when 1940..1950 then 'Bebop'
         else 'Jazz'
         end

  result = if some_cond
             calc_something
           else
             calc_something_else
           end

  # bom (e um pouco mais eficiente em relação a largura)
  kind =
    case year
    when 1850..1889 then 'Blues'
    when 1890..1909 then 'Ragtime'
    when 1910..1929 then 'New Orleans Jazz'
    when 1930..1939 then 'Swing'
    when 1940..1950 then 'Bebop'
    else 'Jazz'
    end

  result =
    if some_cond
      calc_something
    else
      calc_something_else
    end
  ```
 
 * Use linhas em branco entre definições de método e também para fragmentar um método em parágrafos lógicos internamente.
 
   ```Ruby
  def some_method
    data = initialize(options)

    data.manipulate!

    data.result
  end

  def some_method
    result
  end
  ```
  
* Evite vírgula após o último parâmetro em uma chamada de método, especialmente quando os parâmetros estiverem todos na mesma linha.

  ```Ruby
  # ruim - mais fácil para mover/adicionar/remover parâmetros, mas ainda assim não recomendado
  some_method(
               size,
               count,
               color,
             )

  # ruim
  some_method(size, count, color, )

  # bom
  some_method(size, count, color)
  ```
  
* Use espaços antes e depois do operador `=` quando atribuir valores padrão a argumentos de métodos.

  ```Ruby
  # ruim
  def some_method(arg1=:default, arg2=nil, arg3=[])
    # do something...
  end

  # bom
  def some_method(arg1 = :default, arg2 = nil, arg3 = [])
    # do something...
  end
  ```
  
  Ainda que muitos livros sobre Ruby sugiram o primeiro estilo, o segundo é muito mais eminente na prática (e indiscutivelmente mais legível)

* Evite continuação de linha com `\` onde não for necessário. Na prática, evite utilizar continuações de linha para qualquer coisa que não seja concatenação de strings.

  ```Ruby
  # ruim
  result = 1 - \
           2

  # bom (mas ainda muito feio)
  result = 1 \
           - 2

  long_string = 'First part of the long string' \
                ' and second part of the long string'
  ```
  
* Adote um estilo consistente de encadeamentto de métodos com várias linhas. Há dois estilos populares na comunidade Ruby, ambos considerados bons - `.` iniciando (Opção A) e `.` fechando (Opção B).

 * **(Opção A)** Ao continuar a chamada de um método encadeado em outra linha passe o `.` para a segunda linha.

    ```Ruby
    # ruim - é preciso olhar a primeira linha para entender a segunda
    one.two.three.
      four

    # bom - fica claro o que está acontecendo na segunda linha
    one.two.three
      .four
    ```

  * **(Opção B)** Ao continuar a chamada de um método encadeado em outra linha,
    inclua o `.` na primeira linha para indicar que a expressão continua.

    ```Ruby
    # ruim - é preciso ler a segunda linha para entender que a chamada continua
    one.two.three
      .four

    # bom - fica claro que a expressão continua além da primeira linha
    one.two.three.
      four
    ```

  Uma discussão sobre os méritos de cada um destes estilos pode ser encontrada [aqui](https://github.com/bbatsov/ruby-style-guide/pull/176).
  
* Alinhe os parâmetros de uma chamada de método caso eles se estendam por mais de uma linha. Quando alinhar parâmetros não for bom devido ao comprimento da linha, usar uma indentação para as linhas após a primeira também é aceitável.

  ```Ruby
  # ponto de partida (a linha é muito longa)
  def send_mail(source)
    Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
  end

  # ruim (indentação dupla)
  def send_mail(source)
    Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
  end

  # bom
  def send_mail(source)
    Mailer.deliver(to: 'bob@example.com',
                   from: 'us@example.com',
                   subject: 'Important message',
                   body: source.text)
  end

  # bom (indentação simples)
  def send_mail(source)
    Mailer.deliver(
      to: 'bob@example.com',
      from: 'us@example.com',
      subject: 'Important message',
      body: source.text
    )
  end
  ```
  
* Alinhe os elementos de arrays literais que ocuparem mais de uma linha.

  ```Ruby
  # ruim - indentação simples
  menu_item = ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
    'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']

  # bom
  menu_item = [
    'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
    'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam'
  ]

  # bom
  menu_item =
    ['Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam',
     'Baked beans', 'Spam', 'Spam', 'Spam', 'Spam', 'Spam']
  ```
  
* Adicione `_` (underscore) a números muito longos para melhorar sua legibilidade.

  ```Ruby
  # ruim - quantos 0s tem nisso?
  num = 1000000

  # bom - mito mais fácil de analisar para o cérebro humano
  num = 1_000_000
  ```
  
* Utilize RDoc e suas convenções para documentar APIs. Não coloque uma linha em branco entre o bloco de comentários e o 'def'.

* Limite linhas a 80 caracteres.

* Evite espaços em branco no final das linhas.

* Termine todo arquivo com uma linha em branco.

* Não use comentários em bloco. Eles não podem ser precedidos por espaços em branco e não são tão facilmente indentificáveis quanto comentários comuns.

  ```Ruby
  # ruim
  =begin
  comment line
  another comment line
  =end

  # bom
  # comment line
  # another comment line
  ```
