#***********************************************************************
#* Meta Pre Processor, a language blender                              *
#* (c) 2013 by Philippe Wang <philippe.wang@cl.cam.ac.uk>              *
#* Licence : ISC                                                       *
#* http://www.isc.org/downloads/software-support-policy/isc-license/   *
#***********************************************************************


MPP_LIB = mpp_charset.ml mpp_charstream.ml mpp_out.ml mpp_stringmap.ml  mpp_actions.ml mpp_init.ml mpp_core.ml mpp_load.ml
MPP_CMOs = $(addsuffix .cmo, $(basename $(MPP_LIB)))
MPP_CMXs = $(addsuffix .cmx, $(basename $(MPP_LIB)))

OCAMLLIBS = str
OCAMLLIBS_CMA = $(addsuffix .cma, $(OCAMLLIBS))
OCAMLLIBS_CMXA = $(addsuffix .cmxa, $(OCAMLLIBS))

OCAMLC = ocamlc -g
OCAMLOPT = ocamlopt -g

default:mpp

mpp:mpp.opt
	cp mpp.opt mpp

mpp.byte:mpp.cma mpp_main.ml
	$(OCAMLC) $(OCAMLLIBS_CMA)  -o $@ $+

mpp.opt:mpp.cmxa mpp_main.ml
	$(OCAMLOPT)  $(OCAMLLIBS_CMXA)  -o $@ $+

mpp.cma:$(MPP_CMOs)
	$(OCAMLC)  -a -o $@ $+

mpp.cmxa:$(MPP_CMXs)
	$(OCAMLOPT)  -a -o $@ $+

frag:
	cd lib/fragment && make frag
frag.opt:
	cd lib/fragment && make frag.opt

all:mpp mpp.out mpp-ocaml-toplevel-output

%.cmo:%.ml
	$(OCAMLC) -c $<

%.cmx:%.ml
	$(OCAMLOPT) -c $<

clean:
	rm -f *.cm[oxi] mpp mpp.opt *~ *.o

Makefile.depend:$(MPP_LIB) mpp_main.ml
	ocamldep $+ > $@

install:installmpp

installmpp:mpp
	@if [[ "${PREFIX}" == "" ]] ; then echo "# To build MPP and copy it to ${PREFIX}/bin/, please run the following command:\nPREFIX=/path/to/prefix make install\n" ; else ( cp mpp.opt "${PREFIX}/bin/" || echo 'Installation failed, please check you have the rights.' ) || echo 'I could not build MPP.' ; fi

# Makefile.depend is generated by ocamldep
-include Makefile.depend
