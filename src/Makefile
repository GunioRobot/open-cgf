ERL=$(shell which erl)
ERLC=$(shell which erlc)

EMULATOR=beam

ifdef debug
  ERLC_FLAGS+=-Ddebug
endif

ifdef trace
  ERLC_FLAGS=+trace
endif

ifdef export_all
  ERLC_FLAGS+=-Dexport_all
endif

INSTALL=install -c
INSTALL_DATA=${INSTALL} -m 644

../ebin/%.$(EMULATOR): %.erl
	$(ERLC) -b $(EMULATOR) $(ERLC_FLAGS) -o ../ebin $<

%.$(EMULATOR): %.erl
	$(ERLC) -b $(EMULATOR) $(ERLC_FLAGS) $<


ifeq ($(TYPE),debug)
DEBUG_FLAGS = -Ddebug
else
DEBUG_FLAGS =
endif

MODULES=open-cgf open-cgf_app open-cgf_sup gtpp_decode gtpp_encode open-cgf_state gtpp_udp_server open-cgf_ctl open-cgf_logger open-cgf_config gtpp_tcp_server gtpp_tcp_connection cdr_filename open-cgf_cdr_sup cdr_file_srv open-cgf_cdr_srv


EBIN_FILES=$(MODULES:%=../ebin/%.$(EMULATOR)) 

ERLC_FLAGS+=-W +debug_info $(DEBUG_FLAGS) -I../include -pa ../ebin

#
# Targets
#

all: $(EBIN_FILES) 

debug:
	$(MAKE) TYPE=debug

clean:
	rm -f $(EBIN_FILES)

install: all
	install -d ../ebin
	(cd ../ebin; find . -name '*' -print | xargs tar cf - ) | (cd ../ebin; tar xf - )

tags:
	erl  -noshell -pa ../ebin -s mail_debug mktags

xref:
	erl -noshell -pa ../ebin -s mail_debug xref ../ebin

