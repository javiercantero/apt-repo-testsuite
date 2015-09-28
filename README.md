apt repository testsuite
========================

The goal is to create synthetic repositories to test apt/aptitude
dependency resolution in several scenarios.

Requirements
------------

 * Debian toolchain to build packages (dpkg-dev, debhelper, ...)
 * reprepro (or [equivalent][1]) to generate an apt repository

Usage
-----

The script `gentestpackage` creates dummy .deb source packages ready to
be built with dpkg-buildpackage:

    gentestpackage test-a
    cd test-a/
    fakeroot dpkg-buildpackage -b -us -uc

We must do this for every package needed in that scenario.

To set dependencies between the packages, we should manually edit the
`debian/control` file and add the package names to the `Depends:` field:
    
    Depends: \${misc:Depends}, test-b, test-c, test-d

When all the .deb packages are built, we need to create the apt
repository using reprepro. For that, we create a empty directory
somewhere and inside a `conf/distributions` text file with:

    Origin: Debian
    Label: Debian-All
    Suite: testing
    Codename: testsuite-repo
    Version: 3.0
    Architectures: i386 amd64
    Components: main
    Description: testing repository

Now we move all the *.deb files into the new directory and invoke
reprepro:

    reprepro -Vb . includedeb testsuite-repo *.deb

There are two methods to get access to the apt repository: local or set
up a HTTP server. For the local one, the `sources.list` line should be
like this:

    deb file:///home/user/path/to/debian-repo testsuite-repo main

where `/home/user/path/to/debian-repo` is the directory created for
reprepro.

The second method requires to install a HTTP server and publish through
it the repository tree created by reprepro. The `source.list` should
reflect that:

    deb http://localhost/maybe-some-path testsuite-repo main

Note: it's possible to set up a debian repository within a GitHub repo,
uploading the required .deb files using git push, but I don't want to
encourage the distribution of binary .deb packages from unverified
sources.

 [1]: https://wiki.debian.org/HowToSetupADebianRepository

