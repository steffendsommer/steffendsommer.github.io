---
layout: post
title:  "A list of useful Xcode plugins"
date:   2015-08-27 18:45:12
group: blog
summary: Because I like to get inspired by how others work, I decided to do a small list of the Xcode plugins I use daily. I use Alcatraz for handling my Xcode plugins. It works fairly well, except sometimes when Apple updates Xcode. The list of plugins in this post has been used with Xcode 6.4.

---

![Vacation on Bali](/assets/posts/bali.jpg){:height="300px"}

After three weeks of vacation on Bali, it feels really good to be back.

Because I like to get inspired by how others work, I decided to do a small list of the Xcode plugins I use daily. I use [Alcatraz](http://alcatraz.io){:target="_blank"} for handling my Xcode plugins. It works fairly well, except sometimes when Apple updates Xcode. The list of plugins in this post has been used with Xcode 6.4. Feel free to leave a comment if you use a plugin I did not mention.

# [AdjustFontSize](https://github.com/zats/AdjustFontSize-Xcode-Plugin){:target="_blank"}
Handy little plugin for changing font size when showing code to others or doing presentations. Just press `⌘ +` or `⌘ -` to adjust the font size of your editor.

# [ATProperty](https://github.com/Draveness/ATProperty){:target="_blank"}
Call me lazy, but I tend to get pretty tired of declaring properties. I tried using my snippet manager, but I actually prefer this small plugin. Just type e.g. `@t` to get a `strong` property.

# [AutoImporter](https://github.com/citrusbyte/Auto-Importer-for-Xcode){:target="_blank"}
After using [AppCode](https://www.jetbrains.com/objc/){:target="_blank"} for quite some time, I certainly missed how easy that IDE makes it to import headers. This plugin helps you importing header files by pressing a shortcut and entering the filename of the header you want to import (with autocompletion). This is indeed handy if you're in the middle of your source file and you want to avoid having to scroll to the top of the file and then back to where you were coding.

# [Backlight](https://github.com/limejelly/Backlight-for-XCode){:target="_blank"}
Highlights the current line you're editing.

# [CocoaPods](https://github.com/kattrali/cocoapods-xcode-plugin){:target="_blank"}
Using [CocoaPods](https://cocoapods.org){:target="_blank"}, this plugin gives you some shortcuts to the most common operations.

# [FuzzyAutocomplete](https://github.com/FuzzyAutocomplete/FuzzyAutocompletePlugin){:target="_blank"}
This is probably one of my most favourite plugins for Xcode. It takes a small amount a time to get used to, but once you do, it's really useful. FuzzyAutocomplete makes your autocompletion less strict, meaning that you do not have to type in the correct letters in the correct order in order to get autocompletion suggestions.

# [GitDiff](https://github.com/johnno1962/GitDiff){:target="_blank"}
Highlights any changes against the git repository.

# [Jumper](https://github.com/deszip/Jumper){:target="_blank"}
Let's you jump 10 lines up/down in your editor using shortcuts.

# [KSImageNamed](https://github.com/ksuther/KSImageNamed-Xcode){:target="_blank"}
Autocompletes calls to `imageNamed:`.

# [OMQuickHelp](https://github.com/omz/Dash-Plugin-for-Xcode){:target="_blank"}
I use [Dash](https://kapeli.com/dash){:target="_blank"} for viewing documentation and this plugin makes sure that documentation fired from Xcode is opened in Dash.

# [QuickLocalization](https://github.com/nanaimostudio/Xcode-Quick-Localization){:target="_blank"}
A plugin to make it easier to go from `@"content"` to `NSLocalizedString(@"content", @"content")`.

# [SCXcodeTabSwitcher](https://github.com/stefanceriu/SCXcodeTabSwitcher){:target="_blank"}
Makes it possible to change your Xcode tabs by pressing `⌘ + 1..9`.

# [VVDocumenter-Xcode](https://github.com/onevcat/VVDocumenter-Xcode){:target="_blank"}
After having declared a property or method, this plugin generates your documentation by just writing `///` above the declaration. The plugin takes any arguments and return value into account, making it easy for you to fill in the details.

# [XToDo](https://github.com/trawor/XToDo){:target="_blank"}
Gives you a list of all the `TODO`, `FIXME` and more you've added in your code.

# Am I missing any plugins?
That's it. These are the plugins I currently use in Xcode 6.4. Please feel free to pitch in with any other useful plugins I might have missed.