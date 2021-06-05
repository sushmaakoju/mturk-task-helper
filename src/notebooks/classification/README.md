
## Steps to Create Classification tasks:
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

1. Generate Sub images
* The Jupyter Notebook to generate subimages from each of main image file, annotations json file pairs for Selwyn Image dataset are available: [](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/generate_subimages_for_classify_hit-changes.ipynb)
* [Notebook viewer](https://nbviewer.jupyter.org/github/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/generate_subimages_for_classify_hit-changes.ipynb)
* The components covered in this Notebook are:
    * Load config file settings
    * Draw marker
    * Crop subimages for 300px side length, white marker color
    * Validate cropped images
    * For each cropped image, filter annotations available in that cropped image and save to a dictionary
    * Saves each of cropped image
    * For each main image, saves all of the sub image slice locations with corresponding annotation locations that exist in that cropped image, to a csv file.

2. Upload sub images to AWS S3:
* Given an input folder, upload the images from that folder to AWS S3 using Python library boto3.
* The Jupyter notebook: [upload_images_to_s3](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/upload_images_to_s3.ipynb)
* [Notebook viewer](https://nbviewer.jupyter.org/github/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/upload_images_to_s3.ipynb)

3. Generate image urls CSV file, for creating batch of HIT tasks on Mechanical Turk requester website:
* The Jupyter notebook:[generate_csv_image_urls](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/generate_csv_image_urls.ipynb)
* [Notebook Viewer](https://nbviewer.jupyter.org/github/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/generate_csv_image_urls.ipynb)
* If you are using a local server that is serving the sub images, make sure to update the csv file with image urls/update generate csv file image url script.

4. Download objects from S3:
* The Jupyter notebook: [download_from_s3](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/download_from_s3.ipynb)

5. Before heading to Mechanical Turk scripts, the following provides a warm-up for Boto3 library:
* Boto3 warm up and comple MTurk HIT creation details to practice [Mechanical Turk warm-up tutorial](https://blog.mturk.com/tutorial-a-beginners-guide-to-crowdsourcing-ml-training-data-with-python-and-mturk-d8df4bdf2977)

6. Create Qualification test 
* Create a an XML format questions template. Refer question template [](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/layouts/questions.xml)
* Create a qualification test with following script: [Create Qualification test for Classification tasks](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/create_qualtest_hit.ipynb)
* For answer key: <refer this link>

7. Create HIT Layout on Mechanical Turk Requester website:
* Navigate to Requester Sandbox [https://requestersandbox.mturk.com/create/projects/new](https://requestersandbox.mturk.com/create/projects/new)
* Sign in -> click on Create a HIT -> select Image classification -> Update title, description, Rewards (as decided earlier), # of assignments (in this case 3)

<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/classification/describe-task.PNG" width="400" height="300">
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/classification/settingup-task.PNG" width="400" height="300">
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/classification/worker-rqmt.PNG" width="400" height="300">
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/classification/save-design-layout.PNG" width="400" height="300">

* depending on which layout you would like to create a HIT task for, once you are on Design layout page during creation of HIT task,
Open one of the layout from [select one of Classification layouts](https://github.com/sushmaakoju/mturk-task-helper/tree/main/viz/classification) and copy the layout code and paste it in Design layout editor as follows:

<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/classification/design-layout.PNG" width="400" height="300">
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/classification/preview-task.PNG" width="400" height="300">

* Click finish and exit HIT creation process
* Note down the HIT Layout ID by going to "Create" section -> from among the list of HIT projects displayed, click on title of the HIT project name and it should look like this,
notedown the HIT layout id:

<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/classification/hit-layout-id.PNG" width="400" height="300">

* for creating Prod/Live HIT tasks, follow above steps by repeating steps on [https://requester.mturk.com/create/projects/new](https://requester.mturk.com/create/projects/new)

8. Create Batch of HIT tasks for Classification tasks:
Note: This section is expected to create a batch of scripts, given a set of image urls hosted on a server.

* For the generated image urls from earlier step and after creating qualification test, make a note of path to csv file with image urls and the Qualification Test ID generated.
* You can always login to requester website and get Qualification Type ID.
* Make a note of HIT Layout ID created from previous step
* Now update the paths in this notebook, provide relevant HTML layout path, layout id, qualification test id [Notebook to be updated with values](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/create_batch_hits.ipynb)
* Once each of HIT is created, all HIT outputs are saved to txt files in src/notebooks/output/hits
* To confirm, you should login to [https://requester.mturk.com/manage](https://requester.mturk.com/manage)
* To confirm that the HIT tasks are published, and as worker you are able to view the HIT tasks, login to [https://worker.mturk.com](https://worker.mturk.com) and search by the title of HIT task you just created it should list HIT task and along with count of batch of HITs tasks that were created.

* If you are using a local server that is serving the sub images, make sure to update the csv file with image urls/update generate csv file image url script.

9. Track status
* To track status, you should login to [https://requester.mturk.com/manage](https://requester.mturk.com/manage)
* Select the batch you would like to check status of:

<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/classification/example-batch.PNG" width="400" height="300">

10. Downloading Batch results
* To track status, you should login to [https://requester.mturk.com/manage](https://requester.mturk.com/manage)
* Select the batch you would like to download status of:
<img src="https://github.com/sushmaakoju/mturk-task-helper/blob/main/images/classification/batch-status-completed.PNG" width="400" height="300">

* Click on results -> Click on Download csv.
* Once results are ready, you can download and save it to foldernamed "batch_results" under `src/notebooks/classification/batch_results`.

11. How to evaluate results:
* Assuming you have folders structures with answers downloaded
* This is shared individually with the team so the folders, csv files are not part of this

    1. Analyze received batch results file and its contents. For each worker, extract labels and compare with answers. 

    [classification_task_scoring](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/classification_task_scoring.ipynb)
    [Notebook Viewer](https://nbviewer.jupyter.org/github/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/classification_task_scoring.ipynb)

    It is interesting to note and is also easy to calculate the confusion matrix for each worker's submitted labels.
    Since we have known class labels, it is easier to text answer labels from answers.

    2. We need to find workerwise agreements, Cohen Kappa score is statistic that is used to measure inter-rater reliability.
    [Cohen Kappa Score](https://en.wikipedia.org/wiki/Cohen%27s_kappa)
    [cohen_kappa_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.cohen_kappa_score.html)

    2. Another goal of executing a small batch of HIT tasks for classification is to check how many workers would be enough to get best accuracy.
       We need to identify voted accuracy of workers. There are two voted accuracy groups to consider: a) 5 choose 3 workers and b) 5 workers.
       Invidual code-level comments are available in the notebooks.

    [accuracy_by_voting](https://github.com/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/accuracy_by_voting.ipynb)
    [Notebook Viewer](https://nbviewer.jupyter.org/github/sushmaakoju/mturk-task-helper/blob/main/src/notebooks/classification/accuracy_by_voting.ipynb)


