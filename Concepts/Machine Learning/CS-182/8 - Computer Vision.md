# 8 Computer Vision

## So far
- ![[Pasted image 20250516101122.png]]
- So far, we covered classification
	- Convolutional networks -> map image to output value (semantic category)
- Other problems
	- Where is object in the image?
	- For each pixel in image, which object is present?
## Standard computer vision problems
- Object classification
	- ![[Pasted image 20250516101356.png]]
	- Output categorical variable (discrete)
		- Fails to describe scenes with multiple objects
- Object localization
	- ![[Pasted image 20250516101442.png]]
	- More commonly simultaneous object classification and localization
	- Outputs bounding box to describe an object's location
- Object detection
	- ![[Pasted image 20250516101434.png]]
	- Similar to object localization but handles multiple objects
	- Outputs bounding box for all objects 
- Semantic segmentation
	- ![[Pasted image 20250516101517.png]]
	- AKA scene understanding
	- Labelling every pixel with the semantic category of object at the pixel
		- Accounts for shape of object
# Object Localization
## Object localization setup
- Before (classification) -> $\mathcal{D} = \{(x_{i}, y_{i})\}$
	- $x_{i} =$ image
	- $y_{i} =$ label
- Now (localization) -> $\mathcal{D} = \{(x_{i}, y_{i})\}$
	- $y_{i} = (l_{i}, x_{i}, y_{i}, w_{i}, h_{i})$
		- $y$ now includes label $l$, as well as the bounding box described using a position $x,y$, and dimensions $w,h$
## Measuring localization accuracy
- Did we get it right?
	- ![[Pasted image 20250516102054.png]]
- Intersection over Union (IoU)
	- ![[Pasted image 20250516102048.png]]
	- Localization is correct if there is high overlap between true bounding
	- Intersection -> area of intersection
	- Union -> sum of areas of two bounding boxes
	- $\text{IoU} = \dfrac{I}{U}$
	- Different datasets have different protocols, but reasonable one is
		- Correct if $IoU > 0.5$ and class is correct
	- This is not a loss function -> just an evaluation standard
## Object localization as regression
- ![[Pasted image 20250516102525.png]]
- We can add another feed forward network to output the bounding box information
	- Finding the bounding box is a regression task, since it's predicting a continuous values of the variables that describe a bounding box
- These linear networks attached to the convolutional network are known as **heads**
- Very simple design
- Can either train jointly (multi-task), or train with classification first, then train regression
	- Training entire network (including convolutional network + heads)
	- Training the convolutional network with classification head
		- Then freeze the convolutional network and attach and train a new object localization head
- Works okay
- By itself, not the way it's usually done
## Sliding windows
- ![[Pasted image 20250516103619.png]]
- What if we classify every patch in the image?
	- Patches that have highest probability of the label are used as the bounding box
	- Note that the patches are stretched and resized to the input size the classifier expects
- Algorithm for getting all possible crops
	- ![[Pasted image 20250516103856.png]]
	- We can scale image up and then tile the bounding box
		- We can scale with different dimensions in the x and y to stretch horizontally and vertically
	- Could just take the box with the highest class probability
	- More generally -> non-maximal suppression
		- Don't have to pick the box with the highest class probability
		- Other techniques are known as non-maximal suppression
## A practical approach: OverFeat
- ![[Pasted image 20250516104546.png]]
- Both compensate for each others weaknesses
	- Sliding window -> expensive to cover every pixel
		- Might not get the bounding box right
		- Outputting continuous coordinates lets the model fine-tune the coordinates of the box
			- Otherwise we'd have to consider sliding our window pixel by pixel
- Take different patches
	- Each patch we output the label and the coordinates
	- Provides a little "correction" to the sliding window
- How it works
	- Pretrain on just classification
	- Train regression head on top of classification features
	- Pass over different regions at different scales
	- "Average" together boxes to get single answer
- ![[Pasted image 20250516104803.png]]
	- Actual example from the paper
## Sliding windows & reusing calculations
- Problem -> sliding window is very expensive
	- 36 windows (6 along each dimension) = 36x the compute cost
- Sliding windows looks a lot like convolution
- Can we just reuse calculations across windows?
- "Convolutional classification"
	- ![[Pasted image 20250516105317.png]]
	- Pull out chunks from the final layer of the convolution neural network to then feed it into the classifier
		- Recall that each chunk is a list of filters passed over a small portion of the image
		- [[6 - Convolutional Neural Networks]]
- ![[Pasted image 20250516110602.png]]
	- Our previous classifier condenses the input to just a 1x1 output at the end
	- However we can add a few pixels to the input and make it output a 2 x 2 output at the end
		- This output tells you the probability the object occurs at each corner of the image
	- We can treat the linear classifier as a convolutional layer!
		- Before (top) -> 2x fully connected layers with 4096 units
		- Now (bottom) -> 2x convolutional layers with 1x1x4096 filters
			- We use 1x1 filters with a stride of 1 to ensure they don't use adjacent information
			- Note that the from the previous layer has been flatten into 4096 inputs
				- You can think of this as the "depth" of the final output of the convolution layer
				- So our final output has the size of 2x2x4096
					- Therefore our filters have a size of 1x1x4096, as it reads 4096 chunks of information from each corner
- This kind of calculation reuse is extremely powerful for localization problems with conv nets
- We'll see variants of this idea in every method we'll cover today
## Summary
- Building block -> conv net that outputs class and bounding box
- Evaluate this network at multiple scales and different crops
- Implement the sliding window as just another convolution, with 1x1 convolutions for classifier/regressor at end
	- Saves on computation -> no need to rerun entire convolutional network
# Object Detection
## Problem setup
- ![[Pasted image 20250516133353.png]]
- $(x_{i}, c_{i,1}, x_{i,1}, w_{i,1}, h_{i,1}, c_{i,2}, x_{i,2}, w_{i,2}, h_{i,2}, \dots, c_{i,n_{i}}, x_{i,n_{i}}, w_{i,n_{i}}, h_{i,n_{i}})$
	- $x_{i} =$ image
	- $n_{i} =$ Number of objects in current image
		- This can be different for each image $x_{i}$
- How do we output multiple bounding boxes?
## How do we get multiple outputs?
- Sliding window -> each window can be a different object
	- ![[Pasted image 20250516133443.png]]
- Instead of selecting the window with the highest probability, just output an object in each window above some threshold
- Big problem -> high-scoring window probably has other high-scoring windows, we have duplicate windows!
	- Non-maximal suppression -> kill off any detections that have other higher-scoring detections of the same class nearby
- Actually output multiple things -> Output is a list of bounding boxes
	- Declare true bounding box if the probability is hgih enough
	- Drawback -> need to pick a number of outputs, usually pretty small
		- Difficult to train model with many outputs
## Case study: You only look once (YOLO)
- ![[Pasted image 20250516141022.png]]
- Actually, you look a few times (49 times to be exact)
- Divide image into 7x7 cells
- For each cell
	- Output $B$ times:
		- $(x, y, w, h) =$ bounding box information
		- IoU = confidence
			- Set to zero of there is no object
	- Output conditional class probabilities
		- Each cell **only predicts on set of class probabilities per cell**, regardless of number of boxes $B$
- Training details
	- For each cell, the object it should predict is the object with the highest IoU with the box from all of its 
- See the [YOLO paper](https://arxiv.org/pdf/1506.02640) for more information
- **TODO:** Finish computer vision lectures as needed