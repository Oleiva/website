---
layout: docs/content
---

When working in a team to build a website with Cuttlebelle you may find yourself working with content authors. I like to not assume any technical abilities for
those. I see developers are mainly to empower content authors. To do that Cuttlebelle splits content and code as cleanly as possible.
When a developer creates a new layout, that layout has to make its way to the content authors in a non-technical sort of way.

This is where documentation comes in. Cuttlebelle actually automates this process for you as a build artifact. All developers have to do is add
[`PropTypes`](https://reactjs.org/docs/typechecking-with-proptypes.html) and provide example YAML in a comment for each.

Let’s have a look at an [example documentation](TODO) that describes each layout for this very site.

To enable automatic documentation for a simple layout like this:

```jsx
import PropTypes from 'prop-types';
import React, { Fragment } from 'react';

/**
 * The partial component
 */
const Partial = ({ title, _body }) => (
  <article>
    <h2>{ title }</h2>

    { _body }
  </article>
);

export default Partial;
```

All we have to do it add `PropTypes`:

```diff
import PropTypes from 'prop-types';
import React, { Fragment } from 'react';

/**
 * The partial component
 */
const Partial = ({ title, _body }) => (
  <article>
    <h2>{ title }</h2>

    { _body }
  </article>
);

+ Partial.propTypes = {
+   /**
+    * title: Section title
+    */
+   title: PropTypes.string.isRequired,
+ 
+   /**
+    * _body: (test)(12)
+    */
+   _body: PropTypes.node.isRequired,
+ };
+ 
+ Partial.defaultProps = {
+   title: 'This section',
+ };

export default Partial;
```

The `PropTypes` will give you some type safety but also tell Cuttlebelle what this layouts expects. You can give your content author subtle warnings when they
forget a prop but adding `.isRequired` and even limit it to only a handful of keywords that can be used by using `.oneOf()`.

The other part you need is a comment above each PropType.

```js
Partial.propTypes = {
  /**
   * headline: Section title
   */
  headline: PropTypes.string,
};
```

This will give Cuttlebelle enough information to fill your component with that example data to present your layout in the documentation. It will also show how
to use this component when you use more complex data like nested array-objects.

```js
Partial.propTypes = {
  /**
   * links:
   *  - name: home
   *    link: /
   *  - name: docs
   *    link: /docs
   *  - name: github  # the string 'github' will have the GitHub logo attached to it
   *    link: https://github.com/cuttlebelle/cuttlebelle
   */
  links: PropTypes.array,
};
```

For a reference on some common PropType declarations refer to our [cheat-sheet](/cheatsheet/).

To generate the docs all you have to run is:

```shell
cuttlebelle docs
```

This will generate the documentation into the `docs/` folder by default.
