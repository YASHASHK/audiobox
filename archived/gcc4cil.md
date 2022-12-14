---
title: "Gcc4cil"
lastmodified: '2007-03-15'
redirect_from:
  - /Gcc4cil/
---

Gcc4cil
=======

<table>
<col width="100%" />
<tbody>
<tr class="odd">
<td align="left"><h2>Table of contents</h2>
<ul>
<li><a href="#the-gcc4cil-project">1 The gcc4cil project</a></li>
<li><a href="#the-gcc4cil-linker">2 The gcc4cil linker</a>
<ul>
<li><a href="#what-the-linker-adds-to-the-merge-tool">2.1 What the linker adds to the merge tool</a></li>
<li><a href="#how-the-linker-handles-external-symbols">2.2 How the linker handles external symbols</a></li>
</ul></li>
</ul></td>
</tr>
</tbody>
</table>

The gcc4cil project
===================

A team in a research lab at [[ST Microelectronics](http://www.st.com)] added a CIL backend to gcc (incidentally, they published their results in the same summer in which a Google SOC [[project](/archived/summer2006/#gcc-cil-backend)] was attempting the same thing).

The project has then been accepted as an official gcc branch, so the code lives in the gcc repository ("svn co [svn://gcc.gnu.org/svn/gcc/branches/st/cli](svn://gcc.gnu.org/svn/gcc/branches/st/cli)").

A key point of gcc4cil is that it is only a *compiler*: it provides no standard C library, neitner it makes your life easy if you want to use other external C libraries in binary form.

To solve the "C library" problem, there are two possible approches:

1.  provide a managed implementation of it, or
2.  let the generated code use the native C library in the underlying system.

Approach 1 has the following disadvantages:

-   it takes a *lot* of work to properly reimplement a complete C standard library, and
-   if the compiled program interacts also with other native libraries (each non trivial program does so), it is a nightmare to assure ABI compatibility between the managed C standard library and the native ones (think passing "FILE\*" streams around...).

So, even if (to be fair) there are also advantages in approach 1, we decide to support approach 2. In doing so, we handle the standard C library as any other native library, and instrument the CIL code generated by gcc so that it p/invokes into the correct lib.

This instrumentation is done in the linking stage.

The gcc4cil linker
==================

The gcc4cil compilation pipeline is just the gcc one; this means that gcc4cil produces one "assembler" file (in IL syntax) for each C source file, separately compiles each file using "ilasm", and then needs to link them.

This linking step, in the approach 2 described above, has two goals:

-   resolve managed symbols (symbols defined in other C source files in the current program), and
-   link to unmanaged symbols implemented in native libraries (so thet the standard C library is just "yet another library" in the linking stage).

The linker is based on [Cecil](/Cecil). We had a prototype implementation from Wang Yi (done during Google SOC 2006), but the current implementation (by Massimiliaho Mantione) is based on the Cecil "merge" tool by Alex Prudkiy, and lives in the Cecil svn module under the "gcc4cil" directory. The rationale for reusing the merge tool is that, even if most of the work of the linker when it operates on files generated by the current gcc4cil is highly specialized and has little to do with merging general .NET assemblies. It could happen in the future that gcc4cil will support extensions to produce "proper" .net classes. At that point the linker should evolve into a generalized merge tool, like the one in the Cecil source tree. Since we already have such a tool, and its functionality already partially overlaps with the linker needs, extending it is the way to go.

What the linker adds to the merge tool
--------------------------------------

While merging the assemblies generated by gcc4cil, the linker has these additional tasks:

-   Keep a symbol table of all *internal* symbols (global variables and functions generated by gcc4cil which can be referenced from assemblies that participate in the linking process). These are simply collected during the first pass of the merge tool.
-   Keep a table of all *external* symbols (all the accessible symbols in the native libraries involved in the linking). These are collected upfront, calling "nm -D" on each library.
-   Actually modify the code so that is uses the proper symbols. When gcc4cil compiler refers to a symbol that the linker will have to resolve, it pretends the symbol belongs to a fictious "ExternalAssembly" assembly. Therefore, the linker has two tasks:
    -   discard all the memberrefs referring to "ExternalAssembly" (the members will be directly contained in the produced assembly), and
    -   modify the CIL code so that it refers to the proper members (this is the actual "relolve" phase in the linker), which in turn can be
        -   in the internal symbol table, or
        -   in the external one.

Of course, every symbol belonging to the fictious "ExternalAssembly" and not found in any of the symbol tables will result in a linker error.

How the linker handles external symbols
---------------------------------------

Symbols that are functions (seen as static methods) are converted to the appropriate p/invoke call to the external function.

For symbols that are global variables we are still building a solution.
