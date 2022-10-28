# Basic Dreambooth Guide
-> as of October 20th, 2022 I'm using ShivamShrirao's Repo locally <-

## My Setup
- I am using [Dreambooth by ShivamShrirao](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)
- I am running it locally through Miniconda on Win10 with my 3090.


## The Dataset
Dataset creation is the most important part of getting good, consistent results from Dreambooth training. Be sure to use high quality samples, as artifacts such as motion blur or low resolution will get picked up by the training and appear in the images you generate with your model. When training for a specific style, pick samples with good consistency. Ideally, only pick images from the show or artist you're training. Avoid fan art or anything with a different style, unless you're aiming for something like a style fusion.

Once you've collected photos for a dataset, crop and resize all the images into 512x512 squares and remove any watermarks, logos, people/limbs cut off by the edge of the picture, or anything else you don't want in your final model. For subjects, I've found that including samples with either black or white backgrounds helps immensely. Transparent backgrounds may work as well, but can sometimes leave a fringe or border around the subject in the training results, so for now I can't recommend that. Make sure to include images with a normal background (for example, of your subject in a scene). In my testing, using only plain backgrounds gave a poorer result.

Save the images in the PNG format and put all of them in a "train" folder.

## The Reg Images
Something I've read on the internet, that **I do NOT recommend**, is using Stock Photos or otherwise Real Images as reg images.

I don't know where this theory originates, but I find it to be misinformation. In Dreambooth training, reg images are used as an example of what the model already can generate in that class and prevent it from training any other classes. For example, when training the class _"man"_ you don't want the class _"woman"_ to be affected as well.
Using reg images that weren't created by the model prevents this "prior preservation" from working and instead trains the whole model on your dataset. You can achieve this effect easier by just training without the "prior_preservation_loss".

I usually use 1000 regularization images for my style training consisting of around 10-75 sample images. The paper suggests 200 times the number of samples, but I've never used more than 2000 reg images. Generate the images beforehand or let the script do it before the training process. I use the stable-diffusion-v1-5 model to render the images using the DDIM Sampler, 30 Steps and 512x512 resolution. For the prompt, you want to use the class you intent to train. When training a style I use "artwork style" as the prompt. You can have a look at my reg images here, or use them for your own training:
[Reg Images by Nitrosocke](https://drive.google.com/drive/folders/19pI70Ilfs0zwz1yYx-Pu8Q9vlOr9975M)
_The intended class_prompt for these is the folder name._


<!---
FAQ:

-faq under development
-->

*Guide by Nitrosocke, edited by wavymulder - last updated Oct 28 2022*
