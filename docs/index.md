
# Using AI Tools to Code at the lab

This documentation is intended for developers at Fermilab to configure and use LLMs to assist in coding and related tasks. 

***This whole site is a work in progress, and NOT official recommendations by the lab or it's leadership! This is meant as a simple way to share knowledge about these tools as it's developing. Check and follow the [AI at work](https://ai-atwork.fnal.gov) page for official guidance on tools to use at and around the lab.***
### Using Tools to Code (not to be confused with "Tool Calling")

Generally, there are two main ways to interact with different Large Language Models (LLMs) and use them for coding. 

The first, most common, and recommended way (by me, Ben Hawks) is to use them through your IDE. This is done via a plugin, or via a dedicated IDE built around a specific tool. You can read more about these here: [[IDEs and IDE Plugins]]. I personally recommend this approach due to the increased capabilities that it enables, such as automatic file editing/creation, codebase context awareness, etc.  The downside of this approach is that you are also limited by which plugin(s) you choose, which are currently all somewhat new and not yet fully featured, or have different sets of features compared to each other. 

The other way is through different tools that may or may not be specific to coding. You can read more here: [[Other Coding Tools]].

As some disambiguation, you might sometimes hear about the term "Tool Calling" in regards to LLMs and coding tools. This usually refers to using "[[MCP Servers]]", where an LLM is using other tools that are made available through specific interfaces/APIs on behalf of a user to perform advanced "agentic" actions (such as running a terminal command).

### Models

All of these tools require some model (LLM) to actually power them under the hood. A majority of tools you find advertised are typically powered by [[Cloud Models]], which tend to offer better performance at the trade off of cost (paying a subscription and/or per token used) and little to no guarantee that your data, code, prompts, and other interactions will not be kept private/used to train further models. In some cases, the usage of your data etc. to train a model is an explicit condition to use a given model or tool. Because of this, we must be very careful of what tools we use with what code/data at the lab. In most cases, its better to be safe than sorry and choose a tool that can use [[Fermilab Hosted Models]], which run on lab infrastructure, and are therefor okay for more (see cybersecurity/[AI at work](https://ai-atwork.fnal.gov) for more information) data/code than the cloud hosted models. 

**All this being said, it falls upon you, the user, to adhere to the proper policies, and exercise caution and best judgement while using these tools. I/Any other authors of this documentation are not responsible for misuse of anything mentioned here or beyond. Do your own research, consult with Cybersecurity, AICO, and/or line management first, and if it feels wrong, don't do it.**


 