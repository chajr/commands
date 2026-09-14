## 5.4
* **traits**
* **short array syntax** - `$ar = []`
* **function array dereferencing** - `foo()[0]`
* **Closures support $this** - 
* **<?= always enabled**
* **Class member access on instantiation** - `(new Foo)->bar()`
* **binary number format** - `0b001001101`
* **file upload progress**
* **CLI web server** - `php -S localhost:8000`, `php -S 0.0.0.0:8000`
* **callable as typehint** - `foo(callable $var)`
* **static function from expression** - `static function foo(){}; Class::{'foo'}()`

## 5.5
* **Generators (yield)** - Generators provide an easy way to implement simple iterators without the overhead or complexity of implementing a class that implements the Iterator interface.
* **finally** - `try{} catch {} finally {}`
* **password_hash** - `password_hash($pass, $algorithm)` `hash_algos` - return list of available algorithms
* **list in foreach** - `foreach ($array as list($a, $b)) {`
* **empty() supports arbitrary expressions** - `empty(some_function())`
* **array and string literal dereferencing** - `[1, 2, 3][0] -> 1`, `'PHP'[0] -> P`
* **::class** - get class name `Namespace\SomeClass::class`
* **OPcache**
* **foreach now supports non-scalar keys**
* * **password_get_info** - return information about hash algoritm basing on has string

## 5.6
* **Constant expressions:** `const ONE = 1; const TWO = ONE * 2; const ARR = ['a', 'b'];`
* **Variadic functions via ...:** function f($req, $opt = null, ...$params) {
* **Argument unpacking via ...:** $operators = [2, 3]; echo add(1, ...$operators);
* **Exponentiation via:** `printf("2 ** 3 ==      %d\n", 2 ** 3);`
* **use function and use const:** namespace {use const Name\Space\FOO; use function Name\Space\f;
* **phpdbg:** https://phpdbg.room11.org/introduction.html
* **__debugInfo():** used on var_dump().
* **gost-crypto hash algorithm**
* **pgsql async support**
* **hash_equals() for timing attack safe string comparison:**
```
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
* **Scalar type hints:** `function add(int $a, int $b) {`
* **Return type declarations:** `function add(int $a, int $b): int {`
* **Anonymous classes:** `$foo = new class {`
* **The Closure::call() method:** `$binding = $getFooCallback->bindTo(new Foo,'Foo');` 5 -> 7 `$getFooCallback->call(new Foo).PHP_EOL;`
* **Generator delegation:** `yield from gen2();`
* **Generator return expressions:** `$gen->getReturn()`
* **The null coalesce operator:** `$message = $array['foo'] ?? 'not set';`
* **The space ship operator:** `1 <=> 1` _(0 when both values are equal; -1 when the left value is less than the right value; 1 if the left value is greater than the right value)_
* **Throwables (errors as exceptions):** `ArithmeticError`, `AssertionError`, `DivisionByZeroError`, `ParseError`, `TypeError`
* **Level support for the dirname() function:** `echo dirname('/usr/local/bin',3).PHP_EOL;`
* **The Integer division function:** `(10/3)` 5 -> 7 `intdiv(10, 3)`
* **Uniform variable syntax:** `${$foo['bar']['baz']}` 5 -> 7 `($$foo)['bar']['baz]`; `$foo->{$bar['baz']}` 5 -> 7`($foo->$bar)['baz']`; `$foo->{$bar['baz']}()` 5 -> 7 `($foo->$bar)['baz']()`; `Foo::{$bar['baz']}()` 5 -> 7 `(Foo::$bar)['baz']()`
* **Constant arrays using define():** `define('ANIMALS', [])`
* **Unicode codepoint escape syntax:** `echo "\u{9999}"` => `香`
* **Filtered unserialize():** `unserialize($foo, ["allowed_classes" => ["MyClass", "MyClass2"]]);`
* **IntlChar:** `echo IntlChar::charName('@')` => `COMMERCIAL AT`; `var_dump(IntlChar::ispunct('!'))` => `true`; `printf('%x', IntlChar::CODEPOINT_MAX)` => `10ffff`
* **Expectations:** `ini_set('assert.exception', 1); assert(false, new CustomError('Some error message'))`
* **Group use declarations:** `use some\namespace\{ClassA, ClassB, ClassC as C};`
* **Session options:** `session_start(['cache_limiter' => 'private', 'read_and_close' => true,]);`
* `preg_replace_callback_array()`
* **list() can always unpack objects implementing ArrayAccess** 
* `(clone $foo)->bar()`

## 7.1
* **Nullable types:** `function testReturn(): ?string` `function test(?string $name)`
* **Void functions:** `function swap(&$left, &$right): void`
* **Symmetric array destructuring:** `list($id1, $name1) = $data[0]` 5 -> 7 `[$id1, $name1] = $data[0]`; `foreach ($data as list($id, $name))` 5 -> 7 `foreach ($data as [$id, $name])`
* **Class constant visibility:** `public const PUBLIC_CONST_B = 2;`
* **iterable pseudo-type:** `function iterator(iterable $iter)`
* **Multi catch exception handling:** `catch (FirstException | SecondException $e)`
* **Support for keys in list():** `list("id" => $id1, "name" => $name1) = $data[0]` `["id" => $id1, "name" => $name1] = $data[0]`
* **Support for negative string offsets:** `var_dump("abcdef"[-2]);` `var_dump(strpos("aabbcc", "b", -3));`
* **Support for AEAD in ext/openssl**
* **Convert callables to Closures with Closure::fromCallable():** `return Closure::fromCallable([$this, 'privateFunction']);`
* **Asynchronous signal handling:** `pcntl_async_signals(true); pcntl_signal(SIGHUP,  function($sig) {echo "SIGHUP\n";}); posix_kill(posix_getpid(), SIGHUP);`
* **HTTP/2 server push support in ext/curl**
* **Too few arguments exception:** `Fatal error: Uncaught ArgumentCountError: Too few arguments to function sayHello()`

## 7.2
* **Object type:** `function test(object $obj) : object`
* **Extension loading by name:** Shared extensions no longer require their file extension (.so for Unix or .dll for Windows)
* **Abstract method overriding:** `abstract class A;` `abstract class B extends A`
* **Password hashing with Argon2:** http://php.net/manual/en/ref.password.php; `password_hash('password', PASSWORD_ARGON2I);`
* **Extended string types for PDO:** `$db->quote('über', PDO::PARAM_STR | PDO::PARAM_STR_NATL);` _ATTR_DEFAULT_STR_PARAM_ _PARAM_STR_CHAR_
* **Additional emulated prepares debugging information for PDO**
* **Support for extended operations in LDAP**
* **Address Information additions to the Sockets extension**
* **Parameter type widening:** Parameter types from overridden methods and from interface implementations may now be omitted
* **Allow a trailing comma for grouped namespaces:** `use Foo\Bar\{Foo, Bar, Baz,};`
* **pack() and unpack() endian support**
* **Enhancements to the EXIF extension**
* **SQLite3 allows writing BLOBs**
* **Oracle OCI8 Transparent Application Failover Callbacks**
* **Enhancements to the ZIP extension:** The ZipArchive class now implements the Countable interface
* **Casting array to object change:** `$array = ['foo','bar','sample_key' => 'baz']; $object = (object) $array; echo $object->0;` object to array `($object = new stdClass(); $object->{0} = 'foo';)`
* **Output of json_decode for object as array:** `var_dump(json_decode($string, null, 512, JSON_OBJECT_AS_ARRAY));`

## 7.3
* **Flexible Heredoc** - Allow to indent for end of herodoc
* **Heredoc and Nowdoc** - allow to add spaces before and after ending of block
* **Allow a trailing comma in function calls** - `function($param, $param2,)`
* **JSON_THROW_ON_ERROR** - When passed to `json_decode/json_encode` allow to throw `JsonException` - `json_decode("{", false, 512, JSON_THROW_ON_ERROR);`
* **list() Reference Assignment** - `list($a, &$b) = $array;`
* **is_countable** - Check is value will be acceptable by `count` function
* **array_key_first(), array_key_last()** - Return first & last key of array
* **Make compact function reports undefined passed variables** - $baz = compact('foz'); // Notice: compact(): Undefined variable: foz
* **Deprecate and Remove image2wbmp()**
* **Deprecate and Remove Case-Insensitive Constants** - all const must be upper case
* **PCRE2 instead of PCRE in regex**
* **Argon2id hash method** - `password_hash('password', PASSWORD_ARGON2ID, ['memory_cost' => 1<<17, 'time_cost' => 4, 'threads' => 2]);`
* **Same site cookie** _SameSite=_ `Lax` ? `Strict`
* **hrtime()** - return time independent of system time
* **DateTime::createFromImmutable()**
* **fpm_get_status()** - return state fastcgi state
* **gmp_binomial()** - calculate Newton symbol
* **gmp_lcm()** - calculating the least common multiple
* **gpm_perfect_power()** - check that number is power perfect
* **gmp_kronecker()** - calculate Kronecker symbol
* **CompileError & ParseError** - two new exceptions (currently throw by `token_get_all` & `eval`)
* **MBString – full support of case-mapping & case-folding**
* **instanceof - no fatal on literal** - `'test' instanceof \stdClass` - don't throw fatal
* **warning on continue in switch** - warning: "continue" targeting switch is equivalent to "break"
* **ArrayAccess don't cast string $offset into integer**

## 7.4
* **Deprecate alternate access to array elements and chars in string** - unable to use `{}` to access char on string, or array element
* **E_WARNING for invalid containers** - show warning when try to access not existing array key `$a=[1];var_dump($a[1])`
* **Base Convert improvements** - Error on ignored characters & Allow negative arguments
* **Numeric Literal Separator** - Allow to add separator into int val `$i=1_000_000_000`, `$i=135_00`, available for float, int, octal, hex, bin
* **Allow throwing exceptions from __toString()**
* **Spread Operator in Array Expression** - `$parts = ['apple', 'pear'];$fruits = ['banana', 'orange', ...$parts, 'watermelon'];`
* **Deprecate left-associative ternary operator**
* **Arrow Functions** - `function ($x) use ($arr) { return $arr[$x]; }` -> `fn($x) => $arr[$x]`
* **Weak References**
* **FFI - Foreign Function Interface** - allow to execute C language as script
* **Typed Properties 2.0** - `class User { public int $id;}`
* **Null Coalescing Assignment Operator** - `??=` operator, `$this->request->data['comments']['user_id'] ??= 'value';`
* **Preloading (opcache.preload)** - Allow to preload PHP file to OPcache, before it execution
* **Hash extension is always available**
* **Password Hashing Registry**
* **mb_str_split**
* **Reflection for references**
* **New custom object serialization mechanism** - `__serialize()` & `__unserialize(array $data)` methods
* **Escape PDO "?" parameter placeholder** - `$pdo->prepare('SELECT * FROM tbl WHERE json_col ?? ?');` will be convert into `SELECT * FROM tbl WHERE json_col ? 'foo'`
* **Covariant Returns and Contravariant Parameters** - Allow to override defined type in child by similar type
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
    // accepts all iterables, not just Iterator
    function concat(iterable $input) {/* . . . */}
}
```

## 8.0
* **JIT (Just-In-Time Compilation)** - On-the-fly code compilation aimed at improving PHP performance. This option is optional and can be disabled.
* **Nullsafe operator (`?->`)** - Allows convenient method/property calls on objects that may be `null`. `$result = $user?->getProfile()?->getEmail();`
* **Named Arguments** - Allows passing arguments to functions/methods by name, regardless of order.
    ```php
    public function doSomething(int $param1, string $param2) {}
    $obj->doSomething(
        param2: 'foo',
        param1: 123
    );
    ```
* **match()** - New construct as an alternative to `switch`, based on `===` comparison, no need for `break`.
    ```php
    $result = match($value) {
        1 => 'one',
        2 => 'two',
        3, 4, 5 => 'created',
        default => 'other'
    };
    ```
* **Union Types** - Allows declaring multiple types for parameters and return values, e.g. `int|float`.
* **Mixed type** - New type that can accept any value (int, float, string, array, object, null).
* **Short property syntax in class constructor** - Allows declaring properties directly in the constructor:
  ```php
  class User {
      public function __construct(
          public string $name,
          public int $age
      ) {}
  }
  ```
  - instead of:
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
* **::class on object** - Allows getting the class name of an object via `$object::class` (previously `get_class()`).
* **Allowed trailing comma in function parameter list** - Allows adding a comma at the end of the function/method parameter list.
* **New static methods for creating date objects** - `DateTime::createFromInterface(DateTimeInterface $date)`, `DateTimeImmutable::createFromInterface(DateTimeInterface $date)`
* **Catching exceptions without a mandatory variable** - You can omit the variable in the `catch` block, e.g. `catch (Exception) { ... }`
* **Stringable interface** - Requires implementation of the `__toString()` method.
* **New string functions**
    - `str_contains($haystack, $needle)` — checks if the string $needle occurs in the string $haystack. Returns a boolean value.
    - `str_starts_with($haystack, $needle)` — checks if the string $haystack starts with $needle.
    - `str_ends_with($haystack, $needle)` — checks if the string $haystack ends with $needle.
* **Abstract private methods in traits** - Allows declaring abstract private methods in traits.
* **get_debug_type()** - New function for determining variable type, better than `gettype()`.
* **BC (Backward Compatibility) changes**
    - Default error reporting changed to `E_ALL`.
    - Fatal error is no longer silenced by `@`.
    - Built-in functions now throw exceptions.
    - New order of operations for concatenation.
        - echo "result: " . $x + $y;
        - echo ("result: " . $x) + $y; // < PHP 8.0
        - echo "result: " . ($x + $y); // > PHP 8.0
    - Types in magic methods.
    - `display_startup_errors` enabled by default.
    - Constructor only via `__construct()`.
    - Accessing an undefined constant throws an error instead of a warning.
* **Attributes (Annotations)** - Native syntax for adding metadata to classes, methods, properties, etc.
      ```php
      #[Route('/api/users', methods: ['GET'])]
      class UserController {
          // ...
      }

      $reflector = new \ReflectionClass(UserController::class);
      $attributes = $reflector->getAttributes();
      ```
    -- Predefined attributes include `#[\Deprecated]`, `#[\Override]`, `#[\AllowDynamicProperties]`, `#[\ReturnTypeWillChange]`, `#[\SensitiveParameter]`, and `#[Attribute]` for defining custom attributes.

## 8.1
* **Enums** - Native enum support for type-safe enumerations, enum can contain methods
  ```php
  enum Status {
      case Draft;
      case Published;
      case Archived;
  }
  ```
* **Readonly properties** - `public readonly string $name;` Properties can only be written once.
* **Array unpacking with string keys** - Throws error if string keys are present.
* **`fsync()` and `fdatasync()` functions** - For file synchronization.
  - `fsync()` - flushes all buffers to disk, including metadata.
  - `fdatasync()` - flushes only data buffers, not metadata.
* **`never` return type** - Indicates a function never returns (e\.g\. always throws).
* **`final` class constants** - Prevents overriding constants in child classes.
* **New `array_is_list()` function** - Checks if array keys form a list. Must have sequential integer keys starting from 0.
* **Serializable is deprecated** - The `Serializable` interface is deprecated in favor of `__serialize()` and `__unserialize()` methods.
* ** New `full_path` in `$_FILES` superglobal** - Provides the full path of the uploaded file on the client machine.
* **Object as default value of a parameter** - Allows using an object as a default parameter value.
 * **Fibers** - Lightweight concurrency mechanism for cooperative multitasking.
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
* **Intersection types** - Allows declaring a parameter or return type that must satisfy multiple type constraints.
  ```php
  interface A {}
  interface B {}
  
  function foo(A&B $param) {
      // $param must implement both A and B
  }
  ```

## 8.2
* **Readonly classes** - All properties of the class are implicitly readonly.
  ```php
  readonly class User {
      public string $name;
      public int $age;
  }
  ```
* **Readonly classes** - All properties of the class are implicitly readonly.
  ```php
  readonly class User {
      public string $name;
      public int $age;
  }
  ```
* **Null, false, and true as standalone types** - Can be used as explicit types for parameters, properties, and return types.
  ```php
  function returnNull(): null { return null; }
  function returnFalse(): false { return false; }
  function returnTrue(): true { return true; }
  ```
* **Disjunctive Normal Form (DNF) Types** - Allows combining union and intersection types.
  ```php
  function foo((A&B)|null $param) {}
  ```
* **New "Random" Extension** - Provides new random number generation features and better random data generation.
  ```php
  $random = new \Random\Randomizer();
  echo $random->getInt(1, 100);
  ```
* **Deprecate dynamic properties** - Creating dynamic properties is deprecated, must use #[AllowDynamicProperties] attribute to allow.
  ```php
  #[AllowDynamicProperties]
  class Example {}
  ```
* **Constants in traits** - Allows constants to be defined in traits.
  ```php
  trait Foo {
      public const BAR = 'baz';
  }
  ```
* **Deprecate ${} string interpolation** - The ${} syntax for string interpolation is deprecated.
* **mysqli error reporting improvements** - Better error handling and reporting in mysqli extension.
* **New "Random" Extension Classes:**
   - Randomizer
   - RandomError
   - BrokenRandomEngineError
   - RandomException

## 8.3
* **Class Types in Constants** - Allow using class types in constant expressions
    ```php
    class Foo {
        const string = self::class;
    }
    ```
* **Dynamic Class Constant Fetch** - Allows fetching constants using dynamic expressions
    ```php
    class Foo {
        const BAR = 'bar';
        const BAZ = 'baz';
    }
    $const = 'BAR';
    echo Foo::{$const}; // Outputs: bar
    ```
* **Anonymous Readonly Classes**
    ```php 
    $obj = new readonly class {
        public string $prop;
    };
    ```
* **JSON Validate Function**
    - New `json_validate()` function to check if JSON string is valid
    - Faster than json_decode() when only validation needed
    ```php
    $isValid = json_validate($string);
    ```
* **#[\Override] Attribute** - Ensures method is actually overriding parent/interface method
    ```php
    class Child extends Parent {
        #[\Override]
        public function method() {}
    }
    ```
* **Data Validation for unserialize()** - New options to validate data during unserialization
    ```php
    unserialize($data, ["allowed_classes" => true, "max_depth" => 5]);
    ```
* **Changes to negative indexes in arrays**
    ```php
    $array = [];
    $array[-3] = 'foo';
    $array[] = 'bar';
    var_dump($array);
    // Output:
    //  array(2) {
      //    [-3]=>
      //    string(3) "foo"
      //    [-2]=>
      //    string(3) "bar"
      //  }
  ```
* **Default values for system variables** - New default values for system variables in ini file
    ```ini
    [server]
    listen = localhost:${MY_APP_FPM_PORT:-8080}
    ```
* **Randomizer Class Enhancements** - New methods for generating random data
    ```php
    $random = new \Random\Randomizer();
    echo $random->getFloat(0.0, 1.0);
    echo $random->nextFloat(); // Alias for getFloat(0, 1, IntervalBoundary::ClosedOpen)
    echo $random->getBytesFromString('abcdef123456', 2)
    ```
* **New Date Exceptions**
    - `DateMalformedIntervalStringException`
    - `DateInvalidOperationException`
    - `DateRangeError`
* **New function mb_str_pad()** - Pad a string to a certain length with another string, multi-byte safe
    ```php
    echo mb_str_pad('foo', 10, '-'); // Outputs: 'foo-------'
    - parameters:
      - $string
      - $length
      - $pad_string
      - $pad_type: (STR_PAD_RIGHT, STR_PAD_LEFT, STR_PAD_BOTH).
      - $encoding
  ```

## 8.4
* **Property Hooks** - Allow to change behavior of property access when try to `get` or `set` property
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
* **New functions array_find, array_find_key, array_all, and array_any**
    - `array_find(array $array, callable $callback)` - Returns the first element in the array that satisfies the provided testing function.
    - `array_find_key(array $array, callable $callback)` - Returns the key of the first element in the array that satisfies the provided testing function.
    - `array_all(array $array, callable $callback)` - Checks if all elements in the array satisfy the provided testing function.
    - `array_any(array $array, callable $callback)` - Checks if at least one element in the array satisfies the provided testing function.
    ```php
    $numbers = [1, 2, 3, 4, 5];
    $firstEven = array_find($numbers, fn($n) => $n % 2 === 0); // returns 2
    $firstEvenKey = array_find_key($numbers, fn($n) => $n % 2 === 0); // returns 1
    $allPositive = array_all($numbers, fn($n) => $n > 0); // returns true
    $anyGreaterThanThree = array_any($numbers, fn($n) => $n > 3); // returns true
    ```
* **New BCMth function bcdivmod** - Performs division and modulus in one operation, returning both results as an array.
    ```php
    list($quotient, $remainder) = bcdivmod('10', '3', 0);
    // $quotient = '3', $remainder = '1'
    ```
* **New functions mb_ucfirst, mb_lcfirst, mb_trim, mb_ltrim and mb_rtrim** - Multi-byte safe versions of ucfirst, lcfirst, trim, ltrim, and rtrim.
    ```php
    echo mb_ucfirst('hello'); // Outputs: 'Hello'
    echo mb_lcfirst('Hello'); // Outputs: 'hello'
    echo mb_trim('  hello  '); // Outputs: 'hello'
    echo mb_ltrim('  hello'); // Outputs: 'hello'
    echo mb_rtrim('hello  '); // Outputs: 'hello'
    ```
* **New CURL options**
    - `CURLOPT_DEBUGFUNCTION` - Allows setting a callback function for debugging CURL requests.
    - `CURLOPT_TCP_KEEPCNT` - Sets the number of TCP keep-alive probes to send before dropping the connection.
    - `CURLOPT_PREREQFUNCTION` - Allows setting a callback function to be called before the request is sent, useful for modifying the request.
    - `CURLOPT_SERVER_RESPONSE_TIMEOUT` - Sets a timeout for waiting for the server response after the request is sent.
* **New round types**
    - `PHP_ROUND_CEILING` - Rounds number to the nearest greater integer. For example 1.1 and 1.5 will be rounded to 2,
      and -1.1 and -1.5 to -1.
    - `PHP_ROUND_FLOOR` - Rounds number to the nearest lesser integer. For example 1.1 and 1.9 will be rounded to 1, and
      -1.1 and -1.9 to -2.
    - `PHP_ROUND_TOWARD_ZERO` - Rounds number towards zero. For example 1.9 and 1.1 will be rounded to 1, and -1.9 and
      -1.1 to -1.
    - `PHP_ROUND_AWAY_FROM_ZERO` - Rounds number away from zero. For example 1.1 and 1.9 will be rounded to 2, and -1.1
      and -1.9 to -2.
    ```
* **Attribute #[\Deprecated]** - Marks a function, method, class, or property as deprecated.
    ```php
    #[\Deprecated(since: "8.5", reason: "Use newFunction() instead")]
    function oldFunction() {
        // ...
    }
    ```

## 8.5
* **URI Extension** - Provides functions for parsing and manipulating URIs.
    ```php
    use Uri\Rfc3986\Uri;
    $uri = 'https://user:';
    $uri = new Uri($uri);
    var_dump($uri->getHost());
    ```
* **Pipe operator for method chaining** - Allows using the pipe operator `|>` to chain method calls in a more readable way.
    ```php
    $result = $object
        |> trim(...)
        |> (fn($str) => str_replace(' ', '-', $str))
        |> (fn($str) => str_replace('.', '', $str))
        |> strtolower(...);
    ```
* **New function array_first() and array_last()**
    - `array_first(array $array)` - Returns the first element in the array.
    - `array_last(array $array)` - Returns the last element in the array.
* **Attribute #[\NoDiscard]** - Indicates that the return value of a function or method should not be discarded.
    ```php
    #[\NoDiscard]
    function importantFunction(): string {
        return "This value should not be ignored";
    }
    ```
* **New error functions get_error_handler() and get_exception_handler()**
    - `get_error_handler()` - Returns the current error handler.
    - `get_exception_handler()` - Returns the current exception handler.
* **Closures and First-Class Callables in Constant Expressions** - Allows using closures and first-class callables in constant expressions.
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
