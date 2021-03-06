# DRP 15 - Server

## Requirements

- Python 3.8 or above
- [FastAPI](https://fastapi.tiangolo.com/tutorial/first-steps/)
- [MongoDB](https://docs.mongodb.com/manual/core/schema-validation/)
- [Docker](https://docs.docker.com/engine/install/)


## Running the app locally

First download and set up MongoDB locally by following the tutorial:

https://medium.com/swlh/get-up-and-running-with-mongodb-in-under-5-minutes-abc8770b1ef8#:~:text=Open%20the%20terminal%2C%20and%20navigate,version%20and%20mongod%20%2D%2Dversion%20.


You can run the server locally as followed:

```shell
# Create a new virtual environment
python3 -m venv env 
source env/bin/activate

# Install required packages
pip3 install -r requirements.txt 
```

**_IMPORTANT_**: Do not use production database when running locally! 

```shell
export DB_CONNECTION_STRING=<dev database connection string>
```

Note: When testing on local database, set it to something like

DB_CONNECTION_STRING="mongodb://127.0.0.1:27017/"

And for MongoDB Atlas, do something like

"mongodb+srv://<host>:<server>@cluster0.jqoga.mongodb.net/<database>?retryWrites=true&w=majority"

```shell
uvicorn --port <port_number> --workers 8 niched.main:app
#replace <port_number> with a port number to run the web app on, different to the database's port number
```

## Testing locally

**_IMPORTANT_**: Again, use development database as before!

### Running unit tests
Unit tests should not connect to the database or communicate with the 'outside world'

```shell
# Create a new virtual environment
python3 -m venv env 
source env/bin/activate
pip3 install -r requirements.txt 
export PYTHONPATH=.

pytest --cov-report term-missing:skip-covered --cov=niched/ test/unit_tests/
```

When running integration tests, use **dev** database! You can also create your own database locally,
and then connect to this database in development stage. 

To create a local mongoDB, follow [this guide](https://docs.mongodb.com/manual/installation/)! 
```shell
# Create a new virtual environment
python3 -m venv env 
source env/bin/activate
pip3 install -r requirements.txt 
export PYTHONPATH=.
export DB_CONNECTION_STRING=<database connection string>
export DB_DATABASE="develop"

pytest --cov-report term-missing:skip-covered --cov=niched/ test/integration_tests/
```

For example, to run the server locally, set `DB_CONNECTION_STRING` to `mongodb://127.0.0.1:27017/`

[Pytest](https://docs.pytest.org/en/6.2.x/index.html) has more options for reports, see the page for more information!

To run GitLab-CI test pipelines:

```shell
gitlab-runner exec docker unit-test
gitlab-runner exec docker integration-test
```