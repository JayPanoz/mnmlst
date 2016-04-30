# less’ design system 

The LESS part of mnmlst has got two distinct parts: core and plugins. Since the whole system may be a little bit abstract at first sight, I’ll try to help you make sense of it.

## architecture [new in v.2]

mnmlst’s architecture has been re-designed to bring some order to chaos so that human beings may cope with it somehow easily.

By architecture we mean folders: 

- `mnmlst` and `custom` are root (global output);
- `rhythm` and `utilities` are “utils” (no output);
- `base`, `hyphens` and `normalize` are “foundation” (output);
- `align`, `fluid`, `pagebreak` and `scale` are “functionalCSS” (output);
- everything optional is “plugins” (optional output or specific CSS).

To sum up, mnmlst is now organized.

## building the foundation

First, two references are imported in `mnmlst.less`: rhythm and utilities. The former is the reactor core, the latter is your toolbox. In other words, their contents won’t be output in the compiled CSS.

Then, step by step:

1. we normalize HTML5 block elements;
2. we lay the ebook foundation (page margins for Nook, normalize for body, styles for headings);
3. we disable hyphens for headings (and tables);
4. at this point, custom styles are injected (because the cascade);
5. we import functional classes for pagebreaks;
6. we import functional classes for the diatonic type scale;
7. we import functional classes for alignment; 
8. we “fluidify” images, videos and svg so that they don’t overflow;
9. we connect optional plugins. **[new in v.2]**

“Why no functional classes for margins by default?” you may ask.

Because they are computed in `ems` and therefore depend on the element’s `font-size`. So they would be pretty useless. Capisce?

## automating vertical rhythm

We’ve got 3 files working together here: 

1. `base` creates the **baseline grid** (`line-height` in `%` for `body` and `inherit` for all other elements);
2. `scale` creates and output the functional classes for the **diatonic scale** (`font-size`); 
3. `rhythm` **checks line heights** i.e. it makes sure the functional classes (and custom styles) can `inherit` from `body` and will push a specific `line-height` to override the `inherit` value when needed (e.g. computed `font-size` is `28px` while computed `line-height` is `24px`).

Easy, huh? Except `rhythm` is a doing a lot more than checking line heights, **it’s also managing margins and paddings for custom styles.** And this is where the system could become obscure.

Here’s the trick: when checking line heights, the parametric mixin for `font-size` pushes a hidden local `var` (called `computeScale`) so that the parametric mixin for `margin` or `padding` can use it and compute `ems` based on `font-size`.

So, basically, it’s a one-two punch. In `scale.less`, when you have

```
.fs-3 {
    .font-size(@columbian);
}
``` 

What you really get is

```
.fs-3 {
    @computeScale: @columbian; 
    font-size: @columbian + 0em;
}
```

And if, in your custom styles, you use `.fs-3` like this

```
h1 {
	.fs-3;
    .margin(0, 3);
}
```

What is happening behind the scenes…

```
h1 {
	@computeScale: @columbian; 
    font-size: @columbian + 0em;
    margin: ((0 * ((@lineHeight / 100) / @computeScale)) + 0em) 0 ((3 * ((@lineHeight / 100) / @computeScale)) + 0em) 0;
}
```

And the final output in CSS is

```
h1 {
  font-size: 1.3125em;
  margin: 0 0 3.42857143em 0;
}
```

And that’s how mnmlst helps you avoid the pain of calculating `ems` by hand: an over-engineered reactor core using a shitload of global and local variables, and parametric mixins relying on previous parametric mixins, just because `px` don’t reflow. What could go wrong? 

## the tools you get

If you want to add styles in LESS and output them in a custom CSS:

- you’ve got variables for typographic stuff;
- you’ve got namespaces to add mixins that are not output by default (see `utilities.less`);
- you’ve got mixins for a lot of stuff (indent, hyphens, font size, small caps, etc.);
- You’ve got parametric mixins for font size (in `rhythm.less`, see why above), margins, paddings, columns, overrides, text decoration, etc.;
- you can set a font size using `.fs-[suffix]` mixins.

### new tools in v.2

There’s a bunch of new powerful mixins: 

- `.width-center` is basically declaring a `width` and using `margin-left: auto` (because “absurmsdk”);
- `.underline` for `text-decoration` (because swag);
- `.small-caps` (your text should be typed in uppercase though because “absurmsdk”);
- `.bordered` if you need boxes with borders (includes `padding`);
- `.border-radius` (because yolo);
- `.linear-gradient` (because “modern design” + it won’t be overriden in iBooks).

There’s a couple of new packages (namespaces) for horizontal rules:

- `#subtle-hr` (because let’s be honest, default `inset` sucks big time);
- `#awesome-hr` (because we had to find a reason for `.linear-gradient` to exist).

## the shit you probably shouldn’t modify

- `base.less`.
- `normalize.less`.
- `rhythm.less` if you can’t dig it.

## the crap you‘ll have to edit

- The top and bottom margins of `hr` (needs `@computeScale`) if you’re declaring margins everywhere else to achieve vertical rhythm. It’s conceptually broken, as is life if you are realist, so deal with it.
- `fluid.less` if you’ve got portrait images in your eBook.
- `table`, `pre`, `code` and all the missing stuff that would have bloated my 853 bytes.

## plugins [new in v.2]

Though some mixins have made it into `utilities.less`, mnmlst has been mainly extended using plugins.

- `subtle-palette` generates a subtle color palette from an argument;
- `complementary-palette` generates a complementary color palette from an argument;
- `grid` generates columns + wrapper + clearfix for images grids;
- `freeform` generates a set of functional classes to achieve swiss style pages (title, part, etc.) – [see this image for previews](http://jiminy.chapalpanoz.com/wp-content/uploads/sites/2/2015/11/freeform-sys.jpg);
- `helpers` generates helpers/tools in the form of classes you can use while designing your eBook: background modes helps you make sure everything is OK (illustrations with transparent background in night mode, color contrast, etc.), baseline grid helps you achieve vertical rhythm if you do give a shit about that, “paginator” helps you preview your design (it is consistent with iBooks on iPad with smallest font-size setting).

Basically, plugins are small snippets taking advantage of functions and arguments to achieve one very specific goal. They are optional by default in `mnmlst.less` and each can be compiled on its own.

### how to build plugins

1. All other parts of mnmlst are the core, plugins extend it when needed.
2. Keep it simple (KISS) and don’t try to achieve a shitload of different things in one single plugin (a plugin = 1 goal). If the unminified output is less than or equal to 1kb, you’re doing it right.
3. A plugin **must** either be output from `mnmlst.less` **or** be compiled directly (specific CSS).
4. Try to keep plugins as independent as possible, don’t use “core variables” when it is not necessary and leverage arguments.
5. If the plugin is making use of one mnmlst’s “core variable” or mixin, import the necessary LESS file, as a reference, in the plugin itself so that it can be compiled directly.
6. Shit you build in plugins should adopt mnmlst’s functional CSS approach.

### List of core variables

- `@lineHeight` (percentage).
- `@scale` (`@lineHeight` as ratio).
- `@computeScale` (phantom variable added when using `.font-size()` mixin and used in `.margin()` and `.padding` mixins). Can be hardcoded locally if needed (e.g. `hr` in `custom.less` example).

**Note:** variables used in `scale.less` (`@pica`, `@columbian`, etc.) are not considered “core variables” since `@computeScale` takes over.
