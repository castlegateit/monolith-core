# Monolith Core Module

Monolith is a collection of utility functions and classes that make PHP and WordPress development a little bit easier. The Core module can be used with any PHP project and uses the `\Castlegate\Monolith\Core` namespace.

## Install

Monolith Core is available on [Packagist](https://packagist.org/) and can be installed via [Composer](https://getcomposer.org/):

    composer require castlegate/monolith-core

## Functions

*   `contains($haystack, $needle)` Does `$haystack` contain `$needle`? Works with strings and arrays.

*   `startsWith($haystack, $needle)` Does `$haystack` start with `$needle`? Works with strings and arrays.

*   `endsWith($haystack, $needle)` Does `$haystack` end with `$needle`? Works with strings and arrays.

*   `fileSize($file, $decimals = 2)` Return a human-readable file size with units and to a particular number of decimal places.

*   `dataUrl($file, $type = null)` Return a base64-encoded data URL from a file path.

*   `formatUrl($url, $human = false)` Provided with something that looks like a URL, return a predictable URL with or without its scheme.

*   `formatLink($url, $content = null, $attributes = [])` Provided with something that looks like a URL, return a valid HTML link with optional content.

*   `formatTel($tel, $human = false, $code = null)` Return a formatted telephone number.

*   `formatTelLink($tel, $content = null, $attributes = [], $code = null)` Return a telephone number link.

*   `obfuscate($text)` Return a string with characters randomly encoded as HTML entities.

*   `obfuscateLink($email, $content = null, $attributes = [])` Return an obfuscated HTML email link.

*   `ordinal($number)` Return a number with its appropriate ordinal suffix, e.g. "1st", "2nd", or "3rd".

*   `truncate($text, $max, $ellipsis = ' &hellip;')` Truncates text to within a particular number of characters, avoiding breaking words.

*   `truncateWords($text, $max, $ellipsis = ' &hellip;')` Truncates text to within a particular number of words, avoiding breaking words.

*   `formatAttributes($attributes)` Converts an associative array into a string containing HTML attributes. Nested arrays are converted into space-separated lists.

*   `embedSvg($file, $args = [])` Return the contents of an SVG file stripped of anything that might cause problems when it is embedded in an HTML file. This function uses the `ScalableVectorGraphic` class described below.

*   `twitterName($url)` Extract and return a Twitter handle from a valid Twitter URL.

*   `splitLines($text)` Split comma- and newline-delimited text (e.g. an address) into array items.

*   `rejoinLines($lines, $sep = ', ')` Rejoin lines, either as array or string parsed by `splitLines`, with new delimiter.

## Classes

### ScalableVectorGraphic

The `ScalableVectorGraphic` class sanitizes SVG code for embedding directly in HTML documents. By default, it removes the XML declaration and attempts to add a `viewBox` attribute if one is not already present.

~~~ php
$svg = new \Cgit\Monolith\Core\ScalableVectorGraphic;
$svg->parse($code); // import SVG code from string
$svg->load($file); // import SVG code from file

echo $svg->embed(); // return sanitized SVG code
~~~

You can also use it to remove attributes from the root element and to remove styles from the entire SVG. This may be useful for SVG icons where the fill colour should be set by the document CSS and not the CSS embedded in the SVG code.

~~~ php
$svg->removeAttributes('viewBox');
$svg->removeAttributes(['width', 'height']);
$svg->removeStyles('fill');
$svg->removeStyles(['fill', 'stroke']);
~~~

You can reset the SVG to its original condition using the `reset()` method. You can also return the original source code and the non-sanitized, parsed SVG code using the `embedSourceCode()` and `embedSourceDom()` methods respectively.

You can use the `fill($color)` method to set a fill attribute on the root SVG element. You can also use the `title($title)` method to set a title element for better accessibility.

### TimeSpanner

The `TimeSpanner` class provides a convenient way of calculating and displaying consistently formatted ranges of dates or times. Its constructor sets the start and end dates, performing some sanitization of the input (integers are assumed to be Unix time; everything else gets fed through `strtotime()`).

~~~ php
$foo = new \Cgit\Monolith\Core\TimeSpanner($start, $end);

$foo->getStartTime($format); // e.g. "1 January 2010"
$foo->getEndTime($format);
$foo->getRange($formats, $tolerance); // e.g. "1-10 January 2010"
$foo->getInterval($formats, $tolerance); // e.g. "4 seconds" or "10 years"
~~~

You can set the tolerance in seconds for displaying ranges of times. If the difference between the start and end times is within the tolerance value, they are considered to be the same time.

~~~ php
$foo->setDefaultRangeTolerance($seconds);
~~~

Time formats can be specified when returning a value or as default values for this instance. Formats are specified in the standard PHP date format.

~~~ php
$foo->setDefaultTimeFormat('j F Y');
$foo->setDefaultRangeFormats([
    'time' => ['H:i', '&ndash;', 'H:i d F Y'],
    'day' => ['d', '&ndash;', 'd F Y'],
    'month' => ['d F', '&ndash;', 'd F Y'],
    'year' => ['d F Y', '&ndash;', 'd F Y'],
]);
~~~

### Video

The `Video` class takes any approximately valid YouTube or Vimeo URL or embed code and provides access to URLs, embed codes, images, and links.

~~~ php
$foo = new \Cgit\Monolith\Core\Video($code);

$foo->url(); // video URL
$foo->image(); // video placeholder image
$foo->embed(); // video iframe embed code
$foo->link(); // HTML image with link to video
$foo->responsiveEmbed(); // iframe embed with responsive wrapper
~~~

Note that `responsiveEmbed` needs an iframe with width and height attributes to calculate the aspect ratio of the video. Otherwise, videos are assumed to be in 16:9 format.

## License

Copyright (c) 2019 Castlegate IT. All rights reserved.

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program. If not, see <https://www.gnu.org/licenses/>.
