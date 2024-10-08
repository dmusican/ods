
sources = intro.tex arrays.tex linkedlists.tex skiplists.tex \
	hashing.tex binarytrees.tex rbs.tex scapegoat.tex redblack.tex \
	heaps.tex sorting.tex graphs.tex integers.tex btree.tex
javs = $(sources:.tex=-java.tex)
javh = $(sources:.tex=-java-html.tex)
cpps = $(sources:.tex=-cpp.tex)
cpph = $(sources:.tex=-cpp-html.tex)
java = $(wildcard ../java/ods/*.java)
cppsource = $(wildcard ../cpp/*.h)
python = $(wildcard ../python/ods/*.py)
pyts = $(sources:.tex=-python.tex)
pyth = $(sources:.tex=-python-html.tex)
pdfs = ods-java.pdf ods-cpp.pdf ods-python.pdf
styles=ods.sty ods-colors.sty

LFLAGS=-interaction=nonstopmode -halt-on-error --shell-escape

all: $(pdfs)

.PHONY: all figs images install clean

figs:
	cd figs ; make
	cd figs-python ; make
   

epsfigs:
	cd figs ; make epss
	cd figs-python ; make epss

images:
	cd images ; make

ods-java.pdf : ods-java.tex $(javs) $(styles) images figs
#	latexmk --shell-escape -pdf ods-java
	pdflatex $(LFLAGS) ods-java
	bibtex ods-java
	pdflatex $(LFLAGS) ods-java
	pdflatex $(LFLAGS) ods-java

ods-java.tex : ods.tex Makefile
	echo "%%%\n%%% Autogenerated file - do not edit \n%%%" | cat - ods.tex \
		| sed 's/-lang/-java/' > ods-java.tex
	sed -i 's/%HEADCOMMAND/\\newcommand{\\cpponly}[1]{}\\newcommand{\\javaonly}[1]{#1}\\newcommand{\\pcodeonly}[1]{}\\newcommand{\\notpcode}[1]{#1}\\newcommand{\\lang}{Java}/' ods-java.tex

ods-cpp.pdf : ods-cpp.tex $(cpps) $(styles) images figs
	latexmk -pdf ods-cpp
#	pdflatex $(LFLAGS) ods-cpp
#	bibtex ods-cpp
#	pdflatex $(LFLAGS) ods-cpp
#	pdflatex $(LFLAGS) ods-cpp

ods-cpp.tex : ods.tex Makefile
	echo "%%%\n%%% Autogenerated file - do not edit \n%%%" | cat - ods.tex \
		| sed 's/-lang/-cpp/' > ods-cpp.tex
	sed -i 's/%HEADCOMMAND/\\newcommand{\\javaonly}[1]{}\\newcommand{\\cpponly}[1]{#1}\\newcommand{\\pcodeonly}[1]{}\\newcommand{\\notpcode}[1]{#1}\\newcommand{\\lang}{C++}/' ods-cpp.tex

ods-python.pdf : ods-python.tex $(pyts) $(styles) images figs
	latexmk -pdf ods-python

ods-python.tex : ods.tex Makefile
	echo "%%%\n%%% Autogenerated file - do not edit \n%%%" | cat - ods.tex \
		| sed 's/-lang/-python/' > ods-python.tex
	sed -i 's/%HEADCOMMAND/\\newcommand{\\javaonly}[1]{}\\newcommand{\\cpponly}[1]{}\\newcommand{\\pcodeonly}[1]{#1}\\newcommand{\\notpcode}[1]{}\\newcommand{\\lang}{pseudocode}/' ods-python.tex


%-java.tex : %.tex $(java) snarf-java.pl Makefile
	echo "%%%\n%%% Autogenerated file - do not edit \n%%%" | cat - $< \
		| ./snarf-java.pl > $*-java.tex 

%-cpp.tex : %.tex $(cppsource) snarf-cpp.pl Makefile
	echo "%%%\n%%% Autogenerated file - do not edit \n%%%" | cat - $< \
		| ./snarf-cpp.pl > $*-cpp.tex 

%-python.tex : %.tex $(python) snarf-python.py Makefile
	echo "%%%\n%%% Autogenerated file - do not edit \n%%%" | cat - $< \
		| ./snarf-python.py > $*-python.tex 

install-pdf : ods-java.pdf ods-cpp.pdf ods-python.pdf ods-java-screen.pdf ods-cpp-screen.pdf ods-python-screen.pdf
	scp -r $^ morin@cg.scs.carleton.ca:public_html/ods/

install-html : java-html cpp-html python-html
	scp -r  ods-java/ ods-cpp/ ods-python/ morin@cg.scs.carleton.ca:public_html/ods/

install : install-pdf install-html

clean:
	rm -f $(javs) $(cpps) $(pyts) $(javh) $(cpph) $(pyth) *.log *.bbl *.out ods.toc $(pdfs) 
	rm -rf ods-java/ ods-cpp/
	cd figs; make clean
	cd figs-python; make clean
	cd images; make clean

# A dirty hack for getting a grayscale file
%-grayscale.pdf : %.pdf
	gs -sOutputFile=$@ -sDEVICE=pdfwrite \
	-sColorConversionStrategy=Gray -dProcessColorModel=/DeviceGray \
	-dCompatibilityLevel=1.4 $< < /dev/null

# Lots of hackery for generating html using latex2html
html: java-html cpp-html python-html

java-html : $(javh) ods-java-html.tex epsfigs
	mkdir -p ods-java
	pdflatex ods-java-html
	bibtex ods-java-html
	latex2html -init_file latex2html-init -dir ods-java ods-java-html

ods-java-html.tex : ods.tex Makefile
	sed 's/-lang/-java-html/' ods.tex > ods-java-html.tex
	sed -i 's/%HEADCOMMAND/\\newcommand{\\cpponly}[1]{}\\newcommand{\\pcodeonly}[1]{}\\newcommand{\\javaonly}[1]{#1}\\newcommand{\\notpcode}[1]{#1}\\newcommand{\\lang}{Java}/' ods-java-html.tex
	sed -i 's/\\include{cpp-preface}//' ods-java-html.tex

%-java-html.tex : %.tex $(java) snarf-java-html.pl Makefile
	cat $< | ./snarf-java-html.pl > $*-java-html.tex 

cpp-html : $(cpph) ods-cpp-html.tex epsfigs
	mkdir -p ods-cpp
	pdflatex ods-cpp-html
	bibtex ods-cpp-html
	latex2html -init_file latex2html-init -dir ods-cpp ods-cpp-html

ods-cpp-html.tex : ods.tex Makefile
	sed 's/-lang/-cpp-html/' ods.tex > ods-cpp-html.tex
	sed -i 's/%HEADCOMMAND/\\newcommand{\\javaonly}[1]{}\\newcommand{\\pcodeonly}[1]{}\\newcommand{\\cpponly}[1]{#1}\\newcommand{\\notpcode}[1]{#1}\\newcommand{\\lang}{C++}/' ods-cpp-html.tex

%-cpp-html.tex : %.tex $(cpp) snarf-cpp-html.pl Makefile
	cat $< | ./snarf-cpp-html.pl > $*-cpp-html.tex 

python-html : $(pyth) ods-python-html.tex epsfigs
	mkdir -p ods-python
	pdflatex ods-python-html
	bibtex ods-python-html
	latex2html -init_file latex2html-init -dir ods-python ods-python-html

ods-python-html.tex : ods-python.tex Makefile
	sed 's/-python/-python-html/' ods-python.tex > ods-python-html.tex

%-python-html.tex : %.tex $(pyt) snarf-cpp-html.pl Makefile
	cat $< | ./snarf-python.py -html > $*-python-html.tex 

%-screen.pdf : %.pdf
	gs -o $@ -sDEVICE=pdfwrite -c "[/CropBox [53 88 379 595] /PAGES pdfmark" -f $<

