Building the repository
=======================

Prepare the repository:

 1. Adjust the `conf/distributions` file if you like. Uncomment the line
    `SignWith:` and replace the PGP public key ID by your own if you
    want to sign the repository (this later prevents a lot of effort
    dealing with untrusted sources). Don't forget to add the key to the
    APT keyring using `apt-key`. 
 2. Copy all the *.deb files of the initial packages into
    `debs-state-0/`
 3. Copy all the *.deb files of the new/modified packages into
    `debs-state-1/`
 4. Add the repository path to /etc/apt/sources.list

To build the initial state of the repository:

    make init
    apt-get update # (or aptitude update, ...)

To build the modified state of the repository:

    make state-1
    apt-get update # (or aptitude update, ...)

(You can perform more complex tests by increasing the number of states)

If you want to repeat the test from the start, first remove the
repository using `make clean`.

