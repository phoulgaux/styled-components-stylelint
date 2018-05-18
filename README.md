# Styled Components `stylelint` bug
Minimal cases for an erroneus linting and fixing nested multi-line templates in stylelint-processor-styled-components.

## Commands

To lint:
```
npm run lint
yarn lint
```

To run fixer:
```
npm run lint -- --fix
yarn lint --fix
```

## Description

### As is
When running the linter on a file with nested multi-line templates (tagged or not), stylelint detects _Expected empty line before comment_ (`comment-empty-line-before`) and _Unexpected empty block_ (`block-no-empty`) errors. Upon fixing, the whole file is replaced with extracted CSS with `/* dummy comment */` in place of the nested templates. The errors are no shown when the nested template is single line (for example: `${css`display: block;`}.

Before fix:

```js
import styled, { css } from 'styled-components';

export const components = [
  styled.div`
    ${`
    `}
  `,
  styled.div`
    ${css`
    `}
  `,
  styled.div`
    ${css`
      display: block;
    `}
  `,
];
```

After `--fix`:
```css
.selector1 {
  -styled-mixin0: dummyValue;

  /* dummyComment */
}

.selector2 {
  -styled-mixin0: dummyValue;

  /* dummyComment */
}

.selector3 {
}

.selector4 {
  -styled-mixin0: dummyValue;

  /* dummyComment */

  /* dummyComment */
}

.selector5 {
  display: block;
}
```

### To be

No errors should be reported. However, in case of an error, the fixer should not convert JS files to CSS files.

### Environment

* Node version: `v10.0.0`
* `stylelint: "^9.2.1"`
* `stylelint-config-standard": "^18.2.0`,
* `stylelint-config-styled-components": "^0.1.1"`
* `stylelint-processor-styled-components": "^1.3.1"`
