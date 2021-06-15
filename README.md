# Netgear GPL Build
Dockers for building Netgear GPL sources

## Purpose
Netgear provides GPL sources for their routers, but they are difficult to build out of the box. Take the following readme for instance:
```
~~~~~~~~~~~~^M
First Compile TOOLChain:^M
1. tar jxf buildroot-gcc463-src-offlinebuild.tar.bz2^M
2. change directory to buildroot-2012.11.1/ and run ./build.sh^M
3. After compile finish:^M
   it will install toolchain to /opt/buildroot-gcc463/^M
^M
~~~~~~~~~~~~^M
Compile environment:^M
Linux distribution: Fedora 12^M
Linux kernel: 2.6.31.5-127.fc12.i686.PAE^M
GCC: 4.4.2 20091027^M
XZ: 5.0.3^M
LZMA: 4.32.7^M
~~~~~~~~~~~~^M
How to Compile Source Code and Upgrade:^M
^M
1. make^M
2. target file is ./image/R6020/R6020.bin and R6020.img
3. HTTP upgrade: please use R6020.img.
```

Gotta love those Windows newlines amiright?

The year is 2021. This readme suggests using Fedora 12. EOL for Fedora 12 was in 2010. I'm certainly not going to be running this out of a VM. Even if I did, the Fedora 12 mirrors probably aren't up. If they are, props to Fedora. Dockerhub doesn't have images going back to V12.

So what to do? I want to leave the GPL sources untouched. This repo aims to utilize some modern-ish distro to build GPL sources. This means many dependencies will have to be built from source at specific versions. Namely, this is GCC.

## Future Work

As environments become available to build GPL sources, specific version of sources will be tagged with said version. I would like a history of what changes betweeen versions for a specific model.
- Branches will be based off of the previous version of a models firmware
  - Changes between versions should be evident in the git history

**Modernization of utilities**
- When I'm feeling silly, maybe update dependencies and tweak the source as needed
  - These branches shall be tagged with an identifier indicating that they are *dirty* or perhaps with a `-s` for `silly`

## Other Comments

Can we take a moment to appreciate how ridiculous some of these modifications from Netgear are? I'm pulling these criticisms from the R6080 GPL and they may be a one-off but I'm not so certain.
- Has anyone told Netgear about version control? Every edit is commented with the editor's name
  - `/* Ron */`
  - `/* Ron Get current directory */`
  - `/* Ron */`
  - `/* Ron */`
  - `/* Ron */`
  - `/* Ron */`
  - `/* JIM add for dns hijact */`
    - "hijack", Jim!!!
      - My man had one chance... 
  - `/* Ron add for auto add setup.cgi?next_file */`
  - ... ad infinitum
- Tons of `ifdef`s without a commented definition for the `endif`s. Sooo difficult to read.
- Several occurences of commented out `fprintf`
  - Maybe build with release and debug / learn about debug printing?
- `#if 0`
  - 😭
- `// qqq`
  - Someone needed a dork to search for...
- Inconsistent formatting
  - Sometimes they decide not to use any spacing at all
  - Sometimes they change spaces for tabs
- My personal favorite are the vulnerability patches fixing only the thing spelled out in the writeup
  - When CLEARLY the same problem exists on the same line, on the other side of an `||` conditional

I could go on and on. It is pretty clear that no one competent is doing code review here. Perhaps no one at all. I have an image in my head. 10 years ago, an employee distributed VMWare images to develop on. That guy quit and everyone has been using the same VM ever since. No one there has ever heard of git, so they made a shared folder where everyone edits the same file at the same time. Code review occurs in meeting room of 5 clueless people looking at code on a projector screen.

Enough ranting. We're here to build some GPL sources and hopefully preserve or repurpose some old hardware.
