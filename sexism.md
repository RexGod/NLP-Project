```py
model = Sequential()
```
* in here A sequential model in deep learning refers to a type of neural network architecture where the layers are arranged in a sequential order.

```py
model.add(Embedding(input_dim=len(tokenizer.word_index) + 1, output_dim=50, input_length=max_sequence_length))

```
* in here we have encoded vocab 
* Embedding layer used to represent categorical or discrete data as continuous vectors

```py
model.add(Bidirectional(LSTM(128, return_sequences=True)))
```
Let's Break this code like This :

```py
LSTM(128, return_sequences=True)
```
* This part creates an LSTM layer with 128 units (memory or neurons)
### what is LSTM and what used for?
* it's layer used in neural network
* LSTM layer will produce an output for each input sequence element

```py
model.add(Dropout(0.5))
```
now for stop overfitting we add dropout layer it will drop 0.5 of in forward move

```py
model.add(Dense(64, activation='relu', kernel_regularizer=l2(0.01)))
```
Dens is represent as fully connected 

* relu is activation function that work like f(x) = max(0,x)

* L2 regularization is applied to the weights of the layer and helps prevent overfitting by adding a penalty term to the loss function

```
model.add(Dense(1, activation='sigmoid'))

```

* **'1'**: This specifies the number of neurons or units in the Dense layer and here '1' bec problem is classification and output single value.

* sigmoid function : The sigmoid activation function is used for the single neuron in the output layer