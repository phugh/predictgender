# predictGender - Node.js based gender prediction!

Predict the gender of a string's author.

## Usage
```javascript
const pg = require('predictgender')
const opts = {  // These are the default options:
  'encoding': 'freq',
  'max': Number.POSITIVE_INFINITY,
  'min': Number.NEGATIVE_INFINITY,
  'nGrams': 'true',
  'output': 'gender',
  'places': 9,
  'sortBy': 'lex',
  'wcGrams': 'false',
}
const text = 'A long string of text....'
const gender = pg(text, opts)
console.log(gender)
```

**Please note: while 'output' will default to 'gender' if no option is presented, if an invalid option is presented (e.g. {output: 'nonesense'}), the module will return as if you had selected 'number' (see below!)**

## The Options Object

The options object is optional and provides a number of controls to allow you to tailor the output to your needs. However, for general use it is recommended that all options are left to their defaults.

### 'encoding'

**String - valid options: 'freq' (default), 'binary'**

Controls how the lexical value is calculated.

Frequency ('freq') encoding takes the overall wordcount and word frequency into account, i.e. (word frequency / word count) * weight.

Binary is simply the addition of lexical weights, i.e. word1 + word2 + word3.

It is recommended that this is always set to 'freq'.

### 'output'

**String - valid options: 'gender' (default), 'lex', 'matches', 'number', or 'full'**

'gender' (Default) returns a string, "Male", "Female", or "Unknown".

'matches' returns an array of matched words along with the number of times each word appears, its weight, and its final lexical value (i.e. (appearances / word count) * weight). See the output section below for an example.

'number' returns -1 for male, 0 for indeterminate or unknown, and 1 for female.

'lex' returns the lexical value, positive values being female, negative being male.

'full' returns an object with number, lex, and matches keys as above.

### 'nGrams'

**String - valid options: 'true' (default) or 'false'**

n-Grams are contiguous pieces of text, bi-grams being chunks of 2, tri-grams being chunks of 3, etc.

Use the nGrams option to include (true) or exclude (false) n-grams. For accuracy it is recommended that n-grams are included, however including n-grams for very long strings can detrement performance.

### 'wcGrams'

**String - valid options: 'true' or 'false' (default)**

When set to true, the output from the nGrams option will be added to the word count.

For accuracy it is recommended that this is set to false.

### 'sortBy'

**String - valid options: 'lex' (default)', 'weight', or 'freq'**

If 'output' = 'matches', this option can be used to control how the outputted array is sorted.

'lex' (default) sorts by final lexical value, i.e. (word frequency * word count) / word weight.

'weight' sorts the array by the matched words initial weight.

'freq' sorts by word frequency, i.e. the most used words appear first.

By default the array is sorted by final lexical value, this is so you can see which words had the greatest impact on the prediction - i.e. the words at the beginning of the array will be the most masculine, progressing toward the most feminine words at the end on the array.

### 'places'

**Number - valid options between 0 and 20 inclusive.**

Number of decimal places to limit outputted values to.

The default is 9 decimal places.

### 'max' and 'min'

**Float**

Exclude words that have weights above the max threshold or below the min threshold.

By default these are set to infinity, ensuring that no words from the lexicon are excluded.

## {'output': 'matches'} output example
Setting "output" to "matches" in the options object makes predictGender output an array containing information about the lexical matches in your query.

Each match between the lexicon and your input is pushed into an array which contains: the word, the number of times that word appears in the text (its frequency), its weight in the lexicon, and its lexical value (i.e. (word freq / total word count) * weight)).

By default the matches output array is sorted ascending by lexical value. This can be controlled using the "sortBy" option.

```javascript
[
  [ 'magnificent', 1, -192.0206116, -1.3914537072463768 ],
  [ 'capital', 1, -133.9311307, -0.9705154398550726 ],
  [ 'note', 3, -34.83417005, -0.7572645663043478 ],
  [ 'america', 2, -49.21227355, -0.7132213557971014 ],
  [ 'republic', 1, -75.5720402, -0.5476234797101449 ]
]
```

| Word          | Frequency | Weight        | Lexical Value       |
| ------------- | --------- | ------------- | ------------------- |
| 'magnificent' | 1         | -192.0206116  | -1.3914537072463768 |
| 'capital'     | 1         | -133.9311307  | -0.9705154398550726 |
| 'note'        | 3         | -34.83417005  | -0.7572645663043478 |
| 'america'     | 2         | -49.21227355  | -0.7132213557971014 |
| 'republic'    | 1         | -75.5720402   | -0.5476234797101449 |

## Acknowledgements

### References
Based on [Schwartz, H. A., Eichstaedt, J. C., Kern, M. L., Dziurzynski, L., Ramones, S. M., Agrawal, M., Shah, A., Kosinski, M., Stillwell, D., Seligman, M. E., & Ungar, L. H. (2013). Personality, Gender, and Age in the Language of Social Media: The Open-Vocabulary Approach. PLOS ONE, 8(9), e73791.](http://journals.plos.org/plosone/article/file?id=10.1371/journal.pone.0073791&type=printable)

### Lexicon
Using the gender lexicon data from [WWBP](http://www.wwbp.org/lexica.html) under the [Creative Commons Attribution-NonCommercial-ShareAlike 3.0 Unported](http://creativecommons.org/licenses/by-nc-sa/3.0/).

## Licence
(C) 2017 [P. Hughes](https://www.phugh.es).

[Creative Commons Attribution-NonCommercial-ShareAlike 3.0 Unported](http://creativecommons.org/licenses/by-nc-sa/3.0/).
