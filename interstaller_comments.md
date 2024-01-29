Copy past code from apify > api client > python version
then get Token from setting and save in your computer user-enviroment 

```py
env_variable_name = 'Token-apify'
token = os.environ.get(env_variable_name)
# Initialize the ApifyClient with your API token
client = ApifyClient(token)
    
# Prepare the Actor input
run_input = {
    "directUrls": [
        "https://www.instagram.com/p/Cr4SocCrFxK/",
        "https://www.instagram.com/p/COk-fzlpVl5/",
        "https://www.instagram.com/p/1CHSY2Ivb6/",
        "https://www.instagram.com/p/1Ldm-uIvR4/",
        "https://www.instagram.com/p/1B2LoDoven/",
        "https://www.instagram.com/p/1BSWWmIvTZ/",
        "https://www.instagram.com/p/0-07N0ovan/",
        "https://www.instagram.com/p/08bwmpIvWR/",
        "https://www.instagram.com/p/08AvcMIvS1/",
        "https://www.instagram.com/p/C1kEzHhoBWp/?img_index=1",
        "https://www.instagram.com/p/C14prPbIwk4/",
        "https://www.instagram.com/p/C14kHM6sGPE/",
        "https://www.instagram.com/p/C06p_LAo_JG/",
        "https://www.instagram.com/p/CzO6fxOouGx/?img_index=1",
        "https://www.instagram.com/p/Cwxrs7wIRqM/",
        "https://www.instagram.com/p/CipzoTiIS-P/",
        "https://www.instagram.com/p/CiNWpwgo_FU/",
        "https://www.instagram.com/p/CaNEVM-o8Ds/",
        "https://www.instagram.com/p/CaXZ-mIJOfr/?img_index=1",
        "https://www.instagram.com/p/CXgkblaons2/?img_index=1",
        "https://www.instagram.com/p/CVvWbXIoqgr/?img_index=1"
        "https://www.instagram.com/p/CR8-M5wo_gQ/",
        "https://www.instagram.com/p/CPvj2lrA4CZ/?img_index=1",
        "https://www.instagram.com/p/CPbAXRGAvEe/",
        "https://www.instagram.com/p/CCJjVP2Iadi/",
        "https://www.instagram.com/p/CBV9NXboSFf/",
        "https://www.instagram.com/p/BydIGw1oYSz/"


    ],
    "resultsLimit": 100,
}

# Run the Actor and wait for it to finish
run = client.actor("SbK00X0JYCPblD2wp").call(run_input=run_input)

# Fetch and print Actor results from the run's dataset (if there are any)
result_list = []
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    result_list.append(item)

# Convert the list of results into a DataFrame
df = pd.DataFrame(result_list)
```

After obtaining the data, we create a dataframe and populate it with the data


# clean data 
```py
def clean_text(text):
    
    if isinstance(text, (str, bytes)):
        # Remove non-alphabetic characters
        text = re.sub('[^a-zA-Z]', ' ', text)
        text = text.lower()
        # Remove extra whitespaces
        text = re.sub('\s+', ' ', text).strip()
        return text
    else:
        return np.nan

data['text'] = data['text'].apply(clean_text)
data = data[data['text'].notna() & (data['text'] != '')].reset_index(drop=True)
```
we clean comments from emojies and other symbol and charcter whom is not english
and drop null value comment from DataFrame



# save
```py
data.to_csv('interstaller_comments_finall.csv')
```
then save it as csv file :D