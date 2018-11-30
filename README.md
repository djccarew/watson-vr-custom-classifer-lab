
#  Create a Watson Visual Recognition custom classifier to detect wedding scenes

[The Watson Visual Recognition service](https://www.ibm.com/watson/services/visual-recognition/) can be trained to recognize your own custom classes in images. In this lab you'll learn how to  train a  Watson Visual Recognition custom classifier  to recognize  wedding scenes using 50 images of weddings to serve as positive examples  and 50 images that are not weddings to serve as  negative examples.

After creating the custom classifier, you'll test it with different images not used to train it to validate it's accuracy.  


## Prerequisites

* An [IBM Cloud Account](https://console.bluemix.net)

* A space in IBM Cloud US South or United Kingdom regions.

As of 2/5/2018, the Machine Learning service on IBM Cloud is only available in the US South or United Kingdom regions.

## 1. Create an instance of the Watson Studio Service
Watson Studio is your IDE for various Watson Services,  Machine Learning and Data Science, combining opensource tools, and libraries into a unified Cloud based platform for discovering and sharing insights. For this lab we're using the Visual  Recognition tooling to create an test a custom classifier.

1. In your browser go to the [IBM Cloud Dashboard](https://console.bluemix.net/dashboard/apps) and click `Catalog`.

2. In the navigation menu at the left, select `AI` and then select `Watson Studio`.

  ![](doc/source/images/watson-studio-service.png?raw=true)

3. Verify this service is being created in the `Dallas region`, and you've selected the `lite/free` pricing plan.

Note the `lite/free` plan only allows you to add a single user to your project, and is limited in the compute capacity hours.  More details on limits and how to monitor usage is available in the [documentation](https://dataplatform.cloud.ibm.com/docs/content/admin/monitor-resources.html?context=analytics&linkInPage=true).

  ![](doc/source/images/watson-studio-create.png?raw=true)

4. Click `Create`

5. Launch your newly created Watson Studio Environment by clicking `Get Started`

  ![](doc/source/images/launch-watson-studio.png?raw=true)



## 2. Welcome to Watson Studio

IBM Watson Studio is a collaborative environment with AI tools that you and your team can use to collect and prepare training data, and to design, train, and deploy machine learning models.

Ranging from graphical tools you can use to build a model in minutes, to tools that automate running thousands of experiment training runs and hyperparameter optimization, Watson Studio AI tools support popular frameworks, including: TensorFlow, Caffe, PyTorch, and Keras.

You can think of Watson Studio AI tools in four categories:

* Visual recognition
* Natural language classification
* Machine learning
* Deep learning

Documentation is available [here](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html?context=analytics)

#### Overview Landing Page

  ![](doc/source/images/watson-studio-overview.png?raw=true)

   1. **Projects** - Organize resources used when working with data; here you see your most recently updated projects
   2. **Toos** - Quick links to commonly used Data Science and ML Tools including RStudio, Data Refinery, Jupyter Notebooks, or a Visual Neturl Network Model Builder
   3. **Catalog** - Create and manage data policies for managed or connected data resources
   4. **Community** - Links to the best content found by IBM Data Scientists, including example notebooks, datasets, and tutorials
   5. **Services** - Create Watson, data, and compute services and connections. Such as Watson Visual Recognition, or Apache Spark
   6. **Manage** - Account wide configuration, including Anaconda environments, security, catalogs, billing
   7. **Hamburger Menu** - Access to IBM Cloud dashboard and tools
   8. **IBM Studio Menu** - Quick link to the Watson Studio welcome page
   9. **Account Profile and Settings** - Personal account settings

## 3. Create a project in Watson Studio and upload training data
A project is how you organize your resources to achieve a particular goal. Your project resources can include data, collaborators, and analytic assets like notebooks and models. Projects depends on a connection to object storage to store assets. Each project has a separate bucket to hold the project's assets.

1. From the Watson Studio dashboard getting started display, click on `Create Project`, or `New Project`

  ![](doc/source/images/new-project.png?raw=true)

2. Select project type. There are many different tools built into Watson Studio and multiple views are built to simplify the features shown to users.  Select the `Visual Recognition`  and click `Create Project`.

  ![](doc/source/images/vr-project.png?raw=true)

3. A Watson Studio Visual Recognition project depends on an instance of the Watson Visual Recognition Service.  If you don't already have an instance of Watson Visual Recognition created you can create a new instance of the service directly from the New Project dialog. Under `Define Watson Visual Recognition` click `Add`. In the Watson Visual Recognition service creation menu, accept the default options and click  on  `Create`.  

4. With the Visual Recognition service instance created,  click `Refresh` allowing Watson Studio to discover the newly created service. Enter _Wedding Detection_ as the project name and click `Create`.

5. From within the new project click on the pencil icon next to the default name of the new custom classifier and change the name to `Weddings`

  ![](doc/source/images/rename-default-classifier.png?raw=true)

  In the  panel on the right labeled _Upload to project_ , click on `Browse` to upload the image file you'll use to create a classifier. Select the file ***weddings50.zip*** in the ***training_data*** sub folder of this project.

  Repeat for the file ***notweddings50.zip*** in the ***training_data*** sub folder of this project.


## 4. Train your custom classifier


1. Click on `Create a class` and name the class _wedding_.

  ![](doc/source/images/create-class.png?raw=true)

2. Select the file **wedding50.zip** at the bottom right of the UI and drag it unto the tile created for the  `wedding` class you just created. It will take a few seconds for all 50 images in the file to be processed.

  ![](doc/source/images/wedding-images.png?raw=true)

  Repeat for the file **notwedding50.zip** dropping it onto the tile for negative examples

  ![](doc/source/images/notwedding-images.png?raw=true)

3. Click on the `Train model` button

  It will take a few minutes to train the model. Now would be a good time for a cup of coffee.


## 5. Test your custom classifier using the GUI tool

1. Click on the link to the testing tool

 ![](doc/source/images/test-tool-link.png?raw=true)

2. Click on the `Test` tab

 ![](doc/source/images/test-tab.png?raw=true)


3. Click on the `browse` link and select a file in ***test_data/wedding*** sub folder of this project.

![](doc/source/images/browse-link.png?raw=true)

The results will indicate how confident your classifier is that the image you selected is a wedding picture. (1.0 being the highest confidence and 0 being the lowest).

![](doc/source/images/positive-example.png?raw=true)

4. Click on `Clear results`. Click on the `browse` again and select a file in ***test_data/notwedding*** sub folder of this project. Generally the confidence for non wedding pictures should be significantly lower.



## 6. Get the credentials needed to access the classifier programmatically

1. Switch over to the `Implementation` tab

2. Click on the icon next to the `Model ID` to copy it to the clipboard

  ![](doc/source/images/model-id.png?raw=true)

3. Create a text file and paste the copied Model ID. Leave the text file open.

4. From the menu select `Services->Watson Services`

  ![](doc/source/images/watson-services.png?raw=true)

5. In the `Visual Recognition` section click on the name of your service instance

6. Click on the `Credentials` tab

  ![](doc/source/images/service-credentials.png?raw=true)

7. Click on the icon to display the service credentials and then copy the `apikey` value to the text file you opened earlier.

  ![](doc/source/images/api-key.png?raw=true)


## 7. Test the classifier programatically

This will test the classifier using 60 test images via a standalone app

#### Requirements
You can install Python/Java locally or use Docker

#### Docker
If you use Docker you can run either app without additional software installs

Docker requirements
[Docker for your platform](https://docs.docker.com/install/)


#### Local Java or Python
If you don't use  Docker then you'll need one of the following installed locally

Python:

  - [Python 3.5](https://www.python.org/downloads) or later.

Java:

  - Java 1.7 or later JVM
  - [Gradle Build Tool](https://gradle.org) Note: Version 4.7 was used to verify this example


### 7.1 Test with a Python app

7.1.1 Edit the file ***settings.py*** Edit the file ***settings.properties*** in the ***watson-vr-python-tester*** sub folder of this project. Put in the values of your **apikey** and **Model ID** that you saved earlier. Note the **Model ID** is referred to as the **CLASSIFIER_ID** in the settings file.

7.1.2 In a command prompt or terminal navigate to the ***watson-vr-python-tester*** sub folder of this project.

### Docker

Run the following commands to get a bash shell in a Python environment to run the subsequent commands

`docker run -it --rm -v "$(cd ../ && pwd):/repo"  python:3.5-jessie  bash`

`cd /repo/watson-vr-python-tester`

### Locally installed Python
Run the subsequent commands  from your command prompt

7.1.3 Run the following command to install the dependencies

`pip install -r requirements.txt`

7.1.4 Run the following command to run the tester application

`python watsonvr-wedding-tester.py`

7.1.5 Verify the app runs without errors and the output looks like the following.

```
Classifying 60 test images ...
Finished classifying  10 images
Finished classifying  20 images
Finished classifying  30 images
Finished classifying  40 images
Finished classifying  50 images
Finished classifying  60 images
Number of files 60
True positives 27
True negatives 30
False positives 0
False negatives 3
Accuracy  95.00%
False negative list
../test_data/wedding/0006.jpg
../test_data/wedding/0029.jpg
../test_data/wedding/0023.jpg
```

7.1.6 Look at the files that were reported as false positives or false negatives and see if you can guess why the classifier had problems with these particular images.


### 7.2 Test with a Java app

7.2.1 Edit the file ***settings.py*** Edit the file ***settings.properties*** in the ***watson-vr-java-tester/src/main/resources*** sub folder of this project. Put in the values of your **apikey** and **Model ID** that you saved earlier. Note the **Model ID** is referred to as the **CLASSIFIER_ID** in the settings file.

7.2.2 In a command prompt or terminal navigate to the ***watson-vr-java-tester*** sub folder of this project.

### Docker

Run the following commands to get a bash shell in a Python environment to run the subsequent commands

`docker run -it --rm -v "$(cd ../ && pwd):/repo"  gradle:slim  bash`

`cd /repo/watson-vr-java-tester`

### Locally installed Java
Run the subsequent commands  from your command prompt

7.2.3 Run the following command to build the app

#### Linux/Mac
`./gradlew build`

#### Windows
`gradlew.bat build`

7.2.4 Run the following command to run the tester application

#### Linux/Mac
`./gradlew run`

#### Windows
`gradlew.bat run`


7.2.5 Verify the app runs without errors and the output looks like the following.

```
Classifying 60 test images ...
Finished classifying  10 images
Finished classifying  20 images
Finished classifying  30 images
Finished classifying  40 images
Finished classifying  50 images
Finished classifying  60 images
Number of files 60
True positives 27
True negatives 30
False positives 0
False negatives 3
Accuracy  95.00%
False negative list
test_data/wedding/0006.jpg
test_data/wedding/0029.jpg
test_data/wedding/0023.jpg
```

7.2.6 Look at the files that were reported as false positives or false negatives and see if you can guess why the classifier had problems with these particular images.

# Conclusion
Congratulations ! You successfully created a classifier to detect wedding pictures. With just 50 examples and 50 negative examples your were able to quickly created a classifier that is approximately  95% accurate on some randomly selected  test examples.


# License
[Apache 2.0](LICENSE)
