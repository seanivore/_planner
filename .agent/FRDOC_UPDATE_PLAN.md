# Our `frdoc` Custom Command Functionality 

`frdoc` command needs adaptation for files that don't exist to be replaced. I've also recognized some other aspects that we could change to make the entire functionality more logical in general. Check it out in the details below. 

```bash
frdoc -n ~/Development/thot/.agent/DEV_RULES.md -s ~/Development/ -r .agent/DEV_RULES.md
frdoc -n ~/Development/thot/.agent/RESEARCH_PROTOCOL.md -s ~/Development/**/.agent/ [hmm]
```

Maybe the arguments should have just been `--find` `-f` where you'd place `.agent/DEV_RULES.md` and then `--replace` `-r` with the absolute path `~/Development/thot/.agent/DEV_RULES.md` because that follows the natural "find and replace" workflow that people are used to in text editing applications. We'd still want the `--search` `-s` search parameter to add `~/Development` however. Then we could use a directory for the `--find` `-f` argument for something like `~/Development/**/.agent/` (with or without the / character at the end of the path), and then create a new argument to add a new file -`-add` `-a` with an absolute path such as `~/Development/thot/.agent/RESEARCH_PROTOCOL.md` so that we can use the same command to manage these similar needs of adding new resources to specific files across a specific directory space. I noticed this because the current `-n -s -r` flow didn't match the command `frdoc` where f and r were meant to represent find and replace. I think we should change the command to `filemgmt` unless you can help think of a better command that is compact and representative of a tool that can be used for Bulk Directory File Management in the Terminal. 

I also just want to make sure that the arguments should be able to be offered in any order after the command (this might be possible now), just like you shouldn't need to put the '/' at the end of a directory for it to recognize it is a directory, it should just know that, and of course the `--flags` and `-f` flags are interchangeable and can be used with any mix of `--flag` and `-f` for the other arguments following the command. 

The last item I've noticed and would like to change is that I think that when I tried to use a full, absolute path like `User/seanivore/Development/...` it didn't recognize it, but it does work with `~/Development`. I accidentally had a `~` before the `~User/seanviore/Development` which might have caused the error so maybe we should have a "did you mean..." prompt come up if someone uses it with something like that slight mistake, or would that be problematic in some way I don't foresee? Wdyt about the other changes? 

Actually as I'm typing examples, it seems like you don't always need a `--search` `-s` argument if you're just adding a new file. If you use an absolute path to a directory for the find argument and then follow it by an `--add` `-a` argument, it can be implied. 

```bash 
filemgmt -f ~/Development/**/.agent -a ~/Development/thot/.agent/RESEARCH_PROTOCOL.md 
filemgmt -f ~/Development/**/.agent/DEV_RULES.md -r ~/Development/thot/.agent/DEV_RULES.md
``` 

So seems like actually if you use `**` you might not need the `--search` `-s` argument at all. I suppose we could leave it and used or not, as long as nothing breaks the logic of whatever the full command with arguments that the user puts in, it could just run as they expect. 


```bash
filemgmt -s ~/Development -f .agent -a ~/Development/thot/.agent/RESEARCH_PROTOCOL.md
```

Wdyt? 

All of that said, maybe when using `--find` `-f` and `--replace` `-r` and the "find" argument just happens to be recognized as a directory, and the directory doesn't contain whatever the "replace" argument contains could be enough to be understood as a command where the user wants a new file to add. To that point, it seems sort of like we should make sure that the "replace" could add subdirectories as well as files as long as part of the replace value presented matches the directory that is found. 

```bash
filemgmt -f ~/Development/**/.agent -r ~/Development/thot/.agent/api/GENERAL_REFERENCE.md 
``` 

So for example the bash command directly above seems like it might be logically solid to assume that it will find and recognize the directory and then see that a similarly named directory is in the replace, and then notice that there is not any subdirectory or file in the found directories that matches, it will just add the new subdirectory and a copy of the file right inside it. 

Am I missing any other logical interpretation that could be misconstrued? If not, it seems like of like we could have the search, find, add, new, and replace commands available for users because then, sort of like Photoshop/Adobe App's approach to UX, there are many ways to do the same thing, making learning how to use a too technically a bit easier because it accommodates divergent ways of thinking and behavior by design. Technically even the first command in this document using just the `-n` and `-s` arguments (where I added 'hmm' originally, but obviously the real command wouldn't have 'hmm') should still technically work because it can see that the presented "new" file is not in the "searched" location, but both arguments happen to have a specific matching subdirectory that is `.agent/` so it would be able to know that the only possible interpretation must be that the user wants the file added to that location. However, is the "search" argument was too broad like just `~/Development~` we wouldn't want it to add "thot/.agent/RESEARCH_PROTOCOL.md" — but maybe even there I'm overthinking things because if that document at that absolute path was presented, then clearly it would be impossible to add the entire rest of the string after what the "search" argument was because is already exists. 

Wdyt? What am I missing? 

I guess my reasoning for all of this is because I've pretty much always had a directory of resources and now rules that was meant for an agent no matter the project directory they were working in. At first I was able to add the "_AI_RESOURCES" directory as a second directory to the IDE workspace. I eventually ran into some kind of UX issues there though I can't remember exactly what it was... I think I had to constantly re add that resource directory, plus once the IDE started scanning and indexing the contents of project directories regularly, it became more and more complicated to get it to do both of the directories that were located at ~/Development. So then eventually I just started manually placing the resources into every project directory which obviously meant things got dated and chaotic over time because of no single source of truth. Of course I've worked a bit with files that were syslinked but I have only ever had problems doing that where the agents got confused; I've also never used it for a full directory outside of the project, it just seems bulky, though would be faster if it worked smoothly. In an ideal world the IDE would just always handle as many base directories of projects you added as you wanted but who knows when/if that will ever happen particularly give the variable of agents embedded in the IDE tool. I explain all of this because, if all of the above does work, then maybe it is a tool I should share, but I've never shared something like a tool that is just a terminal command and nothing really else. How do tools like git and other tools everyone uses get their clout? 