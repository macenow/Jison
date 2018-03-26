# Jison
Jison, a Tiny All-Powerful Json Parser (Python3.6)

Jison is a simple but powerful parser for Json manipulation. It parses a Json string into a dictionary with syntax check like what Python's builtin method `json.loads()` does, or write to file like `json.dump()`, but beside these it provides many additional features:

(the `object` refers any Json `key:value` pair)
1. String search under tree structure & hierarchical search
   (requires a 3rd party string match function which returns int/float in [0, 1] as parameter to work)
2. Single Json object acquisition
   (get one Json object returned as dict)
3. Multiple Json object acquisition
   (get all Json objects under any depth with same key value and return them as a list of dictionary)
4. Json object deletion
   (delete any single Json object)
5. Json object replacement
   (find and replace any single Json object)

## Examples
Assume `sample.json` has following content:
```json
{"id": 1,
 "params": [{"key1": "main",
             "key2": "client",
             "key3": "0/0",
             "key4": 0,
             "key5": 2},
            {"c1": null, "c2": true, "key1": "sub", "key2": "parent"}],
 "sample": "Jison"}
```

```python
# initialization can be a Json string, a Python dict or from a `sample` + `.json` file
jison = Jison(file_name='sample')
```

```python
# get dictionary from Json
pprint(jison.parse())
""" result
{'id': 1,
 'params': [{'key1': 'main',
             'key2': 'client',
             'key3': '0/0',
             'key4': 0,
             'key5': 2},
            {'c1': None, 'c2': True, 'key1': 'sub', 'key2': 'parent'}],
 'sample': 'Jison'}
"""

# get single Json object
pprint(jison.get_object(obj_name='params'))
""" result
{'params': [{'key1': 'main',
             'key2': 'client',
             'key3': '0/0',
             'key4': 0,
             'key5': 2},
            {'c1': None, 'c2': True, 'key1': 'sub', 'key2': 'parent'}]}
"""

# get multiple Json objects
print(jison.get_multi_object(obj_name='key1'))
""" result
[{'key1': 'main'}, {'key1': 'sub'}]
"""

# get a list of multiple Json objects, or None if no key is matched
key1, key99, key2 = jison.get_multi_object(obj_name=['key1', 'key99, 'key2'])
print(key1, key99, key2)
""" result
[{'key1': 'main'}, {'key1': 'sub'}]
None
[{'key2': 'client'}, {'key2': 'parent'}]
"""

# delete Json object and return a Jison instance, this operation will be written to file which the Json is loaded from
print(jison.remove_object(obj_name='params').json)
""" result
{'id': 1, 'sample': 'Jison'}
"""

# replace a Json object with another and return a Jison instance, this operation will be written to file which the Json is loaded from
print(jison.replace_object(obj_name='params', new_chunk={"replaced": "new_data"}).json)
""" result
{'id': 1, 'replaced': 'new_data', 'sample': 'Jison'}
"""
```