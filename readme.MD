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
2. NUM_OF_OUTPUT_TOPICS type: __int__, max number of topics your want, actual
  number of output may be less.

Type in command line for help on positional and optional arguments.
```
python Mao_topic.py -h
```
Optional parameters:
1. -s FILE_NAME_OF_STOPWORDS type: __String__, provide your own list of stopwords
2. -N SIZE_OF_WINDOWS type: __int__, value in range of [2, 10]. The larger, the
  more connections each word will have.
3. -d DAMPING_FACTOR type: __float__, value in range of (0, 1). Default is 0.85.
4. -t THRESHOLD_OF_CONVERGE type: __float__. Default is 1e-4, the smaller, the
  longer time to run.
5. -M MAX_NUMBER_OF_ITERATIONS type: __int__. To stop iterations even if not converged.
6. -S If __flagged__, output will be single words, else output will be a mixture of
  single and collapsed topics.

## Algorithm

## Design Details

## Known Issues
1. Command line arguments might be broke by very long and complicated URL.
  Recommend using quotation mark(") to enclose your URL.
2. May seldomly have wrong encoded characters when output to the command line.

## Reference
1. [TextRank: Bringing Order into Text (Mihalcea and Tara)](https://web.eecs.umich.edu/~mihalcea/papers/mihalcea.emnlp04.pdf)
2. [Default English stopwords list](http://www.ranks.nl/stopwords)

## Contact
[Myles Mao](https://www.linkedin.com/in/mylesmao/)
[myles1316@gmail.com](mailto:myles1316@gmail.com)
