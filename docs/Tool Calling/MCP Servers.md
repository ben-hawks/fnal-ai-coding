MCP Servers are effectively interfaces for LLMs that provide access to different traditional tools and information so that they can function "agentic-ly" (perform actions on their own/on behalf of a user). Different [[IDEs and IDE Plugins]] have different schemes for configuring and installing MCP Servers, but this guide will outline the _general_ way they're installed and configured for use with Coding tools.

If you're interested in the nitty gritty of how to build MCP servers, I recommend [HuggingFace's MCP Course](https://huggingface.co/learn/mcp-course/en/unit0/introduction)
## Installing MCP Servers

MCP Servers (and/or their dependencies) either have to be _installed on your system_ or _installed on a remote system that your system can access_. Most MCP servers that you want to use to develop with will be installed locally on your machine. 

There is no standardized marketplace/repository/central location for finding MCP servers. Some [[IDEs and IDE Plugins]] offer their own, and you can find some marketplaces that exist and/or integrate with given plugins, though. 

Most (if not all) MCP Servers will provide documentation on how to install them. Typically, they are either Node.js based, or python based. They require you to have an active/compatible installation for the MCP server to work. 

For Node.js based MCP Servers, I'd recommend using [Node Version Manager (NVM)](https://github.com/nvm-sh/nvm), since in my experience, not all tools use the same version. This requires a bit of extra configuration in the MCP Config files, but in my opinion gives some greater flexibility. You're also free to install Node.js normally and just pay attention to dependencies and version requirements as you install MCP Servers.  

For Python based MCP Servers, I'd recommend (as most plugins also do) using [UV](https://github.com/astral-sh/uv) to manage the environments and install the servers. 

## Configuring MCP Servers

To use an MCP server with a given plugin/coding tool, you must typically configure it in a JSON file that corresponds to that given plugin/coding tool. The location, format, and options for these files tends to differ, annoyingly. Look at the different [[IDEs and IDE Plugins]] documentation for details on where/how to configure MCP Servers. 

Some tools/plugins also distinguish between "Global" and "Project/Repo Specific" MCP servers, so be sure to pay attention where you're installing a given MCP Server, and if that MCP server is meant to be used in a given location as well. 

Some MCP servers that are publicly available will offer public remote endpoints, so you don't have to install them locally, or otherwise rely on remote services. Be cautious when evaluating and installing MCP Servers if you plan on using them for non-public code, as like with most cloud models and some IDEs and plugins, they will use/retain data to further train models and/or improve their own tools. 

## Example Installation & Configuration - Node.js 

As an example, I'll walk through the steps to install [Context7](https://github.com/upstash/context7) locally using NVM on a mac/linux environment, which is an MCP Server used to access publicly available documentation for various codebases. You only need to install NVM once per machine you're working on, and a given version of Node depending on the version used by the MCP Server. Pay attention to the `args` field, where we configure `nvm use <version>`, as it might differ across different MCP Servers. 

1. Install  [Node Version Manager (NVM)](https://github.com/nvm-sh/nvm)
2. Install Node version >= v18.0.0 via NVM using `nvm install 18`
3. Configure your MCP server json file for your plugin/environment (specifics/location of the file depends on what plugin or other coding tool you're using. Look up the documentation for your plugin/coding tool for more details)

```json
{
  "mcpServers": {
    "context7": {
      "command": "bash",
      "args": [
        "-c",
        "source ~/.nvm/nvm.sh && nvm use 18 && npx -y @upstash/context7-mcp"
      ],
    }
  }
}
```


## Useful MCP Servers

There's a lot of MCP servers of varying quality out there at the moment, especially since the protocol is basically brand new, so again, be careful with what MCP servers you install and use. Treat it like any other application/package you install on your machine.

I've found a few useful ones myself, and will share them below

*  [Context7](https://github.com/upstash/context7) - This is a great tool for accessing public documentation in a format that LLMs can easily use. The short version of how it works is that it indexes the documentation for a given repo/tool (you can search for what's indexed [here](), as well as add any publicly available docs to it's index), generates some context, Q&A, Examples, and summaries of the documentation using an LLM (I believe Claude, but I'm not certain), and makes it available via the MCP server. I **believe** that it only collects information on public documentation, and that any requests sent to the server are simply to query if a given package that is requested by a user exists in the index, and if so will retrieve the documentation for it. I'd still proceed with caution if you do want to start using it with non-public code, and recommend doing more research before relying on it heavily. 
* "Official" MCP Servers - Reference implementations of common features. Generally safe to use (as far as I know) when installed locally. 
	- **[Everything](https://github.com/modelcontextprotocol/servers/blob/main/src/everything)** - Reference / test server with prompts, resources, and tools.
	- **[Fetch](https://github.com/modelcontextprotocol/servers/blob/main/src/fetch)** - Web content fetching and conversion for efficient LLM usage.
	- **[Filesystem](https://github.com/modelcontextprotocol/servers/blob/main/src/filesystem)** - Secure file operations with configurable access controls.
	- **[Git](https://github.com/modelcontextprotocol/servers/blob/main/src/git)** - Tools to read, search, and manipulate Git repositories.
	- **[Memory](https://github.com/modelcontextprotocol/servers/blob/main/src/memory)** - Knowledge graph-based persistent memory system.
	- **[Sequential Thinking](https://github.com/modelcontextprotocol/servers/blob/main/src/sequentialthinking)** - Dynamic and reflective problem-solving through thought sequences.
	- **[Time](https://github.com/modelcontextprotocol/servers/blob/main/src/time)** - Time and timezone conversion capabilities.
- [Playwright](https://github.com/microsoft/playwright-mcp) - Allow for the LLM to interact with a browser, navigate pages, take screenshots of a webpage, etc. Installed locally this interacts with (by default) an installation of Chrome/Chromium, so just be conscious of the actions being taken by the LLM when using it.   
- [Github MCP Server](https://github.com/github/github-mcp-server) - Allows an LLM to interact with Github specific features, such as PRs and Issues, CI/CD pipelines, etc. beyond the reference implementation git MCP server mentioned above. 