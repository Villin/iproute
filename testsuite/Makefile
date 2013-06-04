## -- Config --
DEV := lo
PREFIX := sudo
TC := iproute2/iproute2-this/tc/tc
IP := iproute2/iproute2-this/ip/ip
## -- End Config --

TESTS := $(patsubst tests/%,%,$(wildcard tests/*.t))
IPVERS := $(filter-out iproute2/Makefile,$(wildcard iproute2/*))
$([ -e /boot/config-`uname -r`]) || KENV := $(shell cat /boot/config-`uname -r` | grep ^CONFIG)

.PHONY: compile listtests alltests configure $(TESTS)

configure:
	echo "Entering iproute2" && cd iproute2 && $(MAKE) configure && cd ..;

compile: configure
	echo "Entering iproute2" && cd iproute2 && $(MAKE) && cd ..;

listtests:
	@for t in $(TESTS); do \
		echo "$$t"; \
	done

alltests: $(TESTS)

clean:
	@rm -rf results/*

distclean: clean
	echo "Entering iproute2" && cd iproute2 && $(MAKE) distclean && cd ..;

$(TESTS):
	@mkdir -p results;
	@i=iproute2/iproute-this;
	@o=`echo $$i | sed -e 's/iproute2\///'`; \
	echo -n "Running $@ [$$o/`uname -r`]: "; \
	TC="$$i/tc/tc" IP="$$i/ip/ip" DEV="$(DEV)" IPVER="$@" SNAME="$$i" \
	ERRF="results/$@.$$o.err" $(KENV) $(PREFIX) tests/$@ > results/$@.$$o.out; \
	if [ "$$?" = "127" ]; then \
		echo "SKIPPED"; \
	elif [ -e "results/$@.$$o.err" ]; then \
		echo "FAILED"; \
	else \
		echo "PASS"; \
	fi; \
	dmesg > results/$@.$$o.dmesg; \
