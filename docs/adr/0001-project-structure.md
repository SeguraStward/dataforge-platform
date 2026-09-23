# Context
While creating the project I had to chose among several folder-structures. I wanted to chose the one that can help me maintaine it and keep it easy to read and navigate.

# Options
- Domain/application/infrastructure: clean and organized based on the activities each file will do
- routes/models/services: good but not as clean.
- src/ to wrap the three layers vs putting them at the root
- tests/ as its own top-level folder vs nested inside one layer
- pyproject.toml vs requirements.txt

# Decision
I choose the more adecuate options to keep clean arquitecture. Domain/app/infra structure to keep it organized and easy to navigate, as well it is important to understand that inner layers does not depend on outter layers so if required we can swap technologies easily and without causing problems. Tests must be in its own root folder because it needs access to all the code that we need to test and we can use as well a subfolder structure there to separete the tests of Domain, app and infrastructure. About wrapping all inside src/ was because it separates the actual python package from the root-level project files(Docker config, docs, tests). We chose pyproject.toml over requirements.txt because the new approach is one standardized file for metadata + dependencies + tool.config, instead the old was scattered.

 # Consequences
 - swapping infrastructure technologies without touching business rules.
 - Easier to maintaine.
 - Modularity established
 - I had to create folders that does not have any logic or code yet