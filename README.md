# php-bash-expand

[Bash expansions](http://wiki.bash-hackers.org/syntax/expansion/brace)

You need the [array_cartesian_product](https://github.com/bdelespierre/php-array-cartesian-product) function.

## Function

```PHP
/**
 * Bash like argument expansion (e.g. 'a{b,c}' result in ['ab', 'ac'])
 *
 * @param  string $arg
 * @return array
 */
function bash_expand(string $arg): array
{
    if (!preg_match('/{[^}]+}/', $arg)) {
        return [$arg];
    }

    $parts = preg_split('/{|}/', $arg);

    foreach ($parts as &$part) {
        $part = preg_match('/(\d+)\.\.(\d+)/', $part, $matches)
            ? range($matches[1], $matches[2])
            : explode(',', $part);
    }

    return array_map('implode', array_cartesian_product(...$parts));
}
```

## Example

```
>>> bash_expand('a{b,c,d}e{1..3}')
=> [
     "abe1",
     "abe2",
     "abe3",
     "ace1",
     "ace2",
     "ace3",
     "ade1",
     "ade2",
     "ade3",
   ]
```
