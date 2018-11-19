include ../../../make.inc

mytargets_sqlite3 = KJV.sqlite3

MYTARGETS = KJV_schema.mql KJV_data.mql $(mytargets_sqlite3)

CLEANFILES = *~ $(MYTARGETS)

EXTRA_DIST = parsed/*.txt \
             queries/*.mql \
             EmdrosQueryTool-KJVParsed.cfg \
             EmdrosChunkingTool-KJVParsed.cfg \
             README-KJVParsed.txt \
             queries/*.mql

all: $(MYTARGETS)

clean:
	rm -f $(CLEANFILES)


KJV_schema.mql: $(EMDROS_BINPATH)/pennimport
	$(EMDROS_BINPATH)/pennimport -d KJV --distinct-nt-otns -s > $@

KJV_data.mql: $(EMDROS_BINPATH)/pennimport
	$(EMDROS_BINPATH)/pennimport -d KJV --distinct-nt-otns parsed/*.txt > $@

KJV.sqlite3: KJV_schema.mql KJV_data.mql
	-echo "DROP DATABASE 'KJV.sqlite3' GO" | $(EMDROS_BINPATH)/mql -b 3
	-echo "DROP DATABASE 'KJV' GO" | $(EMDROS_BINPATH)/mql -b 3
	$(EMDROS_BINPATH)/mql -b 3 KJV_schema.mql
	$(EMDROS_BINPATH)/mql -b 3 -d KJV KJV_data.mql
	echo "CREATE INDEXES ON OBJECT TYPES [All]" | $(EMDROS_BINPATH)/mql -b 3 -d KJV
	echo "VACUUM DATABASE ANALYZE GO" | $(EMDROS_BINPATH)/mql -b 3 -d KJV
	mv KJV $@

examples: $(MYTARGETS)
	mkdir -p $(EXAMPLESDIR)/KJVParsed
	mkdir -p $(EXAMPLESDIR)/KJVParsed/MQL
	mkdir -p $(EXAMPLESDIR)/KJVParsed/parsed
	mkdir -p $(EXAMPLESDIR)/KJVParsed/queries
	cp -p KJV_schema.mql KJV_data.mql $(EXAMPLESDIR)/KJVParsed/MQL/
	cp -p parsed/*.txt $(EXAMPLESDIR)/KJVParsed/parsed/
	cp -p queries/*.mql $(EXAMPLESDIR)/KJVParsed/queries/
	cp -p KJV.sqlite3  $(EXAMPLESDIR)/KJVParsed/
	cp -p EmdrosQueryTool-KJVParsed.cfg $(EXAMPLESDIR)/KJVParsed/
	cp -p EmdrosChunkingTool-KJVParsed.cfg $(EXAMPLESDIR)/KJVParsed/
	cp -p README-KJVParsed.txt $(EXAMPLESDIR)/KJVParsed/
