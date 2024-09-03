
Tags: [[Diffusion Models]], [[Generative AI]]

Institution: [[UC Berkeley]]

Links: [(arxiv.org)](https://arxiv.org/abs/2006.11239), [(github.com)](https://github.com/hojonathanho/diffusion)

Key Authors: [[Jonathan Ho]], [[Ajay Jain]], [[Pieter Abbeel]]

Year: 2020


![[Pasted image 20240903132010.png]]
***Figure:*** generation of unconditional CIFAR10 (meaning no preference for a particular class) images sampled over time

| *What They Did?*                                                                                                                         |
| ---------------------------------------------------------------------------------------------------------------------------------------- |
| High quality image synthesis results using diffusion probabilistic models (SOTA of the time). Results equaled or surpasses GANs and VAEs |

| ***What were the results?***                                                                                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| On the unconditional CIFAR10 dataset,<br>we obtain an Inception score of 9.46 and a state-of-the-art FID score of 3.17. On<br>256x256 LSUN, we obtain sample quality similar to ProgressiveGAN                                             |

| ***How they did it?***                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - Trained a reverse diffusion model (which is a latent variable model) which predicts how to denoise and image to eventually produce a high quality image. <br>- The diffusion process (adding noise) takes about 1000 steps (iterations). The reverse diffusion process is performed by a NN that learns to gradually remove noise and has the same steps number as the forward diffusion (n=1000). <br>- A loss function is used to compare the denoised image to the original clean image |

| ***Conclusions / Drawbacks***                                                                                                                                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| - paper represents progress in making diffusion models a generally useful tool<br><br>- The paper also highlights risks associated with generative AI including crime, fraud, and misinformation                                           |

**Reverse diffusion**
Diffusion Models (DM) are probabilistic models designed to learn a data distribution p(x) (Example: CIFAR10 dataset) by gradually denoising a normally distributed variable, which corresponds to learning the reverse process of a (noise adding) fixed Markov Chain of length T.


### Technical Details or Model Architecture

**Denoising Model**
* The core denoising model is a U-Net backbone (similar to an unmasked PixelCNN++) with group normalization throughout

**Pitfalls of GANs**
The promising results of Generative Adversarial Networks GANs have been revealed to be mostly confined to data with comparably limited variability as their adversarial learning procedure does not easily scale to modeling complex, multi-modal distributions. 

GANs also suffer from mode collapse 
	where the generator starts producing a limited variety of outputs, often very similar to each other, instead of a diverse range that represents the real data distribution**

### Conclusion / Discussion

content...

### Gaps in Lit

content...


---
### References

