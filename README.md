# php-bash-expand

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
