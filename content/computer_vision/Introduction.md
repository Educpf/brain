---

tags: ["vision"]
---

# What is computer vision


Computer vision is the field that studies how raw data representation of the world ( like camera and sensor input ) can be interpreted. A process that for humans is natural and simple.

This is a necessity, enormous amount of human knowledge and decision-making depends on visual input, and we might want machines operating in that visual world, which requires them to actually "understand" it.
or *Deep learning*.

## History

- 1959 — First digital image scanner. You can't do computer vision without digital images, so this is the hardware foundation.
- 1966 — The Summer Vision Project. The field officially begins, with overconfident ambition.
- 1989 — Yann LeCun proposes CNNs (Convolutional Neural Networks). A revolutionary architectural idea that would define modern CV — but the hardware and data weren't ready yet.
- 1999 — SIFT (Scale-Invariant Feature Transform) by David Lowe. A landmark classical technique for detecting and matching features across images, robust to scale and rotation changes.
- 2001 — Viola-Jones face detection. The first real-time face detection system, which ended up in your camera. A huge practical milestone.
- 2012 — AlexNet. This is the inflection point. A deep CNN trained on GPUs wins ImageNet by a massive margin, launching the deep learning era in CV.
- 2014 — GANs (Generative Adversarial Networks) by Ian Goodfellow. The first framework capable of generating realistic synthetic images — the origin of modern generative AI.
- 2015 — Human-level performance surpassed in image classification. Machines officially beat humans on the ImageNet benchmark. A landmark moment.
- 2018 — Bengio, Hinton and LeCun win the Turing Award (the "Nobel Prize of computing") for their foundational work on deep learning.
- 2021 — DALL·E released. Text-to-image generation becomes publicly accessible, marking the beginning of widespread generative visual AI.

# Levels of Computer Vision

## Low level

Deals with raw image itself, not yet understanding semantics or meaning. These are the building blocks necessary for something meaningfull.

#### Manipulation

- Resizing
- Color space conversion
- Binarization

#### Feature Extraction

- Detecting edges
- Oriented gradients like HOG
- Segmentation regions

## Mid Level

Start to relate images to each other and the real world. Where geometry and physics start playing a role.

- Image to Image: stiching panoramas
- Image to World: 3d images from 2d stereo | depth sensor like LIDAR
- Image to Time: track motion through flow

## High Level

Meaning enters, connecting images to semantics, understanding what is in a scene, generating images, translating between language and content.

- Classification(tagging): which objects image contains?
- Detection: which objects and where? (bounding box)
- 3D detection:
- Semantic Segmentation: detection pixel precise
- Image Generation: 
- Vision + Language: merge between language and vision understanding

# Modules


- Module 1 — Image Basics and Processing: Camera models, image representation, color spaces, filtering. The physical and mathematical foundation.
- Module 2 — Features and Matching: Edges, corners, HOG, SIFT, feature matching, RANSAC, optical flow. Classical CV techniques for finding and relating image content.
- Module 3 — Deep Learning Basics: Neural networks, gradient descent, backpropagation, CNNs and architectures. The engine behind modern CV.
- Module 4 — Semantic Perception: Structured predictions, semantic segmentation, object detection. Applying deep learning to understand scene content.
- Module 5 — Image Generation: GANs, diffusion models, transformers, vision-language models. The generative and multimodal frontier.
