TESTPACKAGES := test-a test-b test-c test-d test-e test-f test-g test-h test-j test-z
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
