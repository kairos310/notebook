# LLM serving on prem
1. cnvrg endpoints 
2. ollama 
3. custom with transformers_stream_generator

|                      | cnvrg endpoint | ollama | custom |
| -------------------- | -------------- | ------ | ------ |
| streaming            | no             | yes    | yes    |
| history              | yes            | no     | yes    |
| RAG                  | yes            | no     | yes    |
| easy to use/polished | yes            | yes    | no     |

# UI interface for LLMs
1. compose.io
	queries chatGPT3.5
	closed source
2. chatGPT box
	can attached to many different backends
	including custom hosted or cnvrg
	open source

# Open llm benchmarks
1. Many fine tuned models outperform GPT3.5 in benchmarks
2. DBRX and Llama3 are promising base models that outperforms base llama2 and mixtral
3. models approach unusable speeds on the A100 at the 11 billion parameter count (at around half vram)
	1. GPU usage % will hover around 100 but vram will differ based on models
	2. speeds can be improved by using quantized versions of the models, but not proportionally, 
		1. for example llama70b 4bit uses 40gb of vram but generates at 5 tokens/second
	3. at around 4bit quantization, with the bitsandbytes package, some models lose quality, like dbrx
4. the transformers package with huggingface is surprisingly robust and very user friendly, with most models requiring at most 3 lines of code 

# MLOPs testing
*from my limited experience with MLDE
1. cnvrg gives users more permissions, with workspaces reserving hardware rather than batch submissions in MLDE
2. 

