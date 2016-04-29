# mnmlst

mnmlst (“Minimalist” or “Minimal Stylesheet”) is an **existentialist** CSS framework for eBooks. It’s sorta experimental and I haven’t had the time to QA thoroughly yet—hooray for eBooks debugging!

It is, **in many ways**, more of a “political statement” on the [current state of Reading Systems’ defaults](https://github.com/FriendsOfEpub/WillThatBeOverriden) than a practical framework. It doesn’t really help you, it immerses you in an absurd world in which some RS “!important all the things” and [abuse the universal selector](https://github.com/FriendsOfEpub/WillThatBeOverriden/blob/master/ReadingSystems/OverDrive/Book.css) to override your styles, **destroying the fundamental concept of cascade in doing so.**

Beware and tamper with mnmlst at your peril.

## licence 

**The MIT License (MIT)**

Copyright (c) 2016 Jiminy Panoz ([Chapal & Panoz](http://www.chapalpanoz.com))

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## the whys

mnmlst was intended as a 24 hour experiment: creating the most minimalistic eBooks’ CSS that can be. In other words, it is a dead simple stylesheet with which you can achieve “good enough” results. It was partly inspired by [Better Motherfucking Website](http://bettermotherfuckingwebsite.com).

Since I also considered it a means to get myself out of my comfort zone, I decided to change my CSS approach and see what could stick on the wall—if you want to see my usual approach, check [Soma for Ulysses](https://github.com/JayPanoz/Soma).

It finally clarified itself as “a piece of political art” conveying a strong statement when writing this “Read me”. That doesn’t imply this framework is useless, it just means you’ll have to deal with its approach, should you adopt it. 

## approach

1. **mnmlst has minimum impact:** it relies on the Reading Systems’ defaults you can’t trust, it only corrects their oversights (e.g. disabling hyphens for headings, resetting HTML5 block elements, applying `max-width` for images, etc.). 
2. **mnmlst is freaking light:** the minified css is just 853 bytes. You can achieve your custom typography in less than 2 kilobytes (beautified).
3. **mnmlst is functional:** it follows [FCSS principles](https://blog.colepeters.com/building-and-shipping-functional-css/) and provides single-purpose/reusable classes by default, it doesn’t declare styles for elements directly (excepted oversights).
4. **mnmlst is a toolbox:** the framework was designed using [LESS CSS](http://lesscss.org), it offers multiple tools and can be customized.

Out of the box, mnmlst doesn’t override anything, whether the defaults are pretty good or [absolutely terrible](https://github.com/FriendsOfEpub/WillThatBeOverriden/blob/master/ReadingSystems/Kindle/mobi7/mobi7.css).

Nope, **nothing.** 

Its approach could be abstracted to “If it’s broke beyond repair, don’t try to fix it.” It’s up to you to make use of the single-purpose classes or, should you be a nihilist, just load the stylesheet and claim the job done.

Existentialism at its best.

## typography

mnmlst’s typography is simplistic:

1. no `font-family` is declared, the default will be used (either the Reading System’s or the user’s);
2. the `line-height` is declared for `body` as a `%` value; it’s like setting a baseline grid in InDesign: all other elements will inherit the **computed** value, as if the correct ratio/em value was automatically recomputed for each element, depending on its `font-size`);
3. a `line-height` override is automatically pushed when the `font-size` of the class/element is bigger that the inherited `line-height`;
4. the modular scale is diatonic—cos’, well, it’s been proving its worth for the last 400 years and that speaks volumes.
5. pagebreaks and alignments can be managed using functional classes as well.

You can obviously build on top of it. Once again, it’s up to you.

## how to use

Thou who camest from above, face two worthless choices.

### 1. the easy way out

1. Load the css file in your eBook.
2. Add functional classes to your markup if you want. 
3. Write your own styles if you need.

**Please note** you are responsible for your actions.

### 2. the gates of delirium

Open the LESS folder, the file `custom.less` becomes your meaningless—it ships with an example.

To sum things up: 

- `mnmlst.less` is 100% imports and serves as existence (compile CSS);
- `rhythm.less` is essence;
- every other file is called after its specific purpose.

We’ve got a grand total of 11 files, which seems a lot but isn’t if you consider some are uber simple—could have been one file with 250 lines but, you know, modularity and usability and stuff. 

See LESS’ `readme` for further details.

## the waste mnmlst doesn’t support

- Good Old Kindle Mobi7 ([see why here](https://github.com/FriendsOfEpub/WillThatBeOverriden/blob/master/ReadingSystems/Kindle/mobi7/mobi7.css))
- A shitload of apps supporting ePub, especially on Android, because they don’t even support HTML & CSS very well—as a reminder, ePub is using these two languages, which makes the claim of supporting ePub a fucking joke in the first place.
- Web browsers, because they can’t open an ePub file natively.
- Your refusal of responsibility. 

## changelog

### version 2

CSS is left untouched but plugins for image grids, freeform system and design helpers have been added.

LESS part has been re-organized and extended (utilities + plugins).

- LESS design system is now using folders.
- Renamed `reset.less` to `normalize.less`.
- Added `.padding` mixin in `rhythm`.
- Added `.width-center` mixin computing `margin-left` from argument (`width`) to center blocks.
- Added `.underline` mixin (text decoration).
- Added `.small-caps` mixin.
- Added `.bordered` mixin.
- Added `.border-radius` mixin.
- Added `.linear-gradient` mixin.
- Added “subtle” and “awesome” `hr` (packages/namespaces).
- Created LESS plugins to generate subtle and complementary color palettes, image grids, freeform system and design helpers.

### version 1.0.1

A patch has been applied to make the LESS’ design system a little less absurd.

- Reorg of readme files.
- Unlocked “survival edition” of CSS.
- Updated LESS custom example.
- Patched LESS rhythm.
- Patched LESS utilities (pretty packages).