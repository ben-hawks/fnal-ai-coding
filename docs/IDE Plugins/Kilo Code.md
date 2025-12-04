Kilo Code is an open source fork of [[Cline]] and Roo Code. It supports [[Fermilab Hosted Models]] and [[Cloud Models]], but this guide will focus on using [[Fermilab Hosted Models]]. Configuration and use of Kilo Code is very similar to [[Cline]], but with some key differences and improvements. At the moment it is what I use and tend to recommend to users who want to use models hosted at the lab. 

***This page, specifically the "Using Kilo Code" section, is a heavy work in progress. Check back regularly for updates!***
## Links & First time Configuration

- [Kilo Code Docs]https://kilo.ai/docs) - 
    - [Jetbrains Plugin](https://plugins.jetbrains.com/plugin/28350-kilo-code)
    - [VSCode Plugin](https://marketplace.visualstudio.com/items?itemName=kilocode.Kilo-Code)
    - [CLI](https://kilo.ai/docs/cli) (I have not used this myself, so ymmv and you'll have to figure out how to configure it, though I imagine a lot of settings will be similar to what I've put below)



Note that it'll ask you to login when you first install the plugin. **This is optional and you can skip it by pressing "Configure API Keys"** (or something similar, I forget exactly what the button says).

You can also access settings at any time by hitting the little cog wheel icon in the upper righthand corner of the Kilo Code pane/tab.  (Third from the right in the image below) ![[Pasted image 20251204152259.png]]

***Read through all of the settings explanations here before using the plugin, especially "About Kilo Code Configuration"!!!*** 

### Model Access Configuration (Providers tab)

For most [[Fermilab Hosted Models]], enter the API settings and other parameters mentioned on the linked page. You can configure multiple providers so that you can switch between different models/endpoints on the fly. 

For some common models, configure these settings beyond the defaults:

* Qwen3 Coder
	* API Key: **Any random text** (required by the plugin)
	* Image Support: Unchecked/Disabled - Not supported
	* Advanced -> Error & Repetition Limit: 5
* GPT OSS 120b
	* API Key: **Any random text** (required by the plugin)
	* Enable Reasoning Effort: **Enabled**
		* Model Reasoning Effort: **Medium**
	* Advanced -> Error & Repetition Limit: 5

### Auto-Approve Configuration

By default, Kilo Code will always automatically allow the LLM to perform certain agentic actions. **I Highly recommend disabling "Write" as one of these actions**. When "Write" is disabled, it will show a diff of the original code and the LLM generated code, asking you to edit/approve the changes before applying them. **I'd also recommend disabling "Execute" actions by default, as that allows for automatic execution of command line applications.** My auto approve settings look as follows:

![[Pasted image 20251204150009.png]]

### Browser Configuration

Only relevant if you are using a browser for interacting with/testing whatever you're developing. Default settings are likely fine as far as I know. 

### Checkpoints Configuration

By default, Kilo Code will create (git based) checkpoints under the hood for almost every action it takes on your codebase. This can take up a large amount of disk space depending on how large your repositories are, and how many tasks you have. I recommend enabling "Enable automatic task cleanup" and configuring the retention settings to your liking to avoid using to much disk space. As the name implies, this does remove "tasks" (basically chat sessions and their checkpoints) over time, but you can set it so that favorited tasks are never deleted. 

If you work with large repositories, I'd also recommend increasing "Checkpoint Initialization timeout" from the default 15s, as it will typically take longer for large repositories. 


### Display Configuration

Configure this to your liking, but I typically keep the defaults. This just configures some options for how the chat interface looks/works. I'd highly recommend keeping the "task timeline" active if nothing else, as this is how you can quickly restore checkpoints if you have that enabled. 

### Autocomplete Configuration

**This is only present on the VSCode plugin**
**This does not work with Local/[[Fermilab Hosted Models]] at the moment**

### Notifications Configuration

Kilo Code can send notifications when a task is completed/other important events, so that if you have your IDE minimized it can notify you when it needs some sort of interaction. Configure as desired. 

### Context Configuration

Generally, these settings control what and how much information is included in the LLMs context window/what's sent along with your prompt. You typically should find a balance between sending enough context that the LLM can effectively answer your questions and perform tasks correctly, but not so much that you quickly max out the (limited) context window of a given model. I won't go over specific settings here, but will say that I tend to use the default values and only tweak them if I run into issues. 

### Terminal Configuration

Here you can configure some settings on how the LLM interacts with the terminal. I use the defaults here, but you can change them if you run into issues with the LLM either not completing a command successfully, or understanding the context/output of a given command. As with the above context settings, you generally want to find a balance of how much information to send to the LLM. 

### Prompts Configuration

**Unless you know what you're doing, leave these alone by default.** These are the system prompts that Kilo Code uses under the hood when performing specific tasks. They are sent to the LLM as a part of whatever you are sending, and typically provide some baseline rules/instructions on how to perform a given task. To my knowledge, these tend to be tuned for Anthropic models, but have worked fine for me on the [[Fermilab Hosted Models]] that I use.  

### Experimental Configuration

**Here be dragons.**

These are some experimental features that the plugin is currently working on. These change over time, and some can be useful. But, as the name implies, they are experimental and prone to bugs. I'd recommend not using any of these unless you're willing to deal with whatever consequences come from the feature catastrophically failing somehow. 

### Language Configuration

Whatever the default language you want Kilo Code to be in. As far as I know, does not effect the interactions with the LLM/default prompts used. 

### MCP Servers

Here you can install and configure [[MCP Servers]] to use with Kilo Code. The plugin provides a nice UI and marketplace, but you can always edit the MCP configuration JSON file manually to install/tweak anything not supported by the UI. You can click the little "here" text to open the JSON MCP config file: ![[Pasted image 20251204151939.png]]

The "Installed" tab lets you enable and disable certain MCP servers if you want to (from your global and project specific MCP servers). Additionally, you can edit the global or project specific MCP config files by scrolling down and hitting the buttons: ![[Pasted image 20251204152030.png]]


### "About Kilo Code" Configuration (VERY IMPORTANT)

You ***MUST*** disable "Allow Error and Usage Reporting" in this tab!! It is ***REQUIRED*** to use Kilo Code with any non-public code. 

## Using Kilo Code

**This section is a work in progress.**

Kilo Code primarily operates in a very "Agentic" manner. This means that, by default, it will perform actions and tasks (using built in tools and/or [[MCP Servers]]) related to the query you ask it instead of just responding with text to a given query. **Be very aware of this before you start using Kilo Code.**
### Agentic "Modes"
In the bottom left corner of the text box, you'll see a dropdown menu to select a "Mode":
![[Pasted image 20251204155014.png]]
It expands into this:
![[Pasted image 20251204152654.png]]
Kilo code supports multiple "Modes", where each uses a different system prompt to perform various specific tasks. In general, I recommend starting with "Orchestrator" mode, which will determine subtasks and delegate them to the relevant modes as required to complete your request. Each mode is somewhat self explanatory, and you can view the prompt that each is using in **Settings -> Prompts**. 

Additionally, you can create new/custom modes by hitting the "Edit..." button, if so desired. This is an advanced feature, though, and I have not explored it myself. 

### Selecting a Model/Provider
If you have multiple models, or "Providers", configured, you can select which one you are currently using via the dropdown with the name you gave the provider in the left corner of the text entry box:
![[Pasted image 20251204154950.png]]
### Additional Context (Attaching Files)
If you want to specifically reference a given file ***in your project***, you can do so by attaching it to your query via the paperclip icon in the lower right of the text entry box: 
![[Pasted image 20251204154921.png]]
As far as I know, this does not automatically ingest and convert documents (such as PDFs), but is meant for text/code files exclusively.  

By default, Kilo Code will attempt to discover and include any relevant files in your project when queried by the LLM to do so. This is just a way to clarify what file to use specifically if ambiguous and/or improve results quality. 
### Codebase Indexing

![[Pasted image 20251204154903.png]]
This is an advanced feature that you can enable to improve result quality when working with a specific codebase. It requires that you have a [Qdrant](https://qdrant.tech/) vector database setup somewhere to hold all of the indexed code, and an Embedding Model provider setup somewhere to embed/index the code. This is an advanced feature, if you're interested in using it, contact Burt Holzman about setting up/using a Qdrant and Embedding Model endpoint.  

If you're using `nomic-embed-text:latest` as your embedding model, you must set the "Model Dimension" to `768`.

### "Enhance Prompt with Additional Context"
![[Pasted image 20251204154853.png]]
The 'Enhance Prompt' button helps improve your prompt by providing additional context, clarification, or rephrasing. Try typing a prompt in the text box and clicking the button to see how it works.

### Kilo Code Rules & Workflows
![[Pasted image 20251204155055.png]]
Rules allow you to provide Kilo Code with instructions it should follow in all modes and for all prompts. They are a persistent way to include context and preferences for all conversations in your workspace or globally. [Docs](https://kilocode.ai/docs/advanced-usage/custom-rules)

## General Tips & Tricks

 If you're finished doing a specific task, I recommend starting a new chat/task fresh (Click the "+" button in the upper right corner of the pane) so that the LLM doesn't get confused/carry over stuff from the last task accidentally. You do have to tell it about your project and create a new plan from scratch, but its typically better to start clean occasionally in my experience. It has a lot fewer issues if you logically separate what you're doing into different chats/tasks than if you just continuously use the same session.

Generally, you will have better luck with code quality and the LLM understanding things if your project is structured in a typical/standardized way, whatever makes sense for the project/language you're using. It also will do better with more popular languages (so it does great at Python, but probably not as good at VHDL/Verilog), so keep your expectations in line if you deviate from the norm with your project.