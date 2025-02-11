# YUHS-VERTE-X

Code for "Deep‐Learning‐Based Detection of Vertebral Fracture and Osteoporosis Using Lateral Spine X‐Ray Radiography" and "Deep learning-based identification of vertebral fracture and osteoporosis in lateral spine radiographs and DXA VFA to predict incident fracture". 

## 12 Jan 2025: Deep learning-based identification of vertebral fracture and osteoporosis in lateral spine radiographs and DXA VFA to predict incident fracture

We investigated the prognostic value of deep learning scores to detect vertebral fracture and osteoporosis for future incident fracture in lateral spine radiographs and DXA VFA in hospital-based population and community-based older adults. The improvement of discriminatory ability and clinical utility by adding deep learning scores upon models of clinical risk factors was evaluated. The feasibility of transfer learning of CNN models from different spine image domains was tested. The pre-trained DL models from lateral spine radiographs were then fine-tuned in 30% of a DXA VFA dataset (KURE cohort), with performance evaluated in the remaining 70% test set.

The publication will be made available soon.

### X-ray vs. DXA VFA

<p align="center" width="100%">
    <img width="100%" src="./img/xray_vs_vfa.jpg"> 
    <em>Comparison of Imaging Equipment and characteristics for Spine Radiograph and DXA VFA</em>
</p>


### Image processing

Histogram equalization was applied to all images due to heterogeneous intensity distribution in images. Min-max scaling was used to normalize the intensity values. As lateral spine radiographs and DXA VFA images differ in resolution sizes, the width was resized to 512 pixels for spine radiographs, along with proportional height adjustment to maintain the original aspect ratio in the images. For DXA VFA images, where the height consistently exceeds the width, the height was resized to 1024 pixels, and the width was scaled accordingly. Images smaller than the target size (1024 × 512) were centered, with zero-padding applied to fill the remaining areas. For the image resolution, the maximum usable image resolution is limited to 1024x1024 in general settings due to hardware constraints during model training. While resizing images to meet this resolution, loss of some information was inevitable. However, the morphological features of spine were preserved during the downscaling of large images such as spine radiographs. This resizing process reduced the image quality differences related to manufacturers or image acquisition time, which helped models to focus on learning clinically relevant structural and morphological features.

### Data augmentation
Data augmentation techniques, such as Gaussian noise addition, rotation, and scaling were applied to augment the images as training resource. These augmentations increased the difficulty of the learning process, even for high-quality medical images, facilitating models to learn features that are robust to noises and artifacts. As a result, the models would become less reliant on variable visual features associated with manufacturers or data collection time.

### Transfer learning
In our study, the model was fine-tuned using approximately 30% of the VFA dataset, which was selected through random stratified sampling. This approach allowed the model to leverage the generalizable features learned from the large X-ray dataset and adapt them to the smaller VFA dataset.

<p align="center" width="100%">
    <img width="100%" src="./img/fine-tuning.png"> 
    <em>The transfer learning architecture for training the VFA VERTE-X model. Abbreviations: VFA, vertebral fracture assessment; pVF score, deep learning-based score to identify prevalent vertebral fracture; osteo score, deep learning-based score to identify presence of osteoporosis.</em>
</p>

## 10 April 2023: Deep‐Learning‐Based Detection of Vertebral Fracture and Osteoporosis Using Lateral Spine X‐Ray Radiography

We propose a deep learning method for detecting prevalent vertebral factrures and osteoporosis on X-ray images: given an X-ray input image and clinical variables(Age, Sex, BMI), our model provides the risk scores of diseases. 

For more detail, please check our [**Paper**](https://academic.oup.com/jbmr/article/38/6/887/7512425?login=false)!

*Namki Hong, Sang Wouk Cho, Sungjae Shin, Seunghyun Lee, Seol A Jang, Seunghyun Roh, Young Han Lee, Yumie Rhee, Steven R. Cummings, Hwiyoung Kim, Kyoung Min Kim, Deep‐Learning‐Based Detection of Vertebral Fracture and Osteoporosis Using Lateral Spine X‐Ray Radiography, Journal of Bone and Mineral Research, Volume 38, Issue 6, 1 June 2023, Pages 887–895, https://doi.org/10.1002/jbmr.4814*

### Image processing
Because of the intensity difference in individual images, histogram equalization was applied to all images, and Minmax scaling was selected as a normalization method to indicate the relative degree of intensity in the images. 

Considering that the X-ray image, 5% of the image size was cropped for areas not related to analysis, and resized according to the size (1024, 512). Since the width and height sizes may differ for each image, if the width was larger than the height, set the width to 512px, calculate the height according to the resolution ratio, and resize it. If the image was smaller than (1024,512), align the image to the center position and give Zero-padding to the rest of the area. These processed images were used as input for the model.

### Deep learning processing
Our deep learning models predicted the pVF-score and osteoVF-score, the risk scores of prevalent vertebral fracture and osteoporosis, by receiving spine X-ray images, respectively. It was built based on the Efficientnet-b4 model using Adam optimizer with the initial learning rate of 1e-4. To the prevent over-fitting, weight decay was applied to the models. Due to the size of spine X-ray images and memory limitations, the batch size was set to 30. The experiments were up to 100 epochs on 4 NVIDIA Tesla V100 Graphic processor units (GPU). 
