# CTranslate2-no-execstack

Python wheel for CTranslate2 with execstack stripped from the library files. 

Intended for used when building `snap` applications that require CTranslate2.

Approximate notes on how to build the wheel. Some steps may be missing.

```
git clone https://github.com/OpenNMT/CTranslate2
cd CTranslate2

# Run docker the Ctanslate uses to build their wheels
docker run -it -v $(pwd):/ctranslate2 quay.io/pypa/manylinux2014_x86_64 /bin/bash

cd ctranslate2

# Run prepare environemnt script. Script in repo has modifications to account for SSL certificate issues.
# This will also build and install CTranslate2
sh prepare_build_environment_linux.sh

# Fix execstack issue
yum install prelink
execstack --clear-execstack /usr/local/lib64/libctranslate2.so.4.2.1

# Build the python wheel as per CTranslate2 instuctions https://opennmt.net/CTranslate2/installation.html

# Optimize it to inline CTranslate2 libraries 
python3.10 -m pip install auditwheel
auditwheel repair dist/ctranslate2-4.2.1-cp310-cp310-linux_x86_64.whl
```