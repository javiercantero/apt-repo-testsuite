TESTPACKAGES := test-a test-b test-c test-d test-e
PACKAGES := $(patsubst test-%, test-%.deb, $(TESTPACKAGES))

packages: $(PACKAGES)

test: $(TESTPACKAGES)

test-%:
	../../../gentestpackage $@

test-%.deb: test-%
	cd $<; fakeroot dpkg-buildpackage -b -us -uc; debian/rules clean
	rm $<*.changes

clean:
	rm test*.deb

.PHONY: clean packages test
