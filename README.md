



# EEG_classification_1D_CNN


Sleep Stage Classification from Single Channel EEG using Convolutional Neural
Networks

*****

Quality Sleep is an important part of a healthy lifestyle as lack of it can
cause a list of
[issues](https://www.webmd.com/sleep-disorders/features/10-results-sleep-loss#1)
like a higher risk of cancer and chronic fatigue. This means that having the
tools to automatically and easily monitor sleep can be powerful to help people
sleep better.<br> Doctors use a recording of a signal called EEG which measures
the electrical activity of the brain using an electrode to understand sleep
stages of a patient and make a diagnosis about the quality if their sleep.

In this post we will train a neural network to do the sleep stage classification
automatically from EEGs.

### **Data**

In our input we have a sequence of 30s epochs of EEG where each epoch has a
label [{“W”, “N1”, “N2”, “N3”,
“REM”}](https://en.wikipedia.org/wiki/Sleep_cycle).

<span class="figcaption_hack">Fig 1 : EEG Epoch</span>

<span class="figcaption_hack">Fig 2 : Sleep stages through the night</span>

### Model Description

Recent approaches [[1]](https://arxiv.org/pdf/1703.04046.pdf) use a sub-model
that encodes each epoch into a 1D vector of fixed size and then a second
sequential sub-model that maps each epoch’s vector into a class from [{“W”,
“N1”, “N2”, “N3”, “REM”}](https://en.wikipedia.org/wiki/Sleep_cycle).

Here we use a 1D CNN to encode each Epoch and then another 1D CNN or LSTM that. This allows the prediction
for an epoch to take into account the context.

<span class="figcaption_hack">Sub-model 1 : Epoch encoder</span>

<span class="figcaption_hack">Sub-model 2 : Sequential model for epoch classification</span>

The full model takes as input the sequence of EEG epochs ( 30 seconds each)
where the sub-model 1 is applied to each epoch using the TimeDistributed Layer
of [Keras](https://keras.io/) which produces a sequence of vectors. The sequence
of vectors is then fed into a another sub-model like an LSTM or a CNN that
produces the sequence of output labels.<br> We also use a linear Chain
[CRF](https://en.wikipedia.org/wiki/Conditional_random_field) for one of the
models and show that it can improve the performance.

### Training Procedure

The full model is trained end-to-end from scratch using Adam optimizer with an
initial learning rate of 1e⁻³ that is reduced each time the validation accuracy
plateaus using the ReduceLROnPlateau Keras Callbacks.
