## 5.4
* **traits**
* **skrócona składnia tablic** - `$ar = []`
* **odwołanie do wyniku funkcji jako tablicy** - `foo()[0]`
* **domknięcia obsługują $this**
* **`<?=` zawsze włączone**
* **dostęp do składowych klasy przy tworzeniu obiektu** - `(new Foo)->bar()`
* **zapis liczb binarnych** - `0b001001101`
* **postęp wysyłania plików**
* **serwer HTTP w CLI** - `php -S localhost:8000`, `php -S 0.0.0.0:8000`
* **callable jako typ parametru** - `foo(callable $var)`
* **wywołanie metody statycznej z wyrażenia** - `static function foo(){}; Class::{'foo'}()`

## 5.5
* **generatory (yield)** - prosty sposób na iteratory, bez narzutu i złożoności pisania klasy implementującej interfejs `Iterator`
* **finally** - `try{} catch {} finally {}`
* **password_hash** - `password_hash($pass, $algorithm)`, `hash_algos` - zwraca listę dostępnych algorytmów
* **list() w foreach** - `foreach ($array as list($a, $b)) {`
* **empty() przyjmuje dowolne wyrażenia** - `empty(some_function())`
* **odwołanie do literału tablicy i stringa** - `[1, 2, 3][0] -> 1`, `'PHP'[0] -> P`
* **::class** - zwraca nazwę klasy, `Namespace\SomeClass::class`
* **OPcache**
* **foreach obsługuje klucze nieskalarne**
* **password_get_info** - zwraca informacje o algorytmie na podstawie samego hasha

## 5.6
* **wyrażenia w stałych:** `const ONE = 1; const TWO = ONE * 2; const ARR = ['a', 'b'];`
* **funkcje wariadyczne przez `...`:** `function f($req, $opt = null, ...$params) {`
* **rozpakowanie argumentów przez `...`:** `$operators = [2, 3]; echo add(1, ...$operators);`
* **potęgowanie operatorem `**`:** `printf("2 ** 3 ==      %d\n", 2 ** 3);`
* **use function i use const:** `namespace {use const Name\Space\FOO; use function Name\Space\f;`
* **phpdbg:** https://phpdbg.room11.org/introduction.html
* **__debugInfo():** używane przez `var_dump()`
* **algorytm haszujący gost-crypto**
* **asynchroniczna obsługa pgsql**
* **hash_equals() - porównanie stringów odporne na timing attack:**
```php
    $expected  = crypt('12345', '$2a$07$usesomesillystringforsalt$');
    $correct   = crypt('12345', '$2a$07$usesomesillystringforsalt$');
    $incorrect = crypt('1234',  '$2a$07$usesomesillystringforsalt$');
    
    var_dump(hash_equals($expected, $correct));
    var_dump(hash_equals($expected, $incorrect));
```

## 7.0
* `declare(strict_types=1)`
* `random_bytes(10);`
* `random_int(2,10)`
* **typy skalarne w parametrach:** `function add(int $a, int $b) {`
* **deklaracja typu zwracanego:** `function add(int $a, int $b): int {`
* **klasy anonimowe:** `$foo = new class {`
* **metoda Closure::call():** `$binding = $getFooCallback->bindTo(new Foo,'Foo');` 5 -> 7 `$getFooCallback->call(new Foo).PHP_EOL;`
* **delegowanie generatorów:** `yield from gen2();`
* **wartość zwracana z generatora:** `$gen->getReturn()`
* **operator null coalesce:** `$message = $array['foo'] ?? 'not set';`
* **operator statku kosmicznego:** `1 <=> 1` _(0 gdy wartości są równe; -1 gdy lewa jest mniejsza od prawej; 1 gdy lewa jest większa)_
* **Throwable - błędy jako wyjątki:** `ArithmeticError`, `AssertionError`, `DivisionByZeroError`, `ParseError`, `TypeError`
* **poziom zagnieżdżenia w dirname():** `echo dirname('/usr/local/bin',3).PHP_EOL;`
* **dzielenie całkowite:** `(10/3)` 5 -> 7 `intdiv(10, 3)`
* **ujednolicona składnia zmiennych:** `${$foo['bar']['baz']}` 5 -> 7 `($$foo)['bar']['baz]`; `$foo->{$bar['baz']}` 5 -> 7`($foo->$bar)['baz']`; `$foo->{$bar['baz']}()` 5 -> 7 `($foo->$bar)['baz']()`; `Foo::{$bar['baz']}()` 5 -> 7 `(Foo::$bar)['baz']()`
* **tablice jako stałe przez define():** `define('ANIMALS', [])`
* **escapowanie punktów kodowych Unicode:** `echo "\u{9999}"` => `香`
* **filtrowany unserialize():** `unserialize($foo, ["allowed_classes" => ["MyClass", "MyClass2"]]);`
* **IntlChar:** `echo IntlChar::charName('@')` => `COMMERCIAL AT`; `var_dump(IntlChar::ispunct('!'))` => `true`; `printf('%x', IntlChar::CODEPOINT_MAX)` => `10ffff`
* **assercje z wyjątkami:** `ini_set('assert.exception', 1); assert(false, new CustomError('Some error message'))`
* **grupowane deklaracje use:** `use some\namespace\{ClassA, ClassB, ClassC as C};`
* **opcje sesji:** `session_start(['cache_limiter' => 'private', 'read_and_close' => true,]);`
* `preg_replace_callback_array()`
* **list() rozpakowuje obiekty implementujące ArrayAccess**
* `(clone $foo)->bar()`

## 7.1
* **typy nullowalne:** `function testReturn(): ?string`, `function test(?string $name)`
* **funkcje zwracające void:** `function swap(&$left, &$right): void`
* **symetryczna destrukturyzacja tablic:** `list($id1, $name1) = $data[0]` 5 -> 7 `[$id1, $name1] = $data[0]`; `foreach ($data as list($id, $name))` 5 -> 7 `foreach ($data as [$id, $name])`
* **widoczność stałych klasy:** `public const PUBLIC_CONST_B = 2;`
* **pseudotyp iterable:** `function iterator(iterable $iter)`
* **wiele wyjątków w jednym catch:** `catch (FirstException | SecondException $e)`
* **klucze w list():** `list("id" => $id1, "name" => $name1) = $data[0]`, `["id" => $id1, "name" => $name1] = $data[0]`
* **ujemne offsety w stringach:** `var_dump("abcdef"[-2]);`, `var_dump(strpos("aabbcc", "b", -3));`
* **obsługa AEAD w ext/openssl**
* **zamiana callable na domknięcie przez Closure::fromCallable():** `return Closure::fromCallable([$this, 'privateFunction']);`
* **asynchroniczna obsługa sygnałów:** `pcntl_async_signals(true); pcntl_signal(SIGHUP,  function($sig) {echo "SIGHUP\n";}); posix_kill(posix_getpid(), SIGHUP);`
* **obsługa HTTP/2 server push w ext/curl**
* **wyjątek przy zbyt małej liczbie argumentów:** `Fatal error: Uncaught ArgumentCountError: Too few arguments to function sayHello()`

## 7.2
* **typ object:** `function test(object $obj) : object`
* **wczytywanie rozszerzeń po nazwie:** rozszerzenia współdzielone nie wymagają już podawania rozszerzenia pliku (`.so` na Uniksie, `.dll` na Windowsie)
* **nadpisywanie metod abstrakcyjnych:** `abstract class A;`, `abstract class B extends A`
* **haszowanie haseł algorytmem Argon2:** http://php.net/manual/en/ref.password.php; `password_hash('password', PASSWORD_ARGON2I);`
* **rozszerzone typy stringów w PDO:** `$db->quote('über', PDO::PARAM_STR | PDO::PARAM_STR_NATL);` _ATTR_DEFAULT_STR_PARAM_ _PARAM_STR_CHAR_
* **więcej informacji diagnostycznych dla emulowanych prepared statements w PDO**
* **obsługa rozszerzonych operacji w LDAP**
* **obsługa Address Information w rozszerzeniu Sockets**
* **rozszerzanie typów parametrów:** typy parametrów w nadpisanych metodach i implementacjach interfejsów można pominąć
* **przecinek na końcu grupowanych namespace'ów:** `use Foo\Bar\{Foo, Bar, Baz,};`
* **obsługa kolejności bajtów w pack() i unpack()**
* **usprawnienia rozszerzenia EXIF**
* **SQLite3 pozwala zapisywać BLOB-y**
* **Oracle OCI8 - callbacki Transparent Application Failover**
* **usprawnienia rozszerzenia ZIP:** klasa `ZipArchive` implementuje interfejs `Countable`
* **zmiana rzutowania tablicy na obiekt:** `$array = ['foo','bar','sample_key' => 'baz']; $object = (object) $array; echo $object->0;`, w drugą stronę `($object = new stdClass(); $object->{0} = 'foo';)`
* **json_decode zwracający obiekt jako tablicę:** `var_dump(json_decode($string, null, 512, JSON_OBJECT_AS_ARRAY));`

## 7.3
* **elastyczny heredoc** - pozwala wyrównać wcięciem znacznik zamykający heredoc
* **heredoc i nowdoc** - pozwala dodać spacje przed i po znaczniku zamykającym blok
* **przecinek na końcu listy argumentów wywołania** - `function($param, $param2,)`
* **JSON_THROW_ON_ERROR** - podany do `json_decode`/`json_encode` pozwala rzucić `JsonException` - `json_decode("{", false, 512, JSON_THROW_ON_ERROR);`
* **przypisanie przez referencję w list()** - `list($a, &$b) = $array;`
* **is_countable** - sprawdza, czy wartość zostanie przyjęta przez funkcję `count`
* **array_key_first(), array_key_last()** - zwracają pierwszy i ostatni klucz tablicy
* **compact() zgłasza niezdefiniowane zmienne** - `$baz = compact('foz');` // Notice: compact(): Undefined variable: foz
* **wycofanie i usunięcie image2wbmp()**
* **wycofanie i usunięcie stałych niewrażliwych na wielkość liter** - wszystkie stałe muszą być pisane wielkimi literami
* **PCRE2 zamiast PCRE w wyrażeniach regularnych**
* **algorytm Argon2id** - `password_hash('password', PASSWORD_ARGON2ID, ['memory_cost' => 1<<17, 'time_cost' => 4, 'threads' => 2]);`
* **ciasteczka SameSite** _SameSite=_ `Lax` | `Strict`
* **hrtime()** - zwraca czas niezależny od czasu systemowego
* **DateTime::createFromImmutable()**
* **fpm_get_status()** - zwraca stan procesu FastCGI
* **gmp_binomial()** - wylicza symbol Newtona
* **gmp_lcm()** - wylicza najmniejszą wspólną wielokrotność
* **gmp_perfect_power()** - sprawdza, czy liczba jest potęgą doskonałą
* **gmp_kronecker()** - wylicza symbol Kroneckera
* **CompileError i ParseError** - dwa nowe wyjątki (na razie rzucane przez `token_get_all` i `eval`)
* **MBString - pełna obsługa case-mappingu i case-foldingu**
* **instanceof bez fatala na literale** - `'test' instanceof \stdClass` nie rzuca już błędu krytycznego
* **ostrzeżenie przy continue w switch** - warning: "continue" targeting switch is equivalent to "break"
* **ArrayAccess nie rzutuje stringowego $offset na liczbę**

## 7.4
* **wycofanie alternatywnego dostępu do elementów tablicy i znaków stringa** - nie można już używać `{}` do pobrania znaku ze stringa czy elementu tablicy
* **E_WARNING dla nieprawidłowych kontenerów** - ostrzeżenie przy próbie odczytu nieistniejącego klucza tablicy, `$a=[1];var_dump($a[1])`
* **usprawnienia konwersji podstawy liczbowej** - błąd przy ignorowanych znakach i obsługa argumentów ujemnych
* **separator w literałach liczbowych** - pozwala dodać separator w liczbie, `$i=1_000_000_000`, `$i=135_00`; działa dla float, int, ósemkowych, szesnastkowych i binarnych
* **możliwość rzucania wyjątków z __toString()**
* **operator rozproszenia w wyrażeniach tablicowych** - `$parts = ['apple', 'pear'];$fruits = ['banana', 'orange', ...$parts, 'watermelon'];`
* **wycofanie lewostronnie łącznego operatora trójargumentowego**
* **funkcje strzałkowe** - `function ($x) use ($arr) { return $arr[$x]; }` -> `fn($x) => $arr[$x]`
* **słabe referencje**
* **FFI - Foreign Function Interface** - pozwala wywoływać kod w C jak skrypt
* **typowane właściwości 2.0** - `class User { public int $id;}`
* **operator przypisania null coalescing** - operator `??=`, `$this->request->data['comments']['user_id'] ??= 'value';`
* **preloading (opcache.preload)** - pozwala wczytać plik PHP do OPcache przed jego wykonaniem
* **rozszerzenie hash zawsze dostępne**
* **rejestr algorytmów haszowania haseł**
* **mb_str_split**
* **refleksja dla referencji**
* **nowy mechanizm własnej serializacji obiektów** - metody `__serialize()` i `__unserialize(array $data)`
* **escapowanie znaku "?" jako placeholdera w PDO** - `$pdo->prepare('SELECT * FROM tbl WHERE json_col ?? ?');` zostanie zamienione na `SELECT * FROM tbl WHERE json_col ? 'foo'`
* **kowariancja typów zwracanych i kontrawariancja parametrów** - pozwala nadpisać w klasie potomnej zadeklarowany typ typem pokrewnym
```php
interface Factory {
    function make(): object;
}
 
class UserFactory implements Factory {
    function make(): User;
}

interface Concatable {
    function concat(Iterator $input); 
}
 
class Collection implements Concatable {
    // przyjmuje wszystkie iterable, nie tylko Iterator
    function concat(iterable $input) {/* . . . */}
}
```

## 8.0
* **JIT (kompilacja Just-In-Time)** - kompilacja kodu w trakcie działania, mająca poprawić wydajność PHP. Opcjonalna, można wyłączyć.
* **operator nullsafe (`?->`)** - wygodne wywołanie metody lub właściwości na obiekcie, który może być `null`. `$result = $user?->getProfile()?->getEmail();`
* **argumenty nazwane** - pozwala przekazywać argumenty do funkcji i metod po nazwie, niezależnie od kolejności.
    ```php
    public function doSomething(int $param1, string $param2) {}
    $obj->doSomething(
        param2: 'foo',
        param1: 123
    );
    ```
* **match()** - nowa konstrukcja jako alternatywa dla `switch`, oparta na porównaniu `===`, bez potrzeby `break`.
    ```php
    $result = match($value) {
        1 => 'one',
        2 => 'two',
        3, 4, 5 => 'created',
        default => 'other'
    };
    ```
* **typy unii** - pozwala zadeklarować kilka typów dla parametru i wartości zwracanej, np. `int|float`.
* **typ mixed** - nowy typ przyjmujący dowolną wartość (int, float, string, array, object, null).
* **skrócona deklaracja właściwości w konstruktorze** - pozwala zadeklarować właściwości wprost w konstruktorze:
  ```php
  class User {
      public function __construct(
          public string $name,
          public int $age
      ) {}
  }
  ```
  - zamiast:
  - ```php
    class User {
        public string $name;
        public int $age;
          
        public function __construct(string $name, int $age) {
            $this->name = $name;
            $this->age = $age;
        }
    }
    ```
* **::class na obiekcie** - pozwala pobrać nazwę klasy obiektu przez `$object::class` (wcześniej `get_class()`).
* **przecinek na końcu listy parametrów funkcji** - pozwala dodać przecinek po ostatnim parametrze funkcji lub metody.
* **nowe metody statyczne do tworzenia obiektów daty** - `DateTime::createFromInterface(DateTimeInterface $date)`, `DateTimeImmutable::createFromInterface(DateTimeInterface $date)`
* **przechwytywanie wyjątków bez obowiązkowej zmiennej** - można pominąć zmienną w bloku `catch`, np. `catch (Exception) { ... }`
* **interfejs Stringable** - wymaga implementacji metody `__toString()`.
* **nowe funkcje na stringach**
    - `str_contains($haystack, $needle)` — sprawdza, czy `$needle` występuje w `$haystack`. Zwraca wartość logiczną.
    - `str_starts_with($haystack, $needle)` — sprawdza, czy `$haystack` zaczyna się od `$needle`.
    - `str_ends_with($haystack, $needle)` — sprawdza, czy `$haystack` kończy się na `$needle`.
* **abstrakcyjne metody prywatne w traitach** - pozwala deklarować abstrakcyjne metody prywatne w traitach.
* **get_debug_type()** - nowa funkcja do ustalania typu zmiennej, lepsza niż `gettype()`.
* **zmiany łamiące wsteczną kompatybilność (BC)**
    - domyślny poziom raportowania błędów zmieniony na `E_ALL`,
    - błąd krytyczny nie jest już wyciszany przez `@`,
    - funkcje wbudowane rzucają wyjątki,
    - nowa kolejność operacji przy konkatenacji:
        - `echo "result: " . $x + $y;`
        - `echo ("result: " . $x) + $y; // < PHP 8.0`
        - `echo "result: " . ($x + $y); // > PHP 8.0`
    - typy w metodach magicznych,
    - `display_startup_errors` włączone domyślnie,
    - konstruktor tylko przez `__construct()`,
    - odwołanie do niezdefiniowanej stałej rzuca błąd zamiast ostrzeżenia.
* **atrybuty (adnotacje)** - natywna składnia do dodawania metadanych do klas, metod, właściwości itd.
      ```php
      #[Route('/api/users', methods: ['GET'])]
      class UserController {
          // ...
      }

      $reflector = new \ReflectionClass(UserController::class);
      $attributes = $reflector->getAttributes();
      ```
    -- wbudowane atrybuty to m.in. `#[\Deprecated]`, `#[\Override]`, `#[\AllowDynamicProperties]`, `#[\ReturnTypeWillChange]`, `#[\SensitiveParameter]` oraz `#[Attribute]` do definiowania własnych.

## 8.1
* **enumy** - natywna obsługa typów wyliczeniowych, enum może zawierać metody
  ```php
  enum Status {
      case Draft;
      case Published;
      case Archived;
  }
  ```
* **właściwości readonly** - `public readonly string $name;` właściwość można zapisać tylko raz.
* **rozpakowanie tablic z kluczami tekstowymi** - rzuca błąd, jeśli w tablicy są klucze tekstowe.
* **funkcje `fsync()` i `fdatasync()`** - do synchronizacji plików.
  - `fsync()` - zrzuca na dysk wszystkie bufory, razem z metadanymi.
  - `fdatasync()` - zrzuca tylko bufory danych, bez metadanych.
* **typ zwracany `never`** - oznacza, że funkcja nigdy nie zwraca wartości (np. zawsze rzuca wyjątek).
* **stałe klasy `final`** - blokuje nadpisywanie stałych w klasach potomnych.
* **nowa funkcja `array_is_list()`** - sprawdza, czy klucze tablicy tworzą listę. Muszą być kolejnymi liczbami całkowitymi od 0.
* **wycofanie Serializable** - interfejs `Serializable` jest wycofany na rzecz metod `__serialize()` i `__unserialize()`.
* **nowe `full_path` w superglobalnej `$_FILES`** - podaje pełną ścieżkę wysłanego pliku na maszynie klienta.
* **obiekt jako domyślna wartość parametru** - pozwala użyć obiektu jako domyślnej wartości parametru.
* **fibery** - lekki mechanizm współbieżności do kooperatywnej wielozadaniowości.
  ```php
  $fiber = new Fiber(function() {
      echo "Fiber started\n";
      Fiber::suspend();
      echo "Fiber resumed\n";
  });
  $fiber->start();
  echo "Main code\n";
  $fiber->resume();
  ```
* **typy przecięcia** - pozwala zadeklarować typ parametru lub wartości zwracanej, który musi spełniać kilka warunków jednocześnie.
  ```php
  interface A {}
  interface B {}
  
  function foo(A&B $param) {
      // $param musi implementować i A, i B
  }
  ```

## 8.2
* **klasy readonly** - wszystkie właściwości klasy są niejawnie readonly.
  ```php
  readonly class User {
      public string $name;
      public int $age;
  }
  ```
* **null, false i true jako samodzielne typy** - można ich użyć jako jawnych typów parametrów, właściwości i wartości zwracanych.
  ```php
  function returnNull(): null { return null; }
  function returnFalse(): false { return false; }
  function returnTrue(): true { return true; }
  ```
* **typy w postaci normalnej dysjunkcyjnej (DNF)** - pozwala łączyć typy unii i przecięcia.
  ```php
  function foo((A&B)|null $param) {}
  ```
* **nowe rozszerzenie "Random"** - nowe możliwości generowania liczb i lepszych danych losowych.
  ```php
  $random = new \Random\Randomizer();
  echo $random->getInt(1, 100);
  ```
* **wycofanie dynamicznych właściwości** - tworzenie dynamicznych właściwości jest wycofane, trzeba użyć atrybutu `#[AllowDynamicProperties]`, żeby na nie pozwolić.
  ```php
  #[AllowDynamicProperties]
  class Example {}
  ```
* **stałe w traitach** - pozwala definiować stałe w traitach.
  ```php
  trait Foo {
      public const BAR = 'baz';
  }
  ```
* **wycofanie interpolacji `${}` w stringach** - składnia `${}` w stringach jest wycofana.
* **usprawnienia raportowania błędów w mysqli** - lepsza obsługa i raportowanie błędów w rozszerzeniu mysqli.
* **nowe klasy rozszerzenia "Random":**
   - Randomizer
   - RandomError
   - BrokenRandomEngineError
   - RandomException

## 8.3
* **typy klasowe w stałych** - pozwala używać typów klasowych w wyrażeniach stałych
    ```php
    class Foo {
        const string = self::class;
    }
    ```
* **dynamiczne pobieranie stałej klasy** - pozwala pobrać stałą przez wyrażenie
    ```php
    class Foo {
        const BAR = 'bar';
        const BAZ = 'baz';
    }
    $const = 'BAR';
    echo Foo::{$const}; // zwraca: bar
    ```
* **anonimowe klasy readonly**
    ```php 
    $obj = new readonly class {
        public string $prop;
    };
    ```
* **funkcja walidująca JSON**
    - nowa funkcja `json_validate()` sprawdza, czy string jest poprawnym JSON-em
    - szybsza od `json_decode()`, gdy potrzebna jest tylko walidacja
    ```php
    $isValid = json_validate($string);
    ```
* **atrybut #[\Override]** - upewnia się, że metoda faktycznie nadpisuje metodę klasy nadrzędnej lub interfejsu
    ```php
    class Child extends Parent {
        #[\Override]
        public function method() {}
    }
    ```
* **walidacja danych w unserialize()** - nowe opcje walidacji danych przy deserializacji
    ```php
    unserialize($data, ["allowed_classes" => true, "max_depth" => 5]);
    ```
* **zmiany w ujemnych indeksach tablic**
    ```php
    $array = [];
    $array[-3] = 'foo';
    $array[] = 'bar';
    var_dump($array);
    // wynik:
    //  array(2) {
      //    [-3]=>
      //    string(3) "foo"
      //    [-2]=>
      //    string(3) "bar"
      //  }
  ```
* **domyślne wartości zmiennych systemowych** - nowe domyślne wartości zmiennych systemowych w pliku ini
    ```ini
    [server]
    listen = localhost:${MY_APP_FPM_PORT:-8080}
    ```
* **rozbudowa klasy Randomizer** - nowe metody generowania danych losowych
    ```php
    $random = new \Random\Randomizer();
    echo $random->getFloat(0.0, 1.0);
    echo $random->nextFloat(); // alias dla getFloat(0, 1, IntervalBoundary::ClosedOpen)
    echo $random->getBytesFromString('abcdef123456', 2)
    ```
* **nowe wyjątki daty**
    - `DateMalformedIntervalStringException`
    - `DateInvalidOperationException`
    - `DateRangeError`
* **nowa funkcja mb_str_pad()** - dopełnia string do podanej długości innym stringiem, bezpiecznie dla wielobajtowych znaków
    ```php
    echo mb_str_pad('foo', 10, '-'); // zwraca: 'foo-------'
    - parametry:
      - $string
      - $length
      - $pad_string
      - $pad_type: (STR_PAD_RIGHT, STR_PAD_LEFT, STR_PAD_BOTH).
      - $encoding
  ```

## 8.4
* **hooki właściwości** - pozwalają zmienić zachowanie przy odczycie (`get`) i zapisie (`set`) właściwości
    ```php
    class User {
        private string $name {
            get {
                return $this->name;
            }
            set {
                if (strlen($value) < 3) {
                    throw new InvalidArgumentException("Name must be at least 3 characters");
                }
                $this->name = $value;
            }
        }
    }
    ```
* **nowe funkcje array_find, array_find_key, array_all i array_any**
    - `array_find(array $array, callable $callback)` - zwraca pierwszy element tablicy spełniający warunek.
    - `array_find_key(array $array, callable $callback)` - zwraca klucz pierwszego elementu spełniającego warunek.
    - `array_all(array $array, callable $callback)` - sprawdza, czy wszystkie elementy spełniają warunek.
    - `array_any(array $array, callable $callback)` - sprawdza, czy przynajmniej jeden element spełnia warunek.
    ```php
    $numbers = [1, 2, 3, 4, 5];
    $firstEven = array_find($numbers, fn($n) => $n % 2 === 0); // zwraca 2
    $firstEvenKey = array_find_key($numbers, fn($n) => $n % 2 === 0); // zwraca 1
    $allPositive = array_all($numbers, fn($n) => $n > 0); // zwraca true
    $anyGreaterThanThree = array_any($numbers, fn($n) => $n > 3); // zwraca true
    ```
* **nowa funkcja BCMath bcdivmod** - dzieli i wylicza resztę w jednej operacji, zwracając oba wyniki jako tablicę.
    ```php
    list($quotient, $remainder) = bcdivmod('10', '3', 0);
    // $quotient = '3', $remainder = '1'
    ```
* **nowe funkcje mb_ucfirst, mb_lcfirst, mb_trim, mb_ltrim i mb_rtrim** - wersje ucfirst, lcfirst, trim, ltrim i rtrim bezpieczne dla wielobajtowych znaków.
    ```php
    echo mb_ucfirst('hello'); // zwraca: 'Hello'
    echo mb_lcfirst('Hello'); // zwraca: 'hello'
    echo mb_trim('  hello  '); // zwraca: 'hello'
    echo mb_ltrim('  hello'); // zwraca: 'hello'
    echo mb_rtrim('hello  '); // zwraca: 'hello'
    ```
* **nowe opcje CURL**
    - `CURLOPT_DEBUGFUNCTION` - pozwala ustawić callback do debugowania zapytań CURL.
    - `CURLOPT_TCP_KEEPCNT` - ustawia liczbę sond TCP keep-alive przed zerwaniem połączenia.
    - `CURLOPT_PREREQFUNCTION` - pozwala ustawić callback wywoływany przed wysłaniem zapytania, przydatny do jego modyfikacji.
    - `CURLOPT_SERVER_RESPONSE_TIMEOUT` - ustawia limit czasu oczekiwania na odpowiedź serwera po wysłaniu zapytania.
* **nowe tryby zaokrąglania**
    - `PHP_ROUND_CEILING` - zaokrągla do najbliższej większej liczby całkowitej. Na przykład 1.1 i 1.5 zostaną zaokrąglone do 2,
      a -1.1 i -1.5 do -1.
    - `PHP_ROUND_FLOOR` - zaokrągla do najbliższej mniejszej liczby całkowitej. Na przykład 1.1 i 1.9 zostaną zaokrąglone do 1, a
      -1.1 i -1.9 do -2.
    - `PHP_ROUND_TOWARD_ZERO` - zaokrągla w kierunku zera. Na przykład 1.9 i 1.1 zostaną zaokrąglone do 1, a -1.9 i
      -1.1 do -1.
    - `PHP_ROUND_AWAY_FROM_ZERO` - zaokrągla w kierunku od zera. Na przykład 1.1 i 1.9 zostaną zaokrąglone do 2, a -1.1
      i -1.9 do -2.
    ```
* **atrybut #[\Deprecated]** - oznacza funkcję, metodę, klasę lub właściwość jako wycofaną.
    ```php
    #[\Deprecated(since: "8.5", reason: "Use newFunction() instead")]
    function oldFunction() {
        // ...
    }
    ```

## 8.5
* **rozszerzenie URI** - udostępnia funkcje do parsowania i modyfikowania URI.
    ```php
    use Uri\Rfc3986\Uri;
    $uri = 'https://user:';
    $uri = new Uri($uri);
    var_dump($uri->getHost());
    ```
* **operator pipe do łączenia wywołań** - pozwala użyć operatora `|>` do czytelniejszego łączenia wywołań.
    ```php
    $result = $object
        |> trim(...)
        |> (fn($str) => str_replace(' ', '-', $str))
        |> (fn($str) => str_replace('.', '', $str))
        |> strtolower(...);
    ```
* **nowe funkcje array_first() i array_last()**
    - `array_first(array $array)` - zwraca pierwszy element tablicy.
    - `array_last(array $array)` - zwraca ostatni element tablicy.
* **atrybut #[\NoDiscard]** - oznacza, że wartości zwracanej przez funkcję lub metodę nie należy pomijać.
    ```php
    #[\NoDiscard]
    function importantFunction(): string {
        return "This value should not be ignored";
    }
    ```
* **nowe funkcje get_error_handler() i get_exception_handler()**
    - `get_error_handler()` - zwraca aktualny handler błędów.
    - `get_exception_handler()` - zwraca aktualny handler wyjątków.
* **domknięcia i first-class callable w wyrażeniach stałych** - pozwala używać domknięć i first-class callable w wyrażeniach stałych.
    ```php
      final class PostsController
      {
        #[AccessControl(static function (
          Request $request,
          Post $post,
        ): bool {
          return $request->user === $post->getAuthor();
        })]
        public function update(
          Request $request,
          Post $post,
        ): Response {
          // ...
        }
      }
    ```
