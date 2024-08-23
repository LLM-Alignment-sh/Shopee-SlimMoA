# Shopee-SlimMoA-v1

![image-20240823180403264](/Users/yawen.liu/Library/Application Support/typora-user-images/image-20240823180403264.png)

We are pleased to announce that our MoA solution has achieved state-of-the-art (SOTA) performance on the AlpacaEval 2.0 Benchmark with length-controlled evaluation.

# AlpacaEval2.0 Results

| Models                               | **LC Win Rate** | **Win Rate** |
| :----------------------------------- | --------------- | ------------ |
| Shopee-SlimMoA-v1                    | 75.65%          | 73.34 %      |
|                                      |                 |              |
| gemma-2-9b-it-SimPO                  | 72.4%           | 65.9%        |
| Storm-7B                             | 61.6%           | 63.0%        |
| Qwen2-72B-Instruct-AWQ               | Nan             | Nan          |
| Meta-Llama-3.1-70B-Instruct-AWQ-INT4 | Nan             | Nan          |
|                                      |                 |              |
| GPT-4 Omni (05/13)                   | 57.5%           | 51.3%        |
| GPT-4 Turbo (04/09)                  | 55.0%           | 46.1%        |
| GPT-4o Mini (07/18)                  | 50.7%           | 44.7%        |
| GPT-4 Preview (11/06)                | 50.0%           | 50.0%        |



## Efficiency

Minimally, we only need 1xA100 card to run our MoA pipeline.

- Through our ablations, we found that the quality of candidate responses is a second-order effect and has a limited impact on the final performance. For our scenario (chat-related benchmark), using the quantized versions of open-source large models (~70B) exhibit excellent response capabilities, and the final performance even increases slightly compared to using the non-quantized versions.
- By utilizing quantized models, we reduce the computational overhead and resource requirements associated with LLM. This optimization not only accelerates inference times but also lowers operational costs, making our approach both economically and technically advantageous.
- Therefore, in our pipeline, all models can perform single-card inference, achieving utilities far beyond typical MoA implementations, which require multi-cards or distributed MoA implementations. The streamlined efficiency of our pipeline ensures rapid deployment and scalability, enabling us to handle high volumes of interactions with remarkable speed.

## **High Accuracy:**

- Our experiments have unveiled that integrating multiple open-source LLM from a variety of sources can substantially elevate the quality of answer generation. Our resulting choice balances between the strengths and unique perspectives of each model, leading to a richer and more nuanced response pool.
- The diversity plays a crucial role, adding a marginal model from a different source is more beneficial to the overall performance than adding a more capable model (evaluated on its solo performance) but from similar sources. The possible reason is that heterogeneity in the generated answers is instrumental in refining the overall performance, as it mitigates individual model biases and leverages the collective intelligence of the ensemble.

## **Aggregator:**

- We found that the solo performance of an LLM is positively correlated with its overall performance when used as the aggregator. As a result, utilizing models specifically optimized for chat performance as aggregators is markedly more suitable for aggregation of chat outputs, compared to employing open-source large models that are more comprehensive in general abilities. For the AlpacaEval benchmark, models with superior chat performance (designed to handle conversational nuances, manage context transitions smoothly, and generate coherent, contextually relevant responses) are exceptionally well-suited.
- This discovery underscores the importance of tailoring aggregator's model selection to the specific requirements of the task at hand.
- We did not perform any prompt engineering; the aggregator's prompt was directly copied from MoA. This suggests there is room for future improvement.