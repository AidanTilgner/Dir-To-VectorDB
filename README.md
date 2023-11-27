# Dir to Vector Search
Dir to Vector Search is a simple script which lets you vectorize a directory of markdown or text files, then search through them using `sqlite-vss`. 

## How does it work?
The script will run through every markdown or text file in the provided directory, and add its content, as well as its embeddings to a sqlite database. The script uses OpenAI embeddings by default, but you can change this in `ai.py`. In the future I may add support for multiple types of embeddings if people are interested.

Once you've run the script, you will be able to search the database as a normal database, or using vector similarity search. This means you can use it for things like Retrieval Augmented Generation, or just general search functionality.

# Setup
To get set up, it's pretty simple. Just provide an OpenAI API Key in the environment. If you need to add an API key, add it on the [OpenAI Platform](https://platform.openai.com/api-keys). 

## Environment
Simply clone the `.env.example` file in the root directory to a new file called `.env`, and fill in the dummy values. The OpenAI API key is required to generate embeddings.

Once you've set that, you may also notice the other values in the `.env.example`, which you can also customize based on your preference. Such as:
- "DEFAULT_FILE_LIMIT": The maximum amount of files that the script will collect before starting processes
- "DEFAULT_DELAY_PER_REQUEST": The amount of seconds which the program will wait after processing each file, useful for rate limiting

## Dependencies
Don't forget to clone the dependencies in `requirements.txt`. I'd recommend setting up a python virtual environment like so:

```bash
python -m venv ./dtv-env
```

Then activating it using the activate script:

```bash
source ./dtv-env/bin/activate
```

And finally running `pip install`:

```bash
pip install -r requirements.txt
```


# Usage
Using the script is as simple as running it with python.

## Database Generation
After you've gotten everything setup, you can run the main script like so:

```bash
python main.py <path-to-target-directory>
```

This will create a database with vector search abilities, based on the directory provided. However, there are also some flags that you can use to customize the experience:

- `-l`, `--limit`: The maximum amount of files that the script will collect before starting processes, overrides `DEFAULT_FILE_LIMIT`
- `-d`, `--delay`: The amount of seconds which the program will wait after processing each file, useful for rate limiting, overrides `DEFAULT_DELAY_PER_REQUEST`
- `--no-delay`: Sets the delay to 0s between each request


## Querying the Database
Once you've generated the database, you can query it with the `query.py` script:

```bash
python query.py <query>
```

Keep in mind, by default the vector similarity will be on the `content` of each file, however you can switch it to content with the `--on` flag:

```bash
python query.py <query> --on title
```


# Considerations
One thing to consider is rate-limiting. I wrote the script based on the [official rate limits guidance](https://platform.openai.com/docs/guides/rate-limits/usage-tiers?context=tier-free) from OpenAI. This is based on the 'free tier', but if you have a different tier then you can adjust the values accordingly. The possibility of rate limiting increases based on the number of files and the size of your files.


