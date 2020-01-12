# Inheritance, the cascade and specificity 

See:

- [CSS - The Complete Guide 2020 (incl. Flexbox, Grid & Sass)](https://www.udemy.com/course/css-the-complete-guide-incl-flexbox-grid-sass/)
- [Cascade and inheritance](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)

All of these determine how CSS rules with their selectors actually affect the HTML elements

## Inheritance

HTML can be seen as a tree structure of elements (example: `div` containing `p` containing `a`)

Inheritance: some CSS property values set on parent elements are inherited by their child elements

What kind of properties are inherited by default?

- Color: yes
- Margins, padding and border: no

#### Controlling inheritance

For each property, CSS provides some special values that allow you to control inheritance:

- `inherit`: Set the value to the one that the parent has for the property (can be seen as "turning on inheritance for the property").
- `initial`: Set the value to the one that was defined in the browser's default style sheet. If no value is set in the browser's default style sheet and the property would be inherited by default, the effect is the same as using `inherit`.
- `unset`: Acts like `inherit` if the property is inherited by default and `initial` otherwise.

You can also use the CSS property `all` to set these values for (almost) every property. This can be a convenient way to "start from a clean sheet" for a certain element.

```css
.unset-all {
    all: unset;
}
```

## The cascade

The cascade: how conflicts are resolved when multiple CSS rules match the same element and try to set different values for the same property

Important to note: conflicts are resolved at the level of properties, not at the level of CSS rules!

Factors to consider, in decreasing order of priority:

1. Importance
2. Specificity
3. Source order

### Importance

You can add `!important` to a property value to make it the "most important" one, meaning that this value beats all values without  `!important` that could apply to the element

```css
.test {
    background-color: gray;
    border: none !important;
}
```

Using `!important` is generally considered as a code smell and should be used as a last resort to override a style that you really can't override any other way.

Some libraries, for example Bootstrap, also use this for some of their utility classes. Example from Bootstrap:

```css
.w-100 {
    width: 100% !important;
}
```

### Specificity

Specificity: a measure for how specific the selectors of the CSS rules are, where priority is given to more specific rules

See also [Selectors](./Selectors.md)

A rule's specificity is determined by four values. Highest to lowest priority:

- **Inline style:** Inline style (defined directly on an element) is more specific than any rule that matches the element based on a selector
- **ID selectors**: Number of ID selectors in the overall selector of the rule
- **Class selectors, attribute selectors and pseudo-classes:** Number of class selectors, attribute selectors and pseudo-classes in the overall selector of the rule
- **Element selectors and pseudo-elements:** Number of element selectors and pseudo-elements in the overall selector of the rule

Important: the universal selector (`*`), combinators and the negation pseudo-class (`:not`) have no effect on specificity!

Some examples:

- `h1.the-class`  is more specific than `h1`

- `.class-a > .class-b > class-c` is more specific than  `.class-b > class-c`

- `#the-id` is more specific than `.class-a > .class-b > class-c`

Note: **inherited** property values can be seen as rules that are less specific than all of the above --> very easy to override. Can be useful to set some defaults at the level of the body, for example font family. It's possible to "increase the specificity" of inheritance by setting a property's value to `inherit` in a rule with a more specific selector.

### Source order

In case of a tie regarding importance and specificity, the winning rule will be the one that was defined/loaded last. This can matter if the exact same selector is use for multiple rules within the same style sheet, but also when combining different style sheets.

One important thing to keep in mind here is that browsers have a **default stylesheet** which can differ between browsers. These mostly use element selectors and are thus easily overridden by any CSS that is loaded by the actual web page. It is also common to override these default stylesheets using a **CSS reset** stylesheet, which overrides default browser styles with some styles which will then be the same regardless of which browser is used to open the page. Any custom CSS rules can then be loaded after the CSS reset stylesheet and override styles as necessary, starting from the "clean slate" created by the CSS reset stylesheet.