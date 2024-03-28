# Hybrid Attention Transformer

This is an experimental hybrid attention transformer based on the Mistral 7B model.

### Hybrid Attention
We propose a hybrid attention model that uses bidirectional attention for thinking and causal attention for text generation. This allows for the indetermanibilty of causal attention while giving the transformer greater access to implicit data in the embedding space. 

1. Thought Processing: The LLM uses bidirectional self attention to create a set of thought embeddings that are not be directly translated into the language space. Thoughts are iteratively re-evaluated. 
2. Language Processing: The LLM takes these thought embeddings and uses standard masked self attention to produce text output.

### Comparison with Other Language Models
Traditional transformer-based language models, whether encoder-decoder (like those used for translation) or decoder-only (such as GPT), process input sequences and generate text in a unidirectional manner. This limits their ability to fully capture evolving contextual nuances and may hinder coherent outputs when dealing with complex narratives.  Our proposed model extends the standard transformer architecture by introducing an iterative re-evaluation mechanism. This loop processes the model-generated embeddings further, enabling it to re-interpret earlier segments of the sequence based on subsequent textual output.

By facilitating a more dynamic and iterative refinement of its representation of the input, our model has the potential to overcome limitations of existing transformer systems.  This approach could lead to improved contextual understanding, more cohesive generated text, and better performance on tasks involving flashbacks, complex narratives, or nuanced shifts in meaning that depend on an evolving understanding of the overall text. 

Here's how this process integrates into the Transformer architecture:

* Encoder Output as Initial Sequence: The output of the Transformer's encoder (embedding vectors) serves as the initial sequence the LLM works with.
* Iterative Attention Layers: We'll introduce additional layers after the encoder, specifically designed for bidirectional attention and contextual re-weighting. These layers refine the embedding sequence.

* Modified Decoder: The decoder will still translate embedding vectors into language, but it will now operate on the refined sequence rather than the original encoder output.

* Bidirectional Attention: The LLM will apply attention bidirectionally across its initial embedding sequence. This means, at a given step, it focuses not only on the current embedding but also retrospectively on previous embeddings and prospectively on those that follow.

* Contextual Re-weighting: As the LLM applies attention across the sequence, it will adjust the weights associated with different embedding vectors.  This allows it to highlight or downplay certain aspects of the text's initial representation based on evolving understanding. Here's how this could work:

* Similarity Calculation: Compare the current embedding vector with both past and future vectors in the sequence using a similarity function (e.g., cosine similarity)
* Re-weighting: Weights are adjusted based on these comparisons. Embeddings deemed highly relevant to the current focus receive greater weight, while less relevant ones get less weight.
* Refinement Operations: We can define a set of operations the LLM can use to modify existing embedding vectors. This could involve:

* Interpolation: The LLM could interpolate between two embedding vectors, creating a new refined vector that combines information from both.
Gradient-Based Updates: Utilize the LLM's existing backpropagation mechanisms to slightly nudge existing embedding vectors towards a more coherent or meaningful representation.
