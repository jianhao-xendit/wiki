#   Chapter X.4 - Jupyter

connect to your coder server in AZURE, (http://az-jump-lsazure.westeurope.cloudapp.azure.com/)  
then in visual studio code  
press CTRL + `  

in terminal:

```code
jupyter-notebook --ip=0.0.0.0 --port=8081 --no-browser --NotebookApp.token=''
``` 

>Make sure you put your Jupyter server behind a Traefik proxy with authentication if you use the token='' - otherwise anybody can run notebooks on your server.

> coder@00e3201dcdb1:~/project/project_threathunt$ jupyter-notebook --ip=0.0.0.0 --port=8081 --no-browser --NotebookApp.token=''
[W 13:45:37.199 NotebookApp] All authentication is disabled.  Anyone who can connect to this server will be able to run code.
[I 13:45:37.202 NotebookApp] Serving notebooks from local directory: /home/coder/project/project_threathunt
[I 13:45:37.202 NotebookApp] The Jupyter Notebook is running at:
[I 13:45:37.202 NotebookApp] http://00e3201dcdb1:8081/
[I 13:45:37.202 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[I 13:45:45.808 NotebookApp] 302 GET / (94.227.56.52) 0.57ms


You can now browse to http://az-jump-lsazure.westeurope.cloudapp.azure.com:8081 and run your notebooks.