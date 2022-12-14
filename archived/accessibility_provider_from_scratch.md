---
title: "Accessibility: Provider From Scratch"
lastmodified: '2008-11-21'
redirect_from:
  - /Accessibility%3A_Provider_From_Scratch/
---

Accessibility: Provider From Scratch
====================================

<table>
<col width="100%" />
<tbody>
<tr class="odd">
<td align="left"><h2>Table of contents</h2>
<ul>
<li><a href="#introduction">1 Introduction</a></li>
<li><a href="#quick-steps-list">2 Quick Steps List</a></li>
<li><a href="#generating-ui-automation-tree">3 Generating UI Automation Tree</a></li>
<li><a href="#implementing-automation-events">4 Implementing Automation Events</a></li>
</ul></td>
</tr>
</tbody>
</table>

Introduction
------------

Notice this page does not explain how to implement a custom provider for Windows Forms or Moonlight Controls, instead explains how to implement a provider for the mono-a11y implementation.

If you want to support accessibility using UIA in your custom controls then you should follow [Microsoft documentation](http://msdn.microsoft.com/en-us/library/ms747229.aspx):

This page is complementary to [Windows Forms Implementation](/Accessibility:_Winforms_Implementation).

Quick Steps List
----------------

Basically to implement a new mono-a11y provider you should use the following list of steps:

1.  Select your not-implemented provider. Read [Winform Control Status](/Accessibility:_Control_Status).
2.  Write a simple Winform Application that includes your selected Winform Control and run it in Windows Vista to examine it using [UISpy](http://msdn.microsoft.com/en-us/library/ms727247.aspx). (Note: UISpy isn't included in MS VS 2008, so you need to download "Windows Vista SDK" from MSDN)
3.  In UISpy look for the Automation Property labeled as "Control Type", then read the [documentation](http://msdn.microsoft.com/en-us/library/ms743581.aspx) associated.
4.  In your Control Type documentation read the table under the section "Required UI Automation Tree Structure", don't forget to verify UISpy information to match the implementation. If your provider supports navigation you must subclass FragmentRootControlProvider, otherwise you must subclass FragmentControlProvider. This is important to generate a valid [UI Automation Tree](http://msdn.microsoft.com/en-us/library/ms741931.aspx). The difference between FragmentRootControlProvider and FragmentControlProvider is that FragmentRootControlProvider should add/remove children and generate events to keep navigation updated.
5.  Both specializations (either subclassing from FragmentRootControlProvider or FragmentControlProvider) must support the following:
    1.  Automation Properties, defined in the table "Required UI Automation Properties", and
    2.  Provider Interfaces, defined in the table "Required UI Automation Control Patterns",

6.  In your Provider Interface Specialization you have to generate your Automation Events and Automation Property Event.

Generating UI Automation Tree
-----------------------------

Two methods defined in FragmentRootControlProvider are used to initialize and finalize children: InitializeChildControlStructure and FinalizeChildControlStructure. By default FragmentRootControlProvider uses Control.ControlAdded and Control.ControlRemoved events to do so, if your provider's Control does not use those events then, you need to override the methods to generate the valid tree by using the following protected methods:

-   OnNavigationChildAdded: Adds child to. Use true as first argument if you want to generate your Automation Structure Changed Child Added Event.
-   OnNavigationChildRemoved: Removes child. Use true as first argument if you want to generate your Automation Structure Changed Child Removed Event.
-   OnNavigationChildrenCleared: Clears children. Use true as argument if you want to generate your Automation Structure Changed Children Removed Event.

Also, you can use the following public methods:

-   AddChildProvider
-   RemoveChildProvider

both methods are wrappers to OnNavigationChildAdded and OnNavigationChildRemoved.

If you override InitializeChildControlStructure don't forget to add your already added children.

Implementing Automation Events
------------------------------

There are 3 basic types of Automation Events, read [UI Automation Events Overview](http://msdn.microsoft.com/en-us/library/ms748252.aspx) for detailed explanation:

-   Structure Changed Events. Generated when calling FragmentRootControlProvider methods: OnNavigationChildAdded, OnNavigationChildRemoved, OnNavigationChildrenCleared, AddChildProvider and RemoveChildProvider.
-   Automation Events: Generated by your Control depending on internal states.
-   Automation Property Events: Generated by your Control depending on internal states.

There are Automation Events and Automation Property Events always generated (does not matter the specialized provider), those are defined in the [AutomationElementIdentifiers](http://msdn.microsoft.com/en-us/library/system.windows.automation.automationelementidentifiers.aspx), most of the events are already defined in the base class of FragmentRootControlProvider and FragmentControlProvider: SimpleControlProvider, however depending on your Control and Pattern supported you may need to enable or disable it. For example to enable IsEnabledProperty you should do the following:

``` csharp
SetEvent (ProviderEventType.AutomationElementIsEnabledProperty, new AutomationIsEnabledPropertyEvent (this));
```

and to disable it

``` csharp
SetEvent (ProviderEventType.AutomationElementIsEnabledProperty, null);
```

To set AutomationElementIdentifiers you should override InitializeEvents, however don't forget to call base implementation.

However, you must implement your Automation Events and Automation Property Events for each provider specialized, depending on the pattern. For example, if your provider support Range Value Pattern by specializing IRangeValueProvider, then you should generate the events for each of their properties (see [Range Value Provider specialization](http://anonsvn.mono-project.com/viewvc/trunk/uia2atk/UIAutomationWinforms/UIAutomationWinforms/Mono.UIAutomation.Winforms.Behaviors/ScrollBar/RangeValueProviderBehavior.cs?view=markup) in ScrollBar for a working example).

To implement your Automation Events you have to either subclass from BaseAutomationEvent if your event is Automation Event and subclass from BaseAutomationPropertyEvent if your event is Automation Property Event. In both classes you have to override Connect and Disconnect methods to associate/disassociate your Control events that should generate the Automation Event. When your Control Event is generated you have to either call BaseAutomationPropertyEvent.RaiseAutomationPropertyChangedEvent if you are subclassing from BaseAutomationPropertyEvent or BaseAutomationEvent.RaiseAutomationEvent if you are subclass from BaseAutomationEvent.

For example [RangeValuePattern.ValueProperty](http://anonsvn.mono-project.com/viewvc/trunk/uia2atk/UIAutomationWinforms/UIAutomationWinforms/Mono.UIAutomation.Winforms.Events/ScrollBar/RangeValuePatternValueEvent.cs?view=markup) in ScrollBar implementation subclasses BaseAutomationPropertyEvent, and [InvokePattern.InvokedEvent](http://anonsvn.mono-project.com/viewvc/trunk/uia2atk/UIAutomationWinforms/UIAutomationWinforms/Mono.UIAutomation.Winforms.Events/ScrollBar/ButtonInvokePatternInvokedEvent.cs?view=markup) in ScrollBar subclasses BaseAutomationEvent.
