## Dependencies
This script requires 3-rd party libraries for getting and parsing HTTP content.
  1. [Requests](http://docs.python-requests.org/en/master/)
  2. [BeautifulSoup](http://www.crummy.com/software/BeautifulSoup/bs4/doc/)
  3. [lxml](http://lxml.de/)

## Usage
Use with Python 2.7.12 or above. Back-compatibility to lower version is not tested.

In command line, type:
```
python Mao_topic.py YOUR_URL NUM_OF_OUTPUT_TOPICS
```
  1. YOUR_URL type: __String__, complete url for the webpage. e.g. "https://www.brightedge.com/"
  2. NUM_OF_OUTPUT_TOPICS type: __int__, max number of topics your want, actual number of output may be less.

Type in command line for help on positional and optional arguments.
```
python Mao_topic.py -h
```
Optional parameters:
  1. -s FILE_NAME_OF_STOPWORDS type: __String__, provide your own list of stopwords
  2. -N SIZE_OF_WINDOWS type: __int__, value in range of [2, 10]. The larger, the more connections each word will have.
  3. -d DAMPING_FACTOR type: __float__, value in range of (0, 1). Default is 0.85.
  4. -t THRESHOLD_OF_CONVERGE type: __float__. Default is 1e-4, the smaller, the longer time to run.
  5. -M MAX_NUMBER_OF_ITERATIONS type: __int__. To stop iterations even if not converged.
  6. -S If __flagged__, output will be single words, else output will be a mixture of single and collapsed topics.

## Algorithm
Initially, I thought about using LDA or LHA model for a supervised machine learning approach. But, it would require a huge labelled dataset to train a model that can identity arbitrary topics in a random webpage. On the other end of spectrum, I could hack this with a simple-minded harsh table and return most frequent words (plus filtering out general stop words). I am not satisfied with either of them and searched online a little bit and came across with this algorithm called "TextRank".[1] It is an unsupervised learning based on graphical model that do not require any trainings. It is adopted from the Google's "PageRank", which ranks the importance of webpages based on what pages it directs to and be directed from. In computational biology, we used a very similar variant to identify a "central" or "important" protein in biological signal pathways. In brief, words in the webpage are scraped and stripped into a sequence. Then I scan the sequence with a window of size N. If two words are within the same window, they are connected in the graph. Then, I score each word based on how many words are connected with it, and the scores of its neighbors, using a formula. I do this in iterations and stop until the difference (max error of any words) between two iterations is below a given threshold (or max number of iterations has been reached in this case).

In the paper I cited here, the author compared directed vs. undirected and weighted vs. unweighted graph. I implemented a undirected and unweighted graph since their results showed that weights do not make significant improvements. Also its easy to modify my current data structure to implement directed graph.

## Design Details
Apart from modularization and many optional parameters to allow user-defined tuning, I incorporated a collapsed topics feature. After convergence in the graph, I sort the words based on their score or importance and take top 3n of them as a short list (n is the user-defined length for output). Then I go through the article again. If two adjacent words in the article are both in this short list, I will collapse them into one topic. I can keep growing this topic until the next word is not in the short list. e.g. "Minimal Linear System Solution ...". Then I score these collapsed topics and remove the single words I used in these topics. After that, I output the top n of the mixed set, in hope of having longer/more specific topics while maintaining many other aspects of the rankings. However, it worth noticing that, the length of short list is directly proportional to the output length. So, a too short output will hurt to find more adjacent topics and are likely to return only single-word topics. Depending on the length of webpage and information entropy, I would suggest starting from 5.

I adopted a list stopwords to filter out general and uninformative words, for example, (am, is, are, here, there...) I also added several common verbs (make, go, give, tell, etc.). You can also specify your customized stopwords list. You can open the "stopword" file with a common text editor and use it as a template.

## Known Issues
  1. Command line arguments might be broke by very long and complicated URL. Recommend using quotation mark(") to enclose your URL.
  2. May seldomly have wrong encoded characters when output to the command line.

## Reference
  1. [TextRank: Bringing Order into Text (Mihalcea and Tara)](https://web.eecs.umich.edu/~mihalcea/papers/mihalcea.emnlp04.pdf)
  2. [Default English stopwords list](http://www.ranks.nl/stopwords)

## Contact
  [Myles Mao](https://www.linkedin.com/in/mylesmao/)
  [myles1316@gmail.com](mailto:myles1316@gmail.com)
