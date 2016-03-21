# less’ design system 

The LESS part of mnmlst has got two distinct parts: foundation and custom. Since the whole system may be a little bit abstract at first sight, I’ll try to help you make sense of it.

## building the foundation

First, two references are imported in `mnmlst.less`: utilities and rhythm. The former is your tools, the latter is the reactor core. In other words, their contents won’t be output in the compiled CSS.

Then, step by step:

1. we reset HTML5 block elements;
2. we lay the ebook foundation (page margins for Nook, normalize for body, styles for headings);
3. we disable hyphens for headings (and tables);
4. at this point, custom styles are injected (because the cascade);
5. we import functional classes for pagebreaks;
6. we import functional classes for the diatonic type scale;
7. we import functional classes for alignment; 
8. we “fluidify” images, videos and svg so that they don’t overflow.

“Why no functional classes for margins by default?” you may ask.

Because they are computed in `ems` and therefore depend on the element’s `font-size`. So they would be pretty useless. Capisce?

## automating vertical rhythm

We’ve got 3 files working together here: 

1. `base` creates the **baseline grid** (`line-height` in `%` for `body` and `inherit` for all other elements);
2. `scale` creates and output the functional classes for the **diatonic scale** (`font-size`); 
3. `rhythm` **checks line heights** i.e. it makes sure the functional classes (and custom styles) can `inherit` from `body` and will push a specific `line-height` to override the `inherit` value when needed (e.g. computed `font-size` is `28px` while computed `line-height` is `24px`).

Easy, huh? Except `rhythm` is a doing a lot more than checking line heights, **it’s also managing margins for custom styles.** And this is where the system could become obscure.

Here’s the trick: when checking line heights, the parametric mixin for `font-size` pushes a hidden local `var` (called `computeScale`) so that the parametric mixin for `margin` can use it and compute `ems` based on `font-size`.

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
- you’ve got mixins for a lot of stuff (indent, hyphens, font size, etc.);
- You’ve got parametric mixins for font size (in `rhythm.less`, see why above), margins, columns, overrides, etc.;
- you can set a font size using `.fs-[suffix]` mixins.

## the shit you probably shouldn’t modify

- `base.less`.
- `reset.less`.
- `rhythm.less` if you can’t dig it.

## the crap you‘ll have to edit

- The columns generator if you’re using it since there is no `float` and wrap.
- The top and bottom margins of `hr` (needs `.fs-1` declared) if you’re declaring margins everywhere else to achieve vertical rhythm.
- `fluid.less` if you’ve got portrait images in your eBook.
- `table`, `pre`, `code` and all the missing stuff that would have bloated my 853 bytes.