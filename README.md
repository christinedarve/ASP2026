# Interface for pyAT 
This code is an interface running in Jupyter Notebook or Google Colab, removing the necessity of programming skills, to run basic accelerator physics calculations using pyAT in a "clean" environment.
Many of the calculations, element definitions and analysis from pyAT are NOT available!

The interface is developed to allow easy access for early stage students to use a powerful simulation tool without extensive previous knowledge. The modules in the code are chosen and designed to follow simulation exercises in a course given at Lund University, Sweden.

A strong feature of the implementation is that it is possible to run in the cloud on any device, even your mobile phone.

The main difference in the implementation is that the accelerator lattice is described in a Pandas DataFrame. This makes is easier to change and input the lattice without computer skills, BUT limits the parameters and element types possible to define. This can however easily be expanded for more advanced applications.

Reference:
Using Accelerator Toolbox in a teaching environment, S. Werin, J. Bjorklund Svensson, F. Curbis and J. Lundquist, proceedings IPAC 2026, Deauville, France 2026

## How to run

### Running in Colab
This is directly available through this link: \
https://colab.research.google.com/github/werin99/pyAT_interface/blob/master/pyAT_interface_Colab.ipynb 

This will open Colab in a window in your browser, download the code from here (GitHub) and open it. To save changes you have to save to your own Google Drive. From there you can also store different files for different sessions and make changes.

You can also follow the methods for a standard Notebook below.

### Running in a standard Jupyter Notebook environment
### Method 1 
You can download the pyAT_interface_JN.ipynb directly from here (github/werin99/pyAT_interface)
- Click to open the file
- press the "three dots" at the top right corner and choose "Download"

#### Method 2
- Open a new notebook
- Write and execute a cell with the following code:\
*!git clone https://github.com/werin99/pyAT_interface.git*
- You will get a new folder "pyAT_interface". 
Open the folder and select "pyAT_interface_JN.ipynb"

#### Method 3
In some Jupyter Notebook environments you can directly from the top menu "clone" (download all files) from a GitHub repository.
Clone using this link: \
https://github.com/werin99/pyAT_interface.git

You will get a new folder in the left panel "pyAT_interface". 
Open the folder and select "pyAT_interface_JN.ipynb"

(While the Colab version will run, it will also unnecessarily display a lot of code.)

### Needed modules
Some modules are needed. While many are standard some might be needed to install, especially if you run in your own Jupyter Notebook environment.
"accelerator-toolbox" is in principle never available and has to be installed.
This is done by the code itself when running in Colab and should work also in other environments.
(To install manually use: "pip install accelerator-toolbox")

These modules are needed 
- accelerator-toolbox

These are also needed but are by default available in Colab and EOSC
- numpy
- matplotlib
- pandas
- ipywidgets
- IPython

### Differences
The main difference between the Colab version and the Jupyter Notebook version is that Colab has some more options to not show the code. This gives a more clean interface. In the JN version all code is concentrated in a few initial blocks and the main interface only calls one single function for the relevant part. In Colab the code is still distributed in a more standard Notebook structure.

## How to use
The interface is built to work step-wise with looking at a lattice of a storage ring. 

The interace is built in five modules
- Load codes and initialize things (this module you do not need to care about unless you want to look how things work)
- Lattice module (Define, select and load lattice to use. Display the data of the lattice.)\
(In the Jupyter Notebook version this module is divided into two. "Lattice module" is only for defining the lattice, while "Lattice Selection" is for the rest.)
- Matrices and geometry module (Show transfer matrices of different parts of the lattice and plot the geometry.)
- Optics module (optics parameters, plot beta functions and dispersion, plot the tunespace)
- Tracking module (track a single particle, track a beam, plot real space and phase space)



### Step by step
When your Notebook is open by one of the methods above, you should run the complete notebook. This is typically done by "Run all" which is present in the menus at the top or your window. 
On a mobile phone the menus are often hidden and must be opened by pressing an "arrow down".
The "Run all" command is sometimes found in a sub-menu, typically "Run" or translated into your installed language.

The interface check if Accelerator Toolbox is installed, and will install it if necessary. This can take 10-15 seconds but is only necessary once in each session.

The interface will be prepared to interact with.

To open a module press the arrow ">" beside the title (or "cells hidden"). This will open a set of boxes.

#### Modules
The layout of the modules looks slightly different in the two versions, Colab and Jupyter Notebook, but the functionality is the same.

In Colab
Opening the lattice module will give you three boxes
- User lattices
- Select and make the lattice to use
- Display the lattice

In a Jupyter Notebook
- "User lattices" is a separate module\
Opening the "Lattice module" will give you two sections
- latticeSelector
- displayLattice

In "User lattices" you create a new lattice or change an existing one. Initially you can run with the pre-defined lattices. (see below how to create a lattice)

In "Select and make the lattice to use"/"lattticeSelector" you see a list with defined lattices. Select the one you want.
Below you can define the periodicity of the lattice. Finally press "Make the Lattice" to prepare it for calculations. This is necessary to do before attempting any other calculations!

Here you will get the first test if your lattice will work. If you have defined a new lattice or a lattice that is not a ring you will get ".... AtWarning: Unstable ring...". 
While your lattice is not ok, you can still work with some of the options in the following modules (geometry, tracking).

In "Display the lattice"/"displayLattice" you can show the data of the lattice either in a compact form or the complete form that Accelerator Toolbox uses internally. 
You can also clear the output as is can be inconveniently long.

The following modules should be mostly self explanatory.

#### Defining a lattice
In the Lattice module open the box "User lattices (Open to edit lattices)". Doing this show the python code of the module.
A few lines down you will find a section starting with: "# Copy or change from HERE" and a bit further down: "# Copy and change UNTIL here"
You can either change the lines here directly or copy this section directly below to add one more option.

The interface is limited to use a few element types. 
You can define drift sections (Drift), quadrupoles (Quad), sector bending magnets (Sbend) and rectangular bending magnets (Rbend).
(In the given applications sextupoles have been omitted for simplicity, though the chromaticity is calculated. )

The important code is the following:

*myNewLattice=latticeClass()*\
*myNewLattice.name='Empty lattice'*\
*myNewLattice.elements = pd.DataFrame(*\
*{*\
*1: ['Drift', 0,0,0],*\
*2: ['Quad',  0,0,0],*\
*3: ['Sbend', 0,0,0] # no ',' on last element*\
*},*\
*index=['type','length','bending angle','K-value']*\
*)*\
*Lattices.append(myNewLattice) # Add the lattice to the list of lattices*\


To make a new lattice
- Change the name 'Empty lattice'
- Add one numbered line according to the pattern for each element in your lattice.
- Define
type (Drift, Quad, Sbend or Rbend) \
length (in m) \
bending angle (in radians)\
K-value (strength of quadrupole component) \

- Close the "User lattices" box by pressing the arrow down beside the title.
- After defining the lattice re-run the complete notebook ("Run all"). 

In the "Select and make the lattice to use" a new option should now appear with your defined lattice.
