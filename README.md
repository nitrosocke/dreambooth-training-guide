# Basic Dreambooth Guide
This is a short guide on the process of collecting a dataset and basic dreambooth settings.

## My Setup
- I am using [Dreambooth by ShivamShrirao](https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth)
- I am running it locally through Miniconda on Win10 with my 3090.


## The Dataset
Dataset creation is the most important part of getting good, consistent results from Dreambooth training. Be sure to use high quality samples, as artifacts such as motion blur or low resolution will get picked up by the training and appear in the images you generate with your model. When training for a specific style, pick samples with good consistency. Ideally, only pick images from the show or artist you're training. Avoid fan art or anything with a different style, unless you're aiming for something like a style fusion.

Once you've collected photos for a dataset, crop and resize all the images into 512x512 squares and remove any watermarks, logos, people/limbs cut off by the edge of the picture, or anything else you don't want in your final model. For subjects, I've found that including samples with either black or white backgrounds helps immensely. Transparent backgrounds may also work, but can sometimes leave a fringe or border around the subject in the training results, so for now I can't recommend that. Make sure to include images with a normal background (for example, of your subject in a scene). In my testing, using only plain backgrounds gave a poorer result.

Save the images in the PNG format and put all of them in a "train" folder.

## The Reg Images
Something I've read on the internet, that **I do NOT recommend**, is using Stock Photos or otherwise Real Images as reg images.

I don't know where this theory originates, but I find it to be misinformation. In Dreambooth training, reg images are used as an example of what the model already can generate in that class and prevent it from training any other classes. For example, when training the class _"man"_ you don't want the class _"woman"_ to be affected as well.
Using reg images that weren't created by the model prevents this "prior preservation" from working and instead trains the whole model on your dataset. You can achieve this effect easier by just training without the "prior_preservation_loss".

I usually use 1000 regularization images for my style training consisting of around 10-75 sample images. The paper suggests 200 times the number of samples, but I've never used more than 2000 reg images. Generate the images beforehand or let the script do it before the training process. I use the stable-diffusion-v1-5 model to render the images using the DDIM Sampler, 30 Steps and 512x512 resolution. For the prompt, you want to use the class you intent to train. When training a style I use "artwork style" as the prompt. You can have a look at my reg images here, or use them for your own training:
[Reg Images by Nitrosocke](https://drive.google.com/drive/folders/19pI70Ilfs0zwz1yYx-Pu8Q9vlOr9975M)
_The intended class_prompt for these is the folder name._



## FAQ:

How many Images do you use for your Dataset?
- I used anything from 10-120 images so far and the number depends on the desired flexibility of the model.

How long does Dreambooth Training take for you with Shivam's repo?
- On my setup 1000 steps take ~12 minutes.

Do your models take you a lot of attempts or is your first dreambooth always successful?
- Not always, but after training about 20+ models now I have a good feeling of what I'm doing. The [the di-mo model] was the first try, but other models needed refinement or more runs. Some of my models are already on version 5 and had several complete overhauls of the datasets used for training.

What settings do you use for training steps and repeats?
- The repo I'm using doesn’t have the set repeats amount, so I try to set it to roughly 100*samples. The models I trained on 12k steps didn't show a big difference to the one on 9k steps, so there are way more factors in this equation.

Which base ckpt did you use for the mo-di model?
- This is based on SD 1.5

How do I run your models?
- You would need a SD software or repo/colab that can load custom models in the ckpt or diffusers format. Usually, they have a models folder where you put it in and select it from the UI.

I've been searching for models to use lately and all I find are missing the .ckpt file. Is there a reason for this or did they just forget?
- Some models come in the diffusers format. They would need to be converted to the ckpt format in order to use them with automatic

Do you have a video guide for dreambooth?
- I think this guide is closest to what I'm doing over here:
https://www.youtube.com/watch?v=tgRiZzwSdXg


How are the artworks in the examples with perfect faces and no extra limbs or heads?
- That's a side effect of fine tuning a model. While feeding it images with good poses and composition it refines these characteristics as well.

Can I merge an existing model with a model I trained on my face to make myself in that style?
- We had mixed reports on that but a few successfully did their faces with img2img. I don't know about merging though

What does your dataset look like?
- I like to use ~100 images for training a style and they are roughly 70% people, 20% landscapes and 10% animals/objects. I try to include closeups and half body shots of a few main characters. I never use full body shots as they loose too much resolution and SD can actually make pretty good full body poses without them.
I then upscale of all the images before resizing them down to 512x512 for improved clarity.

How much vram do you need for dreambooth
- I think minimum is 10GB right now, but there might be repos down to 8GB already.

Is there an easy way to take an existing image and apply this style so that the subjects still resemble original?
- It's best to use either img2img and adjust the denoising settings and prompt until it gives good results or train a model on the style dataset and the person's picture.

Can we just use images of the same style or everything that is available for the topic?
- For a more precise model the samples should follow the style as best as they can. Adding fan art or images from other styles could make the model less reliable at rendering the specific style.

Do training images need to be 512 by 512 pixels?
- Since the SD was trained on 512x512 I assumed that it works best to use the same resolution. But I have heard of people training with other resolutions and aspect ratios, but I don't how well it works. Some repos crop it to 512x512 automatically as well.



*Guide by Nitrosocke, edited by wavymulder - last updated Oct 28 2022*
