#bigram, trigram model 
##Bigram model
###Create Bigram model
Well, first we created a bigram list, and then we transformed it into a bigram model.
It can show us which word comes after the first word in the bigram list.
```python
bigramsList = list()
for sentence in df["text"]:
    words = sentence.split()
    bigrams = ngrams(words, 2)
    for bigram in bigrams:
        bigramsList.append(bigram)

bigram_model = defaultdict(list)
for bigram in bigramsList:
    first_word, second_word = bigram
    bigram_model[first_word].append(second_word)

bigram_model
```
###Possible next words
Here, we have a function that can provide us with which word comes after a given word.
```python
def possible_next_words(current_word,model):
    return model.get(current_word, [])

```
###Probability
Here, we have a function to calculate the probability of each subsequent word coming after my word.
```python
def probability(next_words):
    word_count = len(next_words)
    word_occurrences = {}
    for word in next_words:
        word_occurrences[word] = word_occurrences.get(word, 0) + 1
    
    percentages = {word: (count / word_count) * 100 for word, count in word_occurrences.items()}
    sorted_percentages = dict(sorted(percentages.items(), key=lambda item: item[1], reverse=True))
    return sorted_percentages
```
###Final function,Top Probability Words 
Here, we have the final prediction function. This function requires your word, model, and the number of words you need. It returns the top words with their probability percentages.
```python
def topProbabilityWords(word,count,model):
  next_words = possible_next_words(word,model)
  prob = probability(next_words)
  return list(prob.items())[:count]
```

##Trigram model
###Create trigram list
Here, we create a trigram list similar to the bigram list described [above](#create-bigram-model).
```py
thregramList = list()

for sentence in df["text"]:
    words = sentence.split()
    bigrams = ngrams(words, 3)
    for bigram in bigrams:
        thregramList.append(bigram)
print(len(thregramList))
thregramList[:10]
```
###Filter the trigram list based on our word.
Here, we have a function that takes our trigram list and our word as input and filters the list based on our word.
```py
def filter_trigrams(trigram_list, specific_word):
    filtered_trigrams = []
    for trigram in trigram_list:
        if trigram[0] == specific_word:
            filtered_trigrams.append(trigram)
    return filtered_trigrams
```
###Which word comes after my word?
Here, we create a function to show us which word comes after our word along with the probability percentage to solve this question.
```py
def calculate_second_word_frequency(data, target_word):
    second_word_count = defaultdict(int)
    total_instances = 0
    
    for first, second, _ in data:
        if first == target_word:
            second_word_count[second] += 1
            total_instances += 1
    
    second_word_percentage = []
    for second, count in second_word_count.items():
        percentage = (count / total_instances) * 100
        second_word_percentage.append((second, percentage))
    
    sorted_result = sorted(second_word_percentage, key=lambda x: x[1], reverse=True)
    
    return sorted_result
```
###Merge the first two words.

Here, we combine the first two words into a tuple and another word into an array like this: ```[(Firstword, secondWord), thirdWord]```
```py
def extract_following_word(filtered_trigrams):
    result = []
    for trigram in filtered_trigrams:
        if len(trigram) >= 3:
            result.append((trigram[:2], trigram[2]))
    return result
```
###Which word comes after the second word?
Here, we create a function to show which word comes after all second words.
```py
def extract_second_word_following(processed_trigrams):
    result = {}
    for trigram in processed_trigrams:
        first_two_words, following_word = trigram
        second_word = first_two_words[1]
        if second_word not in result:
            result[second_word] = []
        result[second_word].append(following_word)
    return result
```
###Which word is more probable to come after my two words?
Here, we create a function to calculate this. It takes a trigram list, the first and second words, and returns a sorted list of words along with their percentages. 
```py
def possible_words_after_second_word(processed_trigrams, specific_word, second_word):
    following_words = []
    total_occurrences = 0
    word_counts = Counter()
    
    
    for trigram in processed_trigrams:
        first_two_words, following_word = trigram
        if first_two_words[0] == specific_word and first_two_words[1] == second_word:
            word_counts[following_word] += 1
            total_occurrences += 1
    
    
    sorted_words_with_percentage = []
    for word, count in word_counts.items():
        percentage = (count / total_occurrences) * 100
        sorted_words_with_percentage.append((word, percentage))
    
    sorted_words_with_percentage.sort(key=lambda x: x[1], reverse=True)
    
    return sorted_words_with_percentage
```
###Final function: Generate Top Probability Sentence
Here, we create a function to generate the top possible sentences. It takes a list, a starting word, and the number of sentences needed, and it returns a list of sentences.
```py
def topSentancePosible(listTrigram,fistWord,count):
  processed_trigrams = extract_following_word(listTrigram)
  filtered_trigrams = filter_trigrams(listTrigram, fistWord)
  result = calculate_second_word_frequency(filtered_trigrams, fistWord)[:count]
  sentences =list() 
  for word in result:
     words = possible_words_after_second_word(processed_trigrams,fistWord,word[0])
     sentences.append(fistWord+' '+ word[0]+' '+ words[0][0] )
  return sentences
```
#
#
###### I'm a JavaScript developer, and I struggle with programming in Python. I used GPT to correct this.

#####Our main goal in solving this exercise is to construct two models, one bigram and the other trigram, and to find the best prediction for the next word given the entered word.
#####The provided code is attached, and I don't want to waste too much of your time with line-by-line explanations. I'll provide descriptions for the important parts for you.