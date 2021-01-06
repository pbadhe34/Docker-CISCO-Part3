
 Docker swarm deployment
  Change to the directory containing swarm cofig files
  
  Ensure NO containers running or stopped on host.
   >docker ps -a 
  >docker stop $(docker ps -aq)
   >docker rm $(docker ps -aq)

  Find out physical ip address of host
  >ifconfig
  --192.168.0.13

   Init the swarm manager on first host
  >docker swarm init --advertise-addr 192.168.0.13

  Swarm initialized: current node (tgzn6xwzisnfdf915kas99fow) is now a manager.

  To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-2xk3rpdcl53jkk60ah800ih8p16fh4jouwl1izjy1ifnb3lid5-e07pwyb770tc9wejzyvbaaqej 192.168.0.13:2377
   


  Check the node status active as leader
  >docker node ls
  Inspect the node
  >docker node inspect tgzn6xwzisnfdf915kas99fow

  To deploy aplication stack on swarm sluster
  >docker stack deploy -c docker-compose-defaultlink-redis.yml swarm-app

  Creating network swarm-app_webnet
  Creating service swarm-app_redis-host
  Creating service swarm-app_web-app


  To check the deployed stack
   >docker stack ls
       NAME        SERVICES       ORCHESTRATOR
        swarm-app        2         Swarm


    Inspect the stack status
     >docker stack ps swarm-app
 
     >docker stack services swarm-app
 
     >docker service ls

   ID                  NAME                   MODE                REPLICAS            IMAGE                   PORTS
d7eu9iqkgmma        swarm-app_redis-host   replicated          1/1                 redis:latest            
idapjd9slbp2        swarm-app_web-app      replicated          2/2                 pbadhe34/my-apps:app1   *:30000->8090/tcp

     Inspect the service 1 
   docker service inspect  swarm-app_redis-host 
       
   VirtualIPs": [
                {
                    "NetworkID":    "gd8hutcppgszzjrdx2dukg1u2",
                    "Addr": "10.0.1.2/24"
                }


  Inspect thew eb app service
   
  docker service inspect  swarm-app_web-app

    "Ports": [
                {
                    "Protocol": "tcp",
                    "TargetPort": 8090,
                    "PublishedPort": 30000,
                    "PublishMode": "ingress"
                }

    "VirtualIPs": [
                {
                    "NetworkID": "m4vxf5js8s8u27fpr890dm4wi",
                    "Addr": "10.0.0.3/24"
                },
                {
                    "NetworkID": "gd8hutcppgszzjrdx2dukg1u2",
                    "Addr": "10.0.1.5/24"
                }


    >docker service ls
     
   Check the service app running, in the browser
      
     http://192.168.0.13:30000
 
Or by curl on commandline

   curl http://localhost:30000
   curl http://192.168.0.13:30000


  List the tasks of one or more services
  >docker service ps swarm-app_web-app
    --service instances running

  Check the log of the service

  >docker service logs swarm-app_web-app

  >docker service logs swarm-app_redis-host
  
  Scale  up the serviec istnaces
  >docker service scale swarm-app_web-app=3

  Verify that the service inarsnaces are getting invoked by sending requests
  
   Scale  up the serviec istnaces one more
  >docker service scale swarm-app_web-app=5

  Run docker ps to identify no of containers running
    >docker ps
  Ki
ll one of the container uisng the id
   >docker kill xg40tkw7y5fc

  Verify the service status
    >docker service ps swarm-app_web-app 

 Scale down the service instances
    >docker service scale swarm-app_web-app=2
  


  To update the node

  >docker node update [options] <node id>

   
Options:
      --availability string   Availability of the node ("active"|"pause"|"drain")
      --label-add list        Add or update a node label (key=value)
      --label-rm list         Remove a node label if exists
      --role string           Role of the node ("worker"|"manager")

  
  To update the service without restarting
   >docker service update [options] <serviecid> 

  image,hostname, replicas,rollback,args,constarints,restart condition string,username,workdir,update parraleosm, resource usage constraints,tty,publish port

  To deploy another app stack

  To deploy aplication stack on swarm sluster
  >docker stack deploy -c docker-compose-redis-link.yml app1

  10.0.0.14

  Cleanup and reboot
  To remove the stack

   >docker stack rm swarm-app
 
  To leave the node from swarm

   >docker swarm leave <node-id>



   

