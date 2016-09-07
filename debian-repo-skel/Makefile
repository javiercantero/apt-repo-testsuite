
init: state-0

state-%: debs-state-%
	reprepro -Vb . includedeb testsuite-repo $</*.deb

clean:
	rm -rf db/ dists/ pool/

.PHONY: init state-% clean
