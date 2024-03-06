Restricted Boltzman machine
stochastic, random sample from latent space, reconstruct with same weights as forward pass

Autoencoder
can feed noise, or masked out parts of images, can also train between label and original image, basis of image segmentation
deterministic

Variational Autoencoder
latent space is now a probability distribution, with a mean and standard deviation, keep this close to 1
this is where reparameterization trick comes in 
![[Pasted image 20231206013914.png]]

Disentangled Autoencoder
add $\beta$ to loss function to ensure that every weight in the latent space is unique and separate from each other, analysis of this can is typically very interpretable

![[Pasted image 20231206013729.png]]

	hyperparameters        typical autoencoder part    disentagled     variational

https://www.youtube.com/watch?v=9zKuYvjFFS8
