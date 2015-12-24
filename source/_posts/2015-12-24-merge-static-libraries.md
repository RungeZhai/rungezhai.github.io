---
layout: post
title: "Merge Static Libraries"
date: 2015-12-24 15:29:24 +0800
comments: true
categories: Xcode
---


Yesterday I was asked to merge multiple Libraries into one. This is a whole new area! Here is the background: Members of our department are developing three submodules of the main iOS application, and each submodule is a subproject of main application and output a `.a` static library file and a `.bundle` resource file. And now the head of our team wants to merge all the libraries into one and only expose this only one `.a` and `.bundle` file to the main application. **`"And with minimum modification"`**, said my boss.

<!--more-->

## What I Did

So I created a new project and make the current three subproject as subprojects of the new project. The new project has two target a `.a` library and  `.bundle` resource. Both targets' output are output of the merging of the three submodule accordingly.

The bundle file is just something like a folder, so merging is simply copying three folder into another. As of Xcode, go to `Build Phases -> Copy Bundle Resources` and add subproject's .bundle target.

The merging of `.a` file are simple too, it's only that I have never touched this area before.
Add the following line to `Build Phases -> Click "+" button on the topleft -> New Run Script Phase` (Say we want merge `A.a`, `B.a`, `C.a` into `AllInOne.a`)

```bash
libtool -static -o ${BUILT_PRODUCTS_DIR}/AllInOne.a ${BUILT_PRODUCTS_DIR}/A.a ${BUILT_PRODUCTS_DIR}/B.a ${BUILT_PRODUCTS_DIR}/C.a
```

> As of `Minimum Modification`, the only one is accessing bundle resources. Previously we get bundle from `MainBundle`, but now we have to get the merged bundle first and then get the bundle we want in the merged bundle, which, I think, is qualified as `Minimum Modification`.

## What I Learned

The solution above is not rigorous. One of the libraries might not support architectures that others do, so the might-not-supported architectures portion of the libraries should not be merged. The following command can figure out the architectures for which the library is being built:

```bash
LIPO_ARCH=$(lipo -info ${BUILT_PRODUCTS_DIR}/${EXECUTABLE_NAME} | awk 'END{ print $NF }')
```

The following command creates a thin version of the library with supported architectures in `LIPO_ARCH`:

```bash
lipo -thin ${LIPO_ARCH} ${LIBRARY_NAME} -output ${LIBRARY_NAME}
```

> A flat library is a library that support multiple architectures.

<br>

Reference

[Linking 2 static libs into 1 for iOS](http://stackoverflow.com/questions/9531014/linking-2-static-libs-into-1-for-ios/21225126#21225126)

[MacOSX: Trimming fat from Mach-O fat files](http://www.theconsultant.net/2005/09/macosx-operating-on-fat-files/)