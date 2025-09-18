The goal for this lecture is to give a high level overview of data-driven decision making models. It will show you when to use specific frameworks, and the key decisions, tradeoffs, and challenges to consider for each one.

# Supervised Machine Learning
Imagine you wanted to write a program that could predict whether it was going to rain tomorrow. You would quickly run into a problem that trying to make that prediction based on what you know is difficult.

Although we don't have the concrete rules, we do have a lot of data about this. How could we use it to help us? This is where supervised machine learning comes in.

We have some input data, and their associated labels. We can feed them into a supervised learning algorithm, which will produce a model that can best predict labels based on the inputs.

So, what could go wrong here?

There's always an underlying assumption that the data is "good". This class will focus on that question, looking at where the data came from and where it could go wrong.

Where can data come from, anyways? Collecting data is typically a hard task, but as we spend more time on digital activities, we are inadvertently creating data (known as digital exhaust). Things such as online activity, financial information, cell phone location, educational data, the list goes on. It's a bit cynical, but this is the reality of the digital age.
Before we move on, let's evaluate digital exhaust:
- Pros
	- There's a ton of data
	- These data collection services are slowly improving over time
	- There's no* cost to it
- Cons
	- The data probably isn't perfect.
		- It has inaccuracies
		- It might not be the data you want
		- It might not be representative
	- There's also a whole set of surveillance and privacy concerns

So, when can we use supervised machine learning? Generally, we should have some measurable inputs that we want to use to predict well-defined, measurable outputs. Ideally, we already have data with input/output examples, and the data is "good", meaning
- The inputs actually do provide information about the outputs
- The examples are very representative
- The future/unseen should be very similar to the past
- The data was accurately recorded
- and so on.

There's a key distinction we should make between **predictions** and **decisions**. 
- A prediction answers the question: What will happen?
- A decision answers the question: What should I do?
If a decision we make affects the thing you are trying to predict, then supervised learning is most likely not the correct method. 
# Causal Inference
Causal Inference involves establishing cause and effect relationships. If I make a decision given the inputs here, what's the outcome? It's used in the situations above, where any given decision can change the prediction. Some examples are:
- What will the effect of this drug be?
- How will different interventions affect homelessness?
- Which link color leads to the most clicks?
Supervised ML is generally not going to work in this situation. In that setting, we know about past data, but we have no information on causal relationships. That is, why did a particular outcome occur? We need this "why" to make decisions.

Let's take a simple example: Does eating ice cream lead to increased drowning?
![[Pasted image 20250918115019.png]]
From this image, it looks like it would.

If we just wanted to predict drowning, then this would be fine, but if we are trying to figure out how to **reduce drowning**, then this breaks down? Do we just ban ice cream consumption? No, that's stupid. Let's think about why we see this pattern:
- Drowning involves swimming in a body of water. When is swimming most popular? I'd say during the summer.
- Eating ice cream is typically done on hot days. When are there a lot of hot days? I'd say during the summer.
- Now we see the causal relationship here: Summer leads to more ice cream consumption, and because people are swimming more often, there is bound to be more opportunities for someone to drown.
This is the classic statistical fallacy of correlation $\neq$ causation.

Causal Inference is difficult because of something called confounding. It occurs when a factor influences both the choice made and the outcome. For example, let's suppose we have two medical treatments for some disease. Treatment 1 has a recovery rate of 25%, and treatment 2 has a recovery rate of 50%. Naturally, one might assume that treatment 2 is better than treatment 1, but this is where you have to be careful. You would want to know how these treatments were being assigned.

As it turns out, treatment 2 was always prescribed in mild cases, whereas treatment 1 was always prescribed in severe cases. Now you can't relate the two, as the severity of the case affects both which treatment was issued and the patient's chance of recovery (a much sicker person usually has poorer outcomes).

So how can we mitigate confounding? The best option is to avoid it when data is collected. This is often done through Randomized Control Trials, where you assign choices at random without considering other factors. RCTs are great, but often very tricky to do correctly.

Alternatively, you can assume that the confounding isn't too bad. Sometimes we have data on hand and no real way to thoroughly check for confounding. Some confounding factors are unseen and we wouldn't know how to determine it. While almost always wrong, if the confounding isn't too strong, there could be some useful results anyways.
# Reinforcement Learning
The way we usually interact with the environment is as such:
- We have an input which is our current state and surroundings
- We make a decision based on it
- It leads to some outcome
- We then repeat this cycle.
Each decision has some kind of cost associated with it. Our goal then is to decide on a set of actions with minimal cost. A typical example of this problem is known as the multi-arm bandit problem, formulated as the following:
- You have a set of slot machines that you can pull. Each has a different cost and average payoff. The goal is to decide on a sequence of pulls that maximizes winnings.
The big decision we have to make is exploration vs. exploitation: do we try pulling on new machines to learn their value, or do we keep pulling on machines where we know the value well?

Typically, decisions will impact the state of the world, which changes the possibility of our future decisions. This is where Reinforcement Learning steps in to algorithmically determine the best set of actions. Additionally, these problems are often modeled using Markov Decision Processes. Some examples where you'd want to find the optimal policy (best set of actions in each state):
- What treatment policy would achieve the best outcome for a patient?
- What lending policy minimizes the chances of defaults, or maximizes the returns?

So why don't we use sequential decision-making everywhere? It turns out the core issue is the amount of data that is typically required. You need to get estimates for the effect of transition to a new state for every possible action. This is fine in settings where you can either 1) run many simulations, such as in games, or 2) only need to consider a few set of states and actions. If you can address these issues, or are able to get all the required data, then this turns out to be a very flexible framework.
# Generative Modeling
The last 3 frameworks often went from an input to an output. Generative modeling can be thought of as working in the opposite direction: for example, how can we generate a picture to match the specified caption? A similar problem is predictive text: the input is the previous few words, and the output is the next word that makes the most amount of sense in context.

If you've ever wondered why GPT-3 and other related models have billions of parameters, it's because having to predict the next word requires an extraordinary amount of data to do well. A phone's predictive text function only considers the last few words. LLMs usually consider a few hundred words at once, and general ones have to consider an extremely large sample space.

The earliest form of generative modeling was a General Adversarial Network, composed of two models:
- The generator model generates new examples, such as images
- The discriminator model tries to classify examples as either real or fake.
- The goal is for the generator to produce examples that fool the discriminator about half the time.

So let's suppose we wanted to do the following: given a prompt, generate an image. Where do we start making a model to do that?

Well, we'll first need some previous data. As it turns out, the internet contains a vast amount of image-captions pairs. Think of services like Pintrest or Wordpress and how many times you see images with captions.

Let's dive into diffusion models. They are a fascinating way of being able to generate images. In training, the model takes an image/caption pair and learns how to add structured noise to the image. From this, a model is able to figure out how to convert random noise into a coherent image, which still blows my mind that this works. The one problem is assuming that you have a lot of "good" data. There's also issues of copyright and creative licensing, but that's a whole field of active discussion.