# PlaceHolder Parser

![Build Status](https://github.com/piroor/webextensions-lib-placeholder-parser/actions/workflows/main.yml/badge.svg?branch=main)

This is a tiny library to parse a text containing placholders like `%SOMETHING%`.

Usage:

```javascript
import * as PlaceHolderParser from './placeholder-parser.js';
import * as Replacer from './replacer.js';

function fillPlaceHoldersWithTab(input, tab) {
  return PlaceHolderParser.process(input, (name, rawArgs, ...args) => {


    switch (name.trim().toLowerCase()) {
      case 'replace':
        return Replacer.replace(args);

      case 'url':
        return tab.url;

      case 'title':
      case 'text':
        return tab.title;
    }

    throw new Error(`Unknown placeholder: ${name}`);
  });
}

console.log(
  fillPlaceHoldersWithTab(`
    Details of the tab.
    The URL is: %URL%
    The URL without query: %REPLACE("%URL", "\?.*$", "")%
    The URL without query except google: %REPLACE("%URL", "^(?!\w+://[^/]*\.google\.[^/]*/.*)\?.*$", "$1")%
    The title is: %TITLE%
  `, await browser.tabs.get(29))
);
```
