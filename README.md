# Quick Dreambooth Guide
-> as of 20.10. I'm using ShivamShrirao's Repo locally <-

## The Setup
- [Dreambooth by ShivamShrirao](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)
- Running locally through Miniconda on Win10
- Folder structure of the training images, reg images and model outputs

## The Dataset
This part is most important for good results in your training program. Make sure to use high quality samples as any low resolution or motion blur gets picked up by the training. When training for a specific style you should pick samples with good consistency. Ideally the images are only from the show or artist you're training and no fan art or anything with a different style, unless you aim for that of course.
I cut all the images into 512x512 squares and remove any watermarks, logos, spots cutoff persons or floating limbs. For subjects I found that including samples with either black or white background helps to train the subject immensely. Transparent backgrounds work as well but might leave a fringe or border around the subject in the training results, so I can't recommend that as for now. Make sure to include images with a normal background as well since using only plain background gave bad results.
Save the images in the PNG format and put all of them in a "train" folder.

## The Reg Images
**Downloading Images of people from stock image sites or similar:**
I don't know where this theory originates from but I find it to be a misinformation. The reg images are supposed to be telling the model what it already knows of that class (for example a _style_) and prevent it from training any other classes. For example when training the class "man" you don't want the class "woman" to be affected as well.
So by adding external images from any other source just prevents this "prior preservation" and trains the whole model on your sample images. If you want to achieve this effect easier you can just train without the "prior_preservation_loss" option and have the same effect.

I usually use 1000 regularization images for my style training consisting of around 10-75 sample images. The paper suggests 200 x num of samples, but I never used more than 2000. Generate the images beforehand or let the script do it before the training process. I use the stable-diffusion-v1-4 model to render the images using the DDIM Sampler, 30 Steps and 512x512 resolution. For the prompt you want to use the class you intent to train, when training a style I use "artwork style" as the prompt. You can have a look at my reg images here, or use them for your own training:
[Reg Images by Nitrosocke](https://drive.google.com/drive/folders/19pI70Ilfs0zwz1yYx-Pu8Q9vlOr9975M)
_The intended class_prompt for these is the folder name._
