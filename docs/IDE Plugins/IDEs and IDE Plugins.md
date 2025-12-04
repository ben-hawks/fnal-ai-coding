
There are many different IDE's and plugins that promise to be the perfect AI coding assistant/platform, but only a few that are actually useful, trustworthy, and have worthwhile features. I'd personally recommend sticking to the tools mentioned below (at least those that I dont explicitly warn against here)

**The TL;DR version of this page is**, at the moment, that you should use a plugin with your preferred IDE instead of a dedicated IDE for primarily privacy reasons. [[Cline]] and [[Kilo Code]] are the most stable/full featured plugins for using [[Fermilab Hosted Models]], and [[Github Copilot]] is the smoothest experience for using [[Cloud Models]] (as long as you're going through Github Copilot, you can't bring your own API key with the Github Copilot plugin). 

If you're using Vim/Emacs or similar, I sadly don't have any specific recommendations for you. Some of these tools also offer command line tools that might be useful for your use case, so if anything, it might be worth researching those. You might also want to look at [[Other Coding Tools]]. 

## IDE Plugins (VSCode & Jetbrains IDEs)

Below is a table overviewing the primary features of each plugin that I've personally tried. For more information on each plugin, see the individual page for each plugin. The plugins that support local/[[Fermilab Hosted Models]] are generally no cost and/or open source unless otherwise mentioned.

| Plugin             | VSCode Support | Jetbrains Support                  | Agentic Mode(s) | Chat | Autocomplete                                      | Tool Use (MCP Servers) | Stable? | Cloud Models? | Local/FNAL Models? |
| ------------------ | -------------- | ---------------------------------- | --------------- | ---- | ------------------------------------------------- | ---------------------- | ------- | ------------- | ------------------ |
| [[Github Copilot]] | ✅              | ✅                                  | ✅               | ✅    | ✅                                                 | ✅                      | ✅       | ✅             | ❌                  |
| [[Cline]]          | ✅              | ⚠️ (Unsure pricing model in 2026+) | ✅               | ✅    | ❌                                                 | ✅                      | ✅       | ✅             | ✅                  |
| [[Kilo Code]]      | ✅              | ✅                                  | ✅               | ✅    | ⚠️ (In Beta, VSCode Only, No local model support) | ✅                      | ✅       | ✅             | ✅                  |
| [[Continue.dev]]   | ✅              | ✅                                  | ✅               | ✅    | ✅                                                 | ✅                      | ❌       | ✅             | ✅                  |



## Dedicated IDEs (Not Reccomended)

There are two dedicated AI-powered IDEs that are popular right now [Cursor](https://www.cursor.com) and [Windsurf](https://windsurf.com/). Both of these provide deep integration and many powerful features, but are paid products that offer minimal local model support, and have concerns regarding privacy and use of code/data/prompts and interactions with models for training and other purposes. 

VSCode does also have native Github Copilot integration nowadays, so it's recommended to turn that off if you're using another plugin to avoid conflicts of features.