# Theming in Atom

This is a proposal to simplify theming in Atom.

Summary:

- Combine UI and Syntax themes into just one kind of Atom theme.
- The styling of Atom's bundled packages will be done by the packages themselves (not rely on UI themes anymore).
- Themes are only required to contain predefined CSS variables. Like colors, sizes, spacing etc.
- Themes only define colors for syntax highlighting. Atom core uses them to add the styling. Language packages can add custom syntax highlighting if needed.
- Atom's UI library (styleguide) gets overhauled and extended beyond what bundled Atom needs.

What follows goes into more detail by first examining current problems and then proposing possible solutions.


---


## Problem 1 (variables):

Theme variables ( [ui-variables.less](https://github.com/atom/atom/blob/4f0209bb8e1d2343b09dba151650c4123ea87a34/static/variables/ui-variables.less) and [syntax-variables.less](https://github.com/atom/atom/blob/4f0209bb8e1d2343b09dba151650c4123ea87a34/static/variables/syntax-variables.less) ) [can't be changed](https://github.com/atom/atom/issues/5903) by users or packages.

It is possible to override styles, but that could get hard to maintain for [many places](https://github.com/atom/atom/issues/3636#issuecomment-75157082).

```less
html,
body,
.tree-view,
.status-bar,
.tab-bar .tab,
.settings-view .settings-panel label,
.settings-view .setting-description,
.form-control {
    font-size: 16px;
}
```

Another option would be to have "[user variables](https://github.com/atom/atom/issues/5903)" and recompile all Less files. There was a prototype, but it had to be abandoned because performance wasn't great. And sometimes there where glitches where Chromium would not update a GPU layer or it missed some styles. 

### Solution:

Themes use CSS **custom properties** instead. This allows to update their values at runtime without needing to recompile all styles.

```less
:root {
  --fg: black;
  --bg: white;
  --spacing: 8px;
  --border-radius: 4px;
  
  --syntax-variable: green;
  --syntax-function: yellow;
```

### Concern:

> Color functions like darken() or lighten() don't work with custom properties.

Color functions will be supported in CSS at some point. But that will probably still take a while. In the meantime, package authors could read the custom property with JS, change it with a library like [chroma.js](https://gka.github.io/chroma.js/) and re-apply the new value to their package. We'll have to see how often this is needed in the real world.



## Problem 2 (UI):

Some bundled packages rely on themes to be styled. In turn, theme authors rely on packages to not change the markup and class names. This symbiosis makes it hard to update bundled packages without breaking themes.

For example the tabs look like this without theming:

![tabs](https://user-images.githubusercontent.com/378023/34474035-fb72c2d4-efbc-11e7-9c0f-6a03123d159d.png)

So themes style it a certain way. This is great since it allowed many variations to exist. But the current markup and class names are seen like a "public API" that theme and package authors expect to not change.

Furthermore, even when trying not to break anything, making changes over time there are still minor details that look off or don't match. Theme authors still need to keep updating their theme, which could become a burden when doing it over the years. As a result, some themes get abandoned by their authors.


### Solution

All bundled packages need to do their own styling. Just like community packages do.

Now that all packages have a certain default style, it makes UI themes somewhat redundant and can be combined with Syntax themes into just one kind of Atom theme. It contains a single file (`my-theme/theme.css`) that defines all the theme variables needed. Having these stripped down themes makes it easier to create, maintain and port them.

It also simplifies the user experience. No need to learn the difference between a UI and Syntax theme. No need to switch both. No need to worry about compatibility.

![theme-picker](https://user-images.githubusercontent.com/378023/34474043-0d75c0da-efbd-11e7-83ae-ff595b339a1a.png)


### Concern

> Some theme authors like to create themes that have a custom look and are different from the default package styles.

Overriding any styles will still be possible. It's just that now it's not expected and theme authors have to be ok with actively maintaining changes.





## Problem 3 (syntax):

Language packages define scopes but have no influence how themes use these scopes. On the flip side, theme authors can't be familiar with all languages and special use cases.

There can also be multiple packages of the same language, each having different scopes. For example `language-markdown` is [forced](https://github.com/burodepeper/language-markdown/issues/52) to use the `.gfm` scope since a lot of themes only style those and not the more appropriate `.markdown` or `.md`.

Same with [Dana](https://github.com/atom/one-light-syntax/issues/12) or [Angular](https://github.com/simurai/duotone-light-syntax/issues/4). Language packages that want custom syntax highlighting have the following options:

- Create PRs for all themes. Which isn't really feasible. And it's also not guaranteed that themes accept these PRs. 
- Change their scopes so it works with themes. This feels ugly and is a bit backwards. It's also tricky because every theme can style scopes differently. So what works in one theme, might not work in another.


### Solution:

Themes are only responsible to define **colors** for the most common [TextMate scopes](https://manual.macromates.com/en/language_grammars#naming_conventions).

```less
--syntax-variable: green;
--syntax-function: yellow;
```

Atom core then uses them to add basic syntax highlighting that works with any language package that using these scopes.

```less
.syntax--variable { color: var(--syntax-variable); }
.syntax--function { color: var(--syntax-function); }
```

If a language package wants to go further, it can add custom syntax highlighting without the need of asking all themes to adopt it. Using the official theme variables lets language authors focus on the relationships without having to worry about colors. More like "ok, this is not a function but should look the same":

```less
.syntax--python.syntax--custom-scope {
    color: var(--syntax-function);
}
```





## Problem 4 (style leaking):

Atom's class names are not unique enough, risking complex selectors or style clashes.

```less
.tree-view .list-nested-item > .list-item > .name { ... }
```


### Solution:

All packages need to **namespace** their selectors. Ideally adopting a naming convention like BEM where most CSS selectors are just a single class. This improves render performance, makes it easy to see where styles come from and guards against style leaking.

```less
.tree-view-ListItem--directory { ... }
```





## Problem 5 (UI library): 

The current [UI library](https://github.com/atom/atom-ui) is limited to what Atom's bundled packages need, but not more. If package authors need something that isn't included, they need to create their own UI and controls which could be a burden when not too familiar with CSS.


### Solution:

The current [UI library](https://github.com/atom/atom-ui) will get overhauled and extended beyond what "Atom out of the box" needs. A richer UI library allows package authors worry less about the styling and they can spend more time on functionality.

Another idea is to create components with a simplified markup. For example: `<atom-button size="small" label="OK">`. Atom will then render a `<button>` element. This would allow Atom to change markup of components at any time without breaking packages. 


---


## Next steps

All these solutions above are still in "theoretical land" and haven't been tested yet. Next steps include:

1. [ ] Do a test drive with 1-2 themes, packages and some UI components.
2. [ ] See if any major issues arise.
3. [ ] Finalize official variables names, naming conventions.
4. [ ] Create lots of PRs for all packages/repos involved.
    - [ ] Core
    - [ ] Bundled themes
    - [ ] Bundled packages
    - [ ] Bundled language packages
    - [ ] Atom UI (+ styleguide)
    - [ ] Settings View
    - [ ] Flight Manual
    - [ ] atom.io/themes
    - [ ] Theme generator, theme template
5. [ ] Merge all PRs

Doing it in multiple steps could also be considered.

One last concern: Since this will break many existing themes and packages, should it wait for Atom 2.0 or can major breakages be mitigated?

- It might be possible to read both `ui-variables.less` and `syntax-variables.less` and then generate the custom properties from it.
- Or CSS custom properties allow to define fallback values which could be the current Less variables: `color: var(--syntax-function, @syntax-color-function);`.
- UI themes could still be installed as "normal" packages.
