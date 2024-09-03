
Tags: [[SAR]], [[Generative AI]], [[Diffusion Models]]

Institution: [[Singapore Management University]]

Key Authors: [[Zichen Tian]], [[Zhaozheng Chen]], [[Qianru Sun]]

Year: Nov 2023

Links: [github.com](https://github.com/doem97/gen_sar_plora), [arxiv.org](https://arxiv.org/pdf/2311.17486)


| ***What they did? (overview)***                                                                                                                                                                                                 | ***What was the result?***                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| They propose a 2-stage low-rank adaptation method, and called 2LoRA                                                                                                                                                             | Results were positive albeit not life altering. The pLoRA model achieved the best accuracy for “building” and “road”, and outperforms conventional augmentation methods significantly (+1.58 AvgAcc). |
| ***How they did it? (method brief)***                                                                                                                                                                                           | ***Conclusions / Drawbacks***                                                                                                                                                                         |
| In the first stage, the model is adapted using aerial-view regular image data (whose structure matches SAR), followed by the second stage where the base model from the first stage is further adapted using SAR modality data. | Results were positive albeit not life altering. The pLoRA model achieved the best accuracy for “building” and “road”, and outperforms conventional augmentation methods significantly (+1.58 AvgAcc). |


### Main Idea

content...

### Technical Details or Model Architecture

* Uses a teacher student model...

### Conclusion / Discussion

content...

### Gaps in Lit

content...




### References

