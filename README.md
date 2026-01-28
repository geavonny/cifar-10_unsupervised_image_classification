# cifar-10_unsupervised_image_classification
Compare two distinct approaches to unsupervised learning on the CIFAR-10 dataset. We moved from a standard approach to a transfer learning approach, resulting in accuracy leap from 23.13% to 74.02%.

In the first phase, we treated images as simple flat vectors of numbers.
- Methodology: Flattened 32×32×3 images into 3,072-dimensional vectors and applied MiniBatchKMeans.
- The "Flaw": This method relies on Euclidean distance between pixel values. If a "dog" is on green grass and another "dog" is on a blue rug, the computer thinks they are completely different because the background colors don't match.
- Result: The model clustered by color and brightness rather than the actual object.

In the second phase, we implemented a sophisticated pipeline using Computer Vision standards used in 2026.
- Upsampling & Transfer Learning: We resized the images to 224×224 and passed them through a pre-trained ResNet50 model. This allowed us to use "pre-trained eyes" that already understand complex shapes like wheels, eyes, and wings.
- L2 Normalization: We projected features onto a Unit Hypersphere. This ensures the model focuses on the direction of the feature (the identity of the object) rather than the magnitude (how bright the image is).
- PCA (Principal Component Analysis): We compressed the 2048 raw ResNet features into 128 principal components. This removed "noise" while keeping the most important data for clustering.
- Over-Clustering Strategy (K=50): Instead of forcing 10 clusters immediately, we found 50 sub-groups and then mapped them to the 10 real classes. This accounts for the fact that one class (like "Bird") has many different-looking subspecies.
