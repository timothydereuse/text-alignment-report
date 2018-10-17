# text-alignment-report

[Wikipedia's Comparison of Major OCR Software](https://en.wikipedia.org/wiki/Comparison_of_optical_character_recognition_software)

[A 2015 test of then-current OCR methods on medieval manuscripts](https://brandonwhawk.net/2015/04/20/ocr-and-medieval-manuscripts-establishing-a-baseline/). Two proprietary OCR engines are used (Adobe Acrobat and ABBYY Finereader). On a printed source, FineReader has a per-character accuracy of 97.78%, and Acrobat has an accuracy of 87.57%. However, on the handwritten source examined (St. Gall 561), only Finereader bears any remote resemblance to the original text (no percentage given, but it'd be quite low).

Non-proprietary OCR applications in active development as of 2018:

#### Google Tesseract
* Written in C++, [python bindings with pytesseract](https://pypi.org/project/pytesseract/)
* Intended only for printed script, but can be trained on handwriting; [paper achieving 90% per-character success rate on handwritten characters using Tesseract](https://arxiv.org/abs/1003.5893)
* [Early latin training set for Tesseract](https://latinocr.org/)

#### Transkribus [[Homepage]](https://transkribus.eu/Transkribus/)
A large application for recognizing handwriting in historical documents - totally centralized, training data shared between users. Main project is GUI only, but there exists a python toolkit that allows interfacing with it and its main server, so in principle it would be possible to use its functionality in a separate program. 

Intended to be used by manually transcribing a large amount of text and then using that to train a model of your own manuscript. This requires manual assistance from their project managers. They do have a few already-trained models of various types of handwriting, including gothic script. Here are some results of transcribing plainchant manuscripts using their gothic script model:

[Einseideln](https://raw.githubusercontent.com/timothydereuse/text-alignment-report/master/transkribus-einseideln.png) |
[Klosterneuberg](https://github.com/timothydereuse/text-alignment-report/blob/master/transkribus-klosterneuberg.png) |
[Salzinnes](https://github.com/timothydereuse/text-alignment-report/blob/master/transkribus-salzinnes.png) |
[St. Maurf](https://github.com/timothydereuse/text-alignment-report/blob/master/transkribus-stmaurf.png) |
[St. Gall 390](https://github.com/timothydereuse/text-alignment-report/blob/master/transkribus-stgall.png)

It's not perfect, but it might be close enough for a sequence alignment approach, at least for some manuscripts.

#### OCRopus [[Homepage]](https://github.com/tmbdev/ocropy)
Collection of OCR utilities, usable from command line. Once contained support for handwriting / manuscripts, but that functionality has been removed. Cannot find anyone using or reccomending its use on anything but printed sources.
