# Loss Functions

## Preliminary on Information Theory

Let $X$ be a discrete random variable taking values in $\mathcal X=\{x_1,\ldots,x_n\}$, and let $p(x_i)=\Pr\left[X=x_i\right]$. The **self-information** of an outcome (x_i) is defined as $I(x_i)=-\log p(x_i)$. The less likely an outcome is, the more difficult it is to predict before it occurs, and therefore the more information its occurrence provides. Conversely, an outcome that is almost certain to occur provides very little new information.

The **entropy** of $X$ is defined as the expected self-information $$H(X)=\mathbb{E}_{x\sim P}[I(x)]=-\sum_{i=1}^n p(x_i)\log p(x_i)$$. Entropy measures the average uncertainty associated with an outcome sampled from (P). Equivalently, it can be interpreted as the average amount of information obtained by observing the outcome of $X$​. Entropy is maximized when $X$ follows the uniform distribution $p(x_1)=\cdots=p(x_n)=\frac{1}{n}$.

Consider two probability distributions $P$ and $Q$ defined over the same set $\mathcal{X}$. Their **cross-entropy** is defined as $H(P,Q)=-\sum_{i=1}^n p(x_i)\log q(x_i)$. For a fixed distribution $P$, the cross-entropy $H(P,Q)$ is minimized when $Q=P$. One can verify this using a simple example. 

## Loss Function for Language Model Training

Language models usually generate text in an autoregressive manner: given the tokens that have already appeared, the model predicts the conditional probability distribution of the next token. The same next-token prediction objective is used during training: the loss is evaluated at every position, while the predictions for all positions are computed in parallel. Below, we explain why the training objective is formulated as minimizing the cross-entropy loss. 

Let the model vocabulary be $\mathcal{V}=\{v_1,...,v_n\}$. After tokenization, a text sequence is represented as $\mathbf{x}=(x_0,...,x_L)$, where each $x_i\in\mathcal{V}$. At position $i$, the model takes the preceding tokens $x_{<i}=(x_0,...,x_{i-1})$ as input and outputs a vector of $n$​ logits. Applying the softmax function to these logits produces a probability distribution over the vocabulary. For each $v_j\in \mathcal{V}$, let $q_i(v_j):=P_M(v_j\mid x_{<i})$, where $q_i(v_j)$ denotes the probability assigned by model $M$ to $v_j$ as the next token at position $i$. Thus, $Q_i=(q_i(v_1),…,q_i(v_n))$. 

During text generation, the next token can be selected from this distribution in several ways. The simplest method chooses the token with the highest probability, which is known as greedy decoding. Other methods, such as random sampling, top-$k$ sampling, and temperature scaling, can be used to control the randomness and diversity of the generated text. 

During training, each next-token prediction can be viewed as a multi-class classification problem over the vocabulary. The observed next token at position $i$ is $x_i$. Its target distribution $P_i$ can be represented as a one-hot distribution $$P_i=\mathbf{e}_{x_i}$$, where $$\mathbf{e}_{x_i}$$ denotes the one-hot vector whose coordinate corresponding to $x_i$ is $1$. Therefore, the cross-entropy at position $i$ is 

$$H(P_i,Q_i)=-\sum_{v\in\mathcal{V}}p_i(v)\log q_i(v)=-\log q_i(x_i)=-\log P_M(x_i\mid x_{<i}).$$

For the entire sequence, the average cross-entropy loss is $-\frac{1}{L}\sum_{i=1}^L\log P_M(x_i\mid x_{<i})$. Minimizing this loss encourages the model to assign higher probabilities to the observed next tokens.

The perplexity of a token sequence is defined as the exponential of the average cross-entropy loss. Intuitively, perplexity measures the effective uncertainty faced by the model at each prediction step. A perplexity of $k$ can be interpreted as an amount of uncertainty equivalent to choosing uniformly among $k$ possible tokens. 

## Loss Function for SFT

🚧 Work in progress. 
