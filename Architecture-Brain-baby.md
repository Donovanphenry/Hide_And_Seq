<center id = 'doc-title'>
    <h1>Hide &amp; Seq</h1>    
</center>

---

* Input

  * Rather than a one-hot for each nucleobase, could have a different channel for each nucleotide.

* Hidden Layers

  * CNN

    * Tinker with smaller kernel sizes (27 seems like it'd be a lot)
      * see what previous researchers have used when convolving 1D nucleotide data
      * While using smaller kernel sizes, try increasing depth for each parallel cnn layer (better feature extraction)
    * Pooling layers are a relic of the past, better to use convolution layers with modified strides such that the condensation method of the feature-maps is also a learnable parameter.
    * Batch Normalization and kernel initializers for properly creating weights and avoiding covariate shift.

  * BiLSTM

    * Utilize global average pooling as opposed to a flatten layer to prepare the result of the BiLSTM for the FCNN input layer. The incredibly significant benefit of Global Average Pooling is in the number of parameters. Consider the following:
      $$
      Let \space M_{BiLSTM} = the \space output \space of \space the \space BiLSTM \space s.t. \space M_{BiLSTM} \space is \space a \space rank \space N \space tensor\\
      i.e. \space dim(M_{BiLSTM}) \space = D_1 \times D_2 \times \space ... \space \times D_N\\
      Let \space flatten(M_{BiLSTM}) = {\vec{F}_{BiLSTM}}\\
      M_{BiLSTM} \space has \space \prod_{i=1}^ND_i \space \# \space of \space elements = dim(\vec{F}_{BiLSTM}) = D\\
      \implies FCNN \space relies \space on \space D \space features
      $$
      However, if we choose to use Global Average Pooling, we find the following:
      $$
      GlobalAveragePooling(M_{BiLSTM}) = O_{BiLSTM} \space s.t. \space O_{BiLSTM} \space is \space a \space 1\times1\space \times \space ...\space \times D_n \space Tensor\\
      \implies FCNN \space only \space relies \space on \space D_n \space features
      $$
      which is significantly less parameters. In turn, we have much less of a probability of over-fitting with our FCNN, and can thus build a more complex BiLSTM or CNN.

  * FCNN

* Output

  * Soft-max yielding 
    $$
    Pr(p_i) \space s.t. \space 0 \leq \space i \leq n - 1 \space where \\
    \space n = the \space number \space of \space nucleobases \space in \space the \space sequence\\
    p_i = promoter \space at \space the \space i_{th} \space nucleobase
    $$
    and the model would then select
    $$
    p_j \space s.t. \space Pr(p_j) \geq Pr(p_k) \space \forall j,k
    $$


# Model architectures 

* considering: https://www.mdpi.com/2073-4425/11/12/1529/htm (which reviews DeePromoter and other similar nucleotide sequence analysis using DL)

* Additionally, DeePromoter's "tuned hyper-parameters are the number of convolution layers, kernel size, number of filters in each layer, the size of the max pooling layer, dropout probability, and the units of Bi-LSTM layer" (section 2.3: the proposed models).
  * at the very least, we should mention all of these hyper-parameters
  * Additionally, loss fn, optimizer and its hyper parameters (lr, momentum, etc. )
  * Some way to summarize the model architecture would be useful as well (we've already diviated from deepromoter's output layer of a single perceptron,  to 2 with softmax)

| Name | loss/acc | val_loss/val_acc | # conv layers | kernel size | # filters/layer | max pool size | Loss fn | opt  and hyper params |
| ---- | -------- | ---------------- | ------------- | ----------- | --------------- | ------------- | ------- | --------------------- |
|      |          |                  |               |             |                 |               |         |                       |
|      |          |                  |               |             |                 |               |         |                       |
|      |          |                  |               |             |                 |               |         |                       |
