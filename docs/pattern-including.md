---
layout: docs
title: Including Patterns | Pattern Lab
heading: Including Patterns
---

To include one pattern within another, for example to create a molecule from several atoms, you can either use:

* a shorthand include syntax or
* the default include syntax for the template language you're using (e.g. Mustache, Twig, Handlebars).

## The Shorthand Include Syntax

The shorthand include syntax is less verbose than the default include syntax for many template languages. The shorthand syntax uses the following format:

    [patternType]-[patternName]

For example, to include the following pattern in a molecule:

    00-atoms/images/landscape-16x9.mustache

The shorthand include syntax would be:

    atoms-landscape-16x9

The pattern type matches the top-level folder and is `atoms`. The pattern name matches the template file and is `landscape-16x9`. Any digits used for ordering are _dropped_ from both the pattern type and pattern name. Pattern subtypes are _never_ a part of the shorthand include syntax. This way patterns can be re-organized within a pattern type and/or by using digits without needing to change your pattern includes.

The following are examples of using the shorthand include syntax with our supported PatternEngines:

```
{% raw %}{{> atoms-landscape-16x9 }}          // Mustache{% endraw %}
{% raw %}{% include "atoms-landscape-16x9" %} // Twig{% endraw %}
```

The shorthand syntax also allows for fuzzy matching on pattern names. This means that if you feel your pattern name is going to be unique within a given pattern type you can supply just the unique part of the pattern name and the partial will be included correctly. For example, using the shorthand syntax the pattern `atoms-landscape-16x9.mustache` could be written as:

    atoms-16x9

*Warning:* Because subtypes are not included in the shorthand include syntax a given pattern name needs to be unique within its _pattern type_ and not just its pattern subtype. If you run into this problem you can do one of two things:

* use the default include syntax for your template language or
* give your pattern a unique name and use [the pattern's documentation](/docs/pattern-documentation.html) to provide the pattern name


## The Default Include Syntax

If you need more specificity when including patterns the PHP and Node versions of Pattern Lab also support the include syntax for the template language that you're using. For example, the syntax for Mustache is the path to the pattern minus the `.mustache` extension. Let's say we wanted to include the following pattern in a molecule:

    00-atoms/images/landscape-16x9.mustache

The default Mustache include syntax would be:

    {% raw %}{{> 00-atoms/images/landscape-16x9 }}{% endraw %}

**Important:** Unlike the shorthand include syntax the template language specific include syntax **must** include any digits used for ordering and subtype directories. Pattern paths need to be updated when either is changed for a given pattern.

## Examples and Gotchas

Here are some examples of how to include patterns as well as some gotchas.

```
{% raw %}// partials to match
00-atoms/global/05-test.mustache
00-atoms/global/06-test.mustache
00-atoms/global/test.mustache
00-atoms/global/test-with-picture.mustache

// using the shorthand partials syntax
{{> atoms-test }}               // will match 00-atoms/global/05-test.mustache
                                // using the shorthand syntax you'll never be able to match 06-test nor test in this scenario
{{> atoms-test-with-picture }}  // will match 00-atoms/global/test-with-picture.mustache
{{> atoms-test-wit }}           // will match 00-atoms/global/test-with-picture.mustache

// using the default mustache partials syntax
{{> atoms/global/05-test }}     // won't match anything because atoms is missing its digits
{{> 00-atoms/global/06-test }}  // will match 00-atoms/global/06-test.mustache{% endraw %}
```
