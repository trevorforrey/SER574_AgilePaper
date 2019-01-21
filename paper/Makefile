#
# Makefile for acmart package
#
# This file is in public domain
#
# $Id: Makefile,v 1.10 2016/04/14 21:55:57 boris Exp $
#

PACKAGE=acmart

SAMPLES = \
	src/sample-manuscript.tex \
	src/sample-acmsmall.tex \
	src/sample-acmlarge.tex \
	src/sample-acmtog.tex \
	src/sample-sigconf.tex \
	src/sample-authordraft.tex \
	src/sample-xelatex.tex \
	src/agile-paper.tex \
	src/sample-sigchi.tex \
	src/sample-sigchi-a.tex


PDF = $(PACKAGE).pdf ${SAMPLES:%.tex=%.pdf} acmguide.pdf

all:  ${PDF}


%.pdf:  %.dtx   $(PACKAGE).cls
	pdflatex $<
	- bibtex $*
	pdflatex $<
	- makeindex -s gind.ist -o $*.ind $*.idx
	- makeindex -s gglo.ist -o $*.gls $*.glo
	pdflatex $<
	while ( grep -q '^LaTeX Warning: Label(s) may have changed' $*.log) \
	do pdflatex $<; done


acmguide.pdf: $(PACKAGE).dtx $(PACKAGE).cls
	pdflatex -jobname acmguide $(PACKAGE).dtx
	- bibtex acmguide
	pdflatex -jobname acmguide $(PACKAGE).dtx
	while ( grep -q '^LaTeX Warning: Label(s) may have changed' acmguide.log) \
	do pdflatex -jobname acmguide $(PACKAGE).dtx; done

%.cls:   %.ins %.dtx
	pdflatex $<

src/%: %
	cp $^ src

src/sample-acmsmall.tex: src/sample-manuscript.tex
	sed 's/documentclass\[manuscript,screen\]{acmart}/documentclass[acmsmall]{acmart}/' $< > $@

src/sample-acmlarge.tex: src/sample-manuscript.tex
	sed 's/documentclass\[manuscript,screen\]{acmart}/documentclass[acmlarge,screen]{acmart}/' $< > $@

src/sample-acmtog.tex: src/sample-manuscript.tex
	sed 's/documentclass\[manuscript,screen\]{acmart}/documentclass[acmtog]{acmart}/' $<   > $@

src/sample-sigconf.tex: src/sample-manuscript.tex
	sed 's/documentclass\[manuscript,screen\]{acmart}/documentclass[sigconf]{acmart}/' $< | sed 's/^%%//'  > $@

src/sample-authordraft.tex: src/sample-manuscript.tex
	sed 's/documentclass\[manuscript,screen\]{acmart}/documentclass[sigconf,authordraft]{acmart}/' $< | sed 's/^%%//'  > $@

src/sample-xelatex.tex: src/sample-sigconf.tex
	cp $< $@

src/agile-paper.tex: src/sample-manuscript.tex
	sed 's/documentclass\[manuscript,screen\]{acmart}/documentclass[sigplan,screen]{acmart}/' $< | sed 's/^%%//'  > $@

src/sample-sigchi.tex: src/sample-manuscript.tex
	sed 's/documentclass\[manuscript,screen\]{acmart}/documentclass[sigchi]{acmart}/' $< | sed 's/^%%//'  > $@

src/sample-sigchi-a.tex: src/sample-manuscript.tex
	sed 's/documentclass\[manuscript,screen\]{acmart}/documentclass[sigchi-a]{acmart}/' $< | \
	sed 's/begin{figure}\[h\]/begin{marginfigure}/' | \
	sed 's/end{figure}/end{marginfigure}/' | \
	sed 's/begin{table}/begin{margintable}/' | \
	sed 's/end{table}/end{margintable}/'   > $@


src/$(PACKAGE).cls: $(PACKAGE).cls
src/ACM-Reference-Format.bst: ACM-Reference-Format.bst

src/%.pdf:  src/%.tex   src/$(PACKAGE).cls src/ACM-Reference-Format.bst
	cd $(dir $@) && pdflatex $(notdir $<)
	- cd $(dir $@) && bibtex $(notdir $(basename $<))
	cd $(dir $@) && pdflatex $(notdir $<)
	cd $(dir $@) && pdflatex $(notdir $<)
	while ( grep -q '^LaTeX Warning: Label(s) may have changed' $(basename $<).log) \
	  do cd $(dir $@) && pdflatex $(notdir $<); done

src/sample-xelatex.pdf:  src/sample-xelatex.tex   src/$(PACKAGE).cls src/ACM-Reference-Format.bst
	cd $(dir $@) && xelatex $(notdir $<)
	- cd $(dir $@) && bibtex $(notdir $(basename $<))
	cd $(dir $@) && xelatex $(notdir $<)
	cd $(dir $@) && xelatex $(notdir $<)
	while ( grep -q '^LaTeX Warning: Label(s) may have changed' $(basename $<).log) \
	  do cd $(dir $@) && xelatex $(notdir $<); done



.PRECIOUS:  $(PACKAGE).cfg $(PACKAGE).cls


clean:
	$(RM)  $(PACKAGE).cls *.log *.aux \
	*.cfg *.glo *.idx *.toc \
	*.ilg *.ind *.out *.lof \
	*.lot *.bbl *.blg *.gls *.cut *.hd \
	*.dvi *.ps *.thm *.tgz *.zip *.rpi \
	src/$(PACKAGE).cls src/ACM-Reference-Format.bst \
	src/*.log src/*.aux src/*.out \
	src/*.bbl src/*.blg src/*.cut

distclean: clean


#
# Archive for the distribution. Includes typeset documentation
#
archive:  all clean
	COPYFILE_DISABLE=1 tar -C .. -czvf ../$(PACKAGE).tgz --exclude '*~' --exclude '*.tgz' --exclude '*.zip'  --exclude CVS --exclude '.git*' $(PACKAGE); mv ../$(PACKAGE).tgz .

zip:  all clean
	zip -r  $(PACKAGE).zip * -x '*~' -x '*.tgz' -x '*.zip' -x CVS -x 'CVS/*'

documents.zip: all
	zip $@ acmart.pdf acmguide.pdf src/sample-*.pdf *.cls *.bst
