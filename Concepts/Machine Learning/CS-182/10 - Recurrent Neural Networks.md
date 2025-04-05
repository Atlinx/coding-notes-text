# 10 Recurrent neural networks
## What if we have variable-size inputs
- Before
	- ![[Pasted image 20250404173949.png]]
- Now
	- ![[Pasted image 20250404173954.png]]
	- **Ex.** 
		- Classifying sentiment for a phrase (sequence of words)
		- Recognizing phoneme from sound (sequence of sound)
		- Classifying activity in a video (sequence of images)
- Simple idea -> zero-pad up to length of longest sequence
	- ![[Pasted image 20250404174111.png]]
	- Very simple, can work in a pinch
	- Doesn't scale very well for very long sequences
## One input per layer?
- Give each input to a different layer
	- ![[Pasted image 20250404174306.png]]
- Now for each layer
	- $\bar{a}^{l - 1} = \begin{bmatrix}a^{l - 1} \\ x_{i,t}\end{bmatrix}$
		- We concatenate the new input to the previous layer's inputs
	- $z^l = W^l \bar{a}^{l - 1} + b^l$
	- $a^l = \sigma(z^l)$

> [!warning]
> This specific design doesn't actually work well in practice

- Question -> What happens to the missing layers?
## Variable layer count?
- ![[Pasted image 20250404175349.png]]
- If we have a shorter sequence, we can pretend activations prior the first layer used to be $= 0$
	- Anything preceding the current layer is 0
- This is more efficient than always $0$-padding the sequence up to the max length
	- Each layer is much smaller than giant first layer we'd need if we feed in the whole sequence at the first layer
	- Shorter the sequence, the fewer layers we have to evaluate
- Drawback
	- Total number of weight matrices increase with max sequence length!
	- First few weight matrices aren't trained as much if most inputs are short
## Can we share weight matrices?
- ![[Pasted image 20250404175812.png]]
- We can have as many "layers" as we want!
	- Can have more layers at test time than at training
- This is called a **recurrent neural network (RNN)**
	- Could also be called "variable-depth" network
	- Depth of network is the length of sequence
	- Every layer gets previous layer's input $a$ + next input $x$
	- First layer gets $0$ from previous layer
## Aside: RNNs and time
- What we learned
	- ![[Pasted image 20250404180031.png]]
- What you often see in textbooks/classes
	- Layer gets its own previous value as input
	- ![[Pasted image 20250404180055.png]]
- RNNs are just neural networks that 
	- Share weights across multiple layers
	- Take input at each layer
	- Have variable number of layers
## How do we train this?
- ![[Pasted image 20250404181222.png]]
- Backpropagation in RNNs
	- $W$, $b$ are shared at all layers
		- Shared = the same
- Problem
	- The gradient at $l - 1$ will "overwrite" the gradient at $l$
	- Most libraries don't have this problem because they do it differently
		- Add the derivative instead of setting it
		- $\dfrac{d\mathcal{L}}{d\theta_{f}} += \dfrac{df}{d\theta_{f}} \delta$
			- This accrues changes from multiple layers
			- Note that we still zero out the all the gradients $\frac{d\mathcal{L}}{d\theta_{f}}$ for all layers $f$ prior to backpropagation
			- "accumulate" the gradient during the backward pass
			- If a layer isn't used more than once, then
				- $\frac{d\mathcal{L}}{d\theta_{f}} = 0$
				- if we add $\frac{df}{d\theta_{f}}\delta$ to this, it's effectively the same thing as setting $\frac{d\mathcal{L}}{d\theta_{f}} = \frac{df}{d\theta_{f}}$.
				- But for recurrent layers we will see accrural
- Convincing yourself
	- $f(x) = g(x, h(x))$ 
	- $\dfrac{d}{dx} f(x) = \dfrac{dg}{dx} + \dfrac{dh}{dx} \dfrac{dg}{dh}$
		- First term is derivative through first argument
		- Second term is the derivative of $x$ through second argument
			- We apply chain rule here
# What if we have variable-size outputs?
- Examples
	- Generating text caption for an image
	- Predicting a sequence of future video frames
	- Generating an audio sequence
- Before -> input at every layer
- Now -> output at every layer
- ![[Pasted image 20250404182418.png]]
## An output at every layer
- ![[Pasted image 20250404182612.png]]
- $\hat{y}_{l} = f(a^l)$
	- This is a "readout" function
	- "Decoder"
	- Could be as simple as linear layer + softmax
- We have loss on each $\hat{y}_{l}$
	- Total loss -> $\mathcal{L}(\hat{y}_{1:T}) = \sum_{l} \mathcal{L}(\hat{y}_{l})$
## Let's draw the computation graph?
- Not obvious how to do backprop on this!
- ![[Pasted image 20250404182956.png]]
## Graph-structured backpropagation
- Also called reverse-mode automatic differentiation
	- Do the following at each layer $f(x_{f}) \to y_{f}$
	- Starting with last function, where $\delta = 1$
- Regular backpropagation
	- ![[Pasted image 20250404183323.png]]
- Generalized version of backpropagation
	- Output goes into **multiple downstream nodes**
	- ![[Pasted image 20250404183611.png]]
	- Simple rule
		- For each node with multiple descendants in computational graph
			- Add up delta vectors from all descendants
## Inputs **and** outputs at each step?
- ![[Pasted image 20250404183844.png]]
- Examples
	- Generating text caption for an image
		- Bit subtle why there are inputs at each time step
	- Translating some text into a different language
		- Though there are much better ways to do it
## RNNs are extremely deep networks
- ![[Pasted image 20250404184422.png]]
- Recall from [[7 - Initialization, Batch Normalization#Advanced initialization|advanced initialization]]
	- If we multiply many many numbers together what will we get?
		- If numbers are < 1, we get 0
			- $0.5^{10} \approx 0$
			- "vanishing gradient"
				- **Big problem!**
				- Gradient signal from later steps never reaches the earlier steps
					- Prevents RNN from "remembering" things from beginning
					- Gradient signal from timestep 100 can't make it back to timestep 3
		- If numbers are > 1, we get infinity
			- $5^{10} \approx \infty$
			- "exploding gradient"
			- Could fix this with [[7 - Initialization, Batch Normalization#Gradient clipping|gradient clipping]]
		- We only get a reasonable answer if the numbers are all close to 
## Promoting better gradient flow
- Basic idea
	- Would like gradients to be close to 1
- Each layer
	- ![[Pasted image 20250404184553.png]]
	- Lets condense each layer into a function **RNN dynamics function** $q$ 
		- $\huge a_{t} = q(a_{t - 1}, x_{t})$
	- Dynamics Jacobian = $\dfrac{dq}{da_{t - 1}} \approx \mathbf{I}$
		- Setting Jacobian close to identity gives us the best gradient flow
		- Not always good to force to identity -> only want it to be close to identity **if we want to remember**
		- Sometimes we may want to **forget**
- Intuition
	- Want $\frac{dq_{i}}{da_{i - 1, i}} \approx 1$ if we choose to remember $a_{t-1,i}$, the $i^{th}$ entry
	- For each unit, we have a little "neural circuit" that decides whether to remember or overwrite
- if "remembering" -> copy previous activation as it is
- If "forgetting" -> just overwrite it with something based on the current input
- ![[Pasted image 20250404185348.png]]
	- "cell state" -> $a_{t-1}$
		- The previous input in this "memory cell"
	- forget gate" -> $f_{t}$ 
		- $f_{t}$ is between $[0, 1]$
		- Can be 0.2, 0.5, 0.99, etc.
- Final equations:
	- $\huge a_{t} = q = a_{t-1} f_{t} + g_{t}$
	- $\huge \dfrac{dq_{i}}{da_{t-1, i}} = f_{t} \in [0, 1]$
## LSTM cells
- Long short-term memory
-  ![[Pasted image 20250404190504.png]]
	- $h_{t-1}$ comes from previous layer
	- $x_{t}$ new input
	- $\huge W \begin{bmatrix}h_{t-1} \\ x_{t}\end{bmatrix} + b = \begin{bmatrix}\bar{f}_{t} \\ \bar{i}_t \\ \bar{g}_{t} \\ \bar{o}_{t}\end{bmatrix}$
		- We take $h_{t-1}$ and $x_{t}$ and concatenate them into a single tall vector, that then is multiplied by a matrix of weights
		- We want our weight matrix to have $4$ rows, which forces our output vector to have $4$ entries which are
			- $\bar{f}_{t}$ -> value we will pass to the "forget gate"
			- $\bar{i}_{t}$ -> value that controls the scale of the the addition operator
			- $\bar{g}_{t}$ -> value that controls whether addition operator is adding or subtracting 
			- $\bar{o}_{t}$ -> scaling the memory $a_{t}$ to use as the final output $h_{t}$
				- This is known as the "output gate"
		- Notice how we don't use nonlinear functions for the "memory" or the "cell state" itself
			- This makes the derivatives simpler
- This seems arbitrary, but this ends up working better than naïve RNN
## Why do LSTMs train better?
- $a_{t} = a_{t-1}f_{t} + g_{t}$
	- Changes very little from step to step
	- Simpler linear processing
	- "Long term" memory
- $h_{t}$
	- Changes all the time
	- Multiplicative processing
	- "Short term" memory
## Practical notes
- RNNs almost always have both an input and output at each step
- In practice, naïve RNNs in part 1 almost never work
- LSTM units are OK -> work fine and better than naïve RNNs
	- Still require way more hyperparameter tuning than standard fully connected or convolutional networks
- Some alternatives can work better for sequences
	- Temporal convolutions
	- Transformers (temporal attention)
- LSTM cells are complicated -> But once implemented, can be used same as any other type of layer
- There are variants of LSTM that are simpler and work just as well
	- Gated recurrent unit (GRU)
		- Like LSTM but without an output gate
		- $h_{t} = \tanh(a_{t})$
# Using RNNs
## Autoregressive models and structured prediction
- Most RNNs used in practice look like this
	- ![[Pasted image 20250404191811.png]]
- Most problems that require multiple outputs have strong **dependencies** between outputs
- This is referred to as **structured** prediction
	- Output sequence has structure
- **Ex.** Text generation
	- There are relationships between words and text
	- ![[Pasted image 20250404192343.png]]
	- ![[Pasted image 20250404192402.png]]
		- If we use a RNN with only variable outputs, we get nonsense output even though the network has the exactly the right probabilities!
	- Key idea -> past outputs should influence future outputs!
		- ![[Pasted image 20250404192529.png]]
- How do we train it?
	- Basic version -> set inputs to be entire training sequences and ground truth outputs to be those same sequences (offset by one step)
	- $x_{1:5} = \{ \text{"I"}, \text{"think"}, \text{"therefore"}, \text{"I"}, \text{"am"} \}$
	- $y_{1:5} = \{ \text{"think"}, \text{"therefore"}, \text{"I"}, \text{"am"}, \text{stop\_token} \}$
	- This teaches network to output "am" if it sees "I think therefore I am" 
## Aside: distributional shift
- What if the model choose a very unlikely input?
	- ![[Pasted image 20250404194225.png]]
- However the model hasn't seen the input "drive" before
	- Model is completely confused
- Problem
	- Training/test discrepency
	- Network always saw **true** sequences as inputs in training
	- But at test-time, it gets as input its own (potentially incorrect) predictions
- Called distributional shift
	- Input distribution shifts from true strings (at training) to synthetic strings (at test time)
	- Even one random mistake can scramble the input
## Aside: scheduled sampling
- Old trick from reinforcement learning adapted to training RNNs
- ![[Pasted image 20250404200138.png]]
- Make a random decision at any step, to either
	- Feed previous output
	- Feed next input
- At beginning of training, mostly feed in ground truth tokens as input, since predictions are mostly nonsense
- At end of training, mostly feed in the model's own predictions, to mitigate distribution shift
- Schedules for probability of using ground truth input token
	- ![[Pasted image 20250404200357.png]]
## Different ways to use RNNs
![[Pasted image 20250404200431.png]]
- One-to-one
- One-to-many
	- Image captioning
- Many-to-one
	- Activity recognition
- Many-to-many -> Read all input at once, then start producing output
	- Machine translation
- Many-to-many
	- frame-level video annotation
	- In reality we almost always use auto-regressive generation like this
## RNN encoders and decoders
- ![[Pasted image 20250404214003.png]]
- Input can be processed by a non-recurrent encoder before going into the RNN
	- Very common with image inputs
- Output is processed by a (non-recurrent) decoder before getting outputted
	- A bit less common, since we could just use more RNN layers
- ![[Pasted image 20250404214218.png]]
- Easy to stack as many RNN layers as necessary
- Could even implement recurrent RNN convolutional layers
	- Just replace the $W$ with convolution 
	- Each "filter" becomes a little LSTM cell
		- Filter takes in previous input + new input
		- "Sliding window" with memory of the past
## Bidirectional models
- **Ex.** Speech recognition
- **Problem:** Word at a particular time step might be hard to guess without looking at the rest of utterance
	- Cannot tell if word is finished until you hear the ending
- Bigger problem in machine translation, but we use slightly different models
- ![[Pasted image 20250405050852.png]]
## Some (vivid) examples
- Generating Shakespeare plays
	- ![[Pasted image 20250405051225.png]]
- Generating fake Latex
	- ![[Pasted image 20250405051158.png]]
- OpenAI GPT-2
	- ![[Pasted image 20250405051311.png]]
	- This uses transformers
## Summary
- Recurrent neural networks (RNNs) can process variable length inputs and outputs
	- Could think of them as networks with an input & output at each layer
	- Vaiable length
	- Depth = time
- Training RNNs is very hard
	- Vanishing and exploding gradients
	- Can use special cells (LSTM, GRU)
	- Generally need to spend more time tuning hyperparameters
- In practice, we almost always have both inputs and outputs at each step
	- We usually want structured prediction
	- Can use scheduled sampling to handle distributional shift
- Many variants for various purposes
	- Sequence to sequence models (later)
	- Bidirectional models
	- Can "RNN-ify" convolutional layers