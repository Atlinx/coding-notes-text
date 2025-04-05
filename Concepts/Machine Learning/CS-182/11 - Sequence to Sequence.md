# 11 - Sequence to Sequence
## Last time: RNNs and LSTMs
![[Pasted image 20250405051927.png]]
- Will focus on many-to-many problems today
## A basic neural language model
- ![[Pasted image 20250405052154.png]]
- Training data: natural sentences
	- ![[Pasted image 20250405052044.png]]
- In reality there could be several million of these
- How are these represented?
- Tokenize the sentence (each word is a token)
- Representation
	- **Simplest**: One-hot vector
		- ![[Pasted image 20250405052119.png]]
	- **More complex:** word embeddings

> [!note]
> These are still very simple language models. Real language models will be explained later.

## A few details
![[Pasted image 20250405052449.png]]
- Question
	- How do we use such a model to complete a sequence?
		- **Ex.** Give it "I think..."
		- Include an `<EOS>` token (End-of-sequence token)
			- Include this in our training data
	- How to kick-off sentence generation?
		- Include `<START>` token at the start as the previous input
- How to generate random sentence?
	- Feed in just start token
- How to complete a sentence?
	- Feed in the incomplete sentence and ignore outputs
	- Then let it continue generating
## A conditional language model
- A language model that generates text **based on** something else
- **Ex.** Image captioning
	- ![[Pasted image 20250405053229.png]]
	- We pipe a convolutional network into the default state $a_{0}$ 
		- Initial state $a_{0}$ is a vector encoding of the desired content of the sequence
	- What do we expect training data to look like?
		- (Image, text)
	- How to tell RNN what to generate?
## What if we condition on another sequence?
- ![[Pasted image 20250405053436.png]]
- We can pipe an RNN into another RNN
	- This is different from one giant RNN because the encoder and decoder parts have different weights
- `<START>` token doubles as the `<EOS>` token for the input sequence
- In reality, input sequence is often read in reverse
	- First part of French sentence is likely related to first part of English sentence
	- If we feed in first part in last, then it's closer to the decoder's first input
## Sequence to sequence models
- Typically two separate RNNs (with different weights)
- Trained end-to-end on paired data (pairs of French and English sentences)
## A more realistic example
- ![[Pasted image 20250405053924.png]]
- LSTMs can be stacked
	- Multiple RNN layers
	- Each RNN layer uses LSTM cells or (GRU)
	- Trained end-to-end on pairs of sequences
	- Sequences can be different lengths
- Not just for cute puppies
	- Translate one language into another language
	- Summarize a long sentence into a short sentence
	- Response to a question with an answer
	- Code generation? Text to Python code
- For more
	- Ilya Sutskever, Sequence to Sequence Learning with Neural Networks
# Decoding with beam search
## Decoding the most likely sequence
- If we just greedily pick the most likely first word, we can end up with an unlikely sentence 
	- ![[Pasted image 20250405054632.png]]
- If we picked a slightly lower probability early on, we could still get higher probabilities in subsequent timesteps
	- ![[Pasted image 20250405054604.png]]
- If we want to maximize the product of all probabilities, we should not just greedily select the highest probability on the first step
	- ![[Pasted image 20250405055120.png]]
- $$\huge p(y_{i,t}\mid x_{i,1:T}, y_{i,0:t-1}$$
- $$\huge p(y_{i,1:T_{y}}\mid x_{i,1:T}) = \prod_{t = 1}^{T_{y}} \underbrace{ p(y_{i,t} \mid x_{i, 1:T}, y_{i, 0:t - 1}) }_{ \text{probability at each step} }$$
## How many possible decodings are there?
- ![[Pasted image 20250405055557.png]]
- Decoding is a search problem
	- ![[Pasted image 20250405055537.png]]
	- We could use any tree search algorithm
	- But exact search in this case is very expensive
	- Fortunately, structure of this problem makes some simple approximate search methods work very well
## Decoding with approximate search
- Basic intuition
	- While choosing highest-probability word on very first step may not be optimal, choosing a **very low-probability word** is very unlikely to lead to a good result either
- Equivalently
	- We can't be greedy, but we can be somewhat greedy
- This is no true in general, and is a guess based on what we know about sequence decoding
- Beam search
	- Store the $k$ best sequences so far, and update each of them
	- Special case $k = 1$ is greedy decoding
	- Often use $k$ around 5 - 10
## Beam search example
- $$\huge p(y_{i,1:T_{y}}\mid x_{i,1:T}) = \prod_{t = 1}^{T_{y}} p(y_{i,t} \mid x_{i, 1:T}, y_{i, 0:t - 1})$$
- $$\huge \log p(y_{i,1:T_{y}}\mid x_{i,1:T}) = \sum_{t = 1}^{T_{y}} \log p(y_{i,t} \mid x_{i, 1:T}, y_{i, 0:t - 1})$$
	- In practice, we sum up log probabilities as we go (to avoid underflow)
- Example
	- Translate French to English
	- k = 2 -> track 2 most likely hypotheses
	- ![[Pasted image 20250405060448.png]]
## Beam search summary
- At every time step $t$
	1. For each hypothesis $y_{1:t - 1, i}$ that we are tracking (there are $k$ hypotheses)
		1. Find top $k$ tokens $y_{t,i,1}, \dots, y_{t,i,k}$
			1. Very easy, we get these from softmax log probabilities
	2. Sort the resulting $k^2$ length $t$ sequences by their total log-probability
	3. Keep top $k$
	4. Advance hypothesis to time $t+1$ and repeat
## When to stop decoding?
- ![[Pasted image 20250405060900.png]]
- Let's say one of highest-scoring hypothesis ends in `<EOS>`
	- Save it, along with its score, but do **not** pick it to expand further
	- Keep expanding the $k$ remaining best hypotheses
- Continue until some cutoff length $T$ or until we have $N$ hypotheses that end in `<EOS>`
## Which sequence do we pick?
- At the end, we might have
	- `he hit me with a pie` $\log p = -4.5$
	- `he threw a pie` $\log p = -3.2$
	- `I was hit with a pie that he threw` $\log p =-7.2$
- Number of steps is different between each sentence
	- Hence longer sentences tend to be more unlikely
- Problem: $p < 1$ always, hence $\log p < 0$ always
	- The longer the sequence the lower its total score (more negative numbers added together)
	- Simple "fix"
		- Divide by length of sequence
		 - $$\huge \text{score}(y_{i,1: T}\mid x_{i, 1:T}) = \frac{1}{T} \sum^T_{t = 1} \log p(y_{i, t} \mid x_{i,1 : T}, y_{i,0: t - 1})$$
## Beam search summary
- $$\huge \text{score}(y_{i,1: T}\mid x_{i, 1:T}) = \frac{1}{T} \sum^T_{t = 1} \log p(y_{i, t} \mid x_{i,1 : T}, y_{i,0: t - 1})$$
- At every time step $t$
	1. For each hypothesis $y_{1:t - 1, i}$ that we are tracking
		1. Find top $k$ tokens $y_{t,i,1}, \dots, y_{t,i,k}$
	2. Sort the resulting $k^2$ length $t$ sequences by their total log-probability
	3. Save any sequences that end in `<EOS>`
	4. Keep top $k$
	5. Advance hypothesis to time $t+1$ and repeat if $t < H$
- Return saved sequence with highest score
	- $k$ is typically 5 - 10
# Attention
## The bottleneck problem
- ![[Pasted image 20250405061646.png]]
 - All information about the source sequence is contained in the activations of the first hidden state
	 - Forms a bottleneck
- Idea -> what if we could somehow "peek" at the source sentence while decoding?
## Can we "peek" at the input?
- ![[Pasted image 20250405061859.png]]
- Key vector
	- Represents what type of info is present at this step
- Query vector
	- Represents what we are looking for at this step
- (Crude) Intution
	- Key might encode "the subject of sentence"
	- Query might ask "the subject of the sentence"
- In reality, what keys and queries are learned -> we do not have to select it manually
## Attention
- ![[Pasted image 20250405062942.png]]
- $e_{t,l} = k_{t} \cdot q_{l}$
	- Attention dot product
	- How similar a key is to a query
- Key $k_{t} = k(h_{t})$
	- $h_{t}$ hidden state at encoder step $t$
- Query: $q_{1} = q(s_{l})$
	- $s_{l}$ encoder state
- Intuitively, send $h_{t}$ for the $\arg \underset{ t }{ \max } e_{t,l}$ to step $l$
	- Let $\alpha_{.,l} = \text{softmax}(e_{.,l})$
	- $\alpha_{t,l} = \dfrac{\exp(e_{t, l})}{\sum_{t'} \exp(e_{t',l})}$
		- This is the "weight" of the attention score, and lets us "favor" attention with the higher score
	- Send $a_{l} = \sum_{t} \alpha_{t,l}h_{t}$
		- This approximates $h_{t}$ for $\arg \underset{ t }{ \max } e_{t,l}$
- What does "send" mean?
	- Who receives it?
		- Output: $\hat{y}_{l} = f(s_{l}, a_{l})$
			- Use both previous decoder state, and the encoder state corresponding to the highest attention for our final output
		- Next RNN layer, if using multi-layer (stacked) RNN
		- Next decoder step
			- $\huge \hat{s}_{l} = \begin{bmatrix}s_{l-1} \\ a_{l-1} \\ x_{l}\end{bmatrix}$
## Attention walkthrough (example)
- ![[Pasted image 20250405063212.png]]
- We calculate a weighted sum of the encoder states by their attention scores
	- $a_{l} = \sum_{t} \alpha_{t} h_{t}$
## Attention equations
- Encoder-side
	- $k_{t} = k(h_{t})$
- Decoder-side
	- $q_{l} = q(s_{l})$
	- $e_{t,l} = k_{t} \cdot q_{l}$
	- $\alpha_{t,l} = \dfrac{\exp(e_{t,l})}{\sum_{t'} \exp(e_{t', l})}$
	- $a_{l} = \sum_{t} \alpha_{t} h_{t}$
- Could use this in various ways
	- Concatenate to hidden state $\begin{bmatrix}s_{l-1}\\ a_{l-1} \\ x_{l}\end{bmatrix}$
	- Use for readout $\hat{y}_{l} = f(s_{t}, a_{l})$
	- Concatenate as input to next RNN layer
## Attention variants
- Simple key-query choice $k$ and $q$ are identify functions
	- $k_{t} = h_{t}$
	- $q_{l} = s_{l}$
	- Decoder-side
		- $e_{t,l} = k_{t} \cdot q_{l}$
		- $\alpha_{t,l} = \dfrac{\exp(e_{t,l})}{\sum_{t'} \exp(e_{t', l})}$
		- $a_{l} = \sum_{t} \alpha_{t} h_{t}$
- Linear multiplicative attention
	- $k_{t} = W_{k} h_{t}$
	- $q_{l} = W_{q} s_{l}$
	- Decoder-side
		- $e_{t, l} = h_{t}^TW_{k}^T W_{q} s_{l} = h_{t}^T W_{e} s_{l}$
			- Just learn $W_{e}$, which is $= W_{k}^TW_{q}$
		- $\alpha_{t,l} = \dfrac{\exp(e_{t,l})}{\sum_{t'} \exp(e_{t', l})}$
		- $a_{l} = \sum_{t} \alpha_{t} h_{t}$
- Learned value encoding
	- Encoder-side
		- $k_{t} = k(h_{t})$
	- Decoder-side
		- $q_{l} = q(s_{l})$
		- $e_{t,l} = k_{t} \cdot q_{l}$
		- $\alpha_{t,l} = \dfrac{\exp(e_{t,l})}{\sum_{t'} \exp(e_{t', l})}$
		- $a_{l} = \sum_{t} \alpha_{t} v(h_{t})$
			- Encoding, we produce key value pairs
			- Then we take value of $h_{t}$ using the function $v$, which is learned
## Attention summary
- Every encoder step $t$ produces a key $k_{t}$
- Every decoder step $l$ produces a query $q_1$
- Decoder gets "sent" encoder activation $h_{t}$ corresponding to largest value of $k_{t} \cdot q_{1}$
	- Actually gets $\sum_{t} \alpha_{t} h_{t}$
- Why is this good?
	- Attention is **very** powerful, because now all decoder steps are connected to **all** encoder steps
	- Connections go from O(T) to O(1) along the attention route
		- Still requires O(T) along the RNN route
	- Becomes important for very long sequences
