# Important note

If you are using mnmlst.css (minified or not) and do not use functional classes for font sizes, don’t forget the default for `<h1>` is usually `2em`, which means its inherited `line-height` will be half the one for `body`, i.e. a ratio of `0.75`.

I will not force you using another `font-size` or declaring a `line-height` for `h1` but at least it is my responsibility to warn you. If you don’t, a rapid test has shown you’ll be almost OK in iBooks and Adobe RMSDK—if Minion Pro is the default—but `0.75` could go very wrong, really fast, in other Reading Systems.