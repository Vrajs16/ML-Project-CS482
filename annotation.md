DEXTR or Deep Extreme Cut is a CNN architecture for semi-automatic segmentation that turns extreme clicking annotations into accurate object masks by having the four extreme locations represented as a heatmap extra input channel to the network. The different methods it uses are Extreme points and Segmentation from extreme points. Extreme points are one of the most common ways to perform weakly supervised segmentation by drawing a bounding box around the object of interest. While making a bounding box there is an increased error rates and labelling times. A more efficient way of obtaining a bounding box is by using extreme clicks. They found out that by extreme clicking leads to high quality bounding boxes that are on par with the ones obtained by traditional methods. Extreme clicking annotations also provide more information than a bounding box. Furthermore, Segmentation from Extreme Points is what DEXTR uses. The way it works is by creating a heatmap with activations in the regions of extreme points. The heatmap is concatenated with the RGB channels of the input image, to form a 4-channel input for the CNN. Both the RGB image and the labeled extreme points are processed by the CNN to produce the segmented mask. For their architecture they decided to go with ResNet-101 as it has been proven successful in a variety of segmentation methods. Using a pyramid scene parsing module to aggregate global context to the final feature map. The ouput of the CNN is a probability map representing whether a pixel belongs to the object that we want to segment or not. The CNN is trained to minimize the standard cross entropy loss, which takes into account that different classes occure with different frequency in a dataset. The different use cases for DEXTR are Class-agnostic Instance Segmentation, Annotation, Video Object Segmentation and Interactive Object Segmentation. To begin with, Class-agnostic instance segmentation is by clicking on the extreme points of an object in an image and it obtains a mask prediction for it. Secondly, The common annotation pipeline for segmentation can also be assisted by DEXTR. the annotator is reduced to only providing the extreme points of an object, and DEXTR produces the desired segmentation. In Section 4. 4, the quality of the produced masks are trained on the ground-truth annotations in terms of quality. Lastly, Video object Segmentation the pipeline of video object segmentation masks as inputs to produce the segmentation of the whole aim is to replace the costly per pixel annotation masks by the masks produced by our algorithm after and re-train strongly supervised state-of-the-art video segmentation architectures. state-of-the-art results can be achieved reducing the annotation time by a factor of 5. than expensive per-pixel annotated masks. The natural thing to do in such case is to annotate an extra point in the region that segmentation fails, and expect for a refined result. Given the nature of extreme points, we expect that the extra point also lies in the boundary of the object. To simulate such behaviour, first train DEXTR on a first split of a training set of images, using the 4 extreme points as input. For the extra point, we infer on an image of the second split of the training set, and compute the accuracy of its segmentation. Using ResNet-101 as the backbone architecture, and compare two different alternatives. The first one is a straightforward fully convolutional architecture where the fully connected and the last two max pooling layers are removed, and the last two stages are substituted with dilated convolutions. In the first architecture, the input is a patch around the object of interest, whereas in the latter the input is the full image, and cropping is applied at the RoI-Align stage. Came to conclude that the output resolution of ResNet-101-C4 is inadequate for the level of detail that we target. In the second case, the extreme points are fed together in a fourth channel of the input to guide the segmentation. Including extreme points to the input increases performance by +3.1%, which suggest that they are a source of very valuable information that the network uses additionally to guide its output. Class-balancing the loss gives more importance to the less frequent classes, and has been successful in various tasks . DEXTR also performs better when the loss is balanced, leading to a performance boost of +3.3%. Having the extreme points annotated allows for focusing on specific regions in an image, cropped by the limits specified by them. To this end, crop the region surrounded by the extreme points, relaxing it by 50 pixel for increased context and compare it against the full image case. This could be explained by the fact that cropping eliminates the scale variation on the input. Compare the two network heads, the original ASPP , and the recent PSP module for our task. The increased results of the PSP module indicate that the PSP module builds a global context that is also useful in our case. Analyze the differences between the results obtained by DEXTR when we input either human-provided extreme points or our simulated ones, to check that the conclusions we draw from the simulations will still be valid in a realistic use case with human annotators. The first one is a certain subset of PASCAL 2012 Segmentation and SBD with extreme points from , which we refer to as PASCALEXT and DAVIS 2016, for which we crowdsourced the extreme point annotations. Table 1 shows that the results are indeed comparable when using both type of inputs. Focus on segmentation from points use the distance transform of positive and negative annotations as an input to the network, in order to guide the segmentation. Compare with their approach by substituting the fixed Gaussians to the distance transform of the extreme points. Notice a performance drop of -1.3%, suggesting that using fixed Gaussians centered on the points is a better representation when coupled with extreme points. As seen in the previous section, DEXTR is able to generate high-quality class-agnostic masks given only extreme points as input. In this experiment we compare the results of a semantic segmentation algorithm trained on either the ground-truth masks or those generated by DEXTR . Specifically, train DEXTR on COCO and use it to generate the object masks of PASCAL train set, on which we train Deeplab-v2 , and the PSP head as the semantic segmentation network. The results trained on DEXTR’s masks are significantly better than those trained from the ground truth on the same budget. DEXTR’s annotations reach practically the same performance than ground truth when given the same number of annotated images. We compare against the state-of-the-art in interactive segmentation by considering extreme points as 4 clicks.
4 clicks, in PASCAL and the Grabcut dataset. DEXTR reaches about 10% higher performance at 4 clicks than the best competing method, and reaches 85% or 90% quality with fewer clicks. We have presented DEXTR, a CNN architecture for semi-automatic segmentation that turns extreme clicking annotations into accurate object masks; by having the four extreme locations represented as a heatmap extra input channel to the network. The applicability of our method is illustrated in a series of experiments regarding semantic, instance, video, and interactive segmentation in five different datasets; obtaining state-of-the-art results in all scenarios. DEXTR can also be used as an accurate and efficient mask annotation tool, reducing labeling costs by a factor of 10.