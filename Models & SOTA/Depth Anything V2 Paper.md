
Tags: [[DepthEstimation]], [[ComputerVison]]

**Key Author**: [[Lithe Yang]]       

**Institution**: Hong Kong University
[Depth Anything v2 Github](https://depth-anything-v2.github.io/)





### Main Idea

content...

### Technical Details or Model Architecture

* Uses a teacher-student model
* ***student model*** should be small and accurate (e.g. DINO VIT-S)
* ***teacher model*** is larger and more robust (e.g. DINO VIT-G)

### Conclusion / Discussion

* Greatly improves upon DAv1 
* Is able to be run in real-time ]
	*  Various formats: small, base, large
* Outputs a heatmap of relative depth with grayscale values that are in 32-bit floating-point format

### Gaps in Lit
* could be fused with other data to convert the floating points to real-world units
### Gaps in Lit
...