This repo is for review of requests for signing shim.  To create a request for review:

- clone this repo
- edit the template below
- add the shim.efi to be signed
- add build logs
- commit all of that
- tag it with a tag of the form "myorg-shim-arch-YYYYMMDD"
- push that to github
- file an issue at https://github.com/rhboot/shim-review/issues with a link to your tag

Note that we really only have experience with using grub2 on Linux, so asking
us to endorse anything else for signing is going to require some convincing on
your part.

Here's the template:

-------------------------------------------------------------------------------
What organization or people are asking to have this signed:
-------------------------------------------------------------------------------
Neverware Inc. (https://www.neverware.com)

-------------------------------------------------------------------------------
What product or service is this for:
-------------------------------------------------------------------------------
CloudReady

-------------------------------------------------------------------------------
What's the justification that this really does need to be signed for the whole world to be able to boot it:
-------------------------------------------------------------------------------
CloudReady is a Linux distro; we'd like to encourage people to boot our OS with secure boot enabled.

-------------------------------------------------------------------------------
Who is the primary contact for security updates, etc.
-------------------------------------------------------------------------------
- Name: Nicholas Bishop
- Position: software developer
- Email address: nbishop@neverware.com
- PGP key, signed by the other security contacts, and preferably also with signatures that are reasonably well known in the linux community: https://github.com/neverware/shim-review/blob/master/nbishop.key

-------------------------------------------------------------------------------
Who is the secondary contact for security updates, etc.
-------------------------------------------------------------------------------
- Name: Paul Nardini
- Position: Director of Engineering
- Email address: pnardini@neverware.com
- PGP key, signed by the other security contacts, and preferably also with signatures that are reasonably well known in the linux community: https://github.com/neverware/shim-review/blob/master/pnardini.key

-------------------------------------------------------------------------------
What upstream shim tag is this starting from:
-------------------------------------------------------------------------------
https://github.com/rhboot/shim/tree/shim-15.1

-------------------------------------------------------------------------------
URL for a repo that contains the exact code which was built to get this binary:
-------------------------------------------------------------------------------
https://github.com/rhboot/shim/tree/shim-15.1

-------------------------------------------------------------------------------
What patches are being applied and why:
-------------------------------------------------------------------------------
A patch for a missing '{' in mok.c is applied to the shim-15.1 code base, as the build fails without it:
https://github.com/neverware/shim-build/blob/v4/build-fix.patch

-------------------------------------------------------------------------------
If bootloader, shim loading is, grub2: is CVE-2020-10713 fixed ?
-------------------------------------------------------------------------------
Yes.

-------------------------------------------------------------------------------
If bootloader, shim loading is, grub2, and previous shims were trusting affected
by CVE-2020-10713 grub2:
* were old shims hashes provided to Microsoft for verification
  and to be added to future DBX update ?
* Does your new chain of trust disallow booting old, affected by CVE-2020-10713,
  grub2 builds ?
-------------------------------------------------------------------------------
1) Yes, our old hashes have been sent to Microsoft for verification via email to ueficamanualreview@microsoft.com.
2) Our new chain of trust will utilize our new EV Code Signing certificate for the first time, which expires in 2022.  All vulnerable versions of grub that we have released to date have been signed with our old now-expired certificate (exp. 09/02/2020), so they will not be allowed to boot in our new chain of trust.

-------------------------------------------------------------------------------
If your boot chain of trust includes linux kernel, is
"efi: Restrict efivar_ssdt_load when the kernel is locked down"
upstream commit 1957a85b0032a81e6482ca4aab883643b8dae06e applied ?
Is "ACPI: configfs: Disallow loading ACPI tables when locked down"
upstream commit 75b0cea7bf307f362057cc778efe89af4c615354 applied ?
-------------------------------------------------------------------------------
1) Yes, 1957a85b0032a81e6482ca4aab883643b8dae06e is applied in our kernel (https://github.com/neverware/kernel/commit/1957a85b0032a81e6482ca4aab883643b8dae06e)
2) Yes, this commit is applied in our kernel but appears as 824d0b6225f3fa2992704478a8df520537cfcb56 (https://github.com/neverware/kernel/commit/824d0b6225f3fa2992704478a8df520537cfcb56)

-------------------------------------------------------------------------------
If you use vendor_db functionality of providing multiple certificates and/or
hashes please briefly describe your certificate setup. If there are whitelisted hashes
please provide exact binaries for which hashes are created via file sharing service,
available in public with anonymous access for verification
-------------------------------------------------------------------------------
We do not use this functionality.


-------------------------------------------------------------------------------
What OS and toolchain must we use to reproduce this build?  Include where to find it, etc.  We're going to try to reproduce your build as close as possible to verify that it's really a build of the source tree you tell us it is, so these need to be fairly thorough. At the very least include the specific versions of gcc, binutils, and gnu-efi which were used, and where to find those binaries.
-------------------------------------------------------------------------------
This repo contains the Dockerfile we use to build shim: https://github.com/neverware/shim-build/tree/v4

-------------------------------------------------------------------------------
Which files in this repo are the logs for your build?   This should include logs for creating the buildroots, applying patches, doing the build, creating the archives, etc.
-------------------------------------------------------------------------------
[build.log](build.log)

-------------------------------------------------------------------------------
Add any additional information you think we may need to validate this shim
-------------------------------------------------------------------------------
Our shim-15.1 build is largely upstream's 15.1 with our new public certificate embedded.  This build has been tested with our new grub, which is now up-to-date with fedora-33.



