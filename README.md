# OCR_Persian_textDeceter:

## The purpose of this project:
* building and OCR model from the scratch to detect Persian text in pictures and turn it to a useful app in the future
* creating the datasets and implementing the steps of Optical Character Detection system


## Creating the Dataset:
The first step in this project is to create the dataset. the dataset should compose of numbers of images with Persian text on them. To create these images:


+ first I used numpy library particulary the zeros function to create an blanck image :

`img = np.zeros((300, 300, 3), np.uint8)`

The function zeros in Numpy library takes 2 argument to create blank images, the first argument is the shape of the array that we want to create, so we give the np.zeros function a  tuple containing three numbers: width, height and the number of channels of image. And the second argument determines the type of the data of this array, for which we set unit8 (integer numbers). The function will return a 300*300 array filled with zeros represents a black blank image. The third number in the tuple represents the 3 RGB channels.

+ The next step was putting text on image:


Firstly I tried putting English text on the image by using OpenCV library. I used cv2.putText function to put the English text as it is shown below:


`cv2.putText(img, 'Hejaz',(80, 150), font, 2, (255, 255, 255), 6, cv2.LINE_AA )`


The function takes 8 arguments:


`cv2.putText(image, the text, left-bottom coordinates of the text position , font, font-size, colour of the text, thikness of text, cv2.LINE_AA )`

+ After putting the text on the image I use for loops to create numbers of images with English text on them. 

But unfortunately, the opencv library does not contain any arabic or Persian fonts, and I was not able to install freetype-py, so I had to used pillow library to create the dataset.


## using PILLOW library to create image with Persian text:

### Creating blank images:

- to creat blank images we need firstly to import the Image module from pillow library, this module contains the new function that helped me to create blank images, this function takes 3 arguments :


`img = Image.new('RGB', (300, 300), (255, 255, 255))`


The arguments of new function are:
 
 1. The mode of the image
 2. The shape of image 
 3. The colour of the image 
     
The colour is given as a tuple of 3 number each of the represent the *red*, the *green* and the *blue* channels of the images and each of them takes a number between 0, 255
the (255, 255, 255)represents the white colour.


### Preparing the Persian text:
After creating the blank images, the next step is to prepare the Persian text.


  * We first needed to install **python-bidi** and __arabic_reshaper__ libraries, and then imported the arabic_reshaper library and the get_display function from bidi.algorithm. 
* We gave the Persian text as a string to reshape function of arabic_reshaper library, then the reshaped text was passed to get_display function.


### Putting the Persian text on the image:
Since the Pillow library has limited number of fonts and does not have any arabic or Persian fonts, we download an external Persian font and passed it to *truetype function* inside the **ImageFont** module of Pillow, to make us able to use it to put text on image and determine the size of the font as well.


To put the text we used *Draw function* of **ImageDraw** module and pass the blank image as an argument and assigned it to *an object*, then we used the *text attribute* of that object to put the text or word on the image:

'''
draw = ImageDraw.Draw(img)
draw.text((100, 150), bidi_word, font=font, fill='blue')
img.save('img.png')
'''
       

In the above mentioned text attribute we passed 4 parameters namely:


- The left-top coordinate of the text position 
- The text we prepared 
- The font_style 
- The colour of the text


Then we saved the image by using *save function* on the img object, otherwise it will return a blank image. 


> One thing to be noticed here, is that in OpenCV library we had to pass the *bottom-left* coordinates of the text, but for the pillow library is the *left-top* coordinates.

### putting each of thses three stages in a function: 
 1. T he function of creating blank image
 2. The function of preparing Persian/Arabic text
 3. The function of putting text on image

Then put thses functions in 2 for loops to create the Dataset of images with Persian text.


For the text, I have used an excel file containing Persian words collected by [REZA MOSHKSAR](https://groups.google.com/forum/#!topic/persian-computing/qM5NxAr344M), converted it to csv file (I have used only *30000* words of the third column in the file), and then passed it as a list to the above mentioned loops to put each of them on seperated images.


To examine the code, I used *matplotlib.pyplot* library to visualize thses images in each steps.


Now the dataset is ready. It is a simple dataset contains 30000 images without any background noise and with just one Persian word on each of them


## The next step is to train a RCNN with this dataset:

We are going to create a **RCNN** network, split the data to *train* and _test_ datasets, then we are going to train and evaluate the *RCNN* by these data.
