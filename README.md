Download Link: https://assignmentchef.com/product/solved-cmpsc-442-homework-5
<br>
<h1>Instructions</h1>

In this assignment, you will gain experience working with Markov models on text.

A skeleton file homework5-cmpsc442.py containing empty definitions for each question has been provided. Since portions of this assignment will be graded automatically, none of the names or function signatures in this file should be modified. However, you are free to introduce additional variables or functions if needed.

You may import definitions from any standard Python library, and are encouraged to do so in case you find yourself reinventing the wheel.

You will find that in addition to a problem specification, most programming questions also include a pair of examples from the Python interpreter. These are meant to illustrate typical use cases to clarify the assignment, and are not comprehensive test suites. In addition to performing your own testing, you are strongly encouraged to verify that your code gives the expected output for these examples before submitting.

You are strongly encouraged to follow the Python style guidelines set forth in <a href="https://legacy.python.org/dev/peps/pep-0008/">PEP 8</a>, which was written in part by the creator of Python. However, your code will not be graded for style.

<h1>1.  Markov Models [95 points]</h1>

In this section, you will build a simple language model that can be used to generate random text resembling a source document. Your use of external code should be limited to built-in Python modules, which excludes, for example, NumPy and NLTK.

<ol>

 <li><strong>[10 points] </strong>Write a simple tokenization function tokenize(text) which takes as input a string of text and returns a list of tokens derived from that text. Here, we define a token to be a contiguous sequence of non-whitespace characters, with the exception that any punctuation mark should be treated as an individual token. <em>Hint: Use the built-in constant </em>string.punctuation<em>, found in the </em>string</li>

</ol>

&gt;&gt;&gt;tokenize(”  This is an example.  “) &gt;&gt;&gt;tokenize(“‘Medium-rare,’ she said.”) [‘This’, ‘is’, ‘an’, ‘example’, ‘.’]        [“‘”, ‘Medium’, ‘-‘, ‘rare’, ‘,’, “‘”,

‘she’, ‘said’, ‘.’]

<ol start="2">

 <li><strong>[10 points] </strong>Write a function ngrams(n, tokens) that produces a list of all <em>n</em>-grams of the specified size from the input token list. Each <em>n</em>-gram should consist of a <em>2</em>-element tuple (context, token), where the context is itself an <em>(n-1)</em>-element tuple comprised of the <em>n-1</em> words preceding the current token. The sentence should be padded with <em>n-1</em> “&lt;START&gt;” tokens at the beginning and a single “&lt;END&gt;” token at the end. If <em>n = 1</em>, all contexts should be empty tuples. You may assume that <em>n ≥ 1</em>.</li>

</ol>

&gt;&gt;&gt;ngrams(1, [“a”, “b”, “c”])                                                                        &gt;&gt;&gt;ngrams(3, [“a”, “b”, “c”])

[((), ‘a’), ((), ‘b’), ((), ‘c’),         [((‘&lt;START&gt;’, ‘&lt;START&gt;’), ‘a’),  ((), ‘&lt;END&gt;’)]      ((‘&lt;START&gt;’, ‘a’), ‘b’), &gt;&gt;&gt;ngrams(2, [“a”, “b”, “c”])         ((‘a’, ‘b’), ‘c’),

[((‘&lt;START&gt;’,), ‘a’), ((‘a’,), ‘b’),               ((‘b’, ‘c’), ‘&lt;END&gt;’)]  ((‘b’,), ‘c’), ((‘c’,), ‘&lt;END&gt;’)]

<ol start="3">

 <li><strong>[15 points] </strong>In the NgramModel class, write an initialization method __init__(self, n) which stores the order <em>n</em> of the model and initializes any necessary internal variables. Then write a method</li>

</ol>

update(self, sentence) which computes the <em>n</em>-grams for the input sentence and updates the internal

counts. Lastly, write a method prob(self, context, token) which accepts an <em>(n-1)</em>-tuple representing a context and a token, and returns the probability of that token occuring, given the preceding context.

<ol start="4">

 <li><strong>[20 points] </strong>In the NgramModel class, write a method random_token(self, context) which returns a</li>

</ol>

random token according to the probability distribution determined by the given context. Specifically, let <em>T = &lt; t</em><em><sub>1</sub>, t</em><em><sub>2</sub>, . . . , t</em><em><sub>n</sub> &gt;</em> be the set of tokens which can occur in the given context, sorted according to Python’s natural lexicographic ordering, and let <em>0 ≤ r &lt; 1</em> be a random number between <em>0</em> and <em>1</em>. Your method should return the token <em>t</em><em><sub>i</sub></em> such that

You should use a single call to the random.random() function to generate <em>r</em>.

&gt;&gt;&gt;m = NgramModel(1)                                                                               &gt;&gt;&gt;m = NgramModel(2)

&gt;&gt;&gt;m.update(“a b c d”)                                                                                 &gt;&gt;&gt;m.update(“a b c d”)

&gt;&gt;&gt;m.update(“a b a b”)                                                                                &gt;&gt;&gt;m.update(“a b a b”)

&gt;&gt;&gt;random.seed(1)                                                                                      &gt;&gt;&gt;random.seed(2)

&gt;&gt;&gt;[m.random_token(())     &gt;&gt;&gt;[m.random_token((“&lt;START&gt;”,))      for i in range(25)]                  for i in range(6)]

[‘&lt;END&gt;’, ‘c’, ‘b’, ‘a’, ‘a’, ‘a’, ‘b’,                                                                  [‘a’, ‘a’, ‘a’, ‘a’, ‘a’, ‘a’]

‘b’, ‘&lt;END&gt;’, ‘&lt;END&gt;’, ‘c’, ‘a’, ‘b’,                                                     &gt;&gt;&gt;[m.random_token((“b”,))

‘&lt;END&gt;’, ‘a’, ‘b’, ‘a’, ‘d’, ‘d’,                                                                        for i in range(6)]

‘&lt;END&gt;’, ‘&lt;END&gt;’, ‘b’, ‘d’, ‘a’, ‘a’]                                                                 [‘c’, ‘&lt;END&gt;’, ‘a’, ‘a’, ‘a’, ‘&lt;END&gt;’]

<ol start="5">

 <li><strong>[20 points] </strong>In the NgramModel class, write a method random_text(self, token_count) which returns a string of space-separated tokens chosen at random using the random_token(self, context) Your starting context should always be the <em>(n-1)</em>-tuple (“&lt;START&gt;”, …, “&lt;START&gt;”), and the context should be updated as tokens are generated. If <em>n = 1</em>, your context should always be the empty tuple. Whenever the special token “&lt;END&gt;” is encountered, you should reset the context to the starting context.</li>

</ol>

&gt;&gt;&gt;m = NgramModel(1)                                                                               &gt;&gt;&gt;m = NgramModel(2)

&gt;&gt;&gt;m.update(“a b c d”)                                                                                 &gt;&gt;&gt;m.update(“a b c d”)

&gt;&gt;&gt;m.update(“a b a b”)                                                                                &gt;&gt;&gt;m.update(“a b a b”)

&gt;&gt;&gt;random.seed(1)                                                                                      &gt;&gt;&gt;random.seed(2)

&gt;&gt;&gt;m.random_text(13)                                                                                &gt;&gt;&gt;m.random_text(15)

‘&lt;END&gt; c b a a a b b &lt;END&gt; &lt;END&gt; c a b’                                                      ‘a b &lt;END&gt; a b c d &lt;END&gt; a b a b a b c’

<ol start="6">

 <li><strong>[10 points] </strong>Write a function create_ngram_model(n, path) which loads the text at the given path and creates an <em>n</em>-gram model from the resulting data. Each line in the file should be treated as a separate sentence.</li>

</ol>

# No random seeds, so your results may vary

&gt;&gt;&gt;m = create_ngram_model(1, “frankenstein.txt”); m.random_text(15)

‘beat astonishment brought his for how , door &lt;END&gt; his . pertinacity to I felt’

&gt;&gt;&gt;m = create_ngram_model(2, “frankenstein.txt”); m.random_text(15)

‘As the great was extreme during the end of being . &lt;END&gt; Fortunately the sun’

&gt;&gt;&gt;m = create_ngram_model(3, “frankenstein.txt”); m.random_text(15)

‘I had so long inhabited . &lt;END&gt; You were thrown , by returning with greater’

&gt;&gt;&gt;m = create_ngram_model(4, “frankenstein.txt”); m.random_text(15)

‘We were soon joined by Elizabeth . &lt;END&gt; At these moments I wept bitterly and’

<ol start="7">

 <li><strong>[10 points] </strong>Suppose we define the perplexity of a sequence of <em>m</em> tokens <em>&lt; w</em><em><sub>1</sub>, w</em><em><sub>2</sub>, . . . , w</em><em><sub>m</sub> &gt; </em>to be</li>

</ol>

For example, in the case of a bigram model under the framework used in the rest of the assignment, we would generate the bigrams <em>&lt; (w</em><em><sub>0</sub> = &lt; START &gt;, w</em><em><sub>1</sub>), (w</em><em><sub>1</sub>, w</em><em><sub>2</sub>), . . . , (w</em><em><sub>m-1</sub>, w</em><em><sub>m</sub>), (w</em><em><sub>m</sub>, w</em><em><sub>m+1</sub> = &lt;END&gt;) &gt;</em>, and would then compute the perplexity as

Intuitively, the lower the perplexity, the better the input sequence is explained by the model. Higher values indicate the input was “perplexing” from the model’s point of view, hence the term perplexity.

In the NgramModel class, write a method perplexity(self, sentence) which computes the <em>n</em>-grams for the input sentence and returns their perplexity under the current model. <em>Hint: Consider performing an intermediate computation in log-space and re-exponentiating at the end, so as to avoid numerical overflow.</em>

<h1>2.   Feedback [5 points]</h1>

<ol>

 <li><strong>[1 point] </strong>Approximately how long did you spend on this assignment?</li>

 <li><strong>[2 points] </strong>Which aspects of this assignment did you find most challenging? Were there any significant stumbling blocks?</li>

 <li><strong>[2 points] </strong>Which aspects of this assignment did you like? Is there anything you would have changed?</li>

</ol>