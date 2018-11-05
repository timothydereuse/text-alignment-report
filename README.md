# text-alignment-report

#### Needleman-Wunsch Algorithm [[wikipedia]](https://en.wikipedia.org/wiki/Needleman%E2%80%93Wunsch_algorithm)

An algorithm from bioinformatics that computes global alignment, finding matching regions between two strings of characters. The assumption is that both sequences are related through the operations of character insertion, character deletion, and character replacement; one way to interpret the algorithm is that it finds the most efficient way to transform one sequence into another using just those operations. Each operation usually has a cost associated with it depending on the application; character replacement may be a less 'costly' operation than insertion and deletion, for example.

In this task, we are given a correct human transcript of a medieval manuscript and an OCRed version of that manuscript that likely contains many errors but has information on where each letter is placed on the page. By identifying similar strings of characters in these two sequences, we can match each word in the correct transcript with its occurrence in the noisy OCR, and thus find that word's location on the image of the manuscript.

Example: first portion of einsiedeln_001v. (In all examples, the first line is the human transcript, the second line is the noisy OCR of the manuscript, and the third shows the result of the alignment: `O` is a match, `X` is a mismatch (or, replacement), and a space ` ` is a gap (insertion / deletion) in one of the sequences.

```
opotentiam venientem et neb__ulam to_tam terra_m te_gentem Ite ob__viam ei et dicite _Nunt
opoten_nain iieenten et nebit lam to tam terrain te gen em__te ob llian e iet bicite slunt
OOOOOO XOXXXXXXOOOOXOOOOOOO  XOOOOOO OOOOOOOOO XOOO OOOXOO  OOOOO  XOOXOOXXOOOXOOOOOO XOOO
```

#### Gap Penalties

Different operations can be assigned different costs in the alignment process. Consider the two strings `abc` and `acb`, and these two different ways to align them:

```
abc  |  abc_
acb  |  a_cb
OXX  |  O O
```

If both mismatches and gaps have the same cost, then the second alignment is better. If mismatches are considered less costly than gaps, however, then the first alignment might be better.

We also consider that a desirable alignment might have gaps that tend to be contiguous rather than scattered about.

## OCR Performance on Medieval Manuscripts

[Wikipedia's Comparison of Major OCR Software](https://en.wikipedia.org/wiki/Comparison_of_optical_character_recognition_software). Most are not adaptive and support only printed text.

[A 2015 test of then-current OCR methods on medieval manuscripts](https://brandonwhawk.net/2015/04/20/ocr-and-medieval-manuscripts-establishing-a-baseline/). Two proprietary OCR engines are used (Adobe Acrobat and ABBYY Finereader). On a printed source, FineReader has a per-character accuracy of 97.78%, and Acrobat has an accuracy of 87.57%. However, on the handwritten source examined (St. Gall 561), only Finereader bears any remote resemblance to the original text (no percentage given, but it'd be quite low; less than 50%).

#### Google Tesseract
Written in C++, [python bindings with pytesseract](https://pypi.org/project/pytesseract/)

Intended only for printed script, but can be trained on handwriting; [paper achieving 90% per-character success rate on handwritten characters using Tesseract](https://arxiv.org/abs/1003.5893) however, these characters are pre-segmented.

[Early latin training set for Tesseract](https://latinocr.org/). Looks promising, and in principle uses a similar character set to the latin characters in plainchant manuscripts, but fails - can't segment handwriting with minims at all. A word like "dominus" is transcribed as "do11111111s." To use Tesseract with gothic script, would need totally pre-segmented characters.

#### Transkribus [[Homepage]](https://transkribus.eu/Transkribus/)
A large application for recognizing handwriting in historical documents - totally centralized, training data shared between users. Also contains quite robust automatic layout detection / text line finding. Main project is GUI only, but there exists a python toolkit that allows interfacing with it and its main server, so in principle it would be possible to use its functionality in a separate program (if the project's maintainers allow). 

Intended to be used by manually transcribing a large amount of text and then using that to train a model of your own manuscript. This requires manual assistance from their project managers. They suggest at least 15,000 words of ground truth before a model can be trained. They do have a few already-trained models of various types of handwriting, though, including gothic script. Here are some results of transcribing plainchant manuscripts using their gothic script model:

[Einseideln](https://raw.githubusercontent.com/timothydereuse/text-alignment-report/master/transkribus-einseideln.png) |
[Klosterneuberg](https://github.com/timothydereuse/text-alignment-report/blob/master/transkribus-klosterneuberg.png) |
[Salzinnes](https://github.com/timothydereuse/text-alignment-report/blob/master/transkribus-salzinnes.png) |
[St. Maurf](https://github.com/timothydereuse/text-alignment-report/blob/master/transkribus-stmaurf.png) |
[St. Gall 390](https://github.com/timothydereuse/text-alignment-report/blob/master/transkribus-stgall.png)

It's not perfect, but it might be close enough for a sequence alignment approach, at least for some manuscripts.

[Another Transkribus-based model of gothic handwriting that might be usable, though not yet public](https://read.transkribus.eu/2017/06/09/medieval-handwriting-and-handwritten-text-recognition/). Interesting note: language models are not that useful for these texts because spellings are so inconsistent even within individual manuscripts written by the same person.

#### OCRopus [[Homepage]](https://github.com/tmbdev/ocropy)
Collection of OCR utilities, usable from command line, based in LSTM. Once contained specific support for handwriting / manuscripts, but that functionality has been removed. It can still be used for historical manuscripts, with some success: [with lots of careful training, this project managed 9.7% error rates on handwriting similar to that of the Salzinnes manuscript](https://graal.hypotheses.org/786). However, the same model did not work well on other manuscripts.

#### In Codice Ratio [[Homepage]](http://www.inf.uniroma3.it/db/icr/)

[Overview Paper](http://ceur-ws.org/Vol-2034/paper_2.pdf)

OCR specifically for latin handwritten religious texts (The Vatican Secret Archives). Code fully available on website (in process of testing) but there is no documentation and many of the comments are in italian. Method based on training a CNN to distinguish images that represent a single character from those that represent a character fragment or more than one character. Ground truth for this purpose obtained through a custom crowdsourcing platform and 120 high school students.

Only works on images of manually extracted words. Does not include any layout analysis / text line detection.

[Results on individual words are encouraging](http://www.inf.uniroma3.it/db/icr/preliminary-results.html). Presents many of the same problems (different forms of same letter, ornamentation, inconsistent spacing) present in the plainchant manuscripts. The particular handwriting they use is similar to that of St. Gall 390, St. Maur.
