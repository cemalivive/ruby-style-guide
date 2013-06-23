#Ruby Kodlama Tarzı Klavuzu

Aşağıdaki maddeler Ruby ile kodlama yapılırken en fazla kullanılan ortak kurallardır.
Ancak resmi bir belge değildir. Belgenin orjinaline [buradan](https://github.com/bbatsov/ruby-style-guide) ulaşabilirsiniz.


## İçindekiler

* [Kaynak Kod Düzeni](#kaynak-kod-dzeni)
* [Söz Dizimi](#sz-dizimi)
* [Adlandırmalar](#adlandrmalar)
* [Yorumlar](#yorumlar)
    * [Yorum Notları](#yorum-notlar)
* [Sınıflar](#snflar--modller)
* [İstisnalar](#stisnalar)
* [Koleksiyonlar](#koleksiyonlar)
* [Dizgeler](#dizgeler)
* [Düzenli İfadeler](#dzenli-fadeler)
* [Yüzde Kullanımları](#yzde-kullanmlar)
* [Meta Programlama](#meta-programlama)
* [Çeşitli Durumlar](#eitli-durumlar)

## Kaynak Kod Düzeni

* Kodlamada `UTF-8` kullanılmalı.
* Girinti başına **2 boşluk** kullanılmalı ancak bu durum sekmeler (tab) 2 boşluk ayarlanarak kullanılmamalı.
* Unix tarzı satır sonlandırma kullanılmalı. (*BSD/Solaris/Linux/OSX gibi sistemlerde varsayılan olarak gelir,
   ancak Windows kullanıcıları bu konuda daha dikkatli olmalıdırlar.)
* İfadeleri birbrinden ayırmak için `;` kullanmayın. Sonuç olarak satır başına bir ifade kullanın.

    ```Ruby
    # kotu kodlama
    puts 'foobar'; # noktali virgul gereksiz

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

    # iyi
    result = 1 \
             - 2
    ```

* Zincirlenmiş method çağırımı devam ederken bir başka satırdaki `.` ikinci satırı korur. 

    ```Ruby
    # kotu - ikinci satırı anlamak için ilk satırı danışmaya gerek duyulur
    one.two.three.
      four

    # iyi- bu sekilde ikinci satırda ne yapildigi hemen aciklanir
    one.two.three
      .four
    ```

* Eğer aralıklar fazlaysa bir satırda, metodun tüm parametreleri hizalanmalı.

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
    # kotu
    == begin
    comment line
    another comment line
    == end

    # iyi
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
* `and` ve `or` ifadeleri yasaklandı. Bunların yerine hep `&&` ve `||` kullanın.

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

* Çok satırdan oluşan ifadelerde `?:` (üçlü operatör) kullanımından kaçının.
  Bunlarınc yerine `if/unless` yapısı kullanın.


* Tek satırdan oluşan `if/unless` ifadesi olduğunda bu ifadeyi değiştirin.
  İyi biralternatif olarak `&&/||` akış kontrollerini kullanabilirsiniz.

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

* Parametresi olan tüm metod çağırımlarında parantezler kullanın. 
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

      # kotu
      def do_something(options = {})
        unless options[:when] == :later
          output(self.options[:message])
        end
      end

      # iyi
      def do_something(params = {})
        unless params[:when] == :later
          output(options[:message])
        end
      end
    end
    ```
* Durum kontrolleri yapılırken aynı zamanda değer ataması için `=` ifadesini kullanmayın.

    ```Ruby
    # kotu ( + bir uyari)
    if (v = array.grep(/foo/))
      do_something(v)
      ...
    end

    # kotu ( + bir uyari)
    if v = array.grep(/foo/)
      do_something(v)
      ...
    end

    # iyi
    v = array.grep(/foo/)
    if v
      do_something(v)
      ...
    end
    ```

* `||=` ifadesini değişkeni başlatmak için kullanabilirsiniz.

    ```Ruby
    # name degiskenine Bozhidar degeri sadece name nil ise ya da degeri false ise atanir.
    name ||= 'Bozhidar'
    ```

* `||=` ifadesini mantıksal değişkenleri başlatmak için kullanmayın
  (değişkenin o anki değeri false ise olabilecekleri dikkate alın).
    ```Ruby
    # kotu - eger false ise bile true deger ataması yapılır
    enabled ||= true

    # iyi
    enabled = true if enabled.nil?
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

* Sınıf ve modüller için `[CamelCase]` ([kelimlerin baş harfi büyük](http://en.wikipedia.org/wiki/CamelCase))
  kullanın. (HTTP, RFC, XML gibi kısaltmalardaki büyük harfleri değiştirmeyin)

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

* Doğrulama metodlarının (true, false gibi mantıksal değerler döndüren methodlar)
  isimlerinde soru işareti karakterini kullanıbilirsiniz. (`Array#empty?`)



* Define the non-bang (safe) method in terms of the bang (dangerous)
  one if possible.

* Çoğunlukla [bang olmayan metod](http://rubylearning.com/satishtalim/writing_own_ruby_methods.html) tanımlamalarını tercih edin.
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

* `map` + `flatten` yerine `flat_map` kullanın. Bu durum diziler için 2 den fazla derinliğe uygulanmaz.
  `users.first.songs == ['a', ['b','c']]` kullanıyorsanız, `flat_map` den ziyade `map + flatten` kullanın.
   
    ```Ruby
    # kotu
    all_songs = users.map(&:songs).flatten.uniq

    # iyi
    all_songs = users.flat_map(&:songs).uniq
    ```

## Yorumlar

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
* Yorum satırı yazdığınız için kötü kod yazmayın. Kendi kendini açıklayabilen, anlaşılabilen kodlar yazın.

### Yorum Notları

* Açıklamalar genellikle gereken kodun hemen üst satırında olmalıdır.
* annotation (not) anahtar kelimesini iki nokta üst üste ve bir boşluk takip etmelidir,
  o zaman bir not problemi açıklıyor.
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

    ```Ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

* Sonraki zamanlarda eklemek istediğiniz şeyleri `TODO` notu ile belirtin.
* Düzeltilmesi gereken yerlere `FIXME` notu bırakın.
* `OPTIMIZE` notunu ise yavaş ya da verimsiz kodlar (performans problemleri) için kullanabilirsiniz.
* Tartışmaya açık olabilecek yerler için `HACK` notunu yazabilirsiniz, sonraki zamanlarda 
  o kod yeniden düzenlenilecektir.
* Çalışmasına yönelik doğrulama için `REVIEW` notunu kullanabilirsiniz.


* Uygunsa diğer özel açıklama anahtar kelimelerini de kullanabilirsiniz, fakat proje `README` 
  ya da benzer başka bir belgede bunları yazdığınıza emin olun.

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

* Sınıflar için uygun `to_s` metotlarını sağlayın. Bu metotlar 
  alan nesnelerini (domain objects) gösterir.

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

* `Struct.new` yapısını genişletmeyin (extend). - zaten `Struct.new` yapısı yeni bir 
  sınıftır. Genişletme `Struct.new` i gereksiz bir sınıf seviyesinde tanıtır ve aynı 
  zamanda dosya bir kaç kez isteniyorsa (require) korkunç hatalarda getirebilir.

* Daha mantıklı yollarla bir sınıfın örneklerini oluşturmayı
  `factory` metodlarını ekleyerek sağlamayı dikkate alın. Factory metotlar için [buradan](http://en.wikipedia.org/wiki/Factory_method)
  yararlanabilirsiniz.

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
    değişkeninin paylaşıldığını görebilirsiniz. Sınıf örnek 
    değişkenleri genelde sadece sınıf değişkenleri üzerinde 
    tercih edilir.

* Uygun görünebilirlik düzeyinde (`private`, `protected`) metodların kullanımlarına 
  yönelik atamalar yapın. Her şeyi `public` olarak kodlamayın.

* public`, `protected`, ve `private metod tanımlamaları bir çok metoda 
  uygulanabilir. Bu tanımlamalardan sonra bir satır boşluk bırakın ve bu 
  şekilde bu tür tanımlaması onun altındaki tüm metodlara uygulanır.

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

* Tek (singleton) sınıfları tanımlamak için `def self.method` kullanın. 

    ```Ruby
    class TestClass
      # kotu
      def TestClass.some_method
        # body omitted
      end

      # iyi
      def self.some_other_method
        # body omitted
      end

      # ayni zamanda bir çok singleton 
      # sinif tanimlamaya elverislidir.
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
  istisna üretilmesi gerçekleştiriliyor)


    ```Ruby
    begin
      fail 'Oops';
    rescue => error
      raise if error.message != 'Oops'
    end
    ```

* `ensure` bloğunu değer geri döndürmek için kullanmayın. `ensure` bloğu 
  kullanıyorsanız geri döndürdüğünüz değer, herhangi bir istisna yerine öncelik alacaktır,
  ve eğer metot istisna yoksa bile değer geri döndürecektir. Bu durumda istisna durumu 
  çöpe atılmış olur. `ensure` bloğu hata oluşsa da, oluşmasa da çalışan bir bloktur.

    ```Ruby
    def foo
      begin
        fail
      ensure
        return 'very bad idea'
      end
    end
    ```
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

* Exception` sınıflarında rescue kullanımından kaçının. Çünkü bu durumda sinyaller yakalanacak 
  ve `exit` çağırılacak, size gereken ise `kill -9` sürecidir.

    ```Ruby
    # kotu
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

## Koleksiyonlar

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
* Hash anahtarları sembol olduğunda hash yazımını kullanın.

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
## Dizgeler

* Stringleri aralara eklemeler yaparak birleştirmek yerine, bütün halinde yazın:

    ```Ruby
    # kotu
    email_with_name = user.name + ' <' + user.email + '>'

    # iyi
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Stringlerin arasına boşluk bırakmaya dikkat edin.

    ```Ruby
    "#{ user.last_name }, #{ user.first_name }"
    ```

* Çift tırnak yerine `\t`, `\n`, `'` gibi özel sembollere ihtiyacınız olmadğında
  tek tırnak kullanın.
    ```Ruby
    # bad
    name = "Bozhidar"

    # good
    name = 'Bozhidar'
    ```

* String içinde global ve örnek (instance) değişkenleri yazarken `{}` kullanın.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # kotu - gecerli, ama kullanissiz
      def to_s
        "#@first_name #@last_name"
      end

      # iyi
      def to_s
        "#{@first_name} #{@last_name}"
      end
    end

    $global = 0
    # kotu
    puts "$global = #$global"

    # iyi
    puts "$global = #{$global}"
    ```
* Değişen büyük string parçalarını `String#+` şeklinde birleştirmek yerine 
  `String#<<` yapısını kullanın.

    ```Ruby
    # iyi ve hizli
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

## Düzenli İfadeler

* Düz metin içerisinde string aramak için düzenli ifadeler kullanmayın:
 `string['text']`

* Basit yapılar için string indeksi sayesinde direkt olarak 
  düzenli ifadeleri kullanabilirsiniz.

    ```Ruby
    match = string[/regexp/]             # get content of matched regexp
    first_group = string[/text(grp)/, 1] # get content of captured group
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    ```

* Karmaşık yapılarda yerine yazma için `sub`/`gsub` yapılarını kullanabilirsiniz.

## Yüzde Kullanımları

* `%()` (`%Q` kısaltmasıdır) birleştirilerek yazılan ve çift tırnak içeren stringler 
  (ikisi bir arada olmalı) için kullanabilirsiniz.

    ```Ruby
    # kotu (string birleştirmesi, araya giren değişkenler gibi ifadeler yok)
    %(<div class="text">Some text</div>)
    # '<div class="text">Some text</div>' şeklinde olmalı

    # kotu (çift tirnak yok)
    %(This is #{quality} style)
    # "This is #{quality} style" şeklinde olmalı

    # kotu (çok satırdan meydana geliyor)
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # 

    # good (requires interpolation, has quotes, single line)
    %(<tr><td class="name">#{name}</td>)
    ```

* Bir stringde `'` ve `"` karakterlerine aynı anda sahip olmadıkça `%q`
  kullanımından kaçının. 

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

* '/' karakteriyle çok fazla eşleşme gerektiren işlem yaptırıyorsanız `%r` kullanın.

    ```Ruby
    # kotu
    %r(\s+)

    # hala kotu
    %r(^/(.*)$)
    # should be /^\/(.*)$/

    # iyi
    %r(^/blog/2011/(.*)$)
    ```

* Bir komutun içerisinde ters tırnakla bir şey çağırmanız gerekmediği sürece 
  `%x` kullanmayın (genelde alışılmadık bir durum).

    ```Ruby
    # kotu
    date = %x(date)

    # iyi
    date = `date`
    echo = %x(echo `date`)
    ```

* `%r` dışında tüm `%` yazımlar için ayırıcı olarak `()` kullanımını tercih edin. 
  Düzenli ifadelerin doğası gereği bir çok durumda `(` kullanımı daha iyidir.
  
    ```Ruby
    # kotu
    %w[one two three]
    %q{"Test's king!", John said.}

    # iyi
    %w(one tho three)
    %q{"Test's king!", John said.}
    ```

## Meta Programlama

* Gereksiz meta program kullanımından kaçının.

* Kütüphane yazarken çevresinde karışıklık yapmayın. 
  ([monkey-patch](http://en.wikipedia.org/wiki/Monkey_patch) yapmaktan kaçının)

* `class_eval` blok biçimlerini araya string katarak tercih edebilirsiniz.
  - araya string katılmış biçimini kullandığınızda, genellikle `__FILE__` ve `__LINE__`sağlanır, 
    bu yüzden geri izleme  mantıklıdır:

    ```ruby
    class_eval 'def use_relative_model_naming?; true; end', __FILE__, __LINE__
    ```

  - `define_method` is preferable to `class_eval{ def ... }`

* `class_eval` (ya da diğer hesaplatıcıları) ile dizgeleri kullanırken, onun görünümünü gösteren 
  yorum blokları ekleyin:

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

* Metaprogramming için `method_missing` kullanımından kaçının çünkü geri izleme dağınık olur. 
  Bu davranış `#metotlar`da listelenmiş değildir, ve yanlış yazılmış metot çağırıları çalışabilir,
  örneğin `nukes.launch_state = false`. Delege (delegation), vekil (proxy) ya da `define_method` 
  kullanmayı dikkate alın. Eğer `method_missing` kullanırsanız:

  - [`respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html) tanımladığınızdan emin olun.
  - Metotları iyi tanımlanmış ön ekleri ile yakalayın, 
    `find_by_*` gibi -- bu durum mümkün kodunuzu olduğu kadar iddialı yapar. 
  - İfadenizin sonunda `super` çağırımı yapın.
  - Delegate to assertive, non-magical methods:
  - Bir satir eksik

    ```ruby
    # kotu
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        # ... lots of code to do a find_by
      else
        super
      end
    end

    # iyi
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        find_by(prop, *args, &block)
      else
        super
      end
    end

    # best of all, though, would to define_method as each findable attribute is declared
    ```

## Çeşitli Durumlar

* Daha güvenli kod için `ruby -w` şeklinde çalıştırın.
* İsteğe bağlı parametreler olarak hash yapısı kullanmayın. 
  (Nesne ilklendiricileri bu kural için istisnadır.)
* 10 satırdan fazla metod kullanmayın. İdeal olarak, metodlar 5 satırdan kısa olmalıdır. 
  Boşluk bırakılan satırlar bu 5 satır içinde sayılmaz.
* 3 ya da 4'ten fazla parametre kullanmayın.
* "global" metodlara gercekten ihtiyacınız varsa, bu metodları çekirdeğe private olarak ekleyin.
* Modül örnek değişkenlerini global değişkenler yerine kullanın.

    ```Ruby
    # kotu
    $foo_bar = 1

    # iyi
    module Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```

* `alias_method` kullanacağınız zaman `alias` kullanmayın.
* Karmaşık komut satırı seçeneklerinde `OptionParser` kullanın ve önemsiz 
  komut satırı seçenekleri için `ruby -s` kullanın.
* Üçten fazla iç içe blok kullanımından kaçının.

# License

![Creative Commons Lisansı](http://i.creativecommons.org/l/by/3.0/88x31.png)
Bu belge [Creative Commons 3.0](http://creativecommons.org/licenses/by/3.0/deed.en_US) ile lisanlanmıştır.


