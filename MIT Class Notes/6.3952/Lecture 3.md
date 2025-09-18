Let's first open with a discussion on Large Language Models.

At a high level, they generate text based on what has previously been seen. By repeatedly predicting the next word in a smart way, they can form what we interpret as coherent sentences and ideas. As it turns out, there are many problems that can be framed as an "autocomplete" problem that can be solved by an LLM, such as language translation or summarization. There are many benefits and risks associated with this:
- Text is a useful medium for conveying and receiving information. As a result, LLMs have broad use cases across many domains.
	- Including some harmful domains.
- LLMs are capable of producing fluent text. It's easy to interact with them without any specialized training.
	- Because it is so coherent, it can confidently produce factually incorrect statements.
- LLMs are widely available, being a benefit to the public.
	- But also accessible to malicious actors.
- The capability of LLMs is still evolving. There are still new applications being developed constantly.
	- Including new risks and vulnerabilities
Creating an LLM is typically a two step process:
1) Use unsupervised learning to fit a large dataset of text to the model, letting it learn various patterns to predict text.
2) Apply alignment to pick specific response patterns, catering its output to specific cases or tones. 
Let's really quickly talk about Alignment. Here's a question: what is the difference between biases introduced in pre-training and alignment? A bias in pre-training is a "true" bias. It's something inherent to the data, and good luck getting that out. A bias in alignment is more like catering responses in a specific way. When is alignment good? When is it bad? It's good when you don't want to give out harmful responses. But maybe it turns bad when you actively restrict very specific outputs.

