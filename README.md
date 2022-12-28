# xhtml2slim

Converts **HTML** and **Vue** like syntax to **Slim** or **Pug** templating language.  
Requires Node.js version `14` or higher.

Turns this

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Hello World!</title>
  </head>
  <body>
    <div id="content">
      <h1 class="title">Hello World!</h1>
    </div>
  </body>
</html>
```

Into this

```slim
doctype html
html(lang='en')
  head
    title Hello World!
   body
    #content
      h1.title Hello World!
```

## Usage

### CLI

Accept input from a file or stdin and write to stdout:

```bash
# choose a file
xhtml2slim < example.html

# use pipe
echo '<h1>foo</h1>' | xhtml2slim -f
```

Write output to a file:

```bash
xhtml2slim < example.html > example.slim
```

See `xhtml2slim --help` for more information.

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
               [string] [choices: "curly", "round", "sqaure"] [default: "round"]
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
