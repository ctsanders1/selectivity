select3
=======

Flexible and modular alternative to Select2.

Motivation
----------

I've used Select2 for a long time because I reckon it is the best and most feature-rich selection
library out there.

However, time and again I've ran into its limitations. It is difficult to customize the styling, and
introducing subtle tweaks to its behavior typically meant maintaining a private fork. More
specifically, I wanted to make the following changes:

- Use custom templates, instead of the ones that are hard-coded in Select2. This would also open up
  the possibility of not loading the image sprite included with Select2, but instead use
  [FontAwesome icons](http://fortawesome.github.io/Font-Awesome/) that I already use in my projects.
- Use custom loading indicators, instead of the spinner GIF included by Select2.
- I wanted to make it easier to support a use case where Select2 is used without any selection
  dropdown, but with the proper tokenization.
- Make Select2 work with jQuery builds without Sizzle for better performance. Patches for this have
  been accepted in Select2, but unfortunately it's a moving target causing repeated breakage. Also,
  once Sizzle is no longer required, it becomes much easier to support Zepto.js.
- Personally, I preferred a more modern codebase to work with, rather than the huge monolithic
  library that is Select2. This also includes proper documentation of the code as well as good test
  coverage. At this point also support for any IE version older than 10 can be dropped.

Having said that, if you're a user of Select2 and don't recognize yourself in any of these issues,
I advise you to keep using Select2. It's feature-rich and actively supported, so don't fix what
ain't broken ;)

Browser Support
---------------

- Chrome
- Firefox
- Internet Explorer 10+
- Safari 6+

Dependencies
------------

Select3 only relies on jQuery or Zepto.js being loaded on the page.

API
---

Call `$(selector).select3(options)` to initialize a Select3 instance on the element specified by the
given selector. The following options are supported:

Option         | Type                   | Description
-------------- | ---------------------- | -----------
closeOnSelect  | `Boolean`              | Set to `false` to keep the dropdown open after the user has selected an item. This is useful if you want to allow the user to quickly select multiple items. The default value is `true`.
data           | `Object` or `Array`    | Initial selection data to set. This should be an object with `id` and `text` properties in the case of a SingleSelect3 instance, or an array of such objects in the case of a MultipleSelect3 instance. This option is mutually exclusive with `value`.
value          | `ID` or `Array`        | Initial value to set. This should be an ID (string or number) in the case of a SingleSelect3 instance, or array of IDs in the case of a MultipleSelect3 instance. This property is mutually exclusive with `data`.
implementation | `String` or `Function` | The implementation to use. Default implementations include 'Multiple' and 'Single', but you can add custom implementations to the `Select3.Implementations` map. The default value is 'Single', unless multiple is true in which case it is 'Multiple'.
initSelection  | `Function`             | Function to map values by ID to selection data. This function receives two arguments, `value` and `callback`. The value is the current value of the selection, which is an ID or an array of IDs depending on the input type. The callback should be invoked with an object or array of objects, respectively, containing `id` and `text` properties.
items          | `Array`                | Array of items from which to select. Should be an array of objects with `id` and `text` properties. As convenience, you may also pass an array of strings, in which case the same string is used for both the ID and the text. If items are given, all items are expected to be available locally and all selection operations operate on this local array only. If `null`, items are not available locally, and the `query` option should be provided to fetch remote data.
matcher        | `Function`             | Function to determine whether text matches a given search term. Note this function is only used if you have specified an array of items. Receives two arguments: `term` and `text`. The term is the search term, which for performance reasons has always been already processed using `Select3.transformText()`. The text is the text that should match the search term.
placeholder    | `String`               | Placeholder text to display when the element has no focus and selected items.
query          | `Function`             | Function to use for querying items. Receives a single object as argument with the `callback`, `offset` and `term` properties. The callback should be invoked when the results are available. It should be passed a single object with `results` and `more` properties. The first is an array with result items and the latter is an optional boolean that may be set to `true` indicate more results are available through pagination. Offset is a property is only used for pagination and indicates how many results should be skipped when returning more results. The term is the search term the user is searching for. Unlike with the matcher function, the term has not been processed using `Select3.transformText()`. This option is ignored if the `items` option is used.
showDropdown   | `Boolean`              | Set to false if you don't want to use any dropdown (you can still open still open it programmatically using `open()`).
templates      | `Object`               | Object with instance-specific templates to override the global templates assigned to `Select3.Templates`.

For documentation about all the available methods once Select3 has been instantiated, I refer to the
inline documentation in the source files for now.

Events
------

All of these events are emitted on the element to which the Select3 instance is attached and can be
listened to using jQuery's `on()` method.

Event             | Description
----------------- | -----------
change            | Emitted when the selection has been changed. Additional properties on the event object: `added`, `data`, `removed` and `val`.
select3-closed    | Emitted when the dropdown is closed.
select3-open      | Emitted when the dropdown is opened.
select3-opening   | Emitted when the dropdown is about to be opened. You can call `preventDefault()` on the event object to cancel the opening of the dropdown.
select3-selected  | Emitted when an item has been selected. Additional properties on the event object: `id` and `item`.
select3-selecting | Emitted when an item is about to be selected. You can call `preventDefault()` on on the event object to prevent the item from being selected. Additional properties on the event object: `id` and `item`.

Build System
------------

Select3 is built modularly and uses Gulp as a build system to build its distributable files. To
install the necessary dependencies, please run:

    $ npm install -g gulp
    $ npm install

Then you can generate new distributable files from the sources, using:

    $ npm run build

While developing, you can start a development server like this (note this will overwrite some
distributable files with development versions which you are not supposed to check in, so either make
a new build, or revert the changes in the dist/ directory before committing):

    $ gulp dev

Unit Tests
----------

Unit tests are available and can be ran using the following command:

    $ gulp unit-tests

Contributing
------------

Patches for bugfixes are always welcome. Please accompany pull requests for bugfixes with a test
case that is fixed by the PR.

If you want to implement a new feature, I advise to open an issue first in the
[GitHub issue tracker](https://github.com/arendjr/select3/issues) so we can discuss its merits.

In the absence of a formal style guide, please take the following into consideration:

- Use four spaces, no tabs.
- Prefer single quotes.
- No lines longer than 100 characters.

Also make sure you don't check-in any JSHint violations.

In order to validate your code before pushing, please run the following script:

    $ tools/install-git-hooks.sh

This will install a Git pre-push hook that will run JSHint and all unit tests before pushing.
