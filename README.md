# Lua COM interface for Microsoft Speech API

Easy text-to-speech in Lua with MS SAPI ( https://en.wikipedia.org/wiki/Microsoft_Speech_API ).

### Dependencies
LuaCOM ( https://github.com/davidm/luacom )

### Note for use on non-Microsoft systems with Wine
SAPI speech functionality depends on the Microsoft Speech API, which is not included by default in Wine, and SpeechSDK51.exe must be separately installed.
You can download it from https://download.microsoft.com/download/B/4/3/B4314928-7B71-4336-9DE7-6FA4CF00B7B3/SpeechSDK51.exe

# API
| Function | Arguments | Returns | Description |
| --- | --- | --- | --- |
| say | *string*&nbsp;message | | Speak the given string. |
| skip_sentence | | | Stop speaking the current line and move to the next line in the SAPI buffer. |
| skip_all | | | Stop speaking and clear the SAPI buffer. |
| set_voice_by_number | *int*&nbsp;SAPI_index, [*bool*&nbsp;quietly] | *int*&nbsp;SAPI_index, *string*&nbsp;SAPI_ID  | Choose the SAPI voice indexed from 1 to n |
| set_voice_by_id | *string*&nbsp;SAPI_ID, [*bool*&nbsp;quietly] | *int*&nbsp;SAPI_index, *string*&nbsp;SAPI_ID | Choose the SAPI voice by its ID. |
| set_rate | *int*&nbsp;rate, [*bool*&nbsp;quietly] | *int*&nbsp;rate | Set the speech rate. |
| slower | [*bool*&nbsp;quietly] | *int*&nbsp;rate | Increment the speech rate. |
| faster | [*bool*&nbsp;quietly] | *int*&nbsp;rate | Decrement the speech rate. |
| set_filtering_level | *int*&nbsp;level, [*bool*&nbsp;quietly] | *int*&nbsp;level | Set the symbol filtering level, 1 being least and 3 being most filtering. |
| get_voice_id | | *string*&nbsp;SAPI_ID | Return the SAPI voice ID string. |
| get_rate | | *int*&nbsp;rate | Return the speech rate number. |
| get_filtering_level | | *int*&nbsp;level | Return the filtering level number. |
| say_current_voice | | | Speaks the current voice index and ID. |
| say_current_rate | | | Speaks the current speech rate. |
| say_current_filtering_level | | | Speaks the current symbol filtering level and description. |
| list_voices | | | Speaks all of the available voices, index and ID. |
| list_filtering_levels | | | Speaks all of the filtering levels, index and description. |
| mute | [*bool*&nbsp;quietly] | | Disables speech. |
| unmute | [*bool*&nbsp;quietly] | | Re-enables speech. |
| speech_demo | | | Speaks a set of demonstration sentences. |
| print_spoken | | | Print spoken lines to the screen to aid debugging. |

| Variables | Description |
| --- | --- |
| replacements | Table of tables in the form {*string*&nbsp;pattern,&nbsp;*string*&nbsp;replacement} for filtering level 3 via string.gsub. |

# Usage Example
```lua
sapi_interface = require "sapi_interface"

if sapi_interface == -1 then
  print([[
  Could not open SAPI.
  Note for non-Microsoft operating systems...
  SAPI speech functionality depends on the Microsoft Speech API.
  This is not included by default in Wine, and SpeechSDK51.exe must be separately installed.
  You can download it from
  https://download.microsoft.com/download/B/4/3/B4314928-7B71-4336-9DE7-6FA4CF00B7B3/SpeechSDK51.exe
  ]])
  return
end

if sapi_interface == -2 then
  print("No SAPI voices found.")
  return
end

-- add a new filter
table.insert(sapi_interface.replacements, {"%f[%a][gG]clan", " G clan"})

sapi_interface.say("SAPI interface is ready")
sapi_interface.speech_demo()
```
