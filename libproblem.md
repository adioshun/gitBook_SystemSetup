Link OpenCV 4 into your Python 3 virtual environment 

```python 
# Let’s create a symbolic link from the OpenCV install directory to our virtual environment:

$ cd ~/.virtualenvs/cv/lib/python3.5/site-packages/
$ ln -s /usr/local/lib/python3.5/site-packages/cv2.cpython-35m-x86_64-linux-gnu.so cv2.so
$ cd ~
```

OSError: libcuda.so.1: cannot open shared object file:

`export LD_LIBRARY_PATH="/path/to/sdk/lib"`



