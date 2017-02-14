## Feedback and Contributions

All feedback and contributions will be very welcome.

In order to make all examples look and feel consistent, please follow the guidelines bellow.

Use pipes and 2 spaces to illustrate directory structure and nesting


**Bad**
```
  |_folder
    |_file

```

**Good**
```
   |_folder
   |  |_file
   |  |
```

Close leaves with a an extra line with a pipe

**Bad**

```
  |_folder
  |  |_nested_folder
```

**Good**

```
  |_folder
  |  |_nested_folder
  |  |
```

Do not add a new line for multiple leaves closed at the same time


**Bad**

```
  |_folder
  |  |_nested_folder
  |  |  |_leaf
  |  |  |
  |  |
  |

```

**Good**

```
  |_folder
  |  |_nested_folder
  |  |  |_leaf
  |  |  |
```


If an item in the folder structure is a level or more ahead of the item bellow it,
separate them by a new line.

Examples:

**Bad**

```
  |_folder
  |  |_item
  |  |_nested_folder
  |  |  |_item
  |  |_nested_folder2
  |  |  |_item

```

**Good**

```
  |_folder
  |  |_item
  |  |_nested_folder
  |  | |_item
  |  | |
  |  |
  |  |_nested_folder2
  |  |  |_item
  |  |  |
```

Do not separate same level items with new lines

**Bad**

```
  |_folder
  |  |_item
  |  |
  |  |_item2
  |  |
  |  |_folder1
  |  |
```

**Good**

```
  |_folder
  |  |_item
  |  |_item2
  |  |_folder1
  |  |
```
