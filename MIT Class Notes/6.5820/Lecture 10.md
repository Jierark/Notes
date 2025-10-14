GenAI has been rapidly changing many fields for the better (and some worse), and networking is one of them. This lecture will cover two different ways GenAI are being used to solve networking problems today.

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

