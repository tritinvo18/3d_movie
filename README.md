# Project 8: 3d_movie

## Extending Tiny NeRF

In Tutorial 10.4, you implemented a 'Tiny NeRF' model capable of rendering simple 3D objects from a set of 2D images. While effective for demonstrating the core idea of Neural Radiance Fields, Tiny NeRF has two important limitations. First, it behaves as a matte renderer, meaning it **cannot model view-dependent effects** such as reflections or specular highlights. Second, it uses **uniform sampling along camera rays**, which wastes computation by sampling many points in empty space where no geometry exists.

Your goal is to extend Tiny NeRF toward the full NeRF model introduced in [NeRF: Representing scenes as neural radiance fields for view synthesis](https://arxiv.org/abs/2003.08934) (Mildenhall et al., 2003). Specifically, you will implement two key engineering improvements that enabled NeRF to produce high-quality view synthesis results:

* View-dependent appearance modelling 
* Hierarchical sampling with a dual-network architecture 

By implementing these components, you will build a more expressive neural renderer and evaluate how these improvements affect rendering quality. 

### Task details

Work through the following tasks to complete this challenge:

#### Dataset setup

Download the following dataset and starter code for this challenge: 

* [tiny_nerf_data.npz](/tiny_nerf_data.npz)
* [IFN680_Week10_Assessment_StarterCode.ipynb](/starter_code.ipynb)

It includes the main rendering pipeline and training loop, along with placeholders for the components required in this assignment. Complete these placeholders to fulfill Tasks 1 and 2. You may modify the code as needed, but the overall pipeline structure should remain consistent with the provided implementation. Once your upgraded NeRF is working, train it to convergence, and compare its results against the baseline.

#### Task 1: Implementing view-dependency

In the current Tiny NeRF implementation, the multi-layer perceptron (MLP) predicts colour c and density σ from the 3D position (x, y, z) only. As a result, the appearance of the object does not change with the viewing direction, producing a matte rendering. In this task, you will modify the model so that colour depends on the viewing direction while density remains independent of it. This separation allows multi-view consistency, meaning that the geometry of the scene does not change depending on the camera angle.

#### Task 2: Hierarchical sampling and dual-network architecture

The baseline Tiny NeRF samples points uniformly along each camera ray. However, most of these samples lie in empty space, making the rendering process inefficient. In this task, you will implement hierarchical sampling, a strategy that allocates more samples to regions where the scene likely contains geometry. This approach uses two networks, the Coarse Network and Fine network, and the rendering pipeline should operate in two passes:

1. Coarse pass:
    * Sample points uniformly along each ray.
    * Evaluate the coarse network at these points.
    * Compute the volume rendering weights, which indicate where the scene geometry is likely located.
2. Fine pass:
    * Use the weights from the coarse pass to build a probability distribution along the ray. (code provided)
    * Draw additional samples from this distribution using importance sampling. (code provided)
    * Evaluate the fine network at all points, including these new points.

#### Task 3: Comparative analysis and evaluation

In the final task, you will evaluate the performance of your upgraded NeRF implementation by comparing it with the Tiny NeRF baseline. Your evaluation should include both quantitative and qualitative comparisons.  

* Quantitative: Compute metrics like PSNR (and optionally SSIM or other image similarity metrics) on held-out test views that weren’t used during training.  The provided start code performs this dataset split.
* Qualitative: Present visual comparisons, including rendered images from novel viewpoints, side-by-side comparisons, or a mosaic or grid of rendered samples illustrating visual differences.

Finally, analyse the results and discuss how the implemented improvements affect rendering quality, view-dependent appearance, and overall model performance. **As a reference performance metric, we expect this improved NeRF model to achieve an average PSNR of 28 dB on the test set of the provided dataset.**

### Submission Instructions

#### Report

Submit a professional report in PDF format (max. 2 pages) on this page. Your report should be clear, concise, and technically precise. It must:

A. Explain the changes you implemented in the upgraded NeRF.
B. Present the results obtained, including quantitative metrics and qualitative visualisations.
C. Discuss your findings, analysing how the modifications affect rendering quality, view-dependent appearance, and overall model performance.

#### Project Code

You will submit a Zipped folder named project8_code.zip on this page. This folder must include:

* TinyNeRF.ipynb: A notebook containing the full implementation of the NeRF model presented in the tutorial with any changes you performed to evaluate its performance.
* ExtendedNeRF.ipynb: ExtendedNeRF.ipynb: A notebook containing the full implementation of the upgraded NeRF discussed in tasks 1 and 2.
* main_report.ipynb: A notebook that reproduces all numerical results and plots reported for Task 3. Include all files and code required for reproducibility, including model setup, trained weights (.pth) and evaluation code. Do not include the training loop. This notebook will be run for grading; if any cell fails, the code evaluation will receive 0 marks.

Please follow the structure of the Tutorial 10.4 Tiny NeRF code as closely as possible and ensure compatibility with the IFN680 computing environment.

### Supporting resources

* [10.4 Tutorial notebook](/ref/IFN680_Week10_Tutorial_Solution.ipynb)
* [PyTorch documentation](https://docs.pytorch.org/docs/stable/index.html)
* Ensure your code is configured to utilise the available hardware via .to(device) to ensure your experiments run within a reasonable timeframe.
* [NeRF: Representing scenes as neural radiance fields for view synthesis](https://arxiv.org/abs/2003.08934) (Mildenhall et al., 2003).
