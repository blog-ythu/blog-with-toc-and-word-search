---
layout: post
comments: true
title: "MATLAB/C++ Mixed Programming: call MATLAB directly in VC++"
excerpt: "Another method for MATLAB/C++ Mixed Programming by calling MATLAB directly in VC++."
date: 2015-12-24 11:00:00
category: tutorials
tags: [tips]
published: True
mathjax: true
---

<style>
p {
  text-align: justify;
}
</style>

There are two ways in VC++ to call MATLAB:

1. distribute MATLAB code into MATLAB independent C++ shared library. Refer to the [previous blog](http://herohuyongtao.blogspot.hk/2012/04/matlabc-cmatlab.html) for details.

2. call MATLAB directly in VC++.

---
This blog will focus on the second approach (also the easier one).

### Pre-work:
- Install MATLAB: unlike the first method, make sure you have the whole MATLAB application installed in order to call MATLAB directly in VC++ (only MATLAB Compiler Runtime is needed for the first method). We will take **MATLAB-x64-R2013a** as an example (installed under `C:\Program Files\MATLAB\R2013a`).
- **Platform consistence:** ***the platforms of MATLAB and VC++ compile platform must be the same***, i.e. Win32/x86 VC\++ compile platform can only use x86 MATLAB and x64 VC\++ compile platform can only use x64 MATLAB.
- Example: take the following example `myadd2.m` (assume under `C:\`):

	```matlab
	function [y, z] = myadd2(a, b)
	% dummy function, just to demonstrate the idea
	y = a+b;
	z = a+2*b;
	disp('Output from MATLAB.');
	end
	```

### VC setup:
1. Add MATLAB include folder to project `Include` files: i.e. `C:\Program Files\MATLAB\R2013a\extern\include`
2. Add MATLAB library folder to project `Library` files: i.e. `C:\Program Files\MATLAB\R2013a\extern\lib\win64\microsoft`
3. Add relevant libraries to `Linker>Input>Additional Dependencies`: `libmx.lib libeng.lib`
4. Add MATLAB Compiler Runtime binary folder to project `Debugging>Environment`: i.e. `PATH=%PATH%;$(ProjectDir)\dlls_x64;C:\Program Files\MATLAB\R2013a\runtime\win64;C:\Program Files\MATLAB\R2013a\bin\win64`
5. Add header to main code: `#include "engine.h"`
2. Call in VC++: refer to [`VC_call_MATLAB_directly.cpp`](https://bitbucket.org/herohuyongtao/files/src/tip/files/cplusplus/VC_call_MATLAB_directly.cpp) for details.

	In this cpp file, there are also ways to handle data passing of different types between VC++ and MATLAB (see the `SHOW_EXTEND_INFO` part), which includes:
    - `double` (or `int`/`float`/…)
    - `string`
    - matrix
    - gray-scale image
    - RGB image

### Other Remarks:
- Try to close the previous MATLAB window first (extra command window) if stuck at `engOpen(NULL)`.
