# Setup
- Before running cells, connect to runtime and upload "state_classification_dataset.zip" (provided in 
  canvas) files to runtime files in google colab
- After this, simply run all cells in order

# Set Device
- Sets device to utilize GPU if available, else CPU

# Extract the Zip
- Extracts the zip file containing the image dataset
- IMPORTANT: zip file must already be uploaded and have the exact name provided in canvas

# Resizing Images
- Data preprocessing/augmentation
- Dataset images are resized and normalized to ImageNet standard (which ResNet is trained on)
- Random horizontal flips and rotations are applied to increase dataset diversity

# Loading the Dataset
- Loads training and validation images from dataset in batches of 32
- No explicit train/test logic is necessary because "state_classification_dataset" provides both 
  training and validation folders

# Baseline Model (ResNet18)
- Imports ResNet18 model to use as a baseline
- All but last two layers are frozen to maintain pre-trained weights
- Output layer is modified to ensure correct number of outputs

# CNN Model
- Primary model used for image recognition
- Squeeze and excitation block is used to improve accuracy by pooling the feature maps, producing a set 
  of weights for each channel, and applying those weights to the original feature map, highlighting the 
  priorities
- Hybrid CNN model combines a pre-trained ResNet34 model with custom convolutional layers and a 
  classifier
- First 3 layers of ResNet34 base are maintained
- Custom convolutional layers and classifier layer utilize squeeze and excitation, as well as SiLU, to 
  improve accuracy
- Custom layers are initialized using Kaiming normal initialization
- Forward pass is defined so that data flows first to resnet base, then to custom convolutional layer, 
  then to custom classifier

# Hyperparameters
- Define hyperparameters, specifically the number of epochs and learning rate

# Initialize Model
- Hybrid CNN is set to device
- Loss function, optimizer, and dynamic learning rate scheduler are defined

# Training Loop and Plot of Loss
- Lists are initialized to store across all training/validation loops
- Training and validation loops iterate through all data batches for each epoch, calculating the average 
  loss for the epoch and storing it
- Baseline training/validation loops follow the same process as the custom model loops
- Plots graphs of the training/validation losses for both custom and baseline models

# Check Accuracy
- Defines function for checking a model's accuracy on a given dataset
- Compares predicted class to actual class. If predicted is correct, increment the number of correctly 
  identified images
- Accuracy is number of correctly classified images divided by total number of images

# Run Accuracy Check Based on Unseen Images
- Runs accuracy check on the validation dataset for both the custom and baseline models
- Visualization of accuracy is provided by taking a sample of 5 images from the validation dataset and 
  assigning a label to them
- Images are unnormalized to improve user accessibility
