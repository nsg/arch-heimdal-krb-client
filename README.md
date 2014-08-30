# Package for Heimdal Kerberos

This package installs the client tools like kinit, ktutil and kadmin but not the server stuff like kdc, iprop, ...

The binaries are installed in `/usr/` and conflicts with the `krb5` package that you most likely already have installed. I provide a package called `lib64-krb5` that only contains the libraries from MIT kerberos.

To use this, first install `lib64-krb5`, answer yes to the conflicting package `krb5` to remove it.
