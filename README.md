## Folder Structure Best Practices for Webpack and AngularJS.


### Abstract

Moving assets to the Node.js world from a pure Rails environment, has posed certain questions.
Hopefully the suggested answers to those questions will help others in their explorations of the Node.js world.

### Motivation

The practices bellow attempt to organize your code into a more decoupled and a more maintainable way.


### Webpack

Webpack is described as a module builder. It takes in all your assets and their dependencies, modifies them to your liking
(using techniques such as minification, concatenation, and compression), and packages them into bundles.

Webpack can handle all types of assets using the concept of loaders.
The Webpack loaders know how to transform some of your more complex asset types, like sass or coffeescript, into
more basic types, like css and javascript, which are understood by your browser.


### Modular Organization

Because Webpack knows how to handle different types of assets, this allows us the flexibility to organize our assets
based on their function, rather than based on their type.

This means that while traditionally you might have had a file structure similar to:

```
project
  |_images
  |_javascripts
  |  |_app1
  |  |  |_directive1
  |  |  |  |_directive_1.js
  |  |  |  |
  |  |  |
  |  |  |_directive2
  |  |  |  |_directive_2.js
  |  |  |  |
  |  |
  |  |_app2
  |  |  |_directive3
  |  |  |  |_directive_3.js
  |  |  |  |
  |  |
  |  |_app1_main.js
  |  |_app2_main.js
  |  |
  |
  |_stylesheets
  |  |_app1
  |  |  |_app1_directive_1.css
  |  |  |_app1_directive_2.css
  |  |  |
  |  |
  |  |_app2
  |  |  |_app2_directive_3.css
  |  |  |
  |  |
  |  |_app1_main.scss
  |  |_app2_main.scss
  |  |
  |
  |_templates
  |  |_app1
  |  |  |_app1_index.html
  |  |  |_directive1
  |  |  |  |_directive_1.html
  |  |  |  |
  |  |  |
  |  |  |_directive2
  |  |  |  |_directive_2.html
  |  |  |  |
  |  |
  |  |_app2
  |  |  |_app2_index.html
  |  |  |_ directive3
  |  |  |  |_directive_3.html
  |  |  |  |
```

Now you can have something like:


```
project
  |_app1
  |  |_app1_main.js
  |  |_app1_index.html
  |  |_app1_main.scss
  |  |_directive1
  |  |  |_directive_1.js
  |  |  |_app1_directive_1.css
  |  |  |_directive_1.html
  |  |  |
  |  |
  |  |_directive2
  |  |  |_directive_2.js
  |  |  |_app1_directive_2.css
  |  |  |_directive_2.html
  |  |  |
  |
  |_app2
  |  |_app2_main.js
  |  |_app2_index.html
  |  |_app2_main.scss
  |  |_directive3
  |  |  |_directive_3.js
  |  |  |_app2_directive_3.css
  |  |  |_directive_3.html
  |  |  |
```

Notice the difference? If it's easier, you can think af the `app` folders as `features`, and the `directive` folders as `components`.
While in a small application this might not make too much of a difference,
the more code you add, the more it will become apparent that you need a sustainable structure, more similar to the second one.


### Best Practices

In order to keep things consistent, you need to abide to a certain set of practices.
The guidelines bellow attempt to address this.

Disclaimer: The guidelines bellow use the terms **Bad** and **Good** as suggestions only, based on personal preference.


#### Index.js Files

Node.js looks for index.js when you use the `require()` method.
(For more information on index files visit the [Node.js documentation](https://nodejs.org/api/modules.html#modules_folders_as_modules))

The `index.js` file is a very powerful tool for loading folders and defining load order.

However sometimes it is not really needed.

##### Single file in a folder

**Bad**

```
  |_main.js
  |_folder
  |  |_file.js
  |  |_index.js
  |  |
```

```javascript
// folder/index.js
require('file.js');
```

```javascript
// main.js
require('folder');
```

**Good**

```
  |_main.js
  |_folder
  |  |_file.js
  |  |
```

```javascript
// main.js
require('folder/file');
```

##### Multiple files in a folder

**Bad**


```
  |_main.js
  |  |_folder
  |  |  |_file.js
  |  |  |_file2.js
  |  |  |_file3.js
  |  |  |
```

```javascript
// main.js
require('folder/file');
require('folder/file2');
require('folder/file3');
```

**Good**

```
  |_main.js
  |  |_folder
  |  |  |_file.js
  |  |  |_file2.js
  |  |  |_file3.js
  |  |  |_index.js
  |  |  |
```

```javascript
// folder/index.js
require('./file');
require('./file2');
require('./file3');
```

```javascript
// main.js

require('folder');
```

##### Cross-referencing files in folders

**Bad**

```
  |_main.js
  |  |_folder
  |  |  |_file.js
  |  |  |_file2.js
  |  |  |_nested_folder
  |  |  |  |_file3.js
  |  |  |  |_file4.js
  |  |  |  |_file5.js
  |  |  |  |
```

```javascript
// main.js
require('folder/file');
require('folder/nested_folder/file4');

```

**Good**

If you need to use file and file4 in main, move it out of the nested folder and place it directly on the feature level

```
  |_main.js
  |  |_folder
  |  |  |_file2.js
  |  |  |_index.js
  |  |  |_nested_folder
  |  |  |  |_file3.js
  |  |  |  |_file5.js
  |  |  |  |_index.js
  |  |  |  |
  |  |
  |  |_file.js
  |  |_file4.js
  |  |
```

```javascript
// folder/nested_folder/index.js
require('file3.js');
require('file5.js');

```

```javascript
// folder/index.js
require('file2.js');
require('nested_folder');

```

```javascript
// main.js
require('./file');
require('./file4');
require('folder');
```

#### Component file structure

It is OK to separate files by types and functions, as long as they are in their own business logic and functionality.

##### Different asset types

**Bad**


```
  |_feature
  |  |_javascripts
  |  |  |_component1.js
  |  |  |_component2.js
  |  |  |
  |  |
  |  |_stylesheets
  |  |  |_component1.css
  |  |  |_component2.css
  |  |  |
  |  |
  |  |_templates
  |  |  |_component1.html
  |  |  |_component2.html
  |  |  |
```

**Good**

```
  |_feature
  |  |_component1
  |  |  |_component1.js
  |  |  |_component1.css
  |  |  |_component1.html
  |  |  |
  |  |
  |  |_component2
  |  |  |_component2.js
  |  |  |_component2.css
  |  |  |_component2.html
  |  |  |
```


**Bad**


```
  |_feature
  |  |_main.js
  |  |_index.html
  |  |_partial_for_index.html
  |  |_partial_for_index2.html
  |  |_partial_for_index3.html
  |  |
```

**Good**

```
  |_feature
  |  |_main.js
  |  |_templates
  |  |  |_index.html
  |  |  |_partial_for_index.html
  |  |  |_partial_for_index2.html
  |  |  |_partial_for_index3.html
  |  |  |
```

Note: this only makes sense if the templates are connected.
If they are not connected, try to separate them logically into bundles, like the example above.

##### Different purpose assets

It is OK to have a single file in a folder with a set functionality:

**Bad** (but still acceptable)

```
  |_feature
  |  |_main.js
  |  |_service1.js
  |  |_model1.js
  |  |_filter1.js
  |  |
```

**Good**


```
  |_feature
  |  |_main.js
  |  |_services
  |  |  |_service1.js
  |  |  |
  |  |
  |  |_models
  |  |  |_model1.js
  |  |  |
  |  |
  |  |_filters
  |  |  |_filter1.js
  |  |  |
```

If you have more than one of the same functionality type, it is better to always apply tho above:

**Bad**

```
  |_feature
  |  |_main.js
  |  |_service1.js
  |  |_service2.js
  |  |_model1.js
  |  |_model2.js
  |  |_model3.js
  |  |
```

**Good**


```
  |_feature
  |  |_main.js
  |  |_services
  |  |  |_service1.js
  |  |  |_service2.js
  |  |  |
  |  |
  |  |_models
  |  |  |_model1.js
  |  |  |_model2.js
  |  |  |_model3.js
  |  |  |
```

Note, the example above only makes sense if the services are related to the main feature and not to any of its components.
Follow the example bellow, if you have multiple services, which are relevant to different components.

**Bad**

```
  |_feature
  |  |_main.js
  |  |_component1
  |  |  |_component1.js
  |  |  |
  |  |
  |  |_services
  |  |  |_service_only_used_in_main.js
  |  |  |_service_only_used_in_component1.js
  |  |  |
```

**Good**


```
  |_feature
  |  |_main.js
  |  |_component1
  |  |  |_component1.js
  |  |  |_services
  |  |  |  |_service_only_used_in_component1.js
  |  |  |  |
  |  |
  |  |_services
  |  |  |_service_only_used_in_main.js
  |  |  |
```

Note: as soon as you want to use the service in the component into your feature, **move it out to the feature level**!
**Never** call a service from your component level, directly into your feature
(refer to the cross-referencing example in the `index.js` section).


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
