# GodotBigNumberClass

Use very BIG numbers in Godot Engine games.

It supports very big numbers with hundreds of digits, useful in an idle game. It can also format the number to a short string in AA-notation like 2.00M for two million, or for bigger numbers something like 4.56AA, 7.89ZZ or even bigger.

## Setup

For `4.1.x` and later, just drop the file in your project folder.

## Creating an instance

Multiple constructors can be used to instantiate a `Big` number:

```GDScript
var big_int = Big.new(100)  # From an integer
var big_exp = Big.new(2, 6) # Using a mantissa and exponent;
                            # here means 2,000,000
var big_cpy = Big.new(big_exp)  # As a copy of another big number;
                                # See why below
```

Some operations executed on a `Big` number modify that instance. As such, you will probably find yourself creating temporary copies of other `Big` numbers to run those operations while leaving the original value untouched.

```GDScript
var unit_cost: Big = Big.new(2, 6)

func cost(count: Big) -> Big:
    var _total = Big.new(count)
    return _total.times_equals(unit_cost)
```

## Mathematical operations

The normal operands used for integer and floating-point operations (e.g. `+`, `-`, `*`, `/`, `%`) cannot be used on a `Big` number.

Instead, you can use the provided functions for a `Big` number:

```GDScript
# 'value' can be numeric or another Big number
my_big_number.plus(value)       # my_big_number + value
my_big_number.minus(value)      # my_big_number - value
my_big_number.times(value)      # my_big_number * value
my_big_number.divided_by(value)  # my_big_number / value
my_big_number.mod(value)        # my_big_number % value
my_big_number.power_of(value) # Only accepts float or int; my_big_number ** value
```

Or for calculating `Big` numbers together:

```GDScript
Big.add(big_a, big_b)       # big_a + big_b
Big.subtract(big_a, big_b)  # big_a - big_b
Big.multiply(big_a, big_b)  # big_a * big_b
Big.divide(big_a, big_b)    # big_a / big_b
Big.modulo(big_a, big_b)    # big_a % big_b
```

Operations can be chained:

```GDScript
var my_big_number = Big.new(100)
print(my_big_number.plus(10).divided_by(10).to_string())  # Will print "11"
```

## Modifying

Some functions can modify the current object as well. It's as simple as writing "equals" after the operation names

```GDScript
# 'value' can be numeric or another Big number
my_big_number.plus_equals(value)         # my_big_number += value
my_big_number.minus_equals(value)        # my_big_number -= value
my_big_number.times_equals(value)        # my_big_number *= value
my_big_number.divided_by_equals(value)    # my_big_number /= value
my_big_number.mod_equals(value)          # my_big_number %= value
my_big_number.power_of_equals(value) # Only accepts float or int; my_big_number **= value

Big.round_down(my_big_number)           # Rounds down the mantissa
```

## Comparisons

The normal operands still cannot be used here (e.g. `>`, `>=`, `==`, `<`, `<=`).

Instead, you can use the provided functions:

```GDScript
# 'value' can be numeric or another Big number

my_big_number.is_greater_than(value)
my_big_number.is_greater_or_equal_to(value)
my_big_number.is_equal_to(value)
my_big_number.is_less_or_equal_to(value)
my_big_number.is_less_than(value)
```

## Static functions

The following static functions are available:

```GDScript
# 'big_value' must be a Big number;
# 'value' can be numeric or another Big number

var smallest = Big.min_value(big_value, value)
var largest = Big.max_value(big_value, value)
var positive = Big.absolute(big_value)
```

## Formatting as a string

An important aspect with big numbers is being able to display them in a way that is readable and makes sense for the player. The following functions can be used to do so:

```GDScript
var big = Big.new(12345, 12)
print(big.to_aa())               # 12.34aa
print(big.to_american_name())     # 12.34quadrillion
print(big.to_european_name())     # 12.34billiard print(big.to_long_name())         # 12.34quadrillion
print(big.to_metric_name())       # 12.34peta
print(big.to_metric_name())       # 12.34peta
print(big.to_prefix())           # 12.34
print(big.to_scientific())       # 1.2345e16
print(big.to_short_scientific())  # 1.2e16
print(big.to_string())           # 12345000000000000
```
Some of the functions have arguments with default values, not displayed in the snippet above.

You can tweak the way the strings are formatted by calling the following static functions:

```GDScript
Big.set_thousand_name("string_value")       # Defaults to "thousand"
Big.set_thousand_separator("string_value")  # Defaults to ".", you should set this with your localization settings
Big.set_decimal_separator("string_value")   # Defaults to ",", you should set this with your localization settings
Big.set_suffix_separator("string_value")   # Defaults to an empty string
Big.set_reading_separator("string_value")   # Defaults to an empty string

Big.set_dynamic_numbers(int_value)  # Defaults to 4, makes it such that values will only have four digits when dynamic_decimals is true, ie. 1,234 or 12,34
Big.set_dynamic_decimals(bool_value)  # Defaults to true

Big.set_small_decimals(int_value)     # Defaults to 2
Big.set_thousand_decimals(int_value)  # Defaults to 2
Big.set_big_decimals(int_value)       # Defaults to 2
```

For example:

```GDScript
var big = Big.new(12345, 12)
print(big.to_aa())  # With default settings: 12,34aa

Big.set_suffix_separator(" ")
Big.set_decimal_separator(".")
Big.set_dynamic_decimals(false)
Big.set_small_decimals(1)
print(big.to_aa())  # With modified settings: 12.3 aa
```
