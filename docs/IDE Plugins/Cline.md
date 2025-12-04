- [Cline](https://docs.cline.bot/introduction/welcome) - 
    - [Jetbrains Plugin](https://plugins.jetbrains.com/plugin/28247-cline)
    - [VSCode Plugin](https://marketplace.visualstudio.com/items?itemName=saoudrizwan.claude-dev)
    - [CLI](https://docs.cline.bot/cline-cli/overview) (I have not used this myself, so ymmv and you'll have to figure out how to configure it, though I imagine a lot of settings will be similar to what I've put below)

Note that it'll ask you to login when you first install the plugin. This is optional and you can skip it by pressing "Configure API Keys" (or something similar, I forget exactly what the button says)


## Configuring Cline 

**_When configuring Cline, use the following settings:_** Under "Settings (Cog wheel in the upper right corner of the side panel that pops open) -> API Configuration"


Qwen3 Coder - Follow settings from [[Fermilab Hosted Models]] for API access, additionally: 
 * OpenAI compatible API Key: **MUST BE SET TO ANY RANDOM TEXT** (plugin requires it to be set, but we don't use api keys on this endpoint)
- Request Timeout (ms): `60000` - Usually wont take this long for responses, but the ollama endpoint will take a bit to "warm up" when you start using it most of the time, or if it's unloading/loading the model. Once it's warmed up the response time is generally pretty reasonable.

Alternatively, if you want to use a larger model that's not specifically tuned for coding:

GPT OSS 120b - Follow settings from [[Fermilab Hosted Models]] for API access, additionally: 

- OpenAI compatible API Key: **MUST BE SET TO ANY RANDOM TEXT** (plugin requires it to be set, but we don't use api keys on this endpoint)
- Under "Model Configuration"
    - Supports Images: Unchecked/Disabled - Not supported by gpt-oss:120b afaik
    - Context Window Size: `128000` - Max for gpt-oss:120b
    - Max Output Tokens: `-1`
    - Temperature: `0` (Not relevant afaik)

Under "Features" Set the following: (a lot of these might be the default settings)

- Enable Checkpoints: Checked/Enabled
- Enable MCP Marketplace: Checked/Enabled (Optional, but can make things easier down the road. Dont worry about MCPs at this point, though)
- MCP Display Mode: `Plain Text`
- OpenAI Reasoning Effort (relevant if using `gpt-oss:120b` only): Medium (Recommended, you can play with this if you want. Lower reasoning is faster but less accurate/analytical, higher is slower but more accurate/analytical most of the time)
- Enable Scrict Plan Mode: Checked/Enabled
- Enable Focus Chain: Checked/Enabled

If you're doing work where it's relevant, you can configure Cline to use a browser automatically under the "Browser" tab, but I have not played with this. The settings seem pretty self explanatory, though.Under "Terminal" Set the following (a lot of these might also be the default settings)  

- Default Terminal Profile: `Bash` if on Mac/Linux (probably) and `Powershell` if on windows. If you are using WSL as your terminal in your IDE, you can also set it to `WSL Bash` , but make sure the terminal integration itself is configured properly or things will fail.
- Shell Integration Timeout: `4` is a good default, try increasing if you're on a remote connection and are running into issues
- Enable Aggressive Terminal Reuse: Checked/Enabled - Disable if you're running into issues with the commands Cline runs, though. Might fix things, but not a guarantee
- Terminal Output Limit: `500` - You can increase if your commands output a lot of relevant info that you want to be included in the context, but it will also fill up your context window faster.

Under "General" Set the following  

- Preferred Language: `English` (for the sake of the models being trained mostly on English)
- **_!! Allow error and usage reporting !!: Unchecked/Disabled - VERY IMPORTANT IF USING ON PRIVATE REPOS_**

Once Configured, hit the blue "Done" button in the top right to return to the main interface.

## Using Cline  

- There are two main modes Cline operates in: "Plan" and "Act"
    - Use "Plan" first to analyze your codebase and docs within your repo, work with it/iterate to refine the plan to your liking. Tell it to ask you questions in your prompt. Be very specific wherever you can be, you will get better results this way. Give it details about the environment you're working in if it's not documented in a file somewhere (Otherwise, it will typically try and look through your repo to figure out whats installed if a `requirements.txt` file or something similar exists).
    - Use "Act" mode to start actually performic agentic actions. As soon as you toggle the mode switch to "Act", it will start executing the plan. You can always hit "Cancel" between steps and clarify/tell it to fix something if it's getting stuck in a loop, doing something wrong, or otherwise needs some help/is going down the wrong rabbit hole.
        - By default, Cline will ask your permission to do most "unsafe" actions first, like creating/editing a file, installing a package, deleting something, etc. That being said, it uses the LLM itself to determine if the action is "Safe" in the first place, so monitor it and stop it if need be. If you're using tools/mcp servers, you can give it permission to "Auto-Approve" certain tools as it tries to use them. You can also give it permission to Auto-Approve most file Read commands right above the text entry box. You can also auto approve edit commands, but I don't recommend this.
        - When editing/creating a file, Cline will give you a side by side diff so you can inspect and edit the changes it's making. It will ask you to save or cancel the changes before it moves on to the next step. If you edit the file or cancel the changes, it will note that as feedback and usually try again taking the feedback into consideration. If you keep rejecting/canceling the changes, it will usually stop acting pretty quickly.
        - You can also tell it to ask you questions in "Act" mode, which I've found very useful. The open models aren't great at doing this unprompted, so be sure to tell it to ask you questions in your prompt.
- You can add specific files/folders in your project as context to your prompt by hitting the "@" button under the text entry box. This is useful if you want to point it to some specific files so it doesn't have to try and guess/find the right files. That being said, a file list of the project you're in is included as part of the prompt under the hood, so if your project is structured/named well and according to standard conventions, it's pretty good at finding the right files in my experience. You can also add specific non project files to your prompt by pressing the "+" button that's next to the "@" button (I don't know if it can parse PDFs, though. Image support is model specific and not supported by gpt-oss:120b afaik)
- At the top of the interface, it'll show a progress bar that indicates how much of your "context window" (basically it's memory) has been used. Keep an eye on this, it can fill up fast at times.
    - If you're using a lot of your context window, you can click the little "Compact Prompt" button next to the progress bar at the top of the window showing how much of the context window that's used. This will use the LLM to summarize what it's done so far and reset the context window to just that summary.
- Under the context window progress bar, there are little colored squares that indicate the history/chain of actions it's taken in the current task. You can click on the squares to scroll back to a given action, then hover over the dashed line and click "restore" if you want to rollback to the state your codebase and task was in at that point.
    - This uses git under the hood afaik, but sometimes struggles with really large projects. It'll give you a warning when you first start a task if it can't initialize the checkpoints properly, so keep that in mind.
- If you're finished doing a specific task, I recommend starting a new chat/task fresh (Click the "+" button in the upper right corner of the pane) so that the LLM doesn't get confused/carry over stuff from the last task accidentally. You do have to tell it about your project and create a new plan from scratch (unless you use some other advanced features, which I can get into later), but its typically better to start clean occasionally in my experience. It has a lot fewer issues if you logically separate what you're doing into different chats/tasks than if you just continuously use the same session.
- Generally, you will have better luck with code quality and the LLM understanding things if your project is structured in a typical/standardized way, whatever makes sense for the project/language you're using. It also will do better with more popular languages (so it does great at Python, but probably not as good at VHDL/Verilog), so keep your expectations in line if you deviate from the norm with your project.
- If you have a lot of good documentation for the libraries your using/your project, I can go over how to use tools to fetch and parse that documentation so the LLM will have a better understanding of what's going on. Let me know if this is the case and I'll write something up/setup a meeting going over how to do this.
- **If you have multiple plugins for AI features/assistants in your IDE, I'd recommend disabling all of them except for what you're using at the moment to avoid issues. The plugins are somewhat fragile at the moment.**  

## Final Notes

Finally, these tools are still very new and very imperfect. This goes not only for the LLMs, but things like the plugins, MCP servers, and general guidance on how to use everything as well. Sadly, you should expect annoyances, incomplete documentation, and rapidly changing functionality at the moment. That being said, if you do run into issues, please open an issue in the relevant location (Cline specifically has a [github repo](https://github.com/cline/cline) and a [discord](https://discord.gg/cline) where you can raise issues past the [docs](https://docs.cline.bot/))This hopefully should get you started using Cline/LLMs for coding. There's more to go over (MCP Servers/tools, Workflows and rules, advanced prompting, etc.), but I can go over those in the future/separate posts. I'm still somewhat new to this myself, so please share back any tips or features that you've figured out yourself