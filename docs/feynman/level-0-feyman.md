# Level 0 Explanation 
First we installed python and the dependencies for the frameworks, tests.
We initialized git to keep track of files.
Creation of a docker compose file to run a database as PostgreSQL for our project, creating a volume so we persist the data even if we turn off the container, so with docker compose up we are setting up all the necessary configurations for example downloading the PostgreSQL image, creating users, password and database name and using or creating the volume for the database.
We established a proper structure of folders to keep standardized and clean architecture practices, Domain/application/infrastructure with this approach we can swap technologies in the infrastructure without affecting other areas such as domain or application in fact, application depends on domain, and infrastructure depends on application. 
Now we have implemented CI, that is, Every time we push to main or when creating a PR. This will create a VM and then this steps will be applied checkout, setup-python, install dependencies from pyproject.toml, and then tests will be applied to the code and depending on the output we could merge the PR to the main branch. 
