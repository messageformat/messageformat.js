# messageformat.js

The experience and subtlety of your program's text can be important. MessageFormat (PluralFormat + SelectFormat) is a mechanism for handling both *pluralization* and *gender* in your applications. It can also lead to much better translations, as it was built by [ICU](http://icu-project.org/apiref/icu4c/classMessageFormat.html) to help solve those two problems for all known [CLDR](http://cldr.unicode.org/) languages - likely all the ones you care about.

There is a good slide-deck on [Plural and Gender in Translated Messages](https://docs.google.com/present/view?id=ddsrrpj5_68gkkvv6hs) by Markus Scherer and Mark Davis. But, again, remember that many of these problems apply even if you're only outputting english.

[See just how many different pluralization rules there are.](http://unicode.org/repos/cldr-tmp/trunk/diff/supplemental/language_plural_rules.html)

MessageFormat in Java-land technically incorporates all other type formatting (and the older ChoiceFormat) directly into its messages, however, in the name of filesize, message.format.js only strives to implement *SelectFormat* and *PluralFormat*. There are plans to pull in locale-aware *NumberFormat* parsing as a "plugin" to this library, but as of right now, it's best to pass things in preformatted (as suggested in the ICU docs).

## What problems does it solve?

A progression of strings in programs:

> There are 1 results.

> There are 1 result(s).

> Number of results: 5.

These are generally unacceptable in this day and age. Not to mention the problem expands when you consider languages with 6 different pluralization rules. You may be using something like Gettext to solve this across multiple languages, but even Gettext falls flat.


## What does it look like?

ICU bills the format as easy to read and write. It may be _more_ easy to read and write, but I'd still suggest a tool for non-programmers. It looks a lot like Java's `ChoiceFormat` - but is different in a few significant ways, most notably its addition of the `plural` keyword, and more friendly `select` syntax.


```
{GENDER, select,
    male {He}
  female {She}
   other {They}
} found {NUM_RESULTS, plural,
            one {1 result}
          other {# results}
        } in {NUM_CATEGORIES, plural,
                  one {1 category}
                other {# categories}
             }.
```

Here's a few data sets against this message:

```javascript
{
  "GENDER"         : "male",
  "NUM_RESULTS"    : 1,
  "NUM_CATEGORIES" : 2
}
> "He found 1 result in 2 categories."

{
  "GENDER"         : "female",
  "NUM_RESULTS"    : 1,
  "NUM_CATEGORIES" : 2
}
> "She found 1 result in 2 categories."

{
  "GENDER"         : "male",
  "NUM_RESULTS"    : 2,
  "NUM_CATEGORIES" : 1
}
> "He found 2 results in 1 category."

{
  "NUM_RESULTS"    : 2,
  "NUM_CATEGORIES" : 2
}
> "They found 2 results in 2 categories."
```

There is very little that needs to be repeated (until gender modifies more than one word), and there are equivalent/appropriate plural keys for every single language in the CLDR database. The syntax highlighting is less than ideal, but parsing a string like this gives you flexibility for your messages even if you're _only_ dealing with english.

## Why not Gettext?

Gettext can generally go only one level deep without hitting some serious roadblocks. For example, two plural elements in a sentence, or the combination of gender and plurals.

### This would be prohibitively difficult with Gettext

> He found 5 results in 2 categories.

> She found 1 result in 1 category.

> He found 2 results in 1 category.

It can likely be done with contexts/domains for gender and some extra plural forms work to pick contexts for the plurals, but it's less than ideal. Not to mention every translation must be completed in its entirety for every combination. That stinks too.

I tend to only use Gettext on projects that are already using it in other languages, so we can share translations, otherwise, I like to live on the wild-side and use PluralFormat and SelectFormat.

Most Gettext tools will look up the Plural Forms for a given locale for you. This is also the opinion of PluralFormat. The library should just contain the known plural forms of every locale, and not force translators to reinput this information each time.

## Version

`0.1.0`

## TODO

* First and foremost - we need to get a standalone `NumberFormat` implementation - I'll probably port Google Closure's good implementation to not need Closure ASAP
* Get all locale Plural Form Function data available in a folder - I have the base line for a few for now, and will try to generate the rest soon.
* Create a tool to help translators generate these.
* Create a transport standard/mechanism - a way to load in different languages, and to exchange this data (like .po files for gettext)
* Template integration - I specifically want to make a build time handlebars.js plugin to build this logic into the template builds.

## License

You may use this software under the WTFPL.

You may contribute to this software under the Dojo CLA - <http://dojofoundation.org/about/cla>


## Author

* Alex Sexton - [@SlexAxton](http://twitter.com/SlexAxton) - <http://alexsexton.com/>


## Credits

Thanks to:

* Google has an implementation that is similar in Google Closure, I tried to vet my code against many of their tests.
* Norbert Lindenberg for showing me how good it can be.