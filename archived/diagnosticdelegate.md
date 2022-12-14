---
title: "Diagnostic:Delegate"
lastmodified: '2005-10-29'
redirect_from:
  - /Diagnostic%3ADelegate/
---

Diagnostic:Delegate
===================

Your Delegate Has Been Garbage Collected
----------------------------------------

The program that you are running passed a delegate into the unmanaged world but failed to keep a reference to this delegate in the managed world, so the delegate was garbage collected.

This means that the the delegate is no longer valid, but the unmanaged library or code tried to call it.

To fix this problem make sure that you keep a handle to the delegate that you are passing into the unmanaged world.

This happens since Mono has no visibility into the unmanaged world and it does not know that the unmanaged world has kept a reference to your delegate. As far as the Mono runtime is concerned the delegate must be garbage collected.

For more information see: [http://blogs.msdn.com/dangriff/archive/2003/11/19/51709.aspx](http://blogs.msdn.com/dangriff/archive/2003/11/19/51709.aspx)
