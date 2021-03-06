# Street View House Number (SVHN)

Using Tensorflow, I designed a convolutional neural network to classify street view house number images (Google's SVHN dataset) into different labels containing the digits that correspond to the digits in the image.

## Dataset & Format

For this project, I utilized **Format 1** shown below.

- Dataset comes in two formats:
   - **Format 1.**
     - **Original images with character level bounding boxes.**
   - Format 2.
      - MNIST-like 32-by-32 images centered around a single character (many of the images do contain some distractors at the sides).
- Labels
  - 10 classes, 1 for each digit. Digit '1' has label 1, '9' has label 9 and '0' has label 10.
- **Number of digits** in all images for each set
  - 73,257 digits for training, 26,032 digits for testing, and 531,131 additional, somewhat less difficult samples, to use as extra training data

Here are the sizes for the datasets I used in this project:

- **Training set**
  - 225,754 gray scale images of size 32 x 32
- **Validation Set**
  - 10,000 gray scale images of size 32 x 32
- **Test set**
  - 13,068 gray scale images of size 32 x 32


Source: http://ufldl.stanford.edu/housenumbers/

## Goal & Metrics

The target is to correctly measure all the digits in the image. Thus, the metric is accuracy, and is determined by how many images we correctly classify ALL digits. Here are some definitions that I use to evaluate the performance of the model:

![equation](https://latex.codecogs.com/gif.latex?\text{multi-digit&space;accuracy}&space;=&space;\frac{\text{correctly&space;classified&space;images}}{\text{total&space;number&space;of&space;images}})

![equation](https://latex.codecogs.com/gif.latex?\text{individual&space;digit&space;accuracy}&space;=&space;\frac{\text{correctly&space;classified&space;digits}}{\text{total&space;number&space;of&space;digits}})

Although it is straight forward, it is important to formally define these equations.


## Steps
1. Explore & preprocess data ([svhn-preprocessing.ipynb](https://github.com/k-chuang/tf-svhn/blob/master/svhn-preprocessing.ipynb) notebook)
  - Given format 2, we want to read in the provided mat files with the bounding box information, crop the images by the bounding boxes (and expand by a percentage), and resize all these images to a standard 32x32 size.
  - Explore & visualize the data get a better understanding of the dataset
    - Plot distributions of the number of digits in each image (0-5 digits per image)
    - Plot frequency of digits (0-9) in all images
    - Use these distributions to properly split training data to train & validation sets
      - Keep the distributions similiar to prevent imbalanced classes in either set
  - Finally, transform the images from RGB to gray scale & normalize these images by subtracting the mean and dividing by the standard deviation of the training set to get the z-score
  - Save preprocessed files using `h5py`
2. Design, train, and evaluate the CNN ([svhn-model.ipynb]())
  - Use `tensorboard` to view training logs (loss & accuracy for each step/epoch)
  - Calculate accuracy for test set
    - On original image with multiple digits
    - On individual digits
  - Use confusion matrix & F1 score to give a better idea of model performance
  - Visualize images (correct & incorrect) for better understanding of model strengths/weaknesses

## Model Architecture Overview
- `xavier` initialization of weights in the network
  - proved to perform better than `he` initialization
- Three blocks with two convolutional layers (`conv -> batch_norm -> leaky_relu -> avg_pooling`)
- Flatten layer
- Two fully connected (dense) layers with Leaky ReLU activation functions
- Five parallel fully connected `softmax` layers
  - For predicting probabilities of 5 digits

See notebook [svhn-model.ipynb](https://github.com/k-chuang/tf-svhn/blob/master/svhn-model.ipynb) for more details.



## Results
I trained the model for 80 epochs with a batch size of 512 images per step.

**Multi-digit Test Accuracy**: 94.399%

**Individual Digit Test Accuracy**: 96.620%

**Individual Digit F1 Score**: 0.9643

For more details about implementation, training, and evaluation of the model, see the jupyter notebook [svhn-model.ipynb](https://github.com/k-chuang/tf-svhn/blob/master/svhn-model.ipynb).

## License

See the [LICENSE](https://github.com/k-chuang/tf-svhn/blob/master/LICENSE) file for license rights and limitations (MIT).

