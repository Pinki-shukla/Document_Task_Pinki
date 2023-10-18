



<center> <u> <h1 style="font-size: 50px;">Grafana Setup Documentation</center></h1> </u> </center><br>


## Task requirement:
"Setting Up Monitoring Infrastructure with Grafana, Prometheus, and Node Exporter".

## Monitoring :

Regular collection of information and data to measure progress of projects and activities so we can track performance and resources utilisation over time.

  
## System Configuration:

* OS Name : Ubuntu 20.04.6 LTS
* Podman version:- 3.4.2
* RAM : 5.6 GiB
* CPU : 12
* STORAGE : 512.1 GB
## Prerequisite tools:
- Podman 
- Grafana
- Prometheus
- Node-exporter

# Definition of tools

## Podman: 
Podman is an open source tool for developing, managing, and running containers
on your Linux® system


## Grafana :

Grafana open-source software enables you to query, visualise, alert on, and explore your metrics, logs, and traces wherever they are stored.

* Query
* Visualise
* Alert

## Prometheus :

Prometheus is an open source monitoring solution written in Go that collects metrics data and stores that data in a time series database. 
Grafana allows to visualise the data stored in prometheus.

## Node-exporter:
Node Exporter is a software tool that collects system-level metrics from servers and nodes, exposing them in a format that can be scraped by Prometheus for monitoring and analysis.

Node Exporter collects a wide range of system metrics, including CPU, usage Memory, usage Disk usage, System load,Filesystem statistics.






# Installation Process:

## Step-1 Install podman:

```
sudo apt install -y podman

```

## Step-2 Create a Grafana container:
To run the latest stable version of Grafana, run the following command:

```
podman run -d -p 3001:3000 --name=grafana docker.io/grafana/grafana-enterprise

```

![](images/vim%20grafana%20command.png)


Where:

<b>  run</b> = run directly from the command line</br>
<b>d </b>= run in the background </br>
<b>p </b>= assign the port number, which in this case is 3001</br>
<b>name</b> = assign a  name to the container, for example, grafana</br>


<b> docker.io/grafana/grafana-enterprise: </b> This is the name of the image we want to run inside the container. It's called 'grafana-enterprise,' and it's stored in a special place on the internet called 'Docker Hub.'</br>

http://localhost:3001 hit on web browser to see Grafana login page in your web browser.


![](images/welcome1.png)

![](images/vim%20demo%20grafana.png)

## Use these options to create dashboards and alerts.
![](images/vim%20manager%20grafana.png)



## Step-3 Create Prometheus container on Podman:

<b>Create directory :-</b>
```
mkdir prometheus
```
<b>Create file:-</b>
```
 vim prometheus.yml
 ```

#### prometheus.yml is a configuration file of prometheus.

(push all the data in your prometheus.yml file which has been given below)

```
global:
  scrape_interval: 5s
  external_labels:
	monitor: 'node'

scrape_configs:
  - job_name: 'prometheus'
	static_configs:
  	- targets: ['192.168.1.57:9090']

  - job_name: 'node-exporter'
	static_configs:
  	- targets: ['192.168.1.57:9100']

  ```



```
  podman run -d --name prometheus -p 9090:9090 -v /home/pinki/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml docker.io/prom/prometheus
 ```

 ![](images/vim%20prometheus.png)

<b>podman run:</b> This part of the command instructs podman to run a container.

<b>d:</b> This flag stands for "detached" mode. It runs the container in the background

<b>--name prometheus:</b> This flag assigns a name to the container. In this case, the container is named "prometheus."

<b>-p 9090:9090:</b> This flag specifies port mapping. It tells podman to map port 9090 on the host to port 9090 inside the container. 

<b>-v /home/pinki/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml:</b> This flag specifies a volume mount. It connects a directory or file on your host system to a location inside the container.

In this case, it's mounting the file /home/pinki/prometheus/prometheus.yml from your host into the container at /etc/prometheus/prometheus.yml 

  * <b>" /home/pinki/prometheus/prometheus.yml":</b> This is the path to the Prometheus configuration file on your host. It's being shared with the container.  

*  <b>"/etc/prometheus/prometheus.yml": </b>This is where Prometheus expects its configuration file to be inside the container.

<b> docker.io/prom/prometheus: </b>This is the name of the Docker image you want to run as a container. It specifies the image's repository and name. In this case, you are running the "prometheus" image from the "prom" repository on Docker Hub

http://localhost:9090 on web browser to see prometheus.



  ![](./images/PROMETHEUS.png)


  ![](expoerters.png)

 
## Step-4 Create Node exporter container on podman

```

podman run -d --name=node-exporter -p 9100:9100 -v"/:/host:ro,rslave" quay.io/prometheus/node-exporter:latest --path.rootfs=/host

 ```

 ![](images/vim%20node%20exporter.png)

 <b>podman run:</b> This part of the command instructs podman to run a container.

 <b>-d:</b> This flag stands for "detached" mode.</b>

 <b>--name=node-exporter:</b> This flag assigns a name to the container. In this case, the container is named "node-exporter." 

 <b>-p 9100:9100: </b>This flag specifies port mapping. it allows you to access the service inside the container via port 9100 on your host.

 <b>"/:/host:ro,rslave": </b> This part specifies the volume configuration. It tells podman to mount the root directory of your host (represented by <b>"/"</b>) to the <b>"/host"</b> directory inside the container. The<b> "ro"</b> option stands for "read-only," which means the container can read the files on the host but can't modify them. The rslave option is related to mount propagation, allowing mounted file systems to be shared among containers.






 <b>quay.io/prometheus/node-exporter:latest:</b> In this case, you are running the "node-exporter" image from the "prometheus" repository on Quay.io. The ":latest" tag indicates that you want to use the latest version of this image.


 <b>--path.rootfs=/host:</b> This is an additional command passed to the container. It specifies the root file system path as "/host" inside the container. This can be important for some containerized applications to correctly access system resources.

 http://localhost:9100 on web browser to see Node-expoerter.

 ![](images/node.png)

 

 ## Now you can see our all containers are ready grafana,prometheus and node-exporter

  


 ```
 podman ps
  ```

  The <b>podman ps</b> command is used to list the currently running containers on your system. It provides information about the containers that are actively running and includes details such as the container ID, names, status, and other relevant information.

 ![](./images/grafana%20images.png)







## Grafana setup has been ready with Dashboard. 

![](images/grafana%20dashboard%20setup.png)


    

    


    












