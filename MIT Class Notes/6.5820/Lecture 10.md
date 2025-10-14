GenAI has been rapidly changing many fields for the better (and some worse), and networking is one of them. We will see how AI is being used to aid in solving the most common networking problem: congestion control.

# Congestion Control with AI
Congestion control is still an open problem with new solutions being proposed and tested constantly. With AI, the number of new ideas or modifications to current systems are huge, but there's a huge bottleneck in actually evaluating their capabilities under different conditions.

In general, there are three different ways to do evaluation:
- Emulation - apply real implementation on a "synthetic" network. By synthetic I mean one where you can control specific conditions.
- Testbed - apply real implementation on a real network
- Simulation - apply synthetic implementation on a synthetic network
Is there such a thing as a synthetic implementation on a real network? 

Ideally, we would do all our evaluation on a testbed, since it is the most realistic. However, this brings about a few issues:
- Testing takes a really long time to do. You have to take your implementation and create a real, concrete instance, and then load it into a network and run for however long.
- While it might capture the most typical traffic well, it won't be able to test all possible conditions.

So our next best option is an emulation. The paper uses the mahimahi emulator to emulate network conditions.

## Emulation
The setup is as follows - there are  N algorithms we want to test, and we have M different network conditions or traces. A network trace is a set of times $t_1,t_2,...,t_p$ with bytes $b_1,b_2,...,b_p$ (windows of packet transmissions). There are many tools & open source traces out there for analysis.

To evaluate an algorithm on a trace, we need a utility function that takes in an algorithm A and a network trace C, and outputs a scalar "score". These can be stored in a matrix. What utility function should we use?
$$\frac{Throughput}{Delay^{\lambda}}$$ is a possible option. This could be extended to more flows by using a similar idea from proportional fairness.

The paper uses the utility function defined as follows:
$$u = \frac{tput-tput_{0}}{tput_{0}} - \lambda\left( \frac{delay - delay_{0}}{delay_{0}} \right)$$
Where $tput_{0}, delay_{0}$ are measured from the best BBR implementation across 243 different implementations. 

# Gigantic Dataset Problems
There's an inherent problem that needs to be addressed. There are a large number of algorithms to test, and traces to be tested on. Running on all pairs would take an extraordinary amount of time (at least O(NM), not accounting for the time it takes for the utility function). Is there a way to approximate the behavior instead? Maybe. The authors noted that during their analysis of many traces and algorithms, there were some interesting properties.
- Some traces show big spread across algorithms, meaning they can distinguish a "good" algorithm from "bad".
- Some traces move together, meaning they share similar trends in their outputs: rising and falling at the same places.
With these, we can choose a subset of "conditions" or traces that can be used to approximate the average performance of an algorithm well enough.

Here's how the authors ran their test
	- Pick a subset of algorithms and run on all traces. The goal is to determine two things:
		- A typical level for each condition: the average output of the utility function across the subset.
		- A table of which traces move with each other.
	- Use these to select K conditions that will make you least uncertain about the final average.
	- Run the remaining algorithms on these K traces, and populate the rest of the entries using the relationships found in the first step.
This is an equivalent formulation of matrix completion for low rank matrices, and this matrix will end up being low rank because matrices of real-world systems tend to be low-rank in practice.

Let's look at an example. In the following table, each column represents an algorithm $S_{i}$, and each row represents a trace. The entries are the result of the utility function using algorithm $S_{i}$ and trace $T_{i}$.

|     | $S_1$ | $S_{2}$ | $S_3$ | $S_4$ |
| --- | ----- | ------- | ----- | ----- |
| A   | 80    | 82      | 78    | 81    |
| B   | 40    | 65      | 55    | 60    |
| C   | 42    | 67      | 57    | 62    |
| D   | 90    | 70      | 85    | 75    |
| E   | 12    | 15      | 12    | 13    |
From general observations, we can notice a few things:
- A has low spread, and does not change much across algorithms.
- B has a high spread.
- C shares a similar trend with B.
- D has modest spread, but has a different trend than B or C.
- E has low spread.

How do we choose traces? We use a greedy approach, choosing the one which reduces leftover uncertainty about the overall average the most. This means we choose ones with the following characteristics:
- Big Spread - traces which have a large range of values across the algorithms used initially.
- Representativeness - a trace which shares many trends with other traces. This means we can infer many other traces with only one.
In general, you want to select a trace with both characteristics first. If these were unrelated, then you choose the one with the largest spread first.

In our example above with K=2, we would first select trace B for having high spread and being able to predict C, and then pick D because it covers information that B/C do not.

When we run the remaining algorithms on these traces, we can extrapolate the other values of other traces:
- A and E had generally consistent values across algorithms, so we just use their average.
- C runs slightly above B, so we take the result of B and nudge it slightly up.
To rank the algorithms in general, we use the average across all traces. From there, we can then select the "best" algorithms based on their average across all traces.

The paper goes over their results and evaluation of different LLMs. I won't really include it here, but it is interesting to see how each LLM (o1-preview, GPT-4o, Qwen-2.5-72B) proposed changes to the existing BBR algorithm.

And for whatever reason, we never got to the AlphaEvolve paper. There are notes but nothing in the recording so uhhhhhhh. The gist is that it's a way to apply evolutionary search to guide LLMs to improve existing algorithms, while maintaining correctness and functionality.