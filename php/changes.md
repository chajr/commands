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
* **Allow a trailing comma in function calls** - `function($param, $param2,)`
* **JSON_THROW_ON_ERROR** - When passed to `json_decode/json_encode` allow to throw `JsonException`
* **PCRE2 Migration**
* **list() Reference Assignment** - `list($a, &$b) = $array;`
* **is_countable** - Check is value will be acceptable by `count` function
* **array_key_first(), array_key_last()** - Return first & last key of array
* **Make compact function reports undefined passed variables** - $baz = compact('foz'); // Notice: compact(): Undefined variable: foz
* **Argon2 Password Hash Enhancements**
* **Deprecate and Remove image2wbmp()**
* **Deprecate and Remove Case-Insensitive Constants** - all const must be upper case
* **Same Site Cookie**

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
```
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
