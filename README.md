# Endpoint with TGI Server

## Concern
GPUUtilization metric remaining 0% when you perform inference on an endpoint which has a TGI server.

## Discussion
The endpoint is behaving as expected:

    - GPUMemoryUtilization shows non-zero values because TGI loads and keeps the model weights in GPU memory, which is reflected in the memory utilization metrics.
    
    - The GPUUtilization showing 0% despite active inference is due to how TGI implements its inference optimization [1]:
        * TGI uses CUDA Graphs and other optimization techniques that pre-compile the computation graphs.
        * The actual GPU compute operations are very efficient and happen in short bursts.
        * The standard GPU utilization metrics may not capture these brief, optimized computation patterns accurately (See attached screenshot).
        
    - The CPUUtilization metric appears during inference calls because TGI's processing pipeline involves both CPU and GPU operations [2].

## References
1. [TGI: In Depth](https://huggingface.co/blog/martinigoyanes/llm-inference-at-scale-with-tgi#tgi-in-depth)
2. [LLM inference at scale with TGI](https://www.adyen.com/knowledge-hub/llm-inference-at-scale-with-tgi)
