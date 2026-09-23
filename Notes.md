 # create an isolated environment
python3 -m venv venv             
 # activate it (Linux/bash)
source venv/bin/activate  
# install your project + dev tools, editable mode       
pip install -e ".[dev]"           
