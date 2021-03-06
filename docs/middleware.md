
## Connect Middleware

 Stylus ships with Connect middleware for auto-compiling Stylus sheets whenever they're modified.

## stylus.middleware(options)

 Return Connect middleware with the given `options`.

#### Options

      `force`     When __true__ styles will always re-compile
      `src`       Source directory used to find .styl files
      `dest`      Destination directory used to output .css files
                  when undefined defaults to `src`.
      `compress`  Whether the output .css files should be compressed
      `compile`   Custom compile function, accepting the arguments
                  `(str, path)` returning the renderer.
      `firebug`   Emits debug infos in the generated css that can
                  be used by the FireStylus Firebug plugin
      `linenos`   Emits comments in the generated css indicating
                  the corresponding stylus line

#### Examples
 
 Here we set up the custom compile function so that we may
 alter the renderer by providing additional settings.
 
 By default, the compile function simply sets the `filename`
 and renders the CSS.
 
        function compile(str, path) {
          return stylus(str)
            .import(__dirname + '/css/mixins/blueprint')
            .import(__dirname + '/css/mixins/css3')
            .set('filename', path)
            .set('warn', true)
            .set('compress', true);
        }
 
 Pass the middleware to Connect, grabbing `.styl` files from this directory
 and saving `.css` files to _./public_. Also supplying our custom `compile` function.
 
 Following that, we have a `static` layer setup to serve the `.css`
 files generated by Stylus.
 
        var stylus = require('stylus');
 
        var app = connect();

        app.use(stylus.middleware({
            src: __dirname
          , dest: __dirname + '/public'
          , compile: compile
        }));

        app.use(connect.static(__dirname + '/public'));

 When `force` is used, the styles will unconditionally be re-compiled. But even without this option, the Stylus middleware is smart enough to detect changes in `@import`ed files.

 For development purposes, you can enable the `firebug` option if you want to
 use the [FireStylus extension for Firebug](//github.com/LearnBoost/stylus/blob/master/docs/firebug.md) 
 and/or the `linenos` option to emit debug info in the generated CSS.

        function compile(str, path) {
          return stylus(str)
            .import(__dirname + '/css/mixins/blueprint')
            .import(__dirname + '/css/mixins/css3')
            .set('filename', path)
            .set('warn', true)
            .set('firebug', true)
            .set('linenos', true);
        }
