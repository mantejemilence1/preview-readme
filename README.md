# Augmentio

Augmentio is a python based application used for removing background and replacing it with images and videos.We have used only images in this. The foreground in the application is the person itself.We have used yolact_minimal model as our ML model.The files newdesg.py and eval.py are the main files we have edited (there are other python modules and deep learning model as well along with other files),newdesg.py is for the gui display which is made using pyqt5 designer  and eval.py contains the runner function which is used for running the application. Right now this app is without qr code.

# Steps to reproduce code with QR code added

1.Add the below lines after the import

```

def image_resize(image, width = None, height = None, inter = cv2.INTER_AREA):
    # initialize the dimensions of the image to be resized and
    # grab the image size
    dim = None
    (h, w) = image.shape[:2]

    # if both the width and height are None, then return the
    # original image
    if width is None and height is None:
        return image

    # check to see if the width is None
    if width is None:
        # calculate the ratio of the height and construct the
        # dimensions
        r = height / float(h)
        dim = (int(w * r), height)

    # otherwise, the height is None
    else:
        # calculate the ratio of the width and construct the
        # dimensions
        r = width / float(w)
        dim = (width, int(h * r))

    # resize the image
    resized = cv2.resize(image, dim, interpolation = inter)

    # return the resized image
    return resized
img_path = './images/qr.jpeg'
logo = cv2.imread(img_path)
#print(logo.shape)
logo=image_resize(logo,height=100)
h_logo, w_logo, _ = logo.shape

```


2. Add these lines after setWindowProperty("portrait_segmentation", cv2.WND_PROP_FULLSCREEN, cv2.WINDOW_FULLSCREEN)

```           
img1=frame_buffer.get()
h_img,w_img, _ = img1.shape
center_y = int(h_img / 2)
center_x = int(w_img / 2)
top_y = int(1.5*(center_y))- int(h_logo / 2)
left_x = int(1.5*(center_x)) + int(w_logo)
bottom_y = top_y + h_logo
right_x = left_x + w_logo
# Get ROI
roi = img1[top_y: bottom_y, left_x: right_x]
                        
#Add the Logo to the Roi
                        #print(roi.shape,logo.shape)
result = cv2.addWeighted(roi, 1, logo, 1, 0)
img1[top_y: bottom_y, left_x: right_x] = result
cv2.imshow('portrait_segmentation', img1)

```


## Installation
Right now we have a folder containing newdesg.py file and eval.py so this is not installable


## Usage
For using this application we use only newdesg.py file and which can be used by the following code.
```
python newdesg.py
```

# Steps for creating exe build from the python code

1.Use pyinstaller to create exe of the newdesg.py file by using code
```
pyinstaller newdesg.py
```
2.exe is created which can be located under dist/newdesg 

3.To create installer we use isso setup tool

# Steps to create deb package

1.Use pyinstaller to create exe of the newdesg.py file by using code
```
pyinstaller newdesg.py
```
2.Use the sample debian folder files and place all the code of newdesg.py in newd/usr/bin/newdesg 

3.To build deb package now we go to parent folder of DEBIAN and go to terminal and type

```
dpkg -b newd
```
