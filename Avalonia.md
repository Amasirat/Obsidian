A [[CSharp(.NET)]] crossplatform GUI framework designed to be the cross-platform successor to [[WPF]].

It is a framework that incorporates an [[MVVM]] design pattern but can be used as an [[MVC]] fashion as well.
# Getting Started

## XAML

XAML is a mark up language designed to indicate the layout of the graphical application. AXAML is the format that avalonia uses.

XAML is built by a series of controls (which are similar to HTML tags).

Window is a control that represents the application window. 

Window only takes one other control in its contents, therefore it is common practice to write a StackPanel control to stack multiple contents inside the application view.

