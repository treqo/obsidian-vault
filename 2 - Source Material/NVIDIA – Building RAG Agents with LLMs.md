**Type:** [[Certificate]] [[computers]] [[LLM]] 
**Topics:** #  
**Date:** 2024-07-06  
**Source/Author:** {{Source/Author}} 
- **Tags**
	- 

---
# Notes

### Introduction

In this course, we learn how to make sophisticated chatbots that can perform general dialogue management and **retrieval augmented generation** (RAG) at scale.

### Environment and LLMs

The Jupyter Lab Interface is a dashboard for iPython notebooks.


![[Screenshot 2024-07-06 at 9.36.35 PM.png]]
*A picture of the setup of jupyter lab*
#### Inputs, Passwords, Secrets

```Python
from getpass import getpass
from pydantic import SecretStr

first_name = input("Enter your name: ")
print(f"Hello {first_name}\n")   ## F-strings. Commonly used to fill in variables

secret = SecretStr(getpass("Please tell me a secret: "))
print("Your secret is:", secret)  ## Another way to print multiple values. sep=" " is default
print("Your secret value is:", secret.get_secret_value())
```

input just takes in a normal input, but getpass results in "dots" being shown for chars in the input, and secret using the SecretStr function turns any char into an asterisks `*`. Here is the output:

```txt
Enter your name:  Tareq

Hello Tareq

Please tell me a secret:  ········

Your secret is: **********
Your secret value is: hello world
```

#### Multi-line Grouping

```Python
################################################################################
## Implicit string concatenation. Interpretted as `print( "Hello" "World" )`
print(
    "Hello" "World"
)

################################################################################
## Implicit string concatenation. Interpretted as `print( "5" " " "8" )`
print((
    "5" " "
    "8"
))

print(
    5 +
    6
)

################################################################################
## The below line is considered bad syntax, and the cell will fail to evaluate
# 5 +
# 6

################################################################################
## The following forcefully tries to run contents of the following line:
try:
    eval("5 + \n6")
except Exception as e:
    print(e)

################################################################################
## Useful example, where lines can be commented out to remove functionality:
(
    f"Hello {first_name}"
        .upper()
        + '!!'
        # '!!!'    ## Uncomment to add even more. Useful for organizing
        ' It\'s so nice to meet you!'

)
```

---

## Questions
- Question 1
- Question 2

## Further Exploration
- Links to related articles, videos, or books

## Personal Insights
- Personal reflections or insights gained from the media
