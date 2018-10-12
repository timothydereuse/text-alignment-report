# text-alignment-report

[Wikipedia's Comparison of Major OCR Software](https://en.wikipedia.org/wiki/Comparison_of_optical_character_recognition_software)

[A 2015 test of then-current OCR methods on medieval manuscripts].(https://brandonwhawk.net/2015/04/20/ocr-and-medieval-manuscripts-establishing-a-baseline/) Two proprietary OCR engines are used (Adobe Acrobat and ABBYY Finereader). On a printed source, FineReader has a per-character accuracy of 97.78%, and Acrobat has an accuracy of 87.57%. However, on the handwritten source examined (St. Gall 561), only Finereader bears any remote resemblance to the original text.

Non-proprietary OCR applications in active development as of 2018:

#### Google Tesseract
* Written in C++, [python bindings with pytesseract](https://pypi.org/project/pytesseract/)
* Intended only for printed script, but can be trained on handwriting; [paper achieving 90% per-character success rate on handwritten characters using Tesseract](https://arxiv.org/abs/1003.5893)
* [Early latin training set for Tesseract](https://latinocr.org/)

#### Transkribus [[Homepage]](https://transkribus.eu/Transkribus/)
A large application for recognizing handwriting in historical documents - totally centralized, training data shared between users. GUI only.

#### OCRopus [[Homepage]](https://github.com/tmbdev/ocropy)
Collection of OCR utilities, usable from command line. Once contained support for handwriting / manuscripts, but that functionality has been removed. Cannot find anyone using or reccomending its use on anything but printed sources.
