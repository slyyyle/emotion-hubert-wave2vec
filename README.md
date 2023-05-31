# Finetuning HUBERT - Audio Emotion Classification on RAVDESS

## Final Remarks:

###As a result of this example, after 3 epochs HUBERT has already achieved 36% accuracy in 47 minutes of training time from a starting accuracy of a random guess (0.125). You might be thinking, why did you do all this work for such measly accuracy when pre-trained models on this exact task exist on huggingface?

-It's just for practice, and now I can repurpose this work for other tasks.  
-There will come a day when I will have custom data and a problem no one else has tackled, which will require fine tuning!  
-I will use those pre-trained models when learning how to deploy and use RESTful API endpoints.  Hopefully I can host those locally, because I don't have $$  

###However, we cannot extrapolate this performance to MANY further epochs (maybe a few), and have a few issues that we may run into with more epochs:

-The model will eventually overfit as we only have about 1200 training samples and <100 samples per class  
-My girlfriend will kill me if I continue to run our power bill through the roof  
-I have maxed out my VRAM at a batch size of 4, using mixed precision (fp16), which may limit performance in further epochs (this depends on learning rate schedule, and may not occur)

###If I were to continue the example, I would:

-Reduce the # of warmup steps and increase learning rate, as it appeared to benefit as learning rate increased.  
-Find a larger labeled dataset, although this may limit the granularity of our emotion labels (I could use the full RAVDESS dataset, as it is much larger than this subset)  
-Increase learning rate  
-Get a new graphics card and increase batch size (NVIDIA 4090 has 24GB which looks very appetizing right now, but is 1700 dollars and I have to pay rent)  
-Try out other models in addition to HUBERT

###Things to note:

-load_metrics will be deprecated soon, the code will have to be altered in the future in order to report metrics using evaluate.load  
-I will need to figure out how to use torch.optim.AdamW as the current implementation of AdamW is deprecated  
-Transformers v4.28.0 had to be used for this example, as future versions have been reported unstable (Partial State error occurs with v4.29.2 in TrainingArguments class).  It is unclear whether or not this error is in v4.29.0, but I used 4.28.0 at the recommendations of a forum post I saw.  
-There has to be a more elegant way to use huggingface's dataset package, as you saw previously I had to really wrangle with it in order to get it in the correct state for training.  It felt awkward the whole way through

###Things still to learn:

-I need to get a grasp on how to separate this notebook into .py scripts, especially if I want to offload this to an Azure job
-I need to practice Azure jobs, using simpler models.  I would really like to use this example, but I can't get my hands on compute that would outperform my 3080 (thanks a lot, Sam Altman)
-I need to learn to deploy API endpoints, but these types of models would run me 400+ bucks per month on huggingface and probably about the same on Azure
-We defined a custom DataCollator in this example, but I need to do more research to figure out if a prepackaged DataCollator from HuggingFace does the exact same thing
