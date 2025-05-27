# 12 - Transformers
## Is Attention All We Need?
## Attention
- ![[Pasted image 20250405064613.png]]
- We we have attention, do we even need recurrent connections?
- Can we transform our RNN into a purely attention-based model
- Attention can access every time step
	- Can in principle do everything that recurrence can, and more!
- Few issues we must overcome
	- Problem 1: step $l = 2$ can't access $s_{1}$ or $s_{0}$
	- Encoder has no temporal dependencies at all
## Self-attention
- ![[Pasted image 20250405065439.png]]
- Not a recurrent model, but still weight sharing
	- To get our hidden state $h_{t} = \sigma(Wx_{t} + b)$
- Equations
	- $v_{t} = v(h_{t})$
		- **Ex.** $W_{v} h_{t}$
		- Produce a value for each timestep
	- $k_{t} = k(h_{t})$
		- **Ex.** $k_{t} = W_{k} h_{t}$
		- Produce a key for each timestep
	- $q_{t} = q(h_{t})$
		- **Ex.** $q_{t} = W_{q} h_{t}$
		- Produce query for each timestep
	- Every step can now access every other step, including itself
	- $e_{l,t} = q_{l} \cdot k_{t}$
	- $\alpha_{l,t} = \dfrac{\exp(e_{l,t})}{\sum_{t'} \exp(e_{l, t'})}$
		- Normalized scores to serve as "weights"
	- $a_{l} = \sum_{t} a_{l,t} v_{t}$
		- Final 
- ![[Pasted image 20250405065619.png]]
- We can chain together multiple self-attention layers
## From Self-Attention to Transformers
- Basic concept of self-attention can be used to develop a powerful sequence model called a transformer
- But to make this actually work, we need to develop a few additional components to address some fundamental problems
	1. Positional encoding
		- Addresses lack of sequence information
		- Order of inputs matters, but we don't take that into account right now
	2. Multi-headed attention
		- Not just one value query and key per input
			- Can think of (key, value, query) tuples as "filters" like in conv nets
		- Allows querying multiple positions at each layer
	3. Adding nonlinearities
		- Each successive layer is linear in the previous one
	4. Masked decoding
		- How to prevent attention lookups into the future?
# Sequence Models with Self-Attention
## Positional encoding: what is the order?
- What we see
	- `he hit me with a pie` -> We care about order
- Naïve self-attention sees bag of words
	- ![[Pasted image 20250405070238.png]]
	- `a pie hit me with he`
	- `a hit with me he pie`
	- `he pie me with a hit`
- **Most** alternative orderings are nonsense, but some change the meaning
- **In general** position of words in sentence carries information
- **Idea:** Add some information to the representation at the beginning that indicates where it is in the sequence!
	- $h_{t} = f(x_{t}, t)$
	- Notice how we also include the timestep $t$ when the information is given
## Positional encoding: sin/cos
- Naïve positional encoding -> just append $t$ to input
	- $\bar{x}_{t} = \begin{bmatrix}x_{t} \\ t\end{bmatrix}$
	- This is not a great idea, because absolute position is less important than relative position
		- `I walk my dog every day`
		- `Every single day I walk my dog`
		- Notice how "my dog" is right after "I walk" is the important part, not its absolute position
- We want to represent position in way that tokens with similar relative position have similar positional encoding
- Idea -> what if we used frequency-based representations?
	- ![[Pasted image 20250405070936.png]]
		- $p_{t}$ is the positional encoding
		- ![[Pasted image 20250405071017.png]]
			- This form of positional encoding is good for relative positioning 
			- 1st entry -> "even-odd" indicator for the first two rows
			- 2nd entry -> First-two, or next-two?
			- $\dfrac{d}{2}$ entry -> first-half vs. second-half indicator?
		- $d =$ dimensionality of the positional encoding
		- This basically represents a matrix
## Positional encoding: learned
- Another idea -> just learn a positional encoding
	- ![[Pasted image 20250405071419.png]]
	- Different for every input sequence
	- Same learned values for every sequence
		- But different for different time steps
- How many values do we need to learn?
	- $P = [p_{1}, p_{2}, \dots, p_{T} \in R^{d \times T}$
		- $d =$ dimensionality
		- $T =$ max sequence length
	- More flexible than sin/cos encoding
	- More complex, need to pick max sequence length (can't generalize beyond it)
## How to incorporate positional encoding?
- At each step, we have $x_{t}$ and $p_{t}$
	- Simple choice -> just concatenate them
		- $\bar{x}_{t} = \begin{bmatrix}x_{t} \\ p_{t}\end{bmatrix}$
	- More often -> just add after embedding the input
	- Input to self-attention is $\text{emb}(x_{t}) + p_{t}$
		- $\text{emb}(x_{t})$ is some learned function
## Multi-head attention
- Since we rely **entirely** on attention, we might want to incorporate **more than one** time step
- ![[Pasted image 20250405071827.png]]
	- Our current attention fixates on a single value, because it's weighted the highest
	- Also hard to specify we want multiple things ins a query
- Idea -> have multiple keys, queries, and values for every timestep
	- ![[Pasted image 20250405071946.png]]
	- Compute weights independently for each level $i$
		- $e_{l,t,i} = q_{l,i} \cdot k_{l, i}$
		- $\alpha_{l,t,i} = \dfrac{\exp(e_{l,t,i})}{\sum_{t'} \exp(e_{1}, t', i)}$
		- $a_{l,i} = \sum_{t} \alpha_{l, t, i} v_{t, i}$
- Full attention vector formed by concatenation
	- $a_{2} = \begin{bmatrix}a_{2,1} \\ a_{2,2} \\ a_{2,3}\end{bmatrix}$
- Around 8 heads seems to work pretty well for big models
	- Even 1 head is still multidimensional
## Self-attention is linear
- ![[Pasted image 20250405065439.png]]
- ![[Pasted image 20250405072503.png]]
- $$\huge a_{l} = \sum_{t} \alpha_{l, t} v_{t} = \sum_{t} \alpha_{l, t} W_{v} h_{t} = W_{v} \sum \alpha_{i, t} h_{t}$$
- Every self-attention "layer" is a linear transformation of the previous layer (with non-linear weights)
- This is not very expressive
## Alternating self-attention & nonlinearity
- ![[Pasted image 20250405072701.png]]
- Non-linear function
	- $h_{t}^l = \sigma(W^l a^l_{t} + b^l)$
	- Self attention is "fetching" information across timesteps
	- This function is known as a "position-wise feedforward network"
## Self-attention can see the future!
- Crude self-attention language "model"
	- ![[Pasted image 20250405073056.png]]
	- (In reality we would have many alternating self-attention layers and position-wise feedforward networks, not just one)
- Big problem
	- Self-attention at step 1 can look at value at steps 2 & 3, which is based on inputs at steps 2 & 3
- At test time
	- Inputs at steps 2 & 3 will be based on output at step 1
	- Which requires knowing inputs at steps 2 & 3
## Masked attention
- Must allow self-attention into the past, but not into the future
- Easy solution
	- $$\huge e_{l,t} = \begin{cases}
		q_{l} \cdot k_{t} &\text{if } l \geq t \\
		-\infty &\text{otherwise} 
		\end{cases}$$
	- If the $k_{t}$ comes from a timestep before $q_{l}$, then we allow it
	- `softmax` will set to 0 when $e_{l,t} = -\infty$
## Implementation summary
- We can implement a practical sequence model based entirely on self-attention
- Alternate self-attention "layers" with nonlinear position-wise feedforward networks (to get nonlinear transformations)
- Use positional encoding (on the input or input embedding) to make the model aware of relative positions of tokens
- Use multi-head attention
- Use masked attention if you want to use the model for decoding
# The Transformer

## Sequence to sequence with self-attention
- ![[Pasted image 20250405074138.png]]
- There are number of model designs that use successive self-attention and position-wise nonlinear layers to process sequences
- Generally called "transformers" because they transform one sequence into another at each layer
	- Vaswani et al. Attention is all you need
- "Classic" transformer is a sequence to sequence model
- Number of well-known follow works also use transformers for language modeling (BERT, GPT, etc.)
## The "classic" transformer
- As compared to a sequence to sequence RNN model
	- ![[Pasted image 20250405074928.png]]
- Encoder
	- Repeated $N$ times
- Decoder
	- Repeated $N$ times
	- Use masked attention
		- $y_{2}$ only depends on $y_{0}$ and $y_{1}$
	- Cross attention -> classic version of attention where it references values in decoder
	- At end, apply softmax at every position, and output values
## Combining encoder and decoder values
- ![[Pasted image 20250405075135.png]]
- Cross-attention
	- Similar to standard attention from previous lecture
	- Query: $q_{l}^\mathcal{l} = W_{q}^l s_{t}^l$
		- We build a query for each decoder state $s_{t}^l$
	- Key: $k_{t}^l = W_{k}^l h_{t}^l$
		- We build a key for each encoder state $h_{t}^l$
	- Value: $v_{t}^l = W_{k}^l h_{t}^l$
		- We build a value for each encoder state $h_{t}^l$
	- Then we plug this into the usual attention equations to calculate $c_{l}^t$
	- $c_{l}^t = \sum_{t} \alpha_{l,t}^l v_{t}^l$
		- Cross-attention output
		- In reality, cross-attention is also multi-headed
		- Final weighted sum of all values based on their relevance to our query
## One last detail: Layer normalization
- Main idea -> [[7 - Initialization, Batch Normalization#Batch Normalization#Batch normalization "layer"|batch normalization]] is very helpful, but hard to use with sequence model
	- Sequences are different lengths, makes normalizing across the batch hard
	- Sequences can be very long, so we sometimes have small batches
		- We don't have memory for a large number of long sequences
- Simple solution "layer normalization" -> like batch norm, but not across the batch
	- ![[Pasted image 20250405080611.png]]
	- Batch norm
		- Uses information from each input $a_{i}$ from a batch of $B$ inputs
		- Computes mean for each dimension in the vector $a_{i}$
		- Computes standard deviation for each dimension in the vector $a_{i}$
	- Layer norm
		- Uses single input $a$
		- Computes a single scalar mean $\mu$
			- Averages mean across different dimensions of $a_{j}$
		- Computes a single scalar
			- Across different dimensions of $a_{j}$
		- $\bar{a}$ is still a vector
## Putting it all together
- The transformer
	- ![[Pasted image 20250405082103.png]]
- Notation
	- Input: $\bar{h}_{t}^{l - 1}$
	- Output: $a_{t}^l$
- Encoder (in order from bottom to top)
	- Add & Norm
		- $\bar{a}_{t}^l = \text{LayerNorm}(\bar{h}_{t}^{l} + h_{t}^l)$
		- Passed to next layer $l + 1$
	- Feed Forward
		- $\bar{h}_{t}^{l} = W_{2}^l \text{ReLU}(W_{1}^l \bar{a}_{t}^l + b_{1}^l) + b_{2}^l$
		- 2-layer neural net at each position
	- Add & Norm
		- $\bar{a}_{t}^l = \text{LayerNorm}(\bar{h}_{t}^{l - 1} + a_{t}^l)$
		- Residual connection with LN
		- They want every layer to be a modification (not just replacing the output)
			- Gets a well-behaved gradient
	- **NOTE:** $N = 6$ layers with $d = 512$
		- $d = 512$ means there are 512 entries in each $a$ or $h_{t}^l$ vector
- Encoder passes multi-head attention keys and values and passes to decoder
- Decoder (in order from bottom to top)
	- Add & Norm -> Residual connection with LN
	- Feed forward
		- Add & Norm -> Residual connection with LN
	- Multi-head cross attention
		- Add & Norm -> Residual connection with LN
	- Same as encoder only masked
	- **NOTE:**
		- Every encoder block corresponds to a decoder block
## Why transformers?
- Downsides
	- Attention computations are technically $O(n^2)$
		- But it usually has a small coefficient for its quadratic part
		- **Ex.** $O(0.1n^2 + 999n)$
	- Somewhat more complex to implement (positional encodings, etc.)
- Benefits
	- Much better long-range connections
	- Much easier to parallelize
	- In practice, can make it much deeper (more layers) than RNN
- The benefits vastly outweigh downsides, and transformers work much better than RNNs and LSTMs in many cases
- One of most important sequence modeling improvements of the past decade
- Supporting data
	- In practice, we can use larger models for same cost
	- No need for ensembling to achieve the high performance
	- ![[Pasted image 20250405082623.png]]