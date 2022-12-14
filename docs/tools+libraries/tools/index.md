---
title: Tools
redirect_from:
  - /Command-Line_Tools/
---

There are several command line utilities included in Mono that constitute the development toolchain. These are key components which you'll want to familiarize yourself with:

## Main tools

-   [mono](/docs/advanced/runtime/) is the Mono runtime and Just In Time compiler (JIT)
-   [mcs](/docs/about-mono/languages/csharp/), Mono's C# compiler
-   [gacutil](/docs/advanced/assemblies-and-the-gac/) is a tool used by developers to install versioned assemblies into the system Global Assembly Cache (GAC) to become part of the assemblies that are available for all applications at runtime.
-   [xsp](/docs/web/aspnet/), Mono's stand alone ASP.NET web services and web application server
-   mono-config - Mono runtime file format configuration
-   [mkbundle](/docs/tools+libraries/tools/mkbundle) - Create and cross-compile self-contained executables with no external dependencies.

## Miscellaneous tools

-   ilasm: IL assembler
-   monodis: IL disassembler and metadata explorer
-   ikdasm: IL disassembler, more robust, but lacks some of the metadata features of monodis.
-   monop: command line class display (Try: `monop System.String`)
-   sn: StrongName (code signing) utility for signing IL assemblies
-   sqlsharp: command line SQL client for most of the the System.Data based DB Connectors
-   [Gendarme](/docs/tools+libraries/tools/gendarme/): Rule-based assembly analyser for developers.
-   [Monoxide](/archived/monoxide): Interactive assembly analyser.
-   [csharp](/docs/tools+libraries/tools/repl/): An interactive shell (REPL) for C#.

## XML & Web Service

(see man pages and Microsoft docs)

-   xsd
-   genxs
-   dtd2xsd
-   wsdl & wsdl2
-   disco
-   soapsuds

## Project conversion & deployment

-   prj2make: Convert Microsoft Visual Studio 2002 and 2003 solution and project files to makefiles to ease the transition to Mono
-   mkbundle: Packages an exe and all assemblies with libmono into a single binary package.
-   macpak: Packages mono and assemblies together so they may run on macOS

## Security

-   caspol: Tool to modify Code Access Security [CAS](/docs/advanced/cas/) policies.
-   cert2spc: Transform a set of X.509 certificates and CRLs into an Authenticode(tm) "Software Publisher Certificate"
-   certmgr: Manage X.509 certificates and CRL from stores.
-   chktrust: Verify if an PE executable has a valid Authenticode(tm) signature
-   makecert: X.509 Certificate Builder, e.g. SSL server/client, Authenticode(tm)
-   mozroots: Download and import trusted root certificates from Mozilla's LXR.
-   permview: Permission Viewer for .NET assemblies.
-   secutil: Extract strongname and X509 certificates from assemblies.
-   setreg: Change settings for public key cryptography.
-   signcode: Sign assemblies and PE files using Authenticode(tm).

## Help & Documentation

-   mod: a command line monodoc reader
-   [assembler](/docs/tools+libraries/tools/mdassembler/): A tool for assembling documentation generated by [monodocer](/docs/tools+libraries/tools/monodocer/).
-   [monodocer](/docs/tools+libraries/tools/monodocer/): Generates documentation out of assemblies.
