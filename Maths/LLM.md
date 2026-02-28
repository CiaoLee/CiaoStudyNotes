#### Transformer

- LLM Page 28~29

核心是一串周期三角函数组合的位置编码

$$
    PE(pos,2i) = sin(\frac{pos}{10000^{2i/d}}) \\
    PE(pos,2i+1) = cos(\frac{pos}{10000^{2i/d}}) \\
$$

对于 $pos+k$ ，令 $ \alpha =\frac{pos}{10000^{2i/d}}, \beta=\frac{k}{10000^{2i/d}}$

$$
    PE(pos+k,2i)\\ 
    = sin(\alpha)cos(\beta) + cos(\alpha)sin(\beta)\\
    = PE(pos,2i)cos(\beta) + PE(pos,2i+1)sin(\beta)\\

    PE(pos+k,2i+1)\\
    =cos(\alpha)cos(\beta)-sin(\beta)sin(\alpha)\\
    =PE(pos,2i+1)cos(\beta)-PE(pos,2i)sin(\beta)
$$

- 对于任意一个固定的偏移量 $k$, 其对应的系数 $cos(\beta)$ 和 $sin(\beta)$ 都是固定的常数。这意味着, 位置 $pos+k$ 的编码可以通过位置 $pos$ 的编码 $PE(pos)$ 经过一个固定的线性变换（旋转举证）得到.
- 变换矩阵只依赖于相对位置 $k$，而与绝对位置 $pos$ 无关，因此，模型在自注意力机制中计算 Query 和 Key 的点积，得到的就是相对位置 k 信息。

#### Self-Attention

自注意力是 基于 Transformer 的机器翻译模型的基本操作，建模源语言、目标语言任意两个单词之间的依赖关系。

输入是 单词语义的 Embedding Codes 嵌入表示的位置编码，为了实现对上下文语义的依赖建模，进一步引入在自注意力机制中设计的三个元素 Query $q_i$, Key $k_i$, Value $v_i$

$$
    Z = Attention(Q,K,V) = softmax(\frac{QK^T}{\sqrt{d}})V
$$

```python
class MultiHeadAttention(nn.Module):
    self.q_linear = nn.Linear(d_model,d_model)
    self.v_linear = nn.Linear(d_model,d_model)
    self.k_linear = nn.Linear(d_model,d_model)
    ## 
    d_k = d_model
    self.h = heads
    self.dropout = nn.Dropout(dropout)
    self.out = nn.Linear(d_model,d_model)
    
```

