# alfred-iterm
Integrate iTerm with Alfred

## Guide

1. Open `Preferences` - `Features` - `Terminal`
2. Set `Application` to `Custom`
3. Paste the following script: 

```applescript
on alfred_script(q)
	tell application "iTerm" to launch
	tell application "iTerm" to activate

	# Count the number of windows active
	set windowCount to 0
	tell application "iTerm"
		repeat with aWindow in windows
			set windowCount to windowCount + 1
			#display dialog "windowCount: " & windowCount
		end repeat
	end tell

	# If there are zero windows, create a new one
	if windowCount = 0 then
		tell application "iTerm"
			create window with default profile
			set windowCount to 1
			#display dialog "windowCount: " & windowCount
		end tell
	end if

	#display dialog "windowCount: " & windowCount

	# If there is one window
	if windowCount > 0 then
		tell application "iTerm"
			tell the current window
				#set alfredTab to create tab with profile "Default"
				#tell the alfredTab
				tell the current session
					write text q
				end tell
				#end tell
			end tell
		end tell
	end if
end alfred_script
```

## Usage

1. Invoke Alfred (`Alt+Space`)
2. Type `>` followed by terminal command, such as `ls -al`
