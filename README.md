# Patches for Ares v121
The last release of Ares put out by Near was listed as "not fully complete or stable" in the release notes, the source code itself can not compile on it's own.

## TLCS900H Processor Changes
The release notes state the following changes to the TLCS900H processor:
- TLCS900H: do not allow interrupts to run between CPDR, LDIR, LDDR instructions; fixes Fantastic Night Dreams Cotton
-   TLCS900H: refactored instruction decoder and disassembler using range-case
-   TLCS900H: improved emulation of (PC+d16) addressing mode (LDAR)
-   TLCS900H: improved prefetch handling and instruction timing (still imperfect)
-   TLCS900H: improved timing further (still imperfect)

Apparently between releases of v120 and v121, Near wanted to remove the name of the structs to make them anonymous. However, this causes a "member with constructor not allowed in anonymous aggregate compiler" error. To resolve this, code was reverted back to it's syntax used in v120.

To apply the patch, place `tlcs900h.diff` in the `/ares/component/processor/` directory and run the following command (linux):
```
patch -p0 < tlcs900h.diff
```

## GCC 11 Compilation Quirk
If you're using a system that has GCC 11 as the primary compiler, a modification is required to ensure Ares v121 compiles. `_serialize` was [introduced as a macro](https://patchwork.ozlabs.org/project/gcc/patch/CAMe9rOp_DLg55kRxw2v75PPeqj-8tDKob5z-+EWPpf-L3OuKKw@mail.gmail.com/) and was pushed into GCC 11. This is intentional. Thus any source code which uses `_serialize` as a local variable will need changes. Not aware of the specific nomenclature Near would use, I personally figured renaming `_serialize` to `__serialize` would be the easiest and most effective method.

To apply this patch, place `gcc11_serialize.diff` in the `/ares/ares/node/` and run the following command (linux):
```
patch system.hpp < gcc11_serialize.diff
```

## Why patches? Why not upload the Ares v121 source and modify as it's now under the ISC permissible license?
The license itself within the code links to the [Open Source Initiative's ICS License page](https://opensource.org/licenses/ISC), but does not provide the Owner or Year information. It's safe to assume that Near is the owner and the year is 2021, but without an actual license included with this information bundled with the code, I do not wish to upload it.

On a much more personal note, I also do not believe this contribution is worthy of Near's legacy. I do not wish to be the first person to fork Ares post v120. Near was obviously trying to tinker with the TLCS900H processor, leaving "still imperfect" in the release notes and what I have done is revert changes to successfully compile. This is a regression of code, and I would like to believe Near was always moving forward to improve their code, and there might have been a reason they wanted to change their code this way, however, I just couldn't figure out how to get it to work in a way Near probably could have.

This patch is released in the hopes that many will enjoy the legacy that Near/Byuu has created. From bsnes, to higan, to Ares. If this is the final version that Near has touched, I believe everyone has a right to play it and use it freely. I hope you these patches useful, and may they help carry on Near's spirit.
