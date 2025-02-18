**Type:** [[]]  
**Topics:** #  
**Date:** 2024-07-09  
**Source/Author:** {{Source/Author}} 
- **Tags**
	- 

---
# Notes

In a Unix-Based system, you can store environment variables in your operating system's environment. [[Environment variables]] are useful for keeping *secret keys* safe and secure.

Some useful commands for the shell

```sh
printenv
# or
env
```
*prints a list of all environment variables and their values.*

```sh
export VARIABLE_NAME="value"
# Temporarily set env variable in current terminal session

echo 'export VARIABLE_NAME="value"' >> ~/.bashrc
source ~/.bashrc # Reload configuration file
# Permanently set env variable in current terminal session
```

```Python
def getenv(  
	key: str,  
	default: _T@getenv  
) -> (str | _T@getenv): ...
```

Get an environment variable, return None if it doesn't exist. The optional second argument can specify an alternate default. key, default and the result are str.

Some useful methods to use in python is to import the built in `os` package, and use `os.environ.get('VARIABLE')` or `os.getenv('VARIABLE')`




---

## Questions
- Question 1
- Question 2

## Further Exploration
- Links to related articles, videos, or books

## Personal Insights
- Personal reflections or insights gained from the media
