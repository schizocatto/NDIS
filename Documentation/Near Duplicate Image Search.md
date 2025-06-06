## NDIS Introduction

### Background

Near-Duplicate Image Search (NDIS) refers to the task of retrieving images that are visually similar to a given query image but are not exact duplicates. These near-duplicates typically originate from the same source image but undergo various transformations such as:
- Resizing or cropping
- Compression artifacts
- Color adjustments
- Overlaying text or watermarks
- Rotations or minor edits  

The need for NDIS has grown significantly with the explosion of user-generated content on social media, image-sharing platforms, and search engines. Efficient detection of near-duplicate images is crucial in applications such as:  
- Copyright protection
- Image clustering and deduplication
- Web-scale image search and recommendation
- Fake news and forgery detection
  
### Problem statement

The core problem in NDIS is:
> Given a large collection of images and a query image, efficiently retrieve all images that are near-duplicates of the query image, even when the duplicates have undergone transformations.

Below are a few cases of near duplicate images from various datasets.

![[Pasted image 20250607015354.png]]
<p align="center">Figure 1. Examples of near duplicate images from INRIA Copydays dataset</p>

![[Pasted image 20250607015622.png]]
<p align="center">Figure 2. Examples of near-duplicate images from California ND dataset</p>
![[Pasted image 20250607015720.png]]
<p align="center">Figure 3. Example of near-duplicate image (visually similar)</p>
![[Pasted image 20250607015802.png]]
<p align="center">Figure 4. Near-duplicate image created by rotation</p>
![[Pasted image 20250607015842.png]]
<p align="center">Figure 5. Near-duplicate images with viewpoint and illumination change</p>

This problem is challenging due to:
- Robustness requirement: The system must tolerate a wide range of image modifications.
- Scalability: Image databases can contain millions or billions of entries.
- Precision vs. recall trade-off: The system must minimize false positives (non-near-duplicates) while ensuring all relevant results are retrieved.
Addressing this problem involves designing robust image representations (e.g., perceptual hashes, deep features), efficient indexing structures (e.g., approximate nearest neighbor search), and transformation-invariant similarity measures.

## Related works

### General method used for NDIS

The steps followed in the detection of near-duplicate images are generally quite similar to that of traditional image matching method. Figure 6 shows the general block diagram for the identification of near duplicate images. Initially, the features of the images (image descriptors), retrieved from the web or in the database, are extracted and stored in another database named ‘Feature Vector Database’.   

The features of the query image are extracted and compared with the feature vectors of all images already stored in the database. If matches are found, they are marked as near-duplicate/ visually similar images. This approach requires an efficient indexing structure to search for similar descriptors in the database. The percentage of similarity required is fixed by a reference value. Some of the methods used for the detection of near-duplicate images are presented in the next section.

![[Pasted image 20250607020023.png]]
<p align="center">Figure 6. General block diagram for identifying near-duplicate images</p>

## Feature extraction methods for near duplicate detection

Feature detection is the process of computing the abstraction of the image information and making a local decision at every image point to see if there is an image feature of the given type existing in that point. An ideal feature detection technique should be robust to image transformations such as rotation, scale, illumination, noise and affine transformations. In addition, ideal features must be highly distinctive, such that a single feature to be correctly matched with high probability. 

There are a few ways to extract features. Most commonly used methods came from the archetype of Key point-based feature extraction (or so I thought). The other methods of extraction are Pixel-based feature extraction and Area-based feature extraction.

### **Key point**‑**Based Feature Extraction Methods**

#### SIFT: Scale Invariant Feature Transform

SIFT proposed by David Lowe in 2004 solves the image rotation, affine transformations, intensity, and viewpoint change in matching features. The SIFT algorithm has 4 basic steps. First is to estimate a scale space extrema using the Difference of Gaussian (DoG). Secondly, a key point localization where the key point candidates are localized and refined by eliminating the low contrast points. Thirdly, a key point orientation assignment based on local image gradient and lastly a descriptor generator to compute the local image descriptor for each key point based on image gradient magnitude and orientation.

A variant of SIFT is PCA-SIFT (Ke et al. 2004) which is not as robust but faster and more memory efficient. However it requires training for the PCA basis.

#### SURF: Speed up Robust Feature

SURF (Speeded Up Robust Features) is a scale and rotation-invariant detector. It is used for classification tasks and is faster to compute. It is suited for object detection, object recognition or image retrieval. Hessian matrix approximation is used for interest point detection. SURF outperforms SIFT in some papers that I've managed to comb up mostly due to computation speed advantage while accuracy is relatively close to SIFT. Below is a table comparing the two algorithms.

| SIFT                                      | SURF                                                |
|------------------------------------------|-----------------------------------------------------|
| Uses Difference of Gaussians (DoG)       | Approximates DoG with box filters and integral image |
| Keypoints via DoG extrema                | Keypoints via determinant of Hessian matrix         |
| Gradient histograms (128-D descriptor)   | Wavelet responses in subregions (64-D descriptor)   |
| No training data needed                  | No training data needed                            |
| Slower due to Gaussian convolutions      | Much faster using integral images                  |

## Retrieval method for near duplicate image detection

ND image retrieval is to find all the images that are near duplicates to a particular query. A major challenge in the image retrieval is building effective features that are invariant to a wide range of variations. Near-duplicate retrieval is used for the management of multimedia contents. Search speed is a key factor to judge any near duplicate retrieval algorithm. Not only the retrieval time but also maintaining the relevance of returned results is important in image retrieval.

