# PHP NameSpace

When you define a function a constant or a class without a namespace definition by
default they will be put in <b>a global space</b>

Including two classes with the same name in PHP can cause errors.

## The Problem:

<ul>
<li>Without namespaces, if you have two classes named Transaction (one from Stripe and one from Paddle),
including both Stripe and Paddle would cause an error.
  
and the same thing would happen with the constants or functions
</li>


<li>
PHP wouldn't know which Transaction class to use.
</li>
</ul>

## The Solution: Namespaces
<ul>
<li>Define a namespace using the <b>namespace</b> keyword at the beginning of your file and after declare statement</li>

```PHP
<?php
declare(strict_types=1);

namespace Paddle; //define namespace

class Transaction
{

}
```

1- Add qualifying class name including the namespace

```PHP
var_dump(new Paddle\Transaction);

```


2- import namespace class 

```PHP

use Paddle\Transaction;
var_dump(new Transaction);

```
</ul>


# Sub-namespaces

Create sub-namespaces using a backslash (\\) within the main namespace 
(e.g., namespace PaymentGateway\Paddle).

# **Note:**

♦️ Recommended and also a standard to match namespace with the folder structure .

♦️ Namespaces can also be applied to constants and functions, although it's less common and not actively encouraged.

♦️ syntax for importing namespaced functions and constants using the use keyword.

`use function (namespace name)`
`use const (namespace name)`

♦️ If you're working on an open-source package (code that anyone can use and contribute to), you should likely use a namespace that reflects your own name or the name of your package.

♦️ If you use a class name without any prefix (namespace or backslash), PHP first tries to locate the class within the current local namespace.

♦️ If the class isn't found in the current local namespace, PHP then if it doesn't exist it will throw you an error.

**Function & Constants**

♦️ By default, PHP searches for functions in the current namespace.
If not found locally, it falls back to the global namespace (built-in functions).


Using (\\)  speed improvement because you're telling php exactly where to load that function from


Ex:

```php
<?php
declare(strict_types=1);

namespace PaymentGateway\Paddle;

class Transaction
{

    public function __construct()
    {
        var_dump(\explode(',', 'hello,world')); // (\)load the function from globle namespace (built-in function)
    }
}
function explode($separator, $str)
{

    return 'foo';
}
```


# Accessing PHP's built-in classes within namespaces:

<ul>

<li>
When you're working inside a namespace, PHP prioritizes finding classes within the current local namespace by default.
</li>

<li>
If a built-in PHP class (like DateTime) isn't defined in your current namespace, you have two options to use it:
  
</li>


1- Using a Backslash (\\): Prefix the class name with a backslash (\) to explicitly tell PHP to load it from the global namespace, where built-in classes reside.

```php
<?php
declare(strict_types=1);

namespace PaymentGateway\Paddle;


class Transaction
{

    public function __construct()
    {
        var_dump(new \DateTime());
    }
}
```

2- Importing the Class: Use the use statement to import the class directly from the global namespace into your current namespace. This can improve code readability and avoid repetitive backslashes.



```php
<?php
declare(strict_types=1);

namespace PaymentGateway\Paddle;

use DateTime; //Import built-in class in php


class Transaction
{

    public function __construct()
    {
        var_dump(new DateTime());
    }
}
```

</ul>





# Accessing Classes from Other Namespaces:


<li>There are two ways to use a class from another namespace:</li>

1- Full Qualified Name (\): Prefix the class name with a backslash (\) and the complete namespace path (e.g., \Paddle\Transaction).

2- Import Statement (use): Use the use statement to import the class directly into your current namespace (e.g., use Paddle\Transaction;).


Ex:

```php
declare(strict_types=1);

namespace PaymentGateway\Paddle;

class Transaction
{

    public function __construct()
    {
        var_dump(new Notification\Email()); # Qualified Name
    }
}
```
> Class "PaymentGateway\Paddle\Notification\Email" not found 

Solve :

```php
declare(strict_types=1);

namespace PaymentGateway\Paddle;

class Transaction
{

    public function __construct()
    {
        var_dump(new \Notification\Email()); # Full Qualified Name (full namespace)
    }
}
```





 
# [Name Resolution Rules](https://www.php.net/manual/en/language.namespaces.rules.php)


# Aliasing

Example : 

```php
use PaymentGateway\Paddle\Transaction;
use PaymentGateway\Stripe\Transaction as StripeTransaction;

$paddleTransaction = new Transaction; // Refers to PaddleTransaction class
$stripeTransaction = new StripeTransaction;// Refers to aliased StripeTransaction class

var_dump($paddleTransaction,$stripeTransaction);

```

***Benefits of Aliasing:***

1- Resolve naming conflicts: Allows you to use two classes with the same name in the same file.

2- Shorter names: Create shorter aliases for long class names for better readability.

3- External library conflicts: Use aliasing when your class name conflicts with a class from an external library.


```php
Ex point 3 : 

<?php

declare(strict_types=1);

namespace PaymentGateway\Paddle;

use VendorName\Transaction  as VendorTransaction;
class Transaction
{

}
```

- Group multiple class imports from the same namespace using square brackets:
`use PaymentGateway\{Transaction, CustomerProfile};`

- Included files don't inherit namespace imports from the parent file.
- You need to import them again in the included file if needed.
- Multiple namespaces in one file ( not recommended).
This approach can lead to naming conflicts and make code harder to maintain.


# Benefits of NameSpace:
<ul>
<li>Prevents naming conflicts</li>
<li>Better structure and organize your classes and code</li>
</ul>


