# forceFeed.js

**Static mockups are liars. They only show the designer's content.**

The `forceFeed.js` script is designed to help you test your design's tolerance for variable, dynamic content. A tiny script that "force feeds" HTML elements with random text content within specified parameters. Set up, keep refreshing your browser until something breaks, then fix the design.

## Get started

### Installation

ForceFeed is available via `npm install forcefeed` or `bower install forcefeed`

### Include the script

First include the script after the page content and before the closing `body` tag:

```HTML
  <script src="path/to/forceFeed.js"></script>
</body>
```

### Attribute elements

`forceFeed.js` uses a special attribute, `data-forcefeed`, to feed randomized content into individual elements. Consider the following example:

```HTML
<div class="person">
  <h3 class="name" data-forcefeed="words|2"></h3>
  <p class="description" data-forcefeed="sentences|3|6">This will be overridden</p>
</div>
```
Here, I have created a dummy person/bio module. Note the "parameters" of the `data-forcefeed` attribute: The first refers to a named array of content from which the random content is sampled. The second and third parameters determine what quantity of content should be sampled. If only one parameter is provided, exactly this many items from the array is included. So, in `data-forcefeed="words|2"`, two random words from a "words" array will be provided. If two numbers are present, as in `data-forcefeed="sentences|3|6`, _between_ 3 and 6 items ("sentences") will make up that sample.

### Executing forceFeed.js

Of course, for the above to work, you need to execute the script, providing an object that defines the arrays from which you aim to generate the sample content. In this case, I need to link up a "words" and a "sentences" array.

Here are my arrays:

```javascript
window.words = ['lorem', 'ipsum', 'dolor', 'sit', 'amet', 'adipsing', 'consectetur', 'elit', 'sed', 'commodo', 'eu', 'ligula', 'vitae', 'mollis'];

window.sentences = ['Curabitur tempus lobortis faucibus.', 'Nulla sed consequat libero.', 'Phasellus bibendum neque eros, vel malesuada ligula fermentum et.', 'Vivamus pretium vulputate imperdiet.', 'Morbi eleifend urna ac purus auctor, non porttitor massa semper.', 'Ut tempor ante et mauris sagittis, sed vestibulum urna rhoncus.', 'Mauris quis augue fermentum, auctor metus quis, imperdiet lectus.', 'Morbi nec rhoncus lectus.',  'Nulla cursus venenatis urna maximus bibendum.', 'Cras at ornare tortor.', 'Nulla nulla justo, posuere id ultricies non, feugiat et nisi.', 'Duis porttitor aliquet viverra.', 'Integer vehicula fringilla velit, at iaculis dolor imperdiet vel.'];
```

And here's how I execute the script:

```javascript
forceFeed({words: window.words, sentences: window.sentences});
```

**Note:** If an array is not defined or an unrecognized array name is used as a parameter, the script will throw an error and cease execution:

```javascript
if (!buffet) {
  throw new Error('A forceFeed.js parameter, ' + split[0] + ', for the element ' + el.outerHTML + ' is not recognized.');
}
```

## COMMAND + R UNTIL IT BREAKS!

The notion that inspired `forceFeed.js` is that static mockups don't tell the whole story: Content will vary in length and quantity as a design is put to use. You need to test the design's tolerance for dynamic content!

So, load up the script and keep hitting COMMAND + R (or CTRL + F5), adjusting the design each time you need to until it's nice and robust.

## FUCK LOREM, LET'S CANNIBALIZE!

In the previous example, I hand coded some arrays using lorem ipsum text. But, if the page already has static content, why not use that? In the following recipe, I take all words from paragraphs on the page to reuse in `forceFeed.js`:

```javascript
// get all the paragraph elements as an array
window.paragraphs = [].slice.call(document.querySelectorAll('p'));

// init allWords array
window.allWords = [];

// iterate over paragraphs
paragraphs.forEach(function(paragraph) { 
  
  // remove punctuation
  var paragraphContent = paragraph.textContent
                        .replace(/[^\w\s]|_/g, "")
                        .replace(/\s+/g, " ");
  
  // make into an array
  var wordsArray = paragraphContent.split(' ');
  
  // remove bogus, empty last item
  wordsArray.pop();
  
  // push each word to our array
  wordsArray.forEach(function(word) {
    allWords.push(word);
  });
  
});

forceFeed({words: allWords});

```

In this implementation, the randomized, force fed content is made up of the original content. This is somewhat of an improvement because it uses the language of the page, not Latin dummy text.

# License

[WTFPL](http://sam.zoy.org/wtfpl/)
