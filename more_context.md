The purpsoe of this template repository is so that anyone can start a new project, or inject this into an existing project to build and maintain ongoing documentation for a project for several reasons:
- To assist with decreasing the amount of work needed to prevent LLMs from making mistakes or create architectural drift
- To be used in conjunction with LLMs to prevent developers from introducing drift when maintaining or creating new features in a project
- To have persistence of knowledge or discussions across sessions (06_memory is the primary tool for this one). 
- To inject one's opinions on how to organize code or use frameworks or tools without needing to specify on every instruction or for new hires
- To prevent hallucinations in general

With that being said, I'd like to add whatever relevant information to the AGENTS.md file to ensure the above can be accomplished along with what's described in the README.md. This is meant to be a systematic approach to context engineering for any size project, so a user can do any of the following:

- If they're just starting out, the LLM will inform them they can generate documentation in any of the first 4 layers to build up context for an existing or new project
- If they're working on a feature over multiple sessions, they should be able to ask the LLM to pick up where they left off and keep going
- Legacy and architectural documents should be updated over time as new shifts, refactors, or decisions are made that would require architectural docs to be rewritten
- A user should be able to tell the LLM to do a memory dump at the end of a session, in which case the LLM will write out the session notes, discoveries, errors, pitfalls, victories, etc into the proper memory directories and files, and consider rewriting legacy docs (mostly found in 04_project ) if need be
- The LLM should assist users with constructing these documents by asking an exhaustive amount of questions to build comprehensive documents for the base, domain, stack, and project directories
- It should assist them with ingesting relevant information and assist with brainstorming new applications, features, bug fixes, etc. 

I'd also like to instruct the LLM to only ingest what's relevant. I don't want it to ingest all the documentation at start of a new session, as it may be unnecessary, which would result in eating up context. This is where links or references to the intro_docs could provide hints at what sort of thing the directories might hold, and with that information the LLM could ingest only the relevant docs based on what it thinks it needs. 

Please write any instructions required to accomplish these sort of tasks in the AGENTS.md file. Ask any and all qualifying questions if you see any gaps or more info is required to create this sort of experience for the user. 