# Basic Dreambooth Guide
-> as of October 20th, 2022 I'm using ShivamShrirao's Repo locally <-

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


<!---
## FAQ:

-faq under development

How long does Dreambooth Training take for you with Shivam's repo?
- 9k steps was ~2h

Do your models take you a lot of attempts? Or is your first dreambooth always successful?
- Not always, but after training about 20+ models now I have a good feeling of what I'm doing. This [the di-mo model] was the first try, but other models needed refinement or more runs.

What settings do you use for training steps and repeats?
- The repo Im using doesnt have the set repeats amount. So I try to set it to roughly 100*samples but the model I trained on 12k steps didn't show a big difference to the one on 8k steps.

What about regularization images? How many? Based on a specific prompt ("illustration style"?) or downloaded?
- the class images of "illustration style" were created with the same model and DDIM sampler

Which base ckpt did you use for the mo-di model?
- This is based on SD 1.5

What token/class? From the example you gave "modern disney lara croft", you did not add the "style" word, why is that? Is "modern" the token and "disney" the class?
- unique or desired token to train + class to train
Arcane + Style / Zelda + Person

How do I run your models?
- Automatic's Repo
- You would need a SD software or repo/colab that can load custom models in the ckpt format. Usually they have a models folder where you put it in and select it with the Ui There are a ton of tutorials on YouTube if you're a visual learner and need a guide.

You only need the ckpt file for this?
- Needs a repo or software to run and the ckpt dile or diffusers

I've been searching for models to use lately and all I find are missing the .ckpt file. Is there a reason for this or did they just forget?
- Some models come in the diffusers format. They would need to be converted to the ckpt format in order to use them with automatic

What sampler, steps and cfg is best to use?
- it should work with any sampler.
Here are the settings for the Lara Croft [mo-di model] image:
modern disney lara croft
Steps: 50, Sampler: Euler a, CFG scale: 7, Size: 512x768

Do you have a video guide for dreambooth?
- I think this guide is closest to what I'm doing over here:
https://www.youtube.com/watch?v=tgRiZzwSdXg

How do I add this to AUTOMATIC1111 on Google colab at the same time with v1. 5?
I would like to have both. Or is it not possible?
- Upload the model to your google drive and mount it with the colab notebook. Then you should be able to copy it into the models folder of the repo.
There might be an easier way as well.

Would you mind sharing what's your PC setup ?
- Here you go: RTX 3090, Ryzen 9, 32GB RAM and a normal HDD

But I'm more interested in how you trained SD and how the artworks in the examples have perfect faces and no extra limbs
- That's a side effect of fine tuning a model. While feeding it images with good poses and composition it refines these characteristics as well.

What kind of results would i get if i run dreambooth training on that ?
- It might overwrite the trained data from the samples images with your new ones. I never actually tried it though.

Can I merge your model with a model trained on my face to make myself in that style?
- We had mixed reports on that but a few successfully did their faces with img2img. I don't know about merging though

Does Shivram retrain the encoder as well or is the encoder frozen with his training script?
- it trains the text encoder as well if you use the flag for that

What sort of iamges do you use for your dataset when training a style?
- I usually try to go for mostly characters with different backgrounds and lighting and maybe 10% scenes and landscape shots.

Is there a youtube guide for dreambooth locally?
- I just followed the instructions on this repo:
https://github.com/ShivamShrirao/diffusers/tree/main/examples/dreambooth
by looking through the colab you can see how it should work.
A little coding might be needed for all of this though. Running it locally isn't as easy as using the google colab.

What does your dataset look like?
- I didnt change much for v3, just some more characters and scenes and I switched some of the more blurry shots with more clear ones.
Also did an upscale of all the images before resizing them to 512 for more clarity.
other than that, I try to include closeups and half body shots of a few main characters. I never use full body shots as they loose too much resolution and SD can actually make pretty good full body poses without them.

How much vram do you need for dreambooth
- I think minimum is 10GB right now, but there might be repos down to 8GB already

Is there an easy way to take an existing image and apply this style so that the subjects still resemble original?
- it's either using i2i and adjust the denoising settings and prompt until it gives good results or train a model on the arcane style dataset and the person's picture

For regularization images, can't we just use images of the same style? Like if we're training style of a particular show, we can upload 1000 screenshots from the show instead of generating 1000 'style of' images.
- The reg images are supposed to be telling the model what it already knows of that class (for example a style) and prevent it from training any other classes. For example when training the class "man" you don't want the class "woman" to be affected as well. So by adding external images from any other source just prevents this "prior preservation" and trains the whole model on your sample images. If you want to achieve this effect easier you can just train without the "prior_preservation_loss" option and have the same effect.

Do training images need to be 512 by 512 pixels?
- Since the SD was trained on 512x512 I assumed that it works best to use the same resolution. But I have heard of people training with other resolutions and aspect ratios, but I don't how well it works. Some repos crop it to 512x512 automatically as well.

Have you tried it on img2img and does it generate good results?
- I haven't tried this one yet with i2i, but the arcane model had good results so I assume this would be even better, since it sticks to the style way better.
-->

*Guide by Nitrosocke, edited by wavymulder - last updated Oct 28 2022*
