# Pairwise test case generator

Inspired by Java implementation https://github.com/RetailMeNot/pairwise. 

## Installation

With [Composer](https://getcomposer.org):

```bash
composer require richenzi/pairwise
```

## Why?

> Pairwise (a.k.a. all-pairs) testing is an effective test case generation technique that is based on the observation that most faults are caused by interactions of at most two factors. Pairwise-generated test suites cover all combinations of two therefore are much smaller than exhaustive ones yet still very effective in finding defects.

Instead of exhaustive testing using all possible combination of all parameters,
we generate just enough test cases to cover all of their pairs. 

In our example below we have three parameters with five, four and five values.
Total number of all combinations is 5x4x5 = 100. With pairwise technique we generate around 25 test cases.
Not much, but with more parameters and with more values, the savings can be huge.

For one of our test (see /tests/files/generator/test_3.txt), where number of all possible combinations reaches millions, 
pairwise gives you only few more than a hundred!

## Usage

### From input

Generate test cases using direct input

```php
$testCases = Pairwise::fromData([
    'browser' => ['Chrome', 'Firefox', 'Opera', 'Safari', 'IE'],
    'os' => ['Windows', 'Ubuntu', 'Debian', 'MacOS'],
    'connectivity' => ['Wi-Fi', 'LTE', '3G', '4G', '5G']
])->generate();
```

### From file

Just pass the path to you data file and that's it. 

```php
$testCases = Pairwise::fromFile('/path/to/file')->generate();
```

Parser expects file in parameter-per-row fashion. It means that each parameter
values are on a single row, separated by a delimiter.

Example:

```text
Chrome	Firefox	Opera	Safari	IE
Windows	Ubuntu	Debian	MacOS
Wi-Fi	LTE 3G	Slow	3G	5G
```

Default delimiter is tab, but you can tell the parser which one to use.  

```php
$testCases = Pairwise::fromFile('/path/to/file', [
    'delimiter' => ';'
])->generate();
```

## Output

Generator returns an array of test cases. Each test case is an array with a combination 
of values - one value for each parameter.

```php
[
    ['Chrome', 'Windows', 'Wi-Fi'],
    ['Chrome', 'MacOS', '5G'],
    ['Safari', 'Ubuntu', 'Wi-Fi'],
    // ...
];
```

## License

This package is licensed under [The MIT License (MIT)](LICENSE).
