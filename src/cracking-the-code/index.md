<!-- toc -->

# Cracking The Code | Interview algos

<!-- toc -->

## Start simple - Strategies

## Lists, Strings, Arrays

### First unique item

```python
# Time O(n), Space O(1)
def first_unique_item(list):
    hash_table = {}
    for item in list:
        if item in hash_table:
            hash_table[item] = hash_table[item] + 1
        else:
            hash_table[item] = 1

    for i, item in enumerate(list):
        if hash_table[item] == 1:
            return item
    return None

assert first_unique_item('mmmok') == 'o'
assert first_unique_item('mmm') == None
assert first_unique_item('abc') == 'a'
assert first_unique_item([1, 1, 2, 1]) == 2
```