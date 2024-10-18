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

## Part 1 - Object detection with YOLOv8 model

data from [here](
https://github.com/HSKPeter/mahjong-dataset-augmentation)

## Detailing the Journey

### Day 1 - It's time for a change

I decided that I've had enough of half-completed projects that are barely showable to anyone because they have been poorly mantained. Such was about to be the fate of this project — the difficulties in making progress with my old methods of rotated bounding boxes and compounding schoolwork simply killed my motivation to keep going.

But it's time to realize that if one sets out to accomplish a task, one should put their best effort in and give it their all, starting right here.

--

My goal today was to figure out a general direction to take this project in that did not require me to create my own model. I am still a beginner when it comes to using AI technology, and I did not want this to be a bottleneck for this project. And so I settled on using the [YOLOv8](https://yolov8.com/) object detection model. This would allow me to detect multiple mahjong tiles in a single picture, of which the predictions should be extractable and a mahjong hand would be able to be read in. This is the first challenge that I wish to solve, and is part 1 of this project.

Part 2 of this project involves writing algorithmic code to compute hand patterns which will determine the overall score of the hand. I plan to do this in C++ or Python, both of which have object-oriented capabilities which will facilitate improvements to the code later on.

Finally, part 3 of the project involves hosting the application by writing a frontend. Since I am not well acquainted with frontend frameworks, I will look into similar projects and figure out exactly I will be displaying to gain a better idea of what to use, which is work for the following few days.

After the project is completed, I will look into some CI/CD tools to ensure that this application stays running over time.

Overall, I feel like with my current knowledge of computer science, I do have the tools to make this project a success. It is hard to gauge whether this project will take a couple weeks, or a couple of months. But regardless, I must now demonstrate that I have the motivation and the discipline to get it done.

### Day 2 - Annotation Grind!

After figuring out what we needed to do yesterday, today was spent grinding out image annotations! This was done on the website [Roboflow](https://roboflow.com/), which contains the YOLOv8 model that I would like to use. I picked up from the previous dataset I uploaded in the summer, which consists of 646 images, and was able to get about 20 annotations done. Due to the ability for AI to generate annotations once enough have been manually labelled, I only have to label about 70 images manually before there should be enough accuracy to automatically label the rest. The website has quite a clean UI and it was really easy to navigate and prepare the data.

<img src="/images/roboflowmainmenu.png" width="1000" alt="an overview of roboflow main menu" />

As for the actual annotations, we simply draw rectangular bounding boxes over the tiles in each image, as follows:

<img src="/images/imageannotation.png" width="1000" alt="an overview of image annotation"/>


I am looking to get some more picture samples of real life mahjong tiles, which I do have, but are back in Waterloo (I am home for reading week currently). These will help the model train on a wider variety of tiles compared to my current dataset, which will allow for better results in the long run.

That's all for today, and probably for the next couple of days! I will update once again once all the image annotations are done :)
