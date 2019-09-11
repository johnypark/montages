# montages

## Presentation

**montages** is a Python 3 module to create montages of images taken from a folder or from a stack file (.tif).

![alt text](https://github.com/vivien-walter/montages/blob/master/images/presentation.png "Example of a montage")

**montages** generates automatically the text on each frame.

## Installation

### How-to

**montages** is distributed with a *setuptools* script to directly install the module in the working environment along all the required packages. To install **montages** copy the *montages/* folder and the *setup.py* script of the repository onto your computer and open a terminal to type the following command

```bash
python3 setup.py install
```

Please refer to the [setuptools](https://pypi.org/project/setuptools/) and the [Virtualenv](https://virtualenv.pypa.io/en/latest/) documentations for all the options and arguments that can be used to make the installation as clean as possible.

Alternatively, the module can be used without installation in the working environment by copying the *montages/* folder in a *custom_module/* folder on your computer and calling the path to the latter into your python script header, such as in

```python
import sys
sys.path.insert(0, "/path/to/custom_module/")
import montages as mn
```

### Requirements

The following modules are required to run the montages module.

* [NumPy](https://numpy.org)
* [PIMS](http://soft-matter.github.io/pims/v0.4.1/)
* [Matplotlib](https://matplotlib.org)
* [Pillow](https://pillow.readthedocs.io/en/stable/)
* [scikit-image](https://scikit-image.org)

The setuptools-based installation will automatically install these modules on your computer is you haven't installed them yet.

## Instructions

### Standard functions

#### 1. Loading the images to make a montage with

All the images from a folder can be directly loaded into an object using the **loadImages** function of the module.

```python
import montages as mn

images = mn.loadImages(folder='test_folder/')
```

The module will load all files inside the folder, so be careful to not mix with non-image files or images from a different sequence.

Alternatively, you can load a stack file with the same function.

```python
import montages as mn

images = mn.loadImages(stackFile='test.tif')
```

#### 2. Creating the montage

When the images are loaded into an object, the module generates basic parameters for the montage so it can be used straight.
The montage can therefore be created using the function **makeMontage**.

```python
montageArray = mn.makeMontage(images)
```

The output of the function is a NumPy array containing all the pixel values of the montage.

To edit the parameters of the montage, please refer to the section *Setting the parameters for the montage* below.

#### 3. Saving the montage into a file

Once the pixel value array of the montage has been generated, it can be directly saved into an image file using the function **saveMontage**

```python
mn.saveMontage(montageArray, fileName='output_file.tif')
```

Alternatively, since the montage is here a simple NumPy array, it can be saved by using the method of your choice.

### Setting the parameters for the montage

The images object generated by the **loadImages** function owns several functions that can be used to edit the settings for the montage generation. These functions are detailed below

* **Intensity scaling**

The intensity of all the pixel values of all images can be scaled by a given float value using the function **.scaleIntensity()**. The function can be used such as in the following example

```python
images.scaleIntensity( scaleFactor )
```

* **Selection of the frames for the montage**

The module will automatically select all the frames in the folder/stack to generate the montage. The selection of the frames can be done using the **.setSelection()** function.

```python
images.setSelection( begin=, end=, skip=, maxFrame= )
```

The user can specify the index of the first frame (*begin=*), the index of the last frame (*end=*), the number of images to skip between each frame (*skip=*) and the maximum number of frame to be displayed in the montage (*maxFrame=*). All of these arguments are optional.

* **Time scale of the frames**

The module will automatically assume that the time unit is equal to 1 picture per frame. A specific time scale (*e.g.* 1 picture every 5 ms), the **.setTimeScale()** function can be called. The time scale can be used mostly to write the time stamps on the frames of the montage (see *.setText()* function below)

```python
images.setTimeScale( scale=, unit= )
```

The user can specify the time value between each frame (*scale=*) and the unit used (*unit=*). All of these arguments are optional.

* **Text on the frames**

The module will not automatically print text on the frames. This can be done by using the function **.setText()**

```python
images.setText( textType=, textSize=, textFont=, position=, padding=, color=, resetText= )
```

The function can write either the file name or the time stamp on each frame (*textType=*, pick either 'file' or 'time' options), and the user can specify the size of the font (*textSize=*, if not given it will automatically calculate the size to cover the whole width), the type of the font (*textFont=*, needs a name such as *Arial.ttf*), its relative position in the frame (*position=*, either 'top' or 'bottom') and the padding surrouding the text (*padding=*, given in pixels). The (black or white only) color of the text can also be specified (*color=*, either 'black' or 'white'). To superimpose multiple types of text on each frame, you have to specify that the previous text should not be automatically removed (*resetText=*, default is False to remove the previous text). All of these arguments are optional.

* **Arrangement of the montage table**

The default settings for the montage will generate a square table based on the number of frames to display. The user can control the number of columns and rows, as well as the existence of margins in-between frames, using the **.setMontage()** function

```python
images.setMontage( column=, row=, margin=, blackMargin= )
```

The number of columns (*column=*) and rows (*row=*) can be specified. If only one is given, the other will automatically be calculated. If the total number of cells is less than the number of frames in the selection, the table will be filled with as many frames as possible, and the rest will be discarded.

A margin between the frames can be given (*margin=*, should be a integer in pixels) and the color of the margin can be specified (*blackMargin=*, either True for black or False for white). Empty cells in the table will assume the color of the margins.
