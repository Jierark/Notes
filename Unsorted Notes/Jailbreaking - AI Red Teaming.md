Tricking an AI into doing or saying something it is not supposed to. Things such as leaking information about the backend processes.

# Common Tactics

## Role Play
- Don the persona of a party that would need to know specific information
	- Example - claim you are a biosafety researcher to get information about building bioweapons
	- Similarly, tell the model that the information is for educational purposes only or similar.
## Prompt Injection
- https://learnprompting.org/docs/prompt_hacking/injection
- Essentially, injecting messages into user messages to trick the AI to ignore their system prompts and do something it should not be doing.
	- Prompt Leaking - tricking the AI to output information about its initial prompt.
- Main goal is to make an AI ignore some developer instruction
- Possible method: echo system prompt and add additional instructions
- Or send a message to emulate a previous conversation.
- Overload the message with a bunch of confusing information?
- To get around blacklists, try writing words in different formats, or substitute letters.