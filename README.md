# mturk-task-helper

### This repository is for for CGSI project at Robotics Institute, Carnegie Mellon University.
### All of this work is in combination of guided analysis and instructions from Prof. Jessica Hodgins and team.

This repository contains examples required to conduct Mechanical Turk Labelling tasks.
Goal: To receive classification labels and location annotations of vehicles in Images dataset (Aerial view), to analyze results, quality and score the output.

Background: considering current annotated Images dataset as a ground truth, analyze annotations, class labels with minimal worker training on vehicale classes.
Thus making the mehacnical turk worker tasks a little more towards "blind" labelling and annotation tasks.
Ground truth annotations are done by labeller who received one-on-one feedback and follows 2-3 rounds of self-inspection as well as 3 reviewers who re-inspect the labels.

### Known information from Ground truth annotations, Nov-Dec 2020:

* There are only 8 classes of objects to be located and labelled.
* Some classes look similar hence a detailed one-on-one discussion, analysis from other inspectors does decide true label, location of the objects.
* The density of objects (vehicles), spatial distance between the object, the similarity of 2-dimensional shape of the real 3-dimensional objects are key factors
that contribute to increasing the complexity of the locating and classification tasks. Hence understanding such compelxity is the "key" to successful labelling.
* The distribution of 8 classes of objects are not balanced. Due to such imbalanced nature of count of objects and spatial distribution in a given image,
the training.
* The training of labellers is conducted over a period of 1 week and continues to be feedback based to better reduce the disagreement between labels, locations.
* The labelling task assumes locations of objects are correct, hence the more important factor is classification.
* The current labelling tasks use 3 labellers, with one-on-one feedback, discussion occassionally happening.
* Hence the trainer dependency is high in the Ground truth annotation tasks.
* In the ground truth annotation process, labellers are not anonymous. Besides assignment to same labellers repeat over several cycles of labelling tasks.
* Thus Ground truth annotations are more agreed, more defined, with high confidence to certify this as ground truth, along with frequent feedback-based training as well as task execution.
* The decision boundaries, classes, locations are continuously evolving with this feedback-based annotation tasks.
* Class-wise data distribution is expected to be imbalanced.

### Considering the exhaustive nature of Ground truth annotations process, we attempt to find following with the small batch of Mechanical Turk tasks:
* The quality of vehicle locations and labels
* The quality of identification of types of vehicles limited to project's context.
* Minimal amount of online based training without any feedback during training.
* Qualifying a labeller to do task based on assessment after online training.
* To simplify the tasks so labeller give enough time to understand instructions and apply them.
* To analyze accuracy with minimal number of labels, locations
* To decide minimal number of labellers required have 80% agreements between workers' annotations and class labels.
* To finally choose optimal solution to conduct object classification and identification tasks.

### Define Object classification task:
* Given an image with an object marked at center of bounding box, label it to appropriate class label.
* We provide 8 classes of labels to select from. Labeller must have to select a single label from the choices.

### Requirements for Object classification task:
* Create a requester account and worker account.
* Create a sandbox developer (requester + worker) account.
* Define storage and server to host the images for the task.
* Define, create HTML layouts that agree with Amazon Mechanical Turk [crowd-html-layouts library](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-ui-template-reference.html).
* Decide on image size that fits various devices display resolutions workers may use, the type of marker, color of marker.
* Create scripts to generate sub-images as per requirements. 
* Generating sub-images for classification needs object locations, so we use ground truth to generate sub images for each of objects.
* Create AWS S3 account, configure various prequisites and create a storage bucket to store the images and also host as a static website.
* Hosting the AWS S3 bucket is helpful since it also takes care of rendering an image to be loaded on browser using EXIF information and CORS policies per ECMA.
* Upload the images to AWS S3 bucket.
* Decide on total number of object images per each classification HIT task that you'd like to show on a given layout.
* Generate a csv file by retrieving the objects from AWS S3.
* Assess the pricing of total number of images, costs.
* Load the amount to AWS acocunts.

### For viewport devices analysis for display resolution on Mechanical Turk worker website, March 2021:
Please refer the analysis for Mechanical Turk website and HTML layouts using crowd-html-layout Javscript library [viewport-analysis](https://github.com/sushmaakoju/mturk-task-helper/blob/main/docs/device-display-analysis/viewport-sizes.xlsx)

### For analysis of costs for AWS and Mechanical Turk costs, March 2021:
* Please refer costs, analysis for Mechanical Turk tasks, based on images, task based requirements and AWS S3 storage costs: [mechanical-turk-aws-s3-cost-projections](https://github.com/sushmaakoju/mturk-task-helper/blob/main/docs/taskwise-costs-assessments/mechanical-turk-aws-s3-cost-projections.xlsx)

### Configuration document and steps for Mechanical Turk and AWS S3 account, Jan 2021:
* Follow the steps from [Prod, Developer environment configuration](https://github.com/sushmaakoju/mturk-task-helper/blob/main/docs/Prod_Developer_Configuration_steps.docx)

### Steps to server images on GPU from VPN, April 2021:
* Please follow instructions from [stesp-to-configure-xampp-server-to-host-images](https://github.com/sushmaakoju/mturk-task-helper/blob/main/docs/steps-serve-images-without-s3/Steps%20to%20configure%20XAMPP%20Web%20server%20using%20PhP%20to%20host%20images.docx)
* Please refer the script to configure and setup XAMPP server on Linux server. Please do note that this is not tested on GPU over VPN due to access, this has been tested on a Linux VM which may not really be the same. [steps-to-install-xampp-server-NEEDS-CHANGES](https://github.com/sushmaakoju/mturk-task-helper/blob/main/docs/steps-serve-images-without-s3/install_xampp.sh)

### Some of HTML layouts for tasks generated, selected HTML layout candidates are between Feb 5th up until April 7th, 2021:
* Layout with thumbnails with zoom-in on mouse-hover alognside labels [classification hit task with labels with with zoomin on mouse hover](https://mturk-s3-cg.s3.amazonaws.com/mar_23/changes/april/classification_hit_labels3_withhover.html)
* Layout with popup of examples alongside labels [classification hit task with labels and examples in popup window](https://mturk-s3-cg.s3.amazonaws.com/mar_23/changes/april/classification_hit_labels4.html)
* Layout with popup of examples alongside labels with tooltip descriptions on mouse hover[classification hit task with labels in a popup window and on-hover tooltip](https://mturk-s3-cg.s3.amazonaws.com/mar_23/changes/april/classification_hit_labels5.html)
* Object Identification layout [identification_hit_apr7](https://mturk-s3-cg.s3.amazonaws.com/hit_layouts/keypoint_hit_apr7.html)
* Marker color variations follow Polar Chromaticity Coordinates and use RGB in Hue Lightness and Saturation (HLS or HSL follows from) [HLS](https://en.wikipedia.org/wiki/HSL_and_HSV#Color-making_attributes):
    * Auto-select contrasting Marker color using RGB to HLS - to get hue and find contrast , select among Red, Green or Blue color ranges on the marked object
    * Auto-select get exact contrast color: 255-R, 255-G, 255-B value of pixel at center of bounding box of the object (this is most commonly used but is not good since if color was 125, we would get exact same color as contrast)  on the marked object (not an ideal case anyway)
    * Auto-select combine above two methpds: find contrast of current pixel color and convert to HLS and get hue range and pick one bright hue (among among Red, Green or Blue colors) on the marked object
 Please do refer layout with the color variations here: [markers_colors_variations](https://mturk-s3-cg.s3.amazonaws.com/mar_23/changes/april/markers_colors_variations.html)

* Marker size variations impact/resonate Viewport sizes, workers/viewer's device display resolution, zoom/browser configfurations. 
* Please refer the marker size here: [marker_size_variations](https://mturk-s3-cg.s3.amazonaws.com/mar_23/images_with_new_markers1.html)
* Image size, zoom-in/out variations that were compared: 
    *  [classification_hit_300_size](https://mturk-s3-cg.s3.amazonaws.com/hit_layouts/classification_hit_300_size.html)
    *  [classification_hit_400_size](https://mturk-s3-cg.s3.amazonaws.com/hit_layouts/classification_hit_400_size.html)
    *  [classification_hit_500_size](https://mturk-s3-cg.s3.amazonaws.com/hit_layouts/classification_hit_500_size.html)
    *  [classification_hit_300_size_2x](https://mturk-s3-cg.s3.amazonaws.com/hit_layouts/classification_hit_300_size_2x.html)
    *  [classification_hit_400_size_2x](https://mturk-s3-cg.s3.amazonaws.com/hit_layouts/classification_hit_400_size_2x.html)
    *  [classification_hit_500_size_2x](https://mturk-s3-cg.s3.amazonaws.com/hit_layouts/classification_hit_500_size_2x.html)
    *  [classification_hit_300_size_3x](https://mturk-s3-cg.s3.amazonaws.com/hit_layouts/classification_hit_300_size_3x.html)

* Finalized image size: 300*300 pixel, marker color: white, marker type: full diagonal lines intersecting at center of bounding box of object location.

### Analysis of communication process with workers, March 2021:
* For the purpose of confidentiality and main goals of the project, we decided to have less online communication with workers.
* There are several processes followed to create a communication for repeating Mechanical Turk tasks, however this is not the case.
* After each Task, when approving/rejecting, we provide the feedback.

### Amazon Mechanical Turk Best practices for Requesters, March 2021:
* Please refer [Amazon Mechanical Turk best practices](https://mturkpublic.s3.amazonaws.com/docs/MTURK_BP.pdf)

### Analysis of Existing Image classification, identification tasks and Qualification tests, Jan 25th, 2021:
| Total Hit Types filtered by Image tasks | 100 |
| ---------------------|---|	
| BoundingBox tasks    | 10|
| Polygon              | 11|
| Image Tagging	       | 3 |
| Image Summarization  | 2 |
| Image classification | 5 |
| Image Contains	   | 5 |
| Custom Image tasks   | 6 |
| Survey Image tasks   | 12|
| Qualification HITS   | 3 |
| Restricted access	   | 37|
| Non-Image tasks	   | 6 |

* All multi-class Bounding Box HITS used single HIT tasks i.e. the task included both marking bounding box and labelling the bounding box 
from multiple class labels provided in the same HIT task.
* Out of 10 Bounding box Image HITs from 100 Image annotation tasks, 6 were single class and 4 were multi-class tasks.
* It seems to be a commonly used technique that HIT task includes image labelling along with bounding boxes or Polygon together.

### Advantages and Disadtanges of using Crowd HTML Elements Javascript library:
* Adjusts images, alignment of labels based on browser and device viewport configurations of viewers.
* Restricts thirdparty plugins to access the content (since all layouts are served as an iFrame on Mechanical Turk worker weebsite).
* Identifies worker's devices. Restricts type of events on a XHTML Question form template for Qualification tests.
* All qualification tests that require events such as drawing or marking on image (polygon or center of bounding boxes of objects), requires advanced tasks.
* Advanced Qualification tasks need to a HIT task which are paid to reward worker for enrolling to a test for tasks.

### The three final candidate HTML layouts for Classification tasks:
* [The three layouts ](https://github.com/sushmaakoju/mturk-task-helper/tree/main/viz/classification )

### The final candidate HTML layouts for Identification tasks:
* [The final identification task layout ](https://github.com/sushmaakoju/mturk-task-helper/tree/main/viz/identification)

### Steps to create a Qualification test:

* Assuming you have decided on questions and answers to create Qualification test **
* Some of the guidelines to create a Custom Qualification Test are: [Steps to configure](https://github.com/sushmaakoju/mturk-task-helper/blob/main/docs/approaches-analysis/mturk-annotation-tasks-documentation.docx)

### Steps to creating Classification tasks are at [Steps for Classification HIT tasks](src/notebooks/classification/README.md)
### Steps to creating Identification tasks are at [Steps for Identification HIT tasks](src/notebooks/identification/README.md)

### A work-in-progress web HTML layout to display MTurk tasks results, statuses (if hosting to check status is a requirement at some point)
* Replace AWS access key and secret key in the index.html file
* Please refer the work-in-progress html code: [MTurk worker task status viewer](https://github.com/sushmaakoju/mturk-task-helper/tree/main/src/web)
