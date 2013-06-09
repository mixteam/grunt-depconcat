# grunt-depconcat

> Concat depended files

## Getting Started
This plugin requires Grunt `~0.4.1`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-depconcat --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-depconcat');
```

## The "depconcat" task

### Overview
In your project's Gruntfile, add a section named `depconcat` to the data object passed into `grunt.initConfig()`.

	grunt.initConfig({
	  depconcat: {
	    options: {
	      // Task-specific options go here.
	    },
	    your_target: {
	      // Target-specific file lists and/or options go here.
	    },
	  },
	})

### Options

#### separator
Type: `String`  
Default: `grunt.util.linefeed`

Concatenated files will be joined on this string. If you're post-processing concatenated JavaScript files with a minifier, you may need to use a semicolon `';'` as the separator.


#### requireEmplate
Type: `String`  
Default: `\\/\\/@require ([\\w\\.]+)`

The RegExp string of depended indication.

#### ext
Type: `String`  
Default: `.js`

The extname for files.

#### except
Type: `Array`  
Default: `[]`

The excepted files.

### Usage Examples

#### basic usage for js
In this example, if `1.js` require `2.js` , and `2.js` require `3.js`, the Concatenated files will be joined as `3.js` + `2.js` + `1.js`.


require js file like this:

	//require 1
	//requite 2

concat them:

	grunt.initConfig({
	  depconcat: {
	    options: {},
	    files: {
	      'dest/all.js': ['src/1.js', 'src/2.js', 'src/3.js'],
	    },
	  },
	})

#### basic usage for css
In this example, if `1.css` import `2.css` , and `2.css` import `3.css`, the Concatenated files will be joined as `3.css` + `2.css` + `1.css`.

require css file like this as usual:

	@import url(1.css)
	@import url(2.css)

concat them:
	grunt.initConfig({
	  depconcat: {
	    options: {},
	    files: {
	      'dest/all.css': ['assets/1.css', 'src/2.css', 'src/3.css'],
	    },
	  },
	})

#### except files

	grunt.initConfig({
	  depconcat: {
	    options: {
			except: ['none.js']
		},
	    files: {
	      'dest/all.js': ['<%= assets/*.js %>'],
	    },
	  },
	})

#### use custom `requiteTemplate`

In this example, we have some tepmlate files named `1.tpl`, `2.tpl`, `3.tpl`.

The file `1.tpl` like this:

	//extend 2

	/*concat for 1.tpl*/

The file `2.tpl` like this:

	//extend 3

	/*concat for 2.tpl*/

The file `3.tpl` like this:

	/*concat for 3.tpl*/


concat them:

	grunt.initConfig({
	  depconcat: {
	    options: {
			ext: '.tpl',
			requiteTemplate: '\\/\\/extend ([\\w.]+)'
		},
	    files: {
	      'dest/all.tpl': ['<%= templates/*.tpl %>'],
	    },
	  },
	})

#### use depended tree

In this example, we have a depended tree for files, like this:

	{
		'1.js': ['2.js', '3.js'],
		'2.js': ['4.js']
		'3.js': [],
		'4.js': []
	}

concat them

	grunt.initConfig({
	  depconcat: {
	    options: {},
	    my_target: {
		  tree: 'src/.tree',
	      dest: 'dest/all.js'
	    },
	  },
	})

## Warning

Be careful of `Circular Dependency`. In version `0.1.0` we don't check it.

## Release History
_(Nothing yet)_
