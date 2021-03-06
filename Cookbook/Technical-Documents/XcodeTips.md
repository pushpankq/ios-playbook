# Xcode tips and tricks

## Keybindings
As we spend so much time in our ~~favourite~~ editor, it's essential to learn keybindings to decrease content switching and maintain focus.

#### Navigating and finding things
As long as the project is indexed, Xcode lets you jump to anything from anywhere.

- **Cmd-Shift-0** to open the Apple documentation window.
- **Cmd-Ctrl-[** or **]** to cycle between targets.
- In Project Navigator (**Cmd-1**), **Opt-Left** folds the structure and **Opt-Right** unfolds.

##### Find symbols, files or text
- **Cmd-Shift-O** and type anything to quickly jump to files or symbols. **Enter** to open, **Opt-Enter** to open in the assistant editor.
- **Cmd-Ctrl-J** (as in “jump”) or **Cmd-Ctrl-Click** on a symbol to go to definition. Add **Opt** to open in assistant editor.
- Right click and “Find Selected Symbol in Workspace” to show in the search pane where the symbol under cursor is used.
- **Cmd-Shift-F** to search in workspace (the whole project). You'll want to select Text on top because searching for references is tricky manually, they require specific syntax.
- **Cmd-Opt-J** and type to filter files by name in the Project Navigator (or tests in the Test Navigator). Useful if you want to find multiple files with the same name, or can't quite remember how a file is called.

As “Jump to definition” is used a lot, it can be configured to only require **Cmd-Click** for immediate jump without displaying the menu:
![Cmd-Click Navigation](./Assets/cmd-ctrl-click-navigation.png)

##### File level navigation
- **Cmd-Shift-J** to show the open file in the Project Navigator.
- **Cmd-Ctrl-Left** or **Right** in a file to navigate between recently open files.
- **Ctrl-6** in a file to open the list of methods and variables. You can then type to apply a filter and hit Enter to jump to the selected completion.
- **Cmd-Opt-Shift-Left** in a file to fold all methods and **Cmd-Opt-Shift-Right** to unfold. May be useful to get a quick overview.

#### Making space for your code
Use these keybindings to give your code more space, and show the panes again quickly when needed. You can keep the panes nice and wide this way, and just hide them when you're not using them, rather than keep them thin and see them all the time.

- **Cmd-Shift-Y** to show or hide the bottom drawer (locals and console output).
- **Cmd-0** and **Cmd-Opt-0** to fold and unfold the left and right panes, respectively.
- **Cmd-Enter** to close the assistant editor and leave only the main editor pane.

You can also pre-set complete layouts and switch between them. Go to *Settings > Behaviors > Edit Behaviors* where you can add custom behaviors telling which pane and editors to show and hide, then assign a custom shortcut on that, and now you can switch for example to “max space to edit my code with only main editor without the project navigator nor inspector nor bottom drawer” with just one shortcut.

Pictured: **Cmd-Ctrl-Opt-D** to switch to a layout set up for debug.
![Custom Behaviors](./Assets/xcode-custom-behaviors.png)

#### Working with the assistant editor
If your screen is big enough or your font is small enough to allow using two editors side by side, this is a very efficient way to work.

- **Cmd-Enter** to close the assistant editor and leave only the main editor pane.
- **Ctrl-`** to switch between the main editor and the assistant editor.

In general, you can add **Opt** to any command that opens or navigates to a file to open it in the assistant editor instead. Here are a few to get you started:

- **Cmd-Ctrl-Opt-J** or **Cmd-Ctrl-Opt-Click** to open the definition for symbol under cursor in the assistant editor.
- **Opt-Click** in workspace search (**Cmd-Shift-F**).
- **Opt-Enter** in Open Quickly (**Cmd-Shift-O**).

Xcode can also be configured to open commands invoked with **Opt** in a tab instead:
![Optional Navigation](./Assets/optional-navigation-tab.png)

#### Navigating with a floating panel
Adding **Opt+Shift** to the shortcuts will show a floating panel allowing you to choose where you want to open the file: replace the content of an existing tab, open in a new tab, in the assistant, in a new window...
![Floating Panel Navigation](./Assets/floating-panel-navigation.png)

- **Cmd-Shift-O** + type anything + **Opt-Shift-Enter** will show you the panel to choose where to open the selected file.
- **Cmd-Ctrl-Opt-Shift-J** (yeah, that's a lot of modifier keys, but once you're used to **Cmd-Ctrl-J** and you know that adding **Opt-Shift** adds that extra behavior, you get used to it) will show the panel to choose where to open the symbol definition.
- **Cmd-Ctrl-Opt-Shift-Click** on the symbol will do the same.
- **Cmd-J** will bring up the same magic floating panel allowing you to move focus to wherever you want.

## Configuring file counterparts

Xcode has the concept of file "counterparts". In Objective-C these are configured to be the `.h` and `.m` files with the same name and you can use **Ctrl+Cmd+Up** and **Ctrl+Cmd+Down** to switch between them. They also appear when you are working with the assitant editor in the `Counterparts` entry of the context menu. 

These counterparts are configurable, and to be be more useful for working in our code base you can configure a suggested set by typing the following at a command line prompt:


```
defaults write com.apple.dt.Xcode IDEAdditionalCounterpartSuffixes -array-add "ViewModel" "Renderer" "FlowController" "Builder"
```

This will make it very easy to switch between the view mode, renderer, flow controller and builder for a given feature. You can obviously add extra suffixes such as `RendererSnapshotTests`.

Now these are the files that appear in the counterparts context menu in the assistant editor:

![Counterparts Context Menu](./Assets/counterparts.png)

And **Ctrl+Cmd+Up** and **Ctrl+Cmd+Down** will cycle between these files.

You can reset these suffixes to the default values with:
```
defaults delete com.apple.dt.Xcode IDEAdditionalCounterpartSuffixes
```

Source: https://gist.github.com/danielmartin/8411c303e5c8702c19c65950b49635b8

## Testing
- Everyone knows you hit **Cmd-U** to start testing… right?
- **Cmd-Ctrl-Opt-G** “Test again” reruns the last test you've run.

It's annoying when your tests fail, but even more annoying to try and compare the failed snapshots in */tmp/SnapshotFailures/*. Thankfully, Xcode can help you out.

After you've run the tests, in the Test Navigator (**Cmd-9**) open the latest test report, switch to Failed tests and you'll find XCTAttachments for before, after and diff attached to the failed test:

![Test Navigator Attachments](./Assets/test-navigator-attachments.png)

You can view the same report on CircleCI by navigating to the job's Artifacts tab and opening `Users/distiller/project/output/scan/html/report.html`.

## Breakpoints with auto-continue
It's possible (and convenient) to use printing or speaking breakpoints instead of adding print statements and having to recompile your code.

- Put a breakpoint in the place you want to print a variable.
- Right click on the breakpoint to edit it.
- There you can ask that the breakpoint prints a variable or expression, or execute a debugger command, every time it's hit.
- Then you can check the checkbox to auto-continue, which means the breakpoint will not actually break and stop your code, but just print the thing you asked or execute the debugger command you told it to without stopping.
- You can also ask the breakpoint to speak a sentence or play a system sound, which is actually very useful if you just want to know when or how often the code is hit. For example, if you want to ensure some code is only called once when you tap the screen, having a breakpoint that stops the app in the simulator and makes you jump into Xcode is not going to help, but having a breakpoint playing a sound you can easily know when it's hit is. Or if you want to know when a network response is received, or a timer is triggered, just by hearing a sound without breaking and interrupting your app.

## Errors when pulling code/switching branches
Various errors can occur when pulling code/switching branches. 
Typically all that is needed is running `bundle exec pod install` and closing
and reopening the project in Xcode. The following directions are
intended to be a more thorough reset to deal with bigger changes.

- Clean Build: Product > Clean (shortcut: Cmd-Shift-K)
- Close Xcode
- Update bundle: run `bundle install`
- Clean install pods: run `rm -rf Pods/ && bundle exec pod install --repo-update`
- Clean DerivedData: can be found at
`~/Library/Developer/Xcode/DerivedData` and/or `babylon-ios/DerivedData`
- Reopen project in Xcode (or run `xed .` from the project directory)

## Project file conflicts
When merging develop into your branch, if the project file has been changed in both locations, there may be a conflict. This file can be very difficult to merge manually. You can use the mergepbx tool to handle these conflicts easier.

- The repo can be found here: [link](https://github.com/simonwagner/mergepbx)
- You can install with brew: `brew install mergepbx`
- After it is installed, you can add these lines to your `~/.gitconfig`:
```
#driver for merging Xcode project files
[mergetool "mergepbx"]
	cmd = mergepbx "$BASE" "$LOCAL" "$REMOTE" -o "$MERGED"
```
- Then whenever you run into a project file conflict you can resolve it with:
`git mergetool --tool=mergepbx [Project File]`
e.g: `git mergetool --tool=mergepbx Babylon.xcodeproj/project.pbxproj`

## Console logs
- To disable autolayout warnings add `-_UIConstraintBasedLayoutLogUnsatisfiable NO` to the scheme launch arguments
- To disable other system activity logs add `OS_ACTIVITY_MODE` environment variable with `disable` value
