MD convert format
===

convert doc format with pandoc:

	# type: markdown, html, latex ...
	$ pandoc -f <type> -t <type> xxx.txt

	# determine type from output file's suffix
	$ pandoc -o xxx.tex xxx.txt

pdf
---

* retext -> print
* markdown2pdf

		$ iconv -t utf-8 input.txt | markdown2pdf

* convert to word -> print to pdf

word
---

	$ pandoc -s xxx.md -o xxx.docx
	$ pandoc -s xxx.md -o xxx.odt

html
---

	$ pandoc -o xxx.html xxx.md

slide
---

for s5:

	$ pandoc -w s5 -s xxx.txt > xxx.html

for slidy:

	$ pandoc -w slidy -s xxx.txt > xxx.html
