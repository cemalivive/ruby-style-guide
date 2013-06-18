# Prelude

> Role models are important. <br/>
> -- Officer Alex J. Murphy / RoboCop

One thing has always bothered me as Ruby developer - Python developers
have a great programming style reference
([PEP-8](http://www.python.org/dev/peps/pep-0008/)) and we never got
an official guide, documenting Ruby coding style and best
practices. And I do believe that style matters. I also believe that
such fine fellows, like us Ruby developers, should be quite capable to
produce this coveted document.

This guide started its life as our internal company Ruby coding guidelines
(written by yours truly). At some point I decided that the work I was
doing might be interesting to members of the Ruby community in general
and that the world had little need for another internal company
guideline. But the world could certainly benefit from a
community-driven and community-sanctioned set of practices, idioms and
style prescriptions for Ruby programming.

Since the inception of the guide I've received a lot of feedback from
members of the exceptional Ruby community around the world. Thanks for
all the suggestions and the support! Together we can make a resource
beneficial to each and every Ruby developer out there.

By the way, if you're into Rails you might want to check out the
complementary
[Ruby on Rails 3 Style Guide](https://github.com/bbatsov/rails-style-guide).

# The Ruby Style Guide

This Ruby style guide recommends best practices so that real-world Ruby
programmers can write code that can be maintained by other real-world Ruby
programmers. A style guide that reflects real-world usage gets used, and a
style guide that holds to an ideal that has been rejected by the people it is
supposed to help risks not getting used at all &ndash; no matter how good it is.

The guide is separated into several sections of related rules. I've
tried to add the rationale behind the rules (if it's omitted I've
assumed that is pretty obvious).

I didn't come up with all the rules out of nowhere - they are mostly
based on my extensive career as a professional software engineer,
feedback and suggestions from members of the Ruby community and
various highly regarded Ruby programming resources, such as
["Programming Ruby 1.9"](http://pragprog.com/book/ruby4/programming-ruby-1-9-2-0)
and ["The Ruby Programming Language"](http://www.amazon.com/Ruby-Programming-Language-David-Flanagan/dp/0596516177).

The guide is still a work in progress - some rules are lacking
examples, some rules don't have examples that illustrate them clearly
enough. In due time these issues will be addressed - just keep them in
mind for now.

You can generate a PDF or an HTML copy of this guide using
[Transmuter](https://github.com/TechnoGate/transmuter).

[RuboCop](https://github.com/bbatsov/rubocop) is a code analyzer,
based on this style guide.

Translations of the guide are available in the following languages:

* [Chinese Simplified](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhCN.md)
* [Chinese Traditional](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhTW.md)
* [French](https://github.com/porecreat/ruby-style-guide/blob/master/README-frFR.md)

## Table of Contents

* [Source Code Layout](#source-code-layout)
* [Syntax](#syntax)
* [Naming](#naming)
* [Comments](#comments)
    * [Comment Annotations](#comment-annotations)
* [Classes](#classes--modules)
* [Exceptions](#exceptions)
* [Collections](#collections)
* [Strings](#strings)
* [Regular Expressions](#regular-expressions)
* [Percent Literals](#percent-literals)
* [Metaprogramming](#metaprogramming)
* [Misc](#misc)
* [Tools](#tools)

## Source Code Layout

> Nearly everybody is convinced that every style but their own is
> ugly and unreadable. Leave out the "but their own" and they're
> probably right... <br/>
> -- Jerry Coffin (on indentation)

* Kodlamada `UTF-8` kullanılmalı.
* Girinti başına **2 boşluk** kullanılmalı ancak bu durum sekmeler (tab) 2 boşluk ayarlanarak kullanılmamalı.
* Unix tarzı satır sonlandırma kullanılmalı. (*BSD/Solaris/Linux/OSX gibi sistemlerde varsayılan olarak gelir,
   ancak Windows kullanıcıları bu konuda daha dikkatli olmalıdırlar.)
* İfadeleri birbrinden ayırmak için `;` kullanmayın. Sonuç olarak satır başına bir ifade kullanın.

    ```Ruby
    # kotu kodlama
    puts 'foobar'; # noktalı virgul gereksiz

    puts 'foo'; puts 'bar' # ayni satirda 2 ifade var

    # iyi kodlama
    puts 'foobar'

    puts 'foo'
    puts 'bar'

    puts 'foo', 'bar' # bu bicim ozellikle puts ifadesinde kullanilir
    ```
* Gövdesiz sınıf tanımlaması için tek satırlık tanımlamaları tercih edilmeli.

    ```Ruby
    # kotu
    class FooError < StandardError
    end

    # kabul edilebilir
    class FooError < StandardError; end

    # iyi
    FooError = Class.new(StandardError)
    ```

* Tek satırlık method tanımlamaları yapılmamalı.

    ```Ruby
    # kotu
    def too_much; something; something_else; end

    # kabul edilebilir
    def no_braces_method; body end

    # kabul edilebilir
    def no_braces_method; body; end

    # kabul edilebilir
    def some_method() body end

    # iyi
    def some_method
      body
    end
    ```

   Bu kurala istisna olarak gövdesiz bir method örneği:

    ```Ruby
    # iyi
    def no_op; end
    ```

* İşleçler (operatorler) arasında boşluk kullanılmalı, sonrasında virgüller,
  iki nokta üst üste ve noktalı virgüller, çevresinde { ve } olmalı. Boşluklar Ruby
  yorumlayıcısı için oldukça gereksiz olabilir, fakat yorumlayıcının bu uygunluğu
  okunurluğu ve yazılabilirliği artırır.

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```
    Buna ilişkin operatorlere tek istina üst alma operatorüdür.

    ```Ruby
    # kotu
    e = M * c ** 2

    # iyi
    e = M * c**2
    ```

    `{` ve `}` kullanımında hash tanımlamaları için boşluklar asağıdaki gibi olabilir:

    ```Ruby
    # iyi -  { sonrasinda bosluk ve } oncesinde bosluk
    { one: 1, two: 2 }

    # good - { sonrasın ve } oncesinde bosluk yok
    {one: 1, two: 2}
    ```

  Gömülü ifadeler için iki tür kabul edilebilir seçenek vardır:

    ```Ruby
    # iyi - bosluk yok
    "string#{expr}"

    # kullanilabilir - muhtemelen daha okunabilir
    "string#{ expr }"
    ```

* `(`, `[`  karakterlerinden sonra ya da  `]`, `)` karakterlerinden önce boşluk olmamalıdır.

    ```Ruby
    some(arg).other
    [1, 2, 3].length
    ```
* `when` ifadesinin girintisi aşağıdaki gibi olmalı. Genelde bu konuda çok anlaşma yok ancak "The Ruby
   Programming Language" ve "Programming Ruby" belgelerinde böyle kullanılmış.

    ```Ruby
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

    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end

* `def` tanımlamaları arasında boşluk bırakılmalı ve method içinde mantıksal paragraflara ayırılmalı.
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

* Methodlardaki parametrelere varsayılan değerler atanırken `=` operatorü çevresinde boşluk bırakılmalı.
    ```Ruby
    # kotu
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # iyi
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```
    Bir kaç Ruby kitabında ilk biçim önerilirken, ikinci biçim okunabilirlik bakımından daha çok tercih edilebilir.

* Avoid line continuation `\` where not required. In practice, avoid using
  line continuations at all.
* Satır devam etmesi gerekli olmayan yerde `\` ile önlenir. Aşağıdaki örneklerin 
  hepsinde `\` kullanılarak önlenmiştir.

    ```Ruby
    # bad
    result = 1 - \
             2

    # good (but still ugly as hell)
    result = 1 \
             - 2
    ```

* Zincirlenmiş method çağırımı devam ederken bir başka satırdaki `.` ikinci satırı korur. 

    ```Ruby
    # kotu - need to consult first line to understand second line
    # kotu - ikinci satırı anlamak için ilk satırı danışmaya gerek duyulu
    one.two.three.
      four

    # good - it's immediately clear what's going on the second line
    # iyi- bu sekilde ikinci satırda ne yapildigi hemen aciklanir
    one.two.three
      .four
    ```

* Eğer aralıklar fazlaysa bir satırda, methodun tüm parametreleri hizalanmalı.

    ```Ruby
    # baslama noktasi (satir cok uzun)
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # kotu (noral girinti)
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
    end

    # kotu (cift girinti)
    def send_mail(source)
      Mailer.deliver(
          to: 'bob@example.com',
          from: 'us@example.com',
          subject: 'Important message',
          body: source.text)
    end

    # iyi
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com',
                     from: 'us@example.com',
                     subject: 'Important message',
                     body: source.text)
    end
    ```

* Büyük sayısal ifadelerin okunabilirliği için alt çizgi eklenmeli.

    ```Ruby
    # kotu - kac sifir oldugunu anlamak zor
    num = 1000000

    # iyi - insan beyni icin bu ifadeyi okumak daha kolay
    num = 1_000_000
    ```

* Uygulama dökümanı için RDoc kullanılmalı. Yorum satırları ve `def`
   arasına boş satır koyulmamalı.
* Satir sınıırı 80 karakterdir.
* Gereksiz boşlukların oluşması engellenmeli.
* Blok şeklindeki yorum satırları kullanılmamalı.

    ```Ruby
    #kotu
    == begin
    comment line
    another comment line
    == end

    #iyi
    # comment line
    # another comment line
    ```

## Söz Dizimi

* `::` ifadesi sadece sabitlere başvurularda kullanılmalı (bu durum sınıfları
  ve modülleri içerir). Method çağırımları için asla `::` kullanılmamalı.

    ```Ruby
    # kotu
    SomeClass::some_method
    some_object::some_method

    # iyi
    SomeClass.some_method
    some_object.some_method
    SomeModule::SomeClass::SOME_CONST
    ```

* `def` parametreler varsa parantezlerle birlikte kullanılmalı. Methodda parametreler yoksa 
  parantez kullanımını atlayabilirsiniz.

     ```Ruby
     # kotu
     def some_method()
       # body omitted
     end

     # iyi
     def some_method
       # body omitted
     end

     # kotu
     def some_method_with_arguments arg1, arg2
       # body omitted
     end

     # iyi
     def some_method_with_arguments(arg1, arg2)
       # body omitted
     end
     ```

* Tam olarak neden olduğunu bilmeden `for` yapısı kullanımamalı. `for` yerine pek çok zaman iteratörler kullanılır.
  `for` yapısı `each` ile sağlanmıştır.

    ```Ruby
    arr = [1, 2, 3]

    # kotu
    for elem in arr do
      puts elem
    end

    # iyi
    arr.each { |elem| puts elem }
    ```

* Çok satırlı `if/unless` için `then` hiç kullanılmamalı.

    ```Ruby
    # kotu
    if some_condition then
      # body omitted
    end

    # iyi
    if some_condition
      # body omitted
    end
    ```

* Genellikle kontrol edilecek durum `if`/`unless` ile aynı satırda bulunur.

    ```Ruby
    # bad
    if
      some_condition
      do_something
      do_something_else
    end

    # good
    if some_condition
      do_something
      do_something_else
    end
    ```

* `if/then/else/end` yerine  üçlü operatorler(`?:`) daha iyidir.

    ```Ruby
    # kotu
    result = if some_condition then something else something_else end

    # iyi
    result = some_condition ? something : something_else
    ```

* Üçlü operatorde dal başına bir ifade kullanılmalı. Bu aynı zamanda üçlü operatorün 
  iç içe olmaması gerektiği anlamına gelir. Bu gibi durumlarda `if/else` yapıcıları tercih edilir.


    ```Ruby
    # kotu
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # iyi
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* `if x: ...` yapısını kullanmayın (Ruby 1.9 da kaldırıldı). Bunun yerine üçlü
  operatör kullanın.

    ```Ruby
    # kotu
    result = if some_condition: something else something_else end

    # iyi
    result = some_condition ? something : something_else
    ```

* `if x; ...` kullanmayın. Bunun yerine üçlü operatör kullanın.

* Tek satırdan oluşan durumlar için `when x: ...` kullanın. 
  Buna alternatif yazım olan `when x: ...` Ruby 1.9'da kaldırıldı.

* `when x; ...` yapısını kullanmayın. Bunun yerine bir önceki kuralda belirtildiği gibi kullanın.

* `not` yerine `!` kullanın.

    ```Ruby
    # kotu 
    x = (not something)

    # iyi
    x = !something
    ```
* `and` ve `or` ifadeleri yasaklandı.  Bunların yerine hep `&&` ve `||` kullanın.

    ```Ruby
    # kotu
    # mantiksal ifadeler
    if some_condition and some_other_condition
      do_something
    end

    # akis kontrolu
    document.saved? or document.save!

    # iyi
    # mantiksal ifadeler
    if some_condition && some_other_condition
      do_something
    end

    # akis kontrolu
    document.saved? || document.save!
    ```

* Cok satırdan oluşan ifadelerde `?:` (üçlü operatör) kullanımını önleyin. 
  `if/unless` yapısı kullanın bunların yerine.


* Tek satırdan oluşan `if/unless` ifadesi olduğunda bu ifadeyi değiştirin.
  İyi bir olarak alternatif olarak `&&/||` akış kontrollerini kullanabilirsiniz.

    ```Ruby
    # kotu
    if some_condition
      do_something
    end

    # iyi
    do_something if some_condition

    # bir diger iyi secim
    some_condition && do_something
    ```

* `unless` kullanımı negatif durum kontrollerinde `if` ifadesinden daha iyidir (ya da akış 
  kontrol ifadeleri de kullanılabilir `||`)

    ```Ruby
    # kotu
    do_something if !some_condition

    # kotu
    do_something if not some_condition

    # iyi
    do_something unless some_condition

    # diger iyi secenek
    some_condition || do_something
    ```

* `else` ifadesini `unless` ile birlikte kullanmayın. 

    ```Ruby
    # kotu
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # iyi
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```
* `if/unless/while/until` ifadelerinde parantezler kullanmayın.

    ```Ruby
    # kotu
    if (x > 10)
      # body omitted
    end

    # iyi
    if x > 10
      # body omitted
    end
    ```

* Tek satırdan oluşan bir gövde varsa, `while/until` kullanın.

    ```Ruby
    # kotu
    while some_condition
      do_something
    end

    # iyi
    do_something while some_condition
    ```

* Negatif koşul ifadelerinde `while` yerine `until` kullanın.

    ```Ruby
    # kotu
    do_something while !some_condition

    # iyi
    do_something until some_condition
    ```

* `begin/end/until` ya da `begin/end/while` kullanmak yerine döngü sonrasında 
   yapılan testler için `Kernel#loop` ifadesini kullanın.

   ```Ruby
   # kotu
   begin
     puts val
     val += 1
   end while val < 0

   # iyi
   loop do
     puts val
     val += 1
     break unless val < 0
   end
   ```

* Parametresi olan tüm method çağırımlarında parantezler kullanın. 
  Özelliklere erişirken de parantez kullanımını atlayın.

    ```Ruby
    class Person
      attr_reader :name, :age

      # omitted
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    puts temperance.age

    x = Math.sin(y)
    array.delete(e)

    bowling.score.should == 0
    ```

* Parametresiz olan tüm method çağırımlarında parantezleri atlayın.

    ```Ruby
    # kotu
    Kernel.exit!()
    2.even?()
    fork()
    'test'.upcase()

    # iyi
    Kernel.exit!
    2.even?
    fork
    'test'.upcase
    ```

* Tek satırdan oluşan bloklarda `do...end` yerine `{...}` kullanın. Çok satırdan 
  oluşan döngülerde de `{...}` kullanmayın. 

    ```Ruby
    names = ['Bozhidar', 'Steve', 'Sarah']

    # kotu
    names.each do |name|
      puts name
    end

    # iyi
    names.each { |name| puts name }

    # kotu
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }

    # iyi
    names.select { |name| name.start_with?('S') }.map { |name| name.upcase }
    ```

* Gerekli değilse `return` kullanmayın.

    ```Ruby
    # kotu
    def some_method(some_arr)
      return some_arr.size
    end

    # iyi
    def some_method(some_arr)
      some_arr.size
    end
    ```

* `self` gerekli değilse kullanmayın. (`self` sadece kendi tanımladığımız 
   erişimciler (accessor) için gereklidir.)

    ```Ruby
    # kotu
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # iyi
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        self.status = :in_progress
      end
      status == :verified
    end
    ```

* Sonuç olarak, her ikiside eşit olmadıkça yerel değişkenlerle methodları gölgelemekten kaçının.

    ```Ruby
    class Foo
      attr_accessor :options

      # ok
      def initialize(options)
        self.options = options
        # both options and self.options are equivalent here
      end

      # bad
      def do_something(options = {})
        unless options[:when] == :later
          output(self.options[:message])
        end
      end

      # good
      def do_something(params = {})
        unless params[:when] == :later
          output(options[:message])
        end
      end
    end
    ```
* Durum kontrolleri yapılırken aynı zamanda değer ataması için `=` ifadesini kullanmayın.

    ```Ruby
    # bad (+ a warning)
    if (v = array.grep(/foo/))
      do_something(v)
      ...
    end

    # bad (+ a warning)
    if v = array.grep(/foo/)
      do_something(v)
      ...
    end

    # good
    v = array.grep(/foo/)
    if v
      do_something(v)
      ...
    end
    ```

* `||=` ifadesini değişkeni başlatmak için kullanabilirsiniz.

    ```Ruby
    # name degiskenine Bozhidar degeri sadece name nil ise ya da degeri false ise atanir.
    # set name to Bozhidar, only if it's nil or false
    name ||= 'Bozhidar'
    ```

* `||=` ifadesini mantıksal değişkenleri başlatmak için kullanmayın. 
  (Değişkenin o anki değeri false ise olabilecekleri dikkate alın.)
    ```Ruby
    # kotu - eger false ise bile true deger ataması yapılır
    enabled ||= true

    # iyi
    enabled = true if enabled.nil?
    ```

* Avoid explicit use of the case equality operator `===`. As it name
  implies it's meant to be used implicitly by `case` expressions and
  outside of them it yields some pretty confusing code.

    ```Ruby
    # bad
    Array === something
    (1..100) === 7
    /something/ === some_string

    # good
    something.is_a?(Array)
    (1..100).include?(7)
    some_string =~ /something/
    ```

* Method ismi ve parantez açma ifadesinin arasına boşluk koymayın.

    ```Ruby
    # kotu
    f (3 + 2) + 1

    # iyi
    f(3 + 2) + 1
    ```

* Eğer methodun argumanları parantez açma ifadesiyle başladıysa, method çağırımlarında hep 
  parantezler kullanın. Örnğin: `f((3 + 2) + 1)`.

* Ruby kodlarını hep `-w` parametresi ile çalıştırın. Yukarıdaki kurallardan 
  herhangi birini unutursanız size uyarı verecektir.

* Tek satırdan oluşan bloklar için yeni lambda tam söz dizimi yapısını kullanın. 
  Çok satırdan oluşan bloklar için ise `lambda`  method yapısını kullanın.

    ```Ruby
    # kotu
    l = lambda { |a, b| a + b }
    l.call(1, 2)

    # dogru, fakat oldukca kullanissiz gorunuyor
    l = ->(a, b) do
      tmp = a * 7
      tmp * b / 50
    end

    # iyi
    l = ->(a, b) { a + b }
    l.call(1, 2)

    l = lambda do |a, b|
      tmp = a * 7
      tmp * b / 50
    end
    ```

* `Proc.new`yerine `proc` kullanbilirsiniz.

    ```Ruby
    # kotu
    p = Proc.new { |n| puts n }

    # iyi
    p = proc { |n| puts n }
    ```

* Kullanılmayan blok parametreleri yerine `_` ifadesini kullanın.

    ```Ruby
    # kotu
    result = hash.map { |k, v| v + 1 }

    # iyi
    result = hash.map { |_, v| v + 1 }
    ```

* `STDOUT/STDERR/STDIN` yerine `$stdout/$stderr/$stdin` kullanın.
  `STDOUT/STDERR/STDIN` ifadeleri sabittir, ve Ruby'de sabitlere yeniden 
   değer ataması yapılırken uyarı verir.


* `$stderr.puts` yerine `warn` ifadesi kullanın. Bu kullanım daha açık ve kısa olur.
  `warn` ifadesi gerekiyorsa uyarıyı kaldırmaya izin verir.


*  `String#%` yerine `sprintf` kullanın.

    ```Ruby
    # kotu
    '%d %d' % [20, 10]
    # => '20 10'

    # iyi
    sprintf('%d %d', 20, 10)
    # => '20 10'
    ```
* `Array#*` ifadesi yerine `Array#join` ifadesini kullanın.

    ```Ruby
    # kotu
    %w(one two three) * ', '
    # => 'one, two, three'

    # iyi
    %w(one two three).join(', ')
    # => 'one, two, three'
    ```

* `Array` yerine `[*var]` ya da `Array()` ifadelerini kullanın.

    ```Ruby
    # kotu
    paths = [paths] unless paths.is_a? Array
    paths.each { |path| do_something(path) }

    # iyi
    [*paths].each { |path| do_something(path) }

    # iyi (ve biraz daha okunabilir)
    Array(paths).each { |path| do_something(path) }
    ```

* Karmaşık mantıksal karşılaştırmalarda mümkünse aralık (range ifadeleri) 
  karşılaştırmalarını kullanın.

    ```Ruby
    # kotu
    do_something if x >= 1000 && x < 2000

    # iyi
    do_something if (1000...2000).include?(x)
    ```

## Adlandırmalar

> The only real difficulties in programming are cache invalidation and
> naming things. <br/>
> -- Phil Karlton

* Tanımlamalardaki isimler İngilizce olmalı.

    ```Ruby
    # kotu - degisken ismi Bulgarca olarak yazilmis
    zaplata = 1_000

    # iyi
    salary = 1_000
    ```

* Semboller, metodlar ve değişkenler için `snake_case` kullanın.

    ```Ruby
    # kotu
    :'some symbol'
    :SomeSymbol
    :someSymbol

    someVar = 5

    def someMethod
      ...
    end

    def SomeMethod
     ...
    end

    # iyi
    :some_symbol

    def some_method
      ...
    end
    ```

* Sınıf ve modüller için `CamelCase` kullanın. (HTTP, RFC, XML gibi kısaltmalardaki 
  büyük harfleri değiştirmeyin.)

    ```Ruby
    # kotu
    class Someclass
      ...
    end

    class Some_Class
      ...
    end

    class SomeXml
      ...
    end

    # iyi
    class SomeClass
      ...
    end

    class SomeXML
      ...
    end
    ```

* Diğer sabitler için `SCREAMING_SNAKE_CASE` kullanın.

    ```Ruby
    # kotu
    SomeConst = 5

    # iyi
    SOME_CONST = 5
    ```

* The names of predicate methods (methods that return a boolean value)
  should end in a question mark.
  (i.e. `Array#empty?`).

* Doğrulama metodlarının (true, false gibi mantıksal değerler döndüren methodlar)
   isimlerinde soru işareti karaterini kullanıbilirsiniz. (`Array#empty?`)

* The names of potentially *dangerous* methods (i.e. methods that
  modify `self` or the arguments, `exit!` (doesn't run the finalizers
  like `exit` does), etc.) should end with an exclamation mark if
  there exists a safe version of that *dangerous* method.

    ```Ruby
    # bad - there is not matching 'safe' method
    class Person
      def update!
      end
    end

    # good
    class Person
      def update
      end
    end

    # good
    class Person
      def update!
      end

      def update
      end
    end
    ```

* Define the non-bang (safe) method in terms of the bang (dangerous)
  one if possible.


    ```Ruby
    class Array
      def flatten_once!
        res = []

        each do |e|
          [*e].each { |f| res << f }
        end

        replace(res)
      end

      def flatten_once
        dup.flatten_once!
      end
    end
    ```

* When using `reduce` with short blocks, name the arguments `|a, e|`
  (accumulator, element).
* When defining binary operators, name the argument `other`(`<<` and
  `[]` are exceptions to the rule, since their semantics are different).

    ```Ruby
    def +(other)
      # body omitted
    end
    ```

* Prefer `map` over `collect`, `find` over `detect`, `select` over
  `find_all`, `reduce` over `inject` and `size` over `length`. This is
  not a hard requirement; if the use of the alias enhances
  readability, it's ok to use it. The rhyming methods are inherited from
  Smalltalk and are not common in other programming languages. The
  reason the use of `select` is encouraged over `find_all` is that it
  goes together nicely with `reject` and its name is pretty self-explanatory.

* Use `flat_map` instead of `map` + `flatten`.
  This does not apply for arrays with a depth greater than 2, i.e.
  if `users.first.songs == ['a', ['b','c']]`, then use `map + flatten` rather than `flat_map`.
  `flat_map` flattens the array by 1, whereas `flatten` flattens it all the way.

    ```Ruby
    # bad
    all_songs = users.map(&:songs).flatten.uniq

    # good
    all_songs = users.flat_map(&:songs).uniq
    ```

## Yorumlar

> Good code is its own best documentation. As you're about to add a
> comment, ask yourself, "How can I improve the code so that this
> comment isn't needed?" Improve the code and then document it to make
> it even clearer. <br/>
> -- Steve McConnell

* Write self-documenting code and ignore the rest of this section. Seriously!
* Yorumları İngilizce yazın.
* Yorum satırlarında `#` karakteri ve metin arasında bir boşluk bırakın.
* Bir kelimeden daha uzun ve noktalalama işaretleri olan yorum satırları yazın. 
  Virgülden sonra bir boşluk bırakın.
* Gereksiz yorumlardan kaçının.

    ```Ruby
    # kotu
    counter += 1 # sayac birer birer artar
    ```
* Yorum satırlarını güncel tutun. Güncel olmayan yorum satırları hiç yorum olmaması durumundan kötüdür.

> Good code is like a good joke - it needs no explanation. <br/>
> -- Russ Olsen

* Yorum satırı yazdığınız için kötü kod yazmayın. Kendi kendini açıklayabilen anlaşılabilen kodlar yazın.
* Avoid writing comments to explain bad code. Refactor the code to
  make it self-explanatory. (Do or do not - there is no try. --Yoda)

### Yorum Açıklamaları

* Açıklamalar genellikle gereken kodun hemen üst satırında olmalıdır.
* annotation anahtar kelimesini iki nokta üst üste ve bir boşluk takip etmelidir,
  daha sonra bir not problem tanımlanıyor. 
* The annotation keyword is followed by a colon and a space, then a note
  describing the problem.
* Çoklu satırlar problem tanımlamayı gerektiriyorsa, sonraki satır 
  `#` ifadesinden iki boşluk sonra içerde yazılmalıdır.

    ```Ruby
    def bar
      # FIXME: This has crashed occasionally since v3.2.1. It may
      #   be related to the BarBazUtil upgrade.
      baz(:quux)
    end
    ```

* Bu gibi durumlarda problem nerede oluşuyorsa böylece bellidir, bu konuda herhangi bir 
  döküman gereksiz olur. 
* In cases where the problem is so obvious that any documentation would
  be redundant, annotations may be left at the end of the offending line
  with no note. This usage should be the exception and not the rule.

    ```Ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

* Sonraki zamanlarda eklemek istediğiniz şeyleri `TODO` notu ile belirtin.
* Düzeltilmesi gereken yerlere `FIXME` notu bırakın.
* `OPTIMIZE` notunu ise yavaş ya da verimsiz kodlar (performans problemleri) için kullanabilirsiniz.
* Use `HACK` to note code smells where questionable coding practices
  were used and should be refactored away.
* Tartışmaya açık olabilecek yerler için `HACK` notunu yazabilirsiniz, sonraki zamanlarda 
  o kod yeniden düzenlenilecektir.
* Çalışmasına yönelik doğrulama için `REVIEW` notunu kullanabilirsiniz.


* Uygunsa diğer özel açıklama anahtar kelimelerini de kullanabilirsiniz, fakat proje `README` 
  ya da benzer başka bir belgede bunları yazdığınıza emin olun.
* Use other custom annotation keywords if it feels appropriate, but be
  sure to document them in your project's `README` or similar.

## Sınıflar & Modüller

* Sınıf tanımlamalarında uygun yapıları kullanın. 

    ```Ruby
    class Person
      # extend and include go first
      extend SomeModule
      include AnotherModule

      # constants are next
      SOME_CONSTANT = 20

      # afterwards we have attribute macros
      attr_reader :name

      # followed by other macros (if any)
      validates :name

      # public class methods are next in line
      def self.some_method
      end

      # followed by public instance methods
      def some_method
      end

      # protected and private methods are grouped near the end
      protected

      def some_protected_method
      end

      private

      def some_private_method
      end
    end
    ```

* Sınıf metodlarına modülleri tercih edin. Sınıflar sadece dışarıdan 
  kendilerine örnek oluşturularak kullanılmalıdır.

    ```Ruby
    # kotu
    class SomeClass
      def self.some_method
        # body omitted
      end

      def self.some_other_method
      end
    end

    # iyi
    module SomeClass
      module_function

      def some_method
        # body omitted
      end

      def some_other_method
      end
    end
    ```

* Favor the use of `module_function` over `extend self` when you want
  to turn a module's instance methods into class methods.

* Sınıf içinde modülün örnek metodlarını döndürmeyi istediğinizde 
  `extend self` kullanmak yerine `module_function` kullanımını destekleyin.

    ```Ruby
    # kotu
    module Utilities
      extend self

      def parse_something(string)
        # do stuff here
      end

      def other_utility_method(number, string)
        # do some more stuff
      end
    end

    # iyi
    module Utilities
      module_function

      def parse_something(string)
        # do stuff here
      end

      def other_utility_method(number, string)
        # do some more stuff
      end
    end
    ```

* Sınıf hiyerarşilerini tasarlarken [Liskov Substitution İlkelerine] (http://en.wikipedia.org/wiki/Liskov_substitution_principle)
  uyduğuna emin olun.

* Sınıfları mümkün olduğu kadar [sağlam nesneye yönelik](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design\)
   bir biçimde yazmayı deneyin.
* Always supply a proper `to_s` method for classes that represent
  domain objects.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#{@first_name} #{@last_name}"
      end
    end
    ```
* Erişimcileri (accessors) tanımlak için `attr` ailesinin fonksiyonlarını kullanın.

    ```Ruby
    # kotu
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # iyi
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```

* Erişimcileri tanımlamak için `Struct.new` kullanımınıda dikkate alabilirsiniz. 

    ```Ruby
    # iyi
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # daha iyi
    Person = Struct.new(:first_name, :last_name) do
    end
    ````

* `Struct.new` yapısını genişletmeyin (extend). - zaten `Struct.new` yapısı yeni bir sınıftır. 
  Genişletme `Struct.new` i gereksiz bir sınıf seviyesinde tanıtır  ve aynı zamanda dosya bir kaç kez isteniyorsa (require)
  korkunç hatalarda getirebilir

* Daha mantıklı yollarla bir sınıfın örneklerini oluşturmayı
  faktör metodlarını ekleyerek sağlamayı dikkate alın. 

    ```Ruby
    class Person
      def self.create(options_hash)
        # body omitted
      end
    end
    ```

* Miras kavramı üzerinde [duck-typing](http://en.wikipedia.org/wiki/Duck_typing) 
  kodlamasını tecih edin.

    ```Ruby
    # kotu
    class Animal
      # abstract method
      def speak
      end
    end

    # extend superclass
    class Duck < Animal
      def speak
        puts 'Quack! Quack'
      end
    end

    # extend superclass
    class Dog < Animal
      def speak
        puts 'Bau! Bau!'
      end
    end

    # iyi
    class Duck
      def speak
        puts 'Quack! Quack'
      end
    end

    class Dog
      def speak
        puts 'Bau! Bau!'
      end
    end
    ```

* Mirasta kötü sonuçlara sebep olabileceği için 
  `@@` değişken kullanımından kaçının.

    ```Ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => will print "child"
    ```

    Tüm sınıfları, sınıf hiyerarşisinde gerçekte bir sınıf 
    değişkeninin paylaşıldığını görebilirsiniz. Sınıf örnek değişkenleri 
    genelde sadece sınıf değişkenleri üzerinde tercih edilir.

* Assign proper visibility levels to methods (`private`, `protected`)
in accordance with their intended usage. Don't go off leaving
everything `public` (which is the default). After all we're coding
in *Ruby* now, not in *Python*.

* Uygun görünebilirlik düzeyinde (`private`, `protected`) metodların kullanımlarına 
  yönelik atamalar yapılır. Emin olamadım.


* Indent the `public`, `protected`, and `private` methods as much the
  method definitions they apply to. Leave one blank line above the
  visibility modifier
  and one blank line below in order to emphasize that it applies to all
  methods below it.

* public`, `protected`, ve `private metod tanımlamaları bir çok metoda 
  uygulanabilir. Bu tanımlamalardan sonra bir satır boşluk bırakın ve bu 
  tür tanımlamasından altındaki tüm metodlara uygulanır.

    ```Ruby
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end

      def another_private_method
        # ...
      end
    end
    ```

* Use `def self.method` to define singleton methods. This makes the code
  easier to refactor since the class name is not repeated.

* Tek (singleton) sınıfları tanımlamak için `def self.method` kullanın. 

    ```Ruby
    class TestClass
      # bad
      def TestClass.some_method
        # body omitted
      end

      # good
      def self.some_other_method
        # body omitted
      end

      # Also possible and convenient when you
      # have to define many singleton methods.
      class << self
        def first_method
          # body omitted
        end

        def second_method_etc
          # body omitted
        end
      end
    end
    ```

## İstisnalar

* `fail` metodu kullanarak istisnaları (exceptions) yayabilirsiniz. `raise` i sadece 
  istisnayı yakalayacağınız zaman kullanın ve bu sayede istisnayı yeniden yükseltin
  (re-raising exceptions) (çünkü burada hata oluşmuyor ama açıkça ve kasıtlı olarak 
  istisna çıkartıyorsun)


    ```Ruby
    begin
      fail 'Oops';
    rescue => error
      raise if error.message != 'Oops'
    end
    ```

* Never return from an `ensure` block. If you explicitly return from a
  method inside an `ensure` block, the return will take precedence over
  any exception being raised, and the method will return as if no
  exception had been raised at all. In effect, the exception will be
  silently thrown away.

* Asla `ensure` bloğundan değer geri döndürmeyin. Açıkça `ensure` bloğu içindeki
  bir metoddan değer döndürüyorsanız, geri döndürdüğünüz şey herhangi bir istisna üzerinde öncelik 
  alacaktır, ve metod istisna yoksa geri döndürülecek. 


    ```Ruby
    def foo
      begin
        fail
      ensure
        return 'very bad idea'
      end
    end
    ```

* Use *implicit begin blocks* where possible.
* Uygun olan yerlerde *begin* bloklarını kullanın.

    ```Ruby
    # kotu
    def foo
      begin
        # main logic goes here
      rescue
        # failure handling goes here
      end
    end

    # iyi
    def foo
      # main logic goes here
    rescue
      # failure handling goes here
    end
    ```

* *beklenmedik durum metodları (contingency methods)* nı kullanarak `begin` bloklarının 
  çoğalmasını azaltın.


    ```Ruby
    # kotu
    begin
      something_that_might_fail
    rescue IOError
      # handle IOError
    end

    begin
      something_else_that_might_fail
    rescue IOError
      # handle IOError
    end

    # iyi
    def with_io_error_handling
       yield
    rescue IOError
      # handle IOError
    end

    with_io_error_handling { something_that_might_fail }

    with_io_error_handling { something_else_that_might_fail }
    ```

* İstisnaları önleyin.
    ```Ruby
    # kotu
    begin
      # burada bir istsina olusuyor
    rescue SomeError
      # the rescue clause does absolutely nothing
    end

    # kotu
    do_something rescue nil
    ```

* `rescue` içinde onun niteleyici özelliklerini kullanmaktan kaçının.

    ```Ruby
    # bad - this catches all StandardError exceptions
    do_something rescue nil
    ```

* Akış kontrolü için istisnaları (exceptions) kulllanmayın.

    ```Ruby
    # kotu
    begin
      n / d
    rescue ZeroDivisionError
      puts 'Cannot divide by 0!'
    end

    # iyi
    if d.zero?
      puts 'Cannot divide by 0!'
    else
      n / d
    end
    ```

* Avoid rescuing the `Exception` class.  This will trap signals and calls to
  `exit`, requiring you to `kill -9` the process.

* Exception` sınıf kurtarımından kaçının. Bu durumda sinyaller yakalanacak 
  ve `exit` çağırılacak, size gereken ise `kill -9` süreci.

    ```Ruby
    # bad
    begin
      # calls to exit and kill signals will be caught (except kill -9)
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # exception handling
    end

    # iyi
    begin
      # a blind rescue rescues from StandardError, not Exception as many
      # programmers assume.
    rescue => e
      # exception handling
    end

    # 
    begin
      # an exception occurs here

    rescue StandardError => e
      # exception handling
    end

    ```

* Dış kaynaklardan elde ettiklerinizi programdan çıkarırken `ensure`
  bloğunu kullanın.

    ```Ruby
    f = File.open('testfile')
    begin
      # .. process
    rescue
      # .. handle error
    ensure
      f.close unless f.nil?
    end
    ```

## Collections

* Gerekmediği sürece dizi ve hash üretimlerini `.new` yazımıyla yapmayın.

    ```Ruby
    # kotu
    arr = Array.new
    hash = Hash.new

    # iyi
    arr = []
    hash = {}
    ```
* Kelimelerden oluşan iki ya da daha fazla elemanı olan dizilerde `%w` 
  yazımını kullanabilirsiniz.

    ```Ruby
    # kotu
    STATES = ['draft', 'open', 'closed']

    # iyi
    STATES = %w(draft open closed)
    ```

* Sembol tanımları için `%i` yazımını tercih edebilirsiniz. Bu yazımı da iki 
  ya da daha fazla elemandan oluşan diziler için tercih edin.

    ```Ruby
    # kotu
    STATES = [:draft, :open, :closed]

    # iyi
    STATES = %i(draft open closed)
    ```

* Çok fazla boş (nil) eleman içeren diziler oluşturmayın.

    ```Ruby
    arr = []
    arr[100] = 1 # now you have an array with lots of nils
    ```

* Bir dizinin ilk ya  da en son elemanına erişmek istiyorsanız `[0]` ya da `[-1]` yerine `first` ya da `last
  tercih edin.

* Use `Set` instead of `Array` when dealing with unique elements. `Set`
  implements a collection of unordered values with no duplicates. This
  is a hybrid of `Array`'s intuitive inter-operation facilities and
  `Hash`'s fast lookup.


* `Array` yerine `Set` kullanın. `Set` yapısı tuttuğunuz elemanların birden 
   fazla kez yapı içerisinde tutulmasına izin vermez. 

* Hash anahtarlarında stringler yerine sembolleri tercih edin.

    ```Ruby
    # kotu
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # iyi
    hash = { one: 1, two: 2, three: 3 }
    ```

* Hash anahtarlaında değiştirilebilir nesneler kullanmaktan kaçının.
* Hash keyleri sembol olduğunda hash yazımını kullanın.

    ```Ruby
    # kotu
    hash = { :one => 1, :two => 2, :three => 3 }

    # iyi
    hash = { one: 1, two: 2, three: 3 }
    ```

* Hash elemanlarını hash anahtarları görünebilir olduğunda `fetch` 
  kullanarak elde edin.

    ```Ruby
    heroes = { batman: 'Bruce Wayne', superman: 'Clark Kent' }
    # kotu - hata yaptigimizda fark edemeyiz
    heroes[:batman] # => "Bruce Wayne"
    heroes[:supermann] # => nil

    # iyi - fetch yapisi KeyError oldugunu anlamamizi saglar
    heroes.fetch(:supermann)
    ```
* Use `fetch` with second argument to use a default value

   ```Ruby
   batman = { name: 'Bruce Wayne', is_evil: false }

   # bad - if we just use || operator with falsy value we won't get the expected result
   batman[:is_evil] || true # => true

   # good - fetch work correctly with falsy values
   batman.fetch(:is_evil, true) # => false
   ```

* Rely on the fact that as of Ruby 1.9 hashes are ordered.

## Strings

* Prefer string interpolation instead of string concatenation:

    ```Ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Consider padding string interpolation code with space. It more clearly sets the
  code apart from the string.

    ```Ruby
    "#{ user.last_name }, #{ user.first_name }"
    ```

* Prefer single-quoted strings when you don't need string interpolation or
  special symbols such as `\t`, `\n`, `'`, etc.

    ```Ruby
    # bad
    name = "Bozhidar"

    # good
    name = 'Bozhidar'
    ```

* Don't leave out `{}` around instance and global variables being
  interpolated into a string.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # bad - valid, but awkward
      def to_s
        "#@first_name #@last_name"
      end

      # good
      def to_s
        "#{@first_name} #{@last_name}"
      end
    end

    $global = 0
    # bad
    puts "$global = #$global"

    # good
    puts "$global = #{$global}"
    ```

* Avoid using `String#+` when you need to construct large data chunks.
  Instead, use `String#<<`. Concatenation mutates the string instance in-place
  and is always faster than `String#+`, which creates a bunch of new string objects.

    ```Ruby
    # good and also fast
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

* When using heredocs for multi-line strings keep in mind the fact
  that they preserve leading whitespace. It's a good practice to
  employ some margin based on which to trim the excessive whitespace.

    ```Ruby
    code = <<-END.gsub(/^\s+\|/, '')
      |def test
      |  some_method
      |  other_method
      |end
    END
    #=> "def\n  some_method\n  \nother_method\nend"
    ```

## Regular Expressions

> Some people, when confronted with a problem, think
> "I know, I'll use regular expressions." Now they have two problems.<br/>
> -- Jamie Zawinski

* Don't use regular expressions if you just need plain text search in string:
  `string['text']`
* For simple constructions you can use regexp directly through string index.

    ```Ruby
    match = string[/regexp/]             # get content of matched regexp
    first_group = string[/text(grp)/, 1] # get content of captured group
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    ```

* Use non-capturing groups when you don't use captured result of parentheses.

    ```Ruby
    /(first|second)/   # bad
    /(?:first|second)/ # good
    ```

* Avoid using $1-9 as it can be hard to track what they contain. Named groups
  can be used instead.

    ```Ruby
    # bad
    /(regexp)/ =~ string
    ...
    process $1

    # good
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```

* Character classes have only a few special characters you should care about:
  `^`, `-`, `\`, `]`, so don't escape `.` or brackets in `[]`.

* Be careful with `^` and `$` as they match start/end of line, not string endings.
  If you want to match the whole string use: `\A` and `\z` (not to be
  confused with `\Z` which is the equivalent of `/\n?\z/`).

    ```Ruby
    string = "some injection\nusername"
    string[/^username$/]   # matches
    string[/\Ausername\z/] # don't match
    ```

* Use `x` modifier for complex regexps. This makes them more readable and you
  can add some useful comments. Just be careful as spaces are ignored.

    ```Ruby
    regexp = %r{
      start         # some text
      \s            # white space char
      (group)       # first group
      (?:alt1|alt2) # some alternation
      end
    }x
    ```

* For complex replacements `sub`/`gsub` can be used with block or hash.

## Percent Literals

* Use `%()`(it's a shorthand for `%Q`) for single-line strings which require both interpolation
  and embedded double-quotes. For multi-line strings, prefer heredocs.

    ```Ruby
    # bad (no interpolation needed)
    %(<div class="text">Some text</div>)
    # should be '<div class="text">Some text</div>'

    # bad (no double-quotes)
    %(This is #{quality} style)
    # should be "This is #{quality} style"

    # bad (multiple lines)
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # should be a heredoc.

    # good (requires interpolation, has quotes, single line)
    %(<tr><td class="name">#{name}</td>)
    ```

* Avoid `%q` unless you have a string with both `'` and `"` in
  it. Regular string literals are more readable and should be
  preferred unless a lot of characters would have to be escaped in
  them.

    ```Ruby
    # bad
    name = %q(Bruce Wayne)
    time = %q(8 o'clock)
    question = %q("What did you say?")

    # good
    name = 'Bruce Wayne'
    time = "8 o'clock"
    question = '"What did you say?"'
    ```

* Use `%r` only for regular expressions matching *more than* one '/' character.

    ```Ruby
    # bad
    %r(\s+)

    # still bad
    %r(^/(.*)$)
    # should be /^\/(.*)$/

    # good
    %r(^/blog/2011/(.*)$)
    ```

* Avoid the use of `%x` unless you're going to invoke a command with backquotes in it(which is rather unlikely).

    ```Ruby
    # bad
    date = %x(date)

    # good
    date = `date`
    echo = %x(echo `date`)
    ```

* Avoid the use of `%s`. It seems that the community has decided
  `:"some string"` is the preferred way to created a symbol with
  spaces in it.

* Prefer `()` as delimiters for all `%` literals, except `%r`. Given
  the nature of regexp in many scenarios a less command character than
  `(` might be a better choice for a delimiter.

    ```Ruby
    # bad
    %w[one two three]
    %q{"Test's king!", John said.}

    # good
    %w(one tho three)
    %q{"Test's king!", John said.}
    ```

## Metaprogramming

* Avoid needless metaprogramming.

* Do not mess around in core classes when writing libraries.
  (Do not monkey-patch them.)

* The block form of `class_eval` is preferable to the string-interpolated form.
  - when you use the string-interpolated form, always supply `__FILE__` and `__LINE__`, so that your backtraces make sense:

    ```ruby
    class_eval 'def use_relative_model_naming?; true; end', __FILE__, __LINE__
    ```

  - `define_method` is preferable to `class_eval{ def ... }`

* When using `class_eval` (or other `eval`) with string interpolation, add a comment block showing its appearance if interpolated (a practice I learned from the Rails code):

    ```ruby
    # from activesupport/lib/active_support/core_ext/string/output_safety.rb
    UNSAFE_STRING_METHODS.each do |unsafe_method|
      if 'String'.respond_to?(unsafe_method)
        class_eval <<-EOT, __FILE__, __LINE__ + 1
          def #{unsafe_method}(*args, &block)       # def capitalize(*args, &block)
            to_str.#{unsafe_method}(*args, &block)  #   to_str.capitalize(*args, &block)
          end                                       # end

          def #{unsafe_method}!(*args)              # def capitalize!(*args)
            @dirty = true                           #   @dirty = true
            super                                   #   super
          end                                       # end
        EOT
      end
    end
    ```

* Avoid using `method_missing` for metaprogramming because backtraces become messy, the behavior is not listed in `#methods`, and misspelled method calls might silently work, e.g. `nukes.launch_state = false`. Consider using delegation, proxy, or `define_method` instead. If you must use `method_missing`:

  - Be sure to [also define `respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html)
  - Only catch methods with a well-defined prefix, such as `find_by_*` -- make your code as assertive as possible.
  - Call `super` at the end of your statement
  - Delegate to assertive, non-magical methods:

    ```ruby
    # bad
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        # ... lots of code to do a find_by
      else
        super
      end
    end

    # good
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        find_by(prop, *args, &block)
      else
        super
      end
    end

    # best of all, though, would to define_method as each findable attribute is declared
    ```

## Misc

* Write `ruby -w` safe code.
* Avoid hashes as optional parameters. Does the method do too much? (Object initializers are exceptions for this rule).
* Avoid methods longer than 10 LOC (lines of code). Ideally, most methods will be shorter than
  5 LOC. Empty lines do not contribute to the relevant LOC.
* Avoid parameter lists longer than three or four parameters.
* If you really need "global" methods, add them to Kernel
  and make them private.
* Use module instance variables instead of global variables.

    ```Ruby
    # bad
    $foo_bar = 1

    #good
    module Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```

* Avoid `alias` when `alias_method` will do.
* Use `OptionParser` for parsing complex command line options and
`ruby -s` for trivial command line options.
* Code in a functional way, avoiding mutation when that makes sense.
* Do not mutate arguments unless that is the purpose of the method.
* Avoid more than three levels of block nesting.
* Be consistent. In an ideal world, be consistent with these guidelines.
* Use common sense.

## Tools

Here's some tools to help you automatically check Ruby code against
this guide.

### RuboCop

[RuboCop](https://github.com/bbatsov/rubocop) is a Ruby code style
checker based on this style guide. RuboCop already covers a
significant portion of the Guide, supports both MRI 1.9 and MRI 2.0
and has good Emacs integration.

### RubyMine

[RubyMine](http://www.jetbrains.com/ruby/)'s code inspections are
[partially based](http://confluence.jetbrains.com/display/RUBYDEV/RubyMine+Inspections)
on this guide.

# Contributing

Nothing written in this guide is set in stone. It's my desire to work
together with everyone interested in Ruby coding style, so that we could
ultimately create a resource that will be beneficial to the entire Ruby
community.

Feel free to open tickets or send pull requests with improvements. Thanks in
advance for your help!

# License

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
This work is licensed under a [Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/deed.en_US)

# Spread the Word

A community-driven style guide is of little use to a community that
doesn't know about its existence. Tweet about the guide, share it with
your friends and colleagues. Every comment, suggestion or opinion we
get makes the guide just a little bit better. And we want to have the
best possible guide, don't we?

Cheers,<br/>
[Bozhidar](https://twitter.com/bbatsov)
