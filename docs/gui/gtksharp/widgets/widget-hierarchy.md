---
title: "GtkSharp: Widget Hierarchy"
redirect_from:
  - /GtkSharp%3A_Widget_Hierarchy/
---

This is the class hierarchy tree used to implement widgets, with links to each class's detailed documentation.

    Gtk.Object  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Object))
     + Gtk.Widget  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Widget))
     | + Gtk.Misc  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Misc))
     | | + Gtk.Label  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Label))
     | | | - Gtk.AccelLabel  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.AccelLabel))
     | | + Gtk.Arrow  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Arrow))
     | | + Gtk.Image  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Image))
     | + Gtk.Container  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Container))
     | | + Gtk.Bin  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Bin))
     | | | + Gtk.Alignment  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Alignment))
     | | | + Gtk.Frame  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Frame))
     | | | | Gtk.AspectFrame  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.AspectFrame))
     | | | + Gtk.Button  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Button))
     | | | | + Gtk.ToggleButton  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.ToggleButton))
     | | | | | - Gtk.CheckButton  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.CheckButton))
     | | | | |   - Gtk.RadioButton  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.RadioButton))
     | | | | + Gtk.OptionMenu  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.OptionMenu))
     | | | + Gtk.Item  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Item))
     | | | | + Gtk.MenuItem  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.MenuItem))
     | | | |   + Gtk.CheckMenuItem  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.CheckMenuItem))
     | | | |   | - Gtk.RadioMenuItem  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.RadioMenuItem))
     | | | |   + Gtk.ImageMenuItem  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.ImageMenuItem))
     | | | |   + Gtk.SeparatorMenuItem  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.SeparatorMenuItem))
     | | | |   + Gtk.TearoffMenuItem  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.TearoffMenuItem))
     | | | + Gtk.Window  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Window))
     | | | | + Gtk.Dialog  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Dialog))
     | | | | | - Gtk.ColorSelectionDialog  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.ColorSelectionDialog))
     | | | | | - Gtk.FileSelection  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.FileSelection))
     | | | | | - Gtk.FontSelectionDialog  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.FileSelectionDialog))
     | | | | | - Gtk.InputDialog  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.InputDialog))
     | | | | | - Gtk.MessageDialog  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.MessageDialog))
     | | | | + Gtk.Plug  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Plug))
     | | | + Gtk.EventBox  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.EventBox))
     | | | + Gtk.HandleBox  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.HandleBox))
     | | | + Gtk.ScrolledWindow  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.ScrolledWindow))
     | | | + Gtk.Viewport  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Viewport))
     | | + Gtk.Box  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Box))
     | | | + Gtk.ButtonBox  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.ButtonBox))
     | | | | - Gtk.HButtonBox  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.HButtonBox))
     | | | | - Gtk.VButtonBox  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.VButtonBox))
     | | | + Gtk.VBox  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.VBox))
     | | | | - Gtk.ColorSelection  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.ColorSelection))
     | | | | - Gtk.FontSelection  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.FontSelection))
     | | | | - Gtk.GammaCurve  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.GammaCurve))
     | | | + Gtk.HBox  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.HBox))
     | | |   - Gtk.Combo  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Combo))
     | | |   - Gtk.Statusbar  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Statusbar))
     | | + Gtk.Fixed  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Fixed))
     | | + Gtk.Paned  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Paned))
     | | | - Gtk.HPaned  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.HPaned))
     | | | - Gtk.VPaned  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.VPaned))
     | | + Gtk.Layout  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Layout))
     | | + Gtk.MenuShell  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.MenuShell))
     | | | - Gtk.MenuBar  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.MenuBar))
     | | | - Gtk.Menu  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Menu))
     | | + Gtk.Notebook  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Notebook))
     | | + Gtk.Socket  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Socket))
     | | + Gtk.Table  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Table))
     | | + Gtk.TextView  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.TextView))
     | | + Gtk.Toolbar  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Toolbar))
     | | + Gtk.TreeView  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.TreeView))
     | + Gtk.Calendar  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Calednar))
     | + Gtk.DrawingArea  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.DrawingArea))
     | | - Gtk.Curve  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Curve))
     | + Gtk.Editable  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Editable))
     | | - Gtk.Entry  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Entry))
     | |   - Gtk.SpinButton  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.SpinButton))
     | + Gtk.Ruler  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Ruler))
     | | - Gtk.HRuler  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.HRuler))
     | | - Gtk.VRuler  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.VRuler))
     | + Gtk.Range  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Range))
     | | + Gtk.Scale  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Scale))
     | | | - Gtk.HScale  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.HScale))
     | | | - Gtk.VScale  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.VScale))
     | | + Gtk.Scrollbar  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Scrollbar))
     | |   - Gtk.HScrollbar  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.HScrollbar))
     | |   - Gtk.VScrollbar  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.VScrollvar))
     | + Gtk.Separator  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Separator))
     | | - Gtk.HSeparator  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.HSeparator))
     | | - Gtk.VSeparator  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.VSeparator))
     | + Gtk.Invisible  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Invisible))
     | + Gtk.Preview  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Preview))
     | + Gtk.ProgressBar  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.ProgressBar))
     + Gtk.Adjustment  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Adjustment))
     + Gtk.CellRenderer  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.CellRenderer))
     | - Gtk.CellRendererPixbuf  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.CellRendererPixbuf))
     | - Gtk.CellRendererText  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.CellRendererText))
     | - Gtk.CellRendererToggle  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.CellRendererToggle))
     + Gtk.ItemFactory  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.ItemFactory))
     + Gtk.Tooltips  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.Tooltips))
     + Gtk.TreeViewColumn  ([Monodoc](http://docs.go-mono.com/index.aspx?link=T:Gtk.TreeViewColumn))
