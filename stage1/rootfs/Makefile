USR=usr_from_$(subst usr-from-,,$(RKT_STAGE1_USR_FROM))

# Only include shim on the coreos flavor
SHIM=$(subst usr_from_coreos,shim,$(findstring usr_from_coreos,$(USR)))

_SUBDIRS=$(USR) waiter $(SHIM) diagexec prepare-app enter net-plugins net init gc
SUBDIRS=$(_SUBDIRS) aggregate
export CFLAGS=-Wall -Os

.PHONY: usr_switch test clean subdirs $(SUBDIRS)

subdirs: $(SUBDIRS)

# cache the usr flavor and detect when it's changed so we can force the new flavor to at least install itself
usr_switch:
	@[ -e .usr ] && LAST_USR=$$(cat .usr);	\
	if [ "$(USR)" != "$$LAST_USR" ]; then	\
		touch "$(USR)/install";		\
		echo -n "$(USR)" > .usr;	\
	fi

$(SUBDIRS): usr_switch
	$(MAKE) -C $@ $(MAKECMDGOALS)

clean: $(SUBDIRS)

test: $(SUBDIRS)

aggregate: $(_SUBDIRS)
