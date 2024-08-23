# 22/2/24

using github mostly now, but this is still good for writing down thoughts

formatted coords in text so 3.7 -> 3.70 meaning easier reading of files and no manual adjustment required.

ADD_NWP

9 locations -> 

Epoch 50/50
363/363 - 1s - loss: 0.0040 - mae: 0.0357 - mse: 0.0040 - val_loss: 0.0059 - val_mae: 0.0437 - val_mse: 0.0059 - 1s/epoch - 3ms/step

Predictions during nighttime 
RMSE: 12.170760117059816

1 location ->

Epoch 50/50
363/363 - 1s - loss: 0.0044 - mae: 0.0383 - mse: 0.0044 - val_loss: 0.0058 - val_mae: 0.0426 - val_mse: 0.0058 - 832ms/epoch - 2ms/step

Predictions during nighttime 
RMSE: 12.016379102871195

NORMAL_NWP

Epoch 50/50
355/355 - 1s - loss: 0.0030 - mae: 0.0274 - mse: 0.0030 - val_loss: 0.0071 - val_mae: 0.0389 - val_mse: 0.0071 - 605ms/epoch - 2ms/step

val graph looks pretty horrible

No struggle with nighttime
RMSE: 13.115371586904276

I think I will experiment with removing certain hours of the day that might be the easiest. I can also base it off solar angle but the pv dataset doesn't have that in it. ADD_NWP was using sgd, NORMAL_NWP was using ADAM. I'm going to try ADD_NWP with adam. 1 location graph is horrible and predictions are shifted up.

I think I can actually remove data based on angle, since pv is actually in the combined dataset so that information is retained. The literature says 80 degrees, it looks like 90 degrees is better, i.e. cut out angles greater than 90. This is only January that I've checked so far though. Throughout the year it seems to pretty much always be at 90 degrees where we lose GHI.

Trying to synchronise which times are removed between the nwp and pv stuff is proving awkward. By just removing nighttime time ranges we can get it working, but the model actually performs better with the nighttime data included. That will be because of the long lines between seasons actually.

==================

I want to use 3 years of data, NSRDB gives 2017, 2018, 2019. Using my add_nwp data collection stuff I should be able to select years easily to split training, validating, and testing.

The issue is with satellite data since one of them was reversed. 2017 is reversed, I also just can't seem to access it. I can't access 2017 because it's backwards! 2018 and 2019 I can get.

Swapped 2017 around, on plotting it now looks correct. I can't seem to concatenate the 3 years together because it's too large. I can't save the times or data either anymore, it just says the files are too big? That's because I was still selecting the whole of the UK!

It doesn't seem to want to concatenate.