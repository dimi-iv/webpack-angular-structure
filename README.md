## Folder Structure Guidelines for Webpack and AngularJS.

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
  |_stylesheets
  |_templates
  |
```

Now you can have something like:


```
project
  |_feature1
  |  |_component1
  |  |_component2
  |  |
  |
  |_feature2
  |  |_component3
  |  |_component4
  |  |
```

While in a small application this might not make too much of a difference,
the more code you add, the more it will become apparent that you need a sustainable structure, more similar to the second one.

### Guidelines

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
