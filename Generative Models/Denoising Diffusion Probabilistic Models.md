
Tags: [[Diffusion Models]], [[Generative AI]]

Institution: [[UC Berkeley]]

Links: [(arxiv.org)](https://arxiv.org/abs/2006.11239), [(github.com)](https://github.com/hojonathanho/diffusion)

Key Authors: [[Jonathan Ho]], [[Ajay Jain]], [[Pieter Abbeel]]

Year: 2020



| What They Did?                                                                                                                                                                                                                             | What were the results?                                                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| High quality image synthesis results using diffusion probabilistic models (SOTA of the time). Results equaled or surpasses GANs and VAEs                                                                                                   | On the unconditional CIFAR10 dataset,<br>we obtain an Inception score of 9.46 and a state-of-the-art FID score of 3.17. On<br>256x256 LSUN, we obtain sample quality similar to ProgressiveGAN |
| **How they did it?**                                                                                                                                                                                                                       | **Conclusions / Drawbacks**                                                                                                                                                                    |
| Trained a reverse diffusion model (which is a latent variable model) which predicts how to denoise and image to eventually produce a high quality image. A loss function is used to compare the denoised image to the original clean image | - paper represents progress in making diffusion models a generally useful tool<br><br>-                                                                                                        |

Reverse difusion




### Technical Details or Model Architecture

* Loss Function:



### Conclusion / Discussion

content...

### Gaps in Lit

content...


---
### References

