# RegisterCommands

### RegisterCommand <a href="#registercommand" id="registercommand"></a>

It is recommended to **always** use this (and not `chatMessage`!) as it allows for the use of the integrated ACL system, and other core functionality (automatic completion, console usage, ...). This native consists of 3 parameters (`commandName`\[string], `handler`\[func] and `restricted`\[boolean]).

#### Example <a href="#example" id="example"></a>

```lua
RegisterCommand("commandName", function(source --[[ this is the player ID (on the server): a number ]], args --[[ this is a table of the arguments provided ]], rawCommand --[[ this is what the user entered ]])
    if source > 0 then
        print("You are not console.")
    else
        print("This is console!")
    end
end, true) -- this true bool means that the user cannot execute the command unless they have the 'command.commandName' ACL object allowed to one of their identifiers.
```

\
