 # Large Language Models
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

# Measurements
So we have this supervised model. How do we evaluate if this supervised model is "good"? Our first metric is the Area Under the Curve (AUC).

Consider the fact that most machine learning models return a prediction in the form of a probability. Oftentimes we turn this into a decision or binary outcome by setting a threshold, like in logistic regression. We then test this on an unseen validation set to evaluate the model's ability to predict correct labels.

One way to summarize this information is with a Receiver Operator Curve.
#drawing ROC curve

Note the y axis is the true positive rate, and the x axis is the false positive rate. The area under this curve is a decent heuristic to evaluate the performance of a model, without having to consider the decision threshold. If the AUC = 1.0, this signals perfect classification (just the area of the 1x1 box), and an AUC of 0.5 is the accuracy of making classifications by coin flips.

But how do we evaluate if an LLM is good? There's a few things you could use to test it:
- Try to re-predict the training set
- Try to predict some validation data
- Test it's performance in general purpose tasks, such as the MMLU
- Use context-specific metrics or performance.
There's a core issue we're looking at: We want to measure a quantity X, but since we can't measure X, we use another quantity Y and relate them by X = f(y), but we can't test or validate f because we never observe X. This subtle issue can have some major real world consequences if you aren't careful. One way to interpret this is Goodhart's Law:
	Any observed statistical regularity will tend to collapse once pressure is placed upon it for control purposes.
A more modern way to say this would be "Once a measure becomes a target, it ceases to be a good measure"
#todo I don't really know how to write this section coherently so I'll figure it out later.

So, how do we make a measurement "good"? There's two properties you should look for:
- Construct Reliability - how consistent are your measurements over repeated trials or time? This is loosely similar to precision in statistics.
- Construct Validity - Do your measurements actually capture the intended theoretical concept in a meaningful way? This is analogous to statistical bias. Some sub categories would be:
	- Face Validity - do the measurements 'look' plausible at 'face value'?
	- Content Validity - does the measurement 'wholly and fully' capture the underlying concept?
		- Contestedness - Is the concept something different people argue about?
		- Substantive Validity - does the measurement include all relevant observable quantities? Does it include superfluous quantities?
		- Structural Validity - does the measurement capture the structure of relationships between observable properties?
	- Convergent Validity - does this measurement match other measurements of the same concept?
	- Discriminant Validity - does the measurement only correlate with other measurements to the extent the concepts relate?
	- Predictive Validity - can the model be used to make predictions of related observations of concepts?
	- Consequential Validity - how does the measurement interact with the underlying concept when applied to some task?

# Qualitative vs Quantitative Analysis
Quantitative Analysis is the more common form of analysis. The basic idea is this: convert the data to numbers, then make predictions and analyze. It's a simple and effective method, but it is heavily dependent on the measurements. The analysis is often just the application of the scientific method: hypothesize, test, revise.

Qualitative analysis works differently. Often you are interacting with the raw data - images, texts, or observations, and you are looking to find common themes and develop theories. The data is more anecdotal, but rich. Often you have little or no pre-conceptualization of the data, and the insights are thorough but sometimes don't generalize well.
# Grounded Theory
Grounded Theory is a framework intended for qualitative analysis. Let's suppose you have some data, whether it is images or text or something else. You can do a qualitative analysis in the following way:
- Perform an open annotation where you add textual descriptions
- Group similar themed descriptions together into a general description. This is your Focused Code
- Go back through the data and only use your Focused Codes to label the data.