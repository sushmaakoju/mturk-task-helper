
## Steps to Create Object Identification tasks:

### All of this work is in combination of guided analysis and instructions from Prof. Jessica Hodgins and team.
### These tasks are not for worker feedback based analysis. They are for annotations, locations that do not qualify for IRB.

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

### Choice of Data representation, formats and reason for choosing the preferred data types
    1. Batch results are saved as csv format from Mechanical Turk website.
    2. We use Pandas DataFrame option to perform easier groupby/inner or outer joins on data.
    3. DataFrames give flexibility to view same results in various representations.
    4. For confusion matrix, a more elaborate statistics library that has been used: PyCM [Multi-class confusion matrix](https://github.com/sepandhaghighi/pycm)    

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

10. Downloading Batch results
* To track status, you should login to [https://requester.mturk.com/manage](https://requester.mturk.com/manage)
* Select the batch you would like to download status of:
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/classification/batch-status-completed.PNG" width="400" height="300">

* Click on results -> Click on Download csv.
* Once results are ready, you can download and save it to foldernamed "batch_results" under `src/notebooks/classification/batch_results`.

11. How to evaluate results:
* Assuming you have folders structures with answers downloaded
* This is shared individually with the team so the folders, csv files are not part of this

    1. Analyze received batch results file and its contents. For each worker, extract annotations and slice from main images of Selwyn dataset and mark the annotations and save images.

    [plot_worker_annotations_from_main_images](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/plot_worker_annotations_from_main_images.ipynb)
    [Notebook Viewer](https://nbviewer.jupyter.org/github/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/plot_worker_annotations_from_main_images.ipynb)

    2. Visualize ground truth vs workers' annotations on main images to understand noisy locations vs true locations.

    [generate_all_locations_visualization_plots](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/generate_all_locations_visualization_plots.ipynb)
    [Notebook Viewer](https://nbviewer.jupyter.org/github/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/generate_all_locations_visualization_plots.ipynb)

    3. Now generate Non Maximum suppression location results from all workers' annotations combined. 

    [nms](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/nms.ipynb)

    4. Now generate True positives, False positives, Fasle negatives and True negatives a) workerwise annotations and b) image wise annotations

    [nms](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/nms.ipynb)

    [Notebook Viewer](https://nbviewer.jupyter.org/github/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/identification/nms.ipynb)

    5. Some of the results are generated, consolidated for more visualization the analysis and uploaded as a HTML file. From this hosted page, [](https://mturk-s3-cg.s3.amazonaws.com/task2_results/worker_wise_annotations.html)
        1. You can see worekrwise annotations marked
        2. Groud truth annotations marked on images
        3. Fast NMS results

        This provides a concise and quick idea of how results look like. Generally, for each batch of HIT tasks, its not a bad idea to just upload resulting images from above scripts and update image urls,
        to present results, as needed.
