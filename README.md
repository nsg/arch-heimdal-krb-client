# Package for Heimdal Kerberos

This package installs the client tools like kinit, ktutil and kadmin but not the server stuff like kdc, iprop, ...

The binaries are installed in `/usr/` and conflicts with the `krb5` package that you most likely already have installed. I provide a package called `lib64-krb5` that only contains the libraries from MIT kerberos.

To use this, first install `lib64-krb5`, answer yes to the conflicting package `krb5` to remove it.

# How to build

Execute `makepkg` as a normal user, the package will be build and called `heimdal-krb-client-version-x86_64.pkg.tar.xz`

# How to install

Make sure `lib64-krb5` is installed and execute `pacman -U heimdal-krb-client-version-x86_64.pkg.tar.xz`

# Repo

Prefere binaries built by me? I host a repo at http://repos.nsg.cc/arch.

Update /etc/pacman.conf and add:

```
[nsg]
Server = http://repos.nsg.cc/arch/$arch/
```
(I only support 64 bit systems)

Update your repos with `pacman -Sy`. All my packages are signed with the key `A5C0970E`, it is listed at the keyserver `hkp://pool.sks-keyservers.net`

To import my key, type `pacman-key -r A5C0970E`, inspect it with `pacman-key -f A5C0970E`.

If you trust it and can verify that this is the right key, trust it with `pacman-key --lsign-key A5C0970E`.

All done, just search and install.

```
# pacman -Ss heimdal
nsg/heimdal-krb-client 1.6rc2-2
    The Heimdal Kerberos network authentication system client tools
```
