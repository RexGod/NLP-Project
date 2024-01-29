#  About Data 
```py
data = data[['label_sexist' , 'text' , 'split']]
```
in here we only need 3 columns 

* **split**: It indicates how to split each row, either into training-validation or testing sets.

* **text**: This column contains the data that we need for training.

* **label_sexist**: This column represents the labels for the data, specifically indicating whether it is classified as **sexist** or not.

# About Data pre-pairing

### convert label using oneHotEncoding
```py
label_encoder = LabelEncoder()
data['label_sexist'] = label_encoder.fit_transform(data['label_sexist'])

num_classes = len(set(data['label_sexist'])) 
data['label_sexist'] = to_categorical(data['label_sexist'], num_classes=num_classes)
```
### Clean data

* remove emojie and other symbol
```py
def clean_text(text):
    text = text.lower()
    text = re.sub('[^a-zA-Z]', ' ', text)
    text = re.sub('\s+', ' ', text).strip()
    return text

```
## Tokenize and create vocab
```py
texts_train = X_train['text'].tolist()
text_val = X_val['text'].tolist()
tokenizer = Tokenizer()
tokenizer.fit_on_texts(texts_train)
X_train_sequences = tokenizer.texts_to_sequences(texts_train)

tokenizer.fit_on_texts(text_val)
X_val_sequences = tokenizer.texts_to_sequences(text_val)


max_sequence_length = 70  
X_train_padded = pad_sequences(X_train_sequences, maxlen=max_sequence_length)
X_val_padded = pad_sequences(X_val_sequences, maxlen=max_sequence_length)
```
The text data is split into training and validation sets, and then the Tokenizer class is employed to convert the raw text into sequences of numerical values.

# About Model

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

```py
model.add(Dense(1, activation='sigmoid'))

```

* **'1'**: This specifies the number of neurons or units in the Dense layer and here '1' bec problem is classification and output single value.

* sigmoid function : The sigmoid activation function is used for the single neuron in the output layer


## Using Early-stopping 

```py
early_stopping = EarlyStopping(monitor='val_loss', patience=5, restore_best_weights=True) 
```

Using early stopping in the **fit** function is generally employed to prevent overfitting. As an argument, we must specify something to monitor. In this case, we should monitor the validation loss. If it increases over 5 epochs, then training is stopped, and the best model with the lowest validation loss is saved.