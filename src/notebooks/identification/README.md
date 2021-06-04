
## Steps to Create Object Identification tasks:
Note: all file paths are absolute, it is not easy to make the paths relative, due to dynamic nature of HTML/XML layouts, csv files and folder path.
* Assumptions for all Jupyter Notebooks/scripts: 
    * You are not just downloading this notebook and running without checking following:
    * This script is for Selwyn Image dataset and ground truth annotations are already available.
    * The path to main image folders contains images, correspondign annotations json file.
    * The Jupyter notebook is only an example and must NOT be attempted as blind run.
    * You will check the paths, foldernames, for config file, image and annotation file pairs.
    * You would have configured AWS S3 storage bucket and updated config file and in corresponding notebook.
    * You would have configured Amazon Mechanical Turk and command line interface AWS CLI
    * Always make sure to run any AWS / Mechanical Turk scripts on sandbox environment i.e. developer sandbox first before running in Prod Endpoint urls. 
    * Although all endpoint urls do point to developer sandbox. It is good idea you get aquainted with this before running on Prod environment.

1. Generate images surrounding the 
* The Jupyter Notebook to generate subimages from each of main image file, annotations json file pairs for Selwyn Image dataset are available: [generate_locations_slices_selwyn_dataset](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/generate_locations_slices_selwyn_dataset.ipynb)
* [Notebook viewer](https://nbviewer.jupyter.org/github/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/generate_locations_slices_selwyn_dataset.ipynb)
* The components covered in this Notebook are:
    * Load config file settings
    * Using sliding window of size 300px with overlap of 20% crop image slices.
    * Validate slices, annotations.
    * For each sliced image, filter annotations available in that cropped image and save to a dictionary
    * Saves each of cropped image
    * For each main image, saves all of the sub image slice locations with corresponding annotation locations that exist in that cropped image, to a csv file.

2. Upload sub images to AWS S3:
* Given an input folder, upload the images from that folder to AWS S3 using Python library boto3.
* The Jupyter notebook: [upload_images_to_s3](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/upload_images_to_s3_identification.ipynb)
* [Notebook viewer](https://nbviewer.jupyter.org/github/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/upload_images_to_s3_identification.ipynb)

3. Generate image urls CSV file, for creating batch of HIT tasks on Mechanical Turk requester website:
* The Jupyter notebook:[generate_csv_image_urls](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/generate_csv_image_urls.ipynb)
* [Notebook Viewer](https://nbviewer.jupyter.org/github/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/generate_csv_image_urls.ipynb)
* If you are using a local server that is serving the sub images, make sure to update the csv file with image urls/update generate csv file image url script.

4. Download objects from S3:
* The Jupyter notebook: [download_from_s3](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/download_from_s3.ipynb)

5. Before heading to Mechanical Turk scripts, the following provides a warm-up for Boto3 library:
* Boto3 warm up and comple MTurk HIT creation details to practice [Mechanical Turk warm-up tutorial](https://blog.mturk.com/tutorial-a-beginners-guide-to-crowdsourcing-ml-training-data-with-python-and-mturk-d8df4bdf2977)

6. Create HIT Layout on Mechanical Turk Requester website:
* Navigate to Requester Sandbox [https://requestersandbox.mturk.com/create/projects/new](https://requestersandbox.mturk.com/create/projects/new)
* Sign in -> click on Create a HIT -> select Image identification -> Update title, description, Rewards (as decided earlier), # of assignments (in this case 3)

<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/identification/describeid-task.PNG" width="400" height="300">
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/identification/settingup-id-task.PNG" width="400" height="300">
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/identification/worker-rqmt-id.PNG" width="400" height="300">
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/identification/save-design-layout.PNG" width="400" height="300">

* depending on which layout you would like to create a HIT task for, once you are on Design layout page during creation of HIT task,
Open one of the layout from [select one of identification layouts](https://github.com/sushmaakoju/mturk-task-helper/tree/main/viz/identification) and copy the layout code and paste it in Design layout editor as follows:
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/identification/design-id-layout.PNG" width="400" height="300">
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/identification/preview-id-task-layout.PNG" width="400" height="300">

* Click finish and exit HIT creation process
* Note down the HIT Layout ID by going to "Create" section -> from among the list of HIT projects displayed, click on title of the HIT project name and it should look like this,
notedown the HIT layout id:
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/identification/hit-layout-id.PNG" width="400" height="300">

* for creating Prod/Live HIT tasks, follow above steps by repeating steps on [https://requester.mturk.com/create/projects/new](https://requester.mturk.com/create/projects/new)

7. Create Batch of HIT tasks for identification tasks:
Note: This section is expected to create a batch of scripts, given a set of image urls hosted on a server.

* For the generated image urls from earlier step and after creating qualification test, make a note of path to csv file with image urls and the Qualification Test ID generated.
* You can always login to requester website and get Qualification Type ID.
* Make a note of HIT Layout ID created from previous step
* Now update the paths in this notebook, provide relevant HTML layout path, layout id, qualification test id [Notebook to be updated with values](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/create_batch_hits.ipynb)
* Once each of HIT is created, all HIT outputs are saved to txt files in src/notebooks/output/hits
* To confirm, you should login to [https://requester.mturk.com/manage](https://requester.mturk.com/manage)
* To confirm that the HIT tasks are published, and as worker you are able to view the HIT tasks, login to [https://worker.mturk.com](https://worker.mturk.com) and search by the title of HIT task you just created it should list HIT task and along with count of batch of HITs tasks that were created.

* If you are using a local server that is serving the sub images, make sure to update the csv file with image urls/update generate csv file image url script.

8. Track status
* To track status, you should login to [https://requester.mturk.com/manage](https://requester.mturk.com/manage)
* Select the batch you would like to check status of:
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/identification/example-batch.PNG" width="400" height="300">


