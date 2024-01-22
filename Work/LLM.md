dashboard > launch jupyterhub
fine tuning is infeasible on consumer hardware
fine-tuned models are the same size as the original pretrained model


baseline model > transfer learning weights

sequence in > sequence out
can use any kind of data format, multi modal LLM approach
where sequences can be patches of images
Med-PaLM M - Google
all use cases can be encoded into a latent space, then can be used to generate output


docker.io/determinedai/environments-dev:lore-base-image-release

LLM to optimize clinical workflow, from dictation to structured format


Single epoch fine tuning 

Quantization of variables, most efficient way of representing digits
round to int8, don't lose much precision
Meticulously researched sections that don't effect much of the output

Parameter Efficient Fine Tuning
Pretraining
Conventional Finetuning
general llm, smaller target dataset, 
parameter efficient fine-tuning original weights are frozen, add and finetune additional parameters, basically represent all parameters in a lower dimensional space

adapters
the lower dimensional weights 

adapters are inserted into the pretrained frozen model
