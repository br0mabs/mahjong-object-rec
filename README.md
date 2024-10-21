# Riichi Mahjong Hand Recognition Application

The goal of this project is split into 3 parts:

1. Apply YOLOv8 object detection model to a dataset containing mahjong tiles to detect mahjong hand composition
    - This would be limited to closed mahjong hands containing 14 tiles.
2. Create an algorithm to determine mahjong hand value once the tiles are detected
3. Host the application online, so that anyone can upload an image of a mahjong hand, and have the hand scored

Possible extensions:
- Generalize to hands with melds, which will involve image segmentation
- Detection straight from the camera

Deadline: By the end of the year

# Part 1 - Object detection with YOLOv8 model

data from [here](
https://github.com/HSKPeter/mahjong-dataset-augmentation)

# Detailing the Journey

## It's time for a change

I decided that I've had enough of half-completed projects that are barely showable to anyone because they have been poorly mantained. Such was about to be the fate of this project — the difficulties in making progress with my old methods of rotated bounding boxes and compounding schoolwork simply killed my motivation to keep going.

But it's time to realize that if one sets out to accomplish a task, one should put their best effort in and give it their all, starting right here.

--

My goal today was to figure out a general direction to take this project in that did not require me to create my own model. I am still a beginner when it comes to using AI technology, and I did not want this to be a bottleneck for this project. And so I settled on using the [YOLOv8](https://yolov8.com/) object detection model. This would allow me to detect multiple mahjong tiles in a single picture, of which the predictions should be extractable and a mahjong hand would be able to be read in. This is the first challenge that I wish to solve, and is part 1 of this project.

Part 2 of this project involves writing algorithmic code to compute hand patterns which will determine the overall score of the hand. I plan to do this in C++ or Python, both of which have object-oriented capabilities which will facilitate improvements to the code later on.

Finally, part 3 of the project involves hosting the application by writing a frontend. Since I am not well acquainted with frontend frameworks, I will look into similar projects and figure out exactly I will be displaying to gain a better idea of what to use, which is work for the following few days.

After the project is completed, I will look into some CI/CD tools to ensure that this application stays running over time.

Overall, I feel like with my current knowledge of computer science, I do have the tools to make this project a success. It is hard to gauge whether this project will take a couple weeks, or a couple of months. But regardless, I must now demonstrate that I have the motivation and the discipline to get it done.

## Annotation Grind!

After figuring out what we needed to do yesterday, today was spent grinding out image annotations! This was done on the website [Roboflow](https://roboflow.com/), which contains the YOLOv8 model that I would like to use. I picked up from the previous dataset I uploaded in the summer, which consists of 646 images, and was able to get about 20 annotations done. Due to the ability for AI to generate annotations once enough have been manually labelled, I only have to label about 70 images manually before there should be enough accuracy to automatically label the rest. The website has quite a clean UI and it was really easy to navigate and prepare the data.

<img src="/images/roboflowmainmenu.png" width="1000" alt="an overview of roboflow main menu" />

As for the actual annotations, we simply draw rectangular bounding boxes over the tiles in each image, as follows:

<img src="/images/imageannotation.png" width="1000" alt="an overview of image annotation"/>


I am looking to get some more picture samples of real life mahjong tiles, which I do have, but are back in Waterloo (I am home for reading week currently). These will help the model train on a wider variety of tiles compared to my current dataset, which will allow for better results in the long run.

That's all for today, and probably for the next couple of days! I will update once again once all the image annotations are done :)

## Deploying Model v1

Several days have passed since the last update, but a lot more annotation work has gone on in the background. I have manually annotated 50+ images, and also contributed some new images through online search of mahjong tiles as well as taking some pictures of my own tiles. This is to increase the complexity that the model will be able to support, such as different camera angles or different lighting conditions, as well as object obstruction that was not available in the initial dataset. Here is an example:
<img src="/data/own_tiles/IMG_3098.jpeg" width="500" alt="an example of an image used for data"/>
Note that there image is blurry, and there are obstructed tiles. This is all cleverly crafted in order to present the model with tough situations to train on.

The process for the actual model training involved exporting the dataset to google colab, importing the YOLOv8 model, training using the freely provided T4 GPU, and then exporting the model and its results back to Roboflow. Fortunately, this process is already outlined in Roboflow's docs, and it was a matter of taking relevant code from [this tutorial](https://colab.research.google.com/github/roboflow/notebooks/blob/main/notebooks/train-yolov8-object-detection-on-custom-dataset.ipynb). I also learned that Roboflow allows for extra augmentation to the complete dataset during the export step, so I added some shear to the images so the model can train with different camera perspectives.

### Results

Here are the results of the first model, which was trained on 198 images, 177 being training images, 13 validation, and 8 test images:

Confusion matrix:
<img src="/data/model info/v1/confusionmatrix.png" width="500" alt="confusion matrix for model v1"/>

Loss graphs:
<img src="/data/model info/v1/lossinfo.png" width="500" alt="loss function graphs"/>

The key takeaways were that many tiles are still not getting detected, or are detected incorrectly or correctly with low confidence. This can cause a lot of overlap with detection, as one tile may be detected as possibly being many different tiles. This means more training is required, with more variety of training data, including different tile sizes, angles, perspectives, and occlusion. It seems like we do not need more than 100 or so of the original computer generated images, since they do not translate well into detection of tiles in real life photos. We will focus on including more real life images into the dataset.
