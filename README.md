# Credit

This package is a derivative/fork of [xhtml2pug](https://www.npmjs.com/package/xhtml2pug), created by [dimensi](https://github.com/dimensi). I simply modified it to generate compliant Slim code (which is very similar to Pug) from HTML that heavily utilizes [TailwindCSS](https://tailwindcss.com/) classes. Traditional converters have not been updated in some time to be able to handle a lot of [TailwindCSS](https://tailwindcss.com/) class names as they contain various special charcters such as `[:, /]` to name a few. This tool fixes that by moving away from the dot notated CSS `h1#myTitle.text-neutral-800.dark:text-neutral-100 foo` classes (which tend to throw off the [Slim](https://github.com/slim-template/slim) language processor) to inline CSS classes `h1#myTitle[class="text-neutral-800 dark:text-neutral-100"] foo`. I'm a heavy user of both [Ruby Slim](https://github.com/slim-template/slim) rendering engine and [TailwindCSS](https://tailwindcss.com/) and tool allows me to move at the speed of light when it comes to converting HTML code to Slim.

# xhtml2slim

Converts **HTML** and **Vue** like syntax to **Slim** or **Pug** templating language.  
Requires Node.js version `14` or higher.

Using the following command (_take note of the **`-w square`** attribute wrapper flag_):

```
xhtml2slim -w square < example.html
```

Turns this:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Hello World!</title>
  </head>
  <body class="bg-neutral-100 dark:bg-neutral-800 group/home:body">
    <div id="content" class="text-neutral-800 dark:text-neutral-100">
      <h1 id="myTitle" class="title">Hello World!</h1>
    </div>
  </body>
</html>
```

Into this:

```slim
doctype html
html[lang='en']
  head
    title Hello World!
   body[class="bg-neutral-100 dark:bg-neutral-800 group/home:body"]
    #content[class="text-neutral-800 dark:text-neutral-100"]
      h1#myTitle.title Hello World!
```

## Install

Get it on [npm](https://www.npmjs.com/package/xhtml2slim):

```bash
npm install -g xhtml2slim
```

## Usage

### CLI

Accept input from a file or stdin and write to stdout:

```bash
# choose a file
xhtml2slim -w square < example.html

# use pipe
echo '<h1 id="myTitle" class="text-neutral-800 dark:text-neutral-100">foo</h1>' | xhtml2slim -w square -f
# yields
h1#myTitle[class="text-neutral-800 dark:text-neutral-100"] foo
```

Write output to a file:

```bash
xhtml2slim -w square < example.html > example.slim
```

See `xhtml2slim --help` for more information. The `-w` aka `attrWrapper` is the key differentiator between [xhtml2pug](https://www.npmjs.com/package/xhtml2pug) and this package. It accepts `curly`, `round`, and `square` attribute wrappers to be able to wrap your tags with the appropriate style of brackets. This makes it useful outside of the scope of my personal use case.

### Programmatically

```js
import { convert } from "xhtml2slim";

const html = '<header><h1 class="title">Hello World!</h1></header>';
const slim = convert(html, { symbol: "\t" });
```

### Cli Options

```bash
  -b, --bodyLess      Don't wrap into html > body      [boolean] [default: true]
  -t, --tabs          Use tabs as indent              [boolean] [default: false]
  -s, --spaces        Number of spaces for indent          [number] [default: 2]
  -a, --attrComma     Commas in attributes            [boolean] [default: false]
  -e, --encode        Encode html characters           [boolean] [default: true]
  -q, --doubleQuotes  Use double quotes for attributes [boolean] [default: true]
  -i, --inlineCSS     Place all classes in class attribute
                                                       [boolean] [default: true]
  -c, --classesAtEnd  Place all classes after attributes
                                                      [boolean] [default: false]
  -w, --attrWrapper   Wrap attributes in round brackets
               [string] [choices: "curly", "round", "square"] [default: "round"]
  -p, --parser        html for any standard html, vue for any vue-like html
                             [string] [choices: "html", "vue"] [default: "html"]
```

### Api Options

```ts
export interface PublicOptions {
  /** Don't wrap into html > body */
  bodyLess: boolean;
  /** Commas in attributes */
  attrComma: boolean;
  /**  Encode html characters */
  encode: boolean;
  /** Use double quotes for attributes */
  doubleQuotes: boolean;
  /** Place all classes in class attribute */
  inlineCSS: boolean;
  /** Symbol for indents, can be anything */
  symbol: string;
  /** Wrapper for attributes: curly{}, default:round(), square[] */
  attrWrapper: "curly" | "round" | "square";
  /** Html for any standard html, vue for any vue-like html */
  parser: "html" | "vue";
  /** Place all classes after attributes */
  classesAtEnd: boolean;
}
```
