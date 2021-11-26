# EJS

All files are stored as `.ejs` files

## Features
- Fast compilation and rendering
- Simple template tags: <% %>
- Custom delimiters (e.g., use [? ?] instead of <% %>)
- Sub-template includes
- Ships with CLI
- Both server JS and browser support
- Static caching of intermediate JavaScript
- Static caching of templates
- Complies with the Express view system

## Rendering

from HTML:
```html
<!-- To introduce existing data to the system-->
<% if (user) { %>
  <h2><%= user.name %></h2>
<% } %>
```

From js:

```js
let template = ejs.compile(str, options);
template(data);
// => Rendered HTML string

ejs.render(str, data, options);
// => Rendered HTML string

ejs.renderFile(filename, data, options, function(err, str){
    // str => Rendered HTML string
});
```

and then just inject as inner html

### Options for rendering

- `cache`: Compiled functions are cached, requires filename
- `filename`: Used by cache to key caches, and for includes
- `root`: Set project root for includes with an absolute path (e.g, /file.ejs). Can be array to try to resolve include from multiple directories.
- `views`: An array of paths to use when resolving includes with relative paths.
- `context`: Function execution context
- `compileDebug`: When false no debug instrumentation is compiled
- `client`: Returns standalone compiled function
- `delimiter`: Character to use for inner delimiter, by default '%'
- `openDelimiter`: Character to use for opening delimiter, by default '<'
- `closeDelimiter`: Character to use for closing delimiter, by default '>'
- `debug`: Outputs generated function body
- `strict`: When set to `true`, generated function is in strict mode
- `_with`: Whether or not to use with() {} constructs. If false then the locals will be stored in the locals object. (Implies `--strict`)
- `localsName`: Name to use for the object storing local variables when not using with Defaults to locals
- `rmWhitespace`: Remove all safe-to-remove whitespace, including leading and trailing whitespace. It also enables a safer version of -%> line slurping for all scriptlet tags (it does not strip new lines of tags in the middle of a line).
- `escape`: The escaping function used with <%= construct. It is used in rendering and is .toString()ed in the generation of client functions. (By default escapes XML).
- `outputFunctionName`: Set to a string (e.g., 'echo' or 'print') for a function to print output inside scriptlet tags.
- `async`: When true, EJS will use an async function for rendering. (Depends on async/await support in the JS runtime.

## Includes

You need to `include` in order to draw from a template 

```js
<ul>
  <% users.forEach(function(user){ %>
    <%- include('user/show', {user: user}); %>
  <% }); %>
</ul>
```

the path as the first argument in the include, is relative to the current template that you are in so in this case the tree would look like

```bash
template.ejs //current file

user/
    show.ejs
```

## Layouts

not officially supported however you can hack around by using nested templates like this:

<%- include('header'); -%>
<h1>
  Title
</h1>
<p>
  My page
</p>
<%- include('footer'); -%>


