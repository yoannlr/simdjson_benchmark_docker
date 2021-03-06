# Simdjson benchmarking tool

![An example of graph generated by this tool](doc/graph.png)

* This script will run benchmarks on each commit of a specific git repo (intended for [simdjson](https://github.com/simdjson/simdjson)).
* It is modular: you can add as much benchmark scripts as you want (these are just python scripts in the `benchmark_scripts` directory).
* The results are stored in JSON files, in the `benchmark_results` directory.
* It should run in a docker container (the container is not finished yet).
* A cron command should ask the container to run `commits.py` once a day, in order to perform the benchmarks on all the new commits on the main repo.
* All variables can be configured by editing `commits.py`.
* If enabled, the script will attempt to upload all the results to a server via FTP. On the FTP home of this server, you should have a `benchmark_results` directory, and the `index.html` and `main.js` files. When accessed from a web browser, the `index.html` file will generate graph of the results.

## Setup

* To create the container, run `docker build -t containername .`.
* A cron job should ask the container to run the script: `docker run containername '/usr/bin/python3 /benchmarks/commits.py'`.
* You may want to edit the configuration. It can be done by editing the script. To do so, you'll have to start a bash on the container: `docker run -it containername /bin/bash`, then you can `vim /benchmarks/commits.py`.
* If you want to use FTP to upload the results to an external server, you will have to create `/benchmarks/ftppass` and put the FTP password inside. Same process, start a bash and edit the file. The server should contain a `benchmark_results` folder and the `index.html` and `main.js` files. The FTP home for the server should be the same as the web server root.

## Evolutions
* Since the tool was originally intended to generate a single image graph and send it to a server, it is using several JSON files to store the results. The whole file are uploaded to the server each time, it is not optimal. Using a database on the server and simply inserting the new rows of data would be a better solution, and would be a much more precise way of selecting data when we need to create the graph.
