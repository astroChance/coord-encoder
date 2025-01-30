# coord-encoder
_Proof of concept project to encode time series data simply based on coordinates_
---
## Question: Can complex time series data be encoded in a fairly simple neural network?
The basis for this question arose from a conversation around the security of sensitive data. Machine learning models are often thought of as derivatives of the data they've been trained on, but could a simple model accurately replicate complex data based on very limited input features?

---

## Test: Try to accurately reconstruct time series data based solely on spatial coordinates.
For this test, I used data from the Mars shallow radar sounder (SHARAD). The data represents a two dimensional subsurface profile across the Martian north polar layered deposits. Based on the X and Y surface coordinates of each radar frame (vertical time series through the subsurface), try to predict the entire time series radar data frame.

---

## Method: Construct a neural network architecture to predict time series data from 2 input features.
After some testing, I came to the conclusion that a simple fully connected neural network was not able to produce accurate time series predictions just based on the two coordinate input features. However, the prediction did look like a rough first-order approximation of the correct output. By using this rough output as the input to a second neural network, fairly good results were achieved.

### _See the code notebook in this repository for full details_

### Model 1
The first model uses the X and Y coordinate values as the sole input features. Twenty six hidden layers of varying dimension were used to decode the two input features into a time series with 1024 samples. 

![Model 1 Schematic](/images/model1_schematic.jpg)

### Model 2
The second model uses the full prediction from Model 1 as the input feature, and the training target is the full radar profile. Something resembling an encoder-decoder network was chosen for this model. Since I'm now working with 2D images I could have used a convolutional neural network, but for this test I wanted to push the limits of simple fully connected neural networks.

![Model 2 Schematic](/images/model2_schematic.jpg)

### Strategy
The training loss plot for Model 1 suggests severe overtraining, but that's really the goal for this task. I didn't withhold any validation data for Model 2 because I don't care about generalization of this model. I simply want the model to reproduce my single radar profile as accurately as possible. Validation for this proof of concept is mainly visual. Does the predicted radar profile look like the ground truth? Is there a meaningful pattern in the residual difference?

---

## Outcome
The output of Model 1 appears to be a very rough first-order approximation of the ground truth. The general structure of the ice deposits can be observed, but the prediction is severly lacking in detail. The difference plot illustrates just how much detail is lacking in the prediction from Model 1.

![Model 1 Prediction](/images/model1_prediction.jpg)

However, once the prediction from Model 1 is used to train Model 2, the results significantly improve. At first glance, the predicted radar profile is an excellent representation of the ground truth radar data. The difference plot has been minimized, and no obvious structure is left in the residual.

![Model 2 Prediction](/images/model2_prediction.jpg)

## Key Takeaways
This exercise shows that fairly complex time series data can be encoded within a relatively simple neural network architecture. Using a two-model approach, I was able to reconstruct a 2D Mars radar profile solely based on the two sptial coordinate values of each radar frame.
