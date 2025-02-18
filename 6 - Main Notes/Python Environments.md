Links: [[python]] [[environments]]

**Python Environments** allow you to manage dependencies for your projects efficiently. the two most common types of environments are virtual environments (`venv`) and `conda` environments.
#### Creating Virtual Environments

Inside your project directory, you can create a virtual environment using the `venv` module.

```sh
python3 -m venv myenv
```
*Creates a `venv` called `myenv` in the current directory*

To activate the environment

```sh
source myenv/bin/activate
```

Once activated, install packages using 

```sh
pip install <package-name>
```

To deactivate 

```sh
deactivate
```

##### Using Conda

```sh
conda create --name myenv python=3.9
```

For a select version of Python. Otherwise disregard the python statement for the latest version.

To activate

```sh
conda activate myenv
```

To deactivate

```sh
conda deactivate
```

Installing packages

```sh
conda install <package-name> 
# or
pip install <package-name>
```
##### Using virtualenv

```sh
virtualenv myenv
```

#### Environment Variables

Used to store configuration settings and secrets, such as API keys and database credentials.

Temporary variables

```sh
export API_KEY='your-api-key'
```

Permanent in shell configuration

```sh
echo "export API_KEY='your-api-key'" >> ~/.bashrc 
source ~/.bashrc
```

```sh
echo "export OPENAI_API_KEY='your-api-key'" >> ~/.zshrc 
source ~/.zshrc
```

for bash and zsh.

Accessing environment variables in python

```python
import os

api_key = os.getenv('API_KEY')
if api_key is None:
	raise ValueError("No API key found. Please set the OPENAI_API_KEY
	environment variable.")
```

#### Extra

Switching virtual environments also switches which python interpreter you're using since it makes a copy of it. Check which interpreter you're using with

```sh
which python
# or
which python3
```

To see which packages you have in your environment use

```sh
pip list
```

Installing package of a specific version

```sh
pip install numpy==1.20.0
```
