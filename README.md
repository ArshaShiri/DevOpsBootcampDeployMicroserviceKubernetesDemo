# Deploy Microservices Application Using Kubernetes

The microservice project can be found [here](https://github.com/nanuchi/microservices-demo). This project deploys an online shop.

Including redis, we have 11 services that are configured under config.yaml file.

After configuring all the services we deploy them on a linode server:

![image](https://user-images.githubusercontent.com/18715119/228036732-01190157-9be4-4f13-b7d5-92cd8b116990.png)

![image](https://user-images.githubusercontent.com/18715119/228040056-52565eb5-dc3b-456c-8baa-c7b3b382e5f5.png)


After downloading the config file from Linode:

    # As a good security practice, we limit the premissions of the config file to read only
    chmod 400 online-shop-microservices-kubeconfig.yaml
    
    export KUBECONFIG=~/Desktop/online-shop-microservices-kubeconfig.yaml
    kubectl get node
      # NAME                           STATUS   ROLES    AGE     VERSION
      # lke99885-150136-6421ed61d170   Ready    <none>   2m19s   v1.25.4
      # lke99885-150136-6421ed623179   Ready    <none>   2m9s    v1.25.4
      # lke99885-150136-6421ed62907f   Ready    <none>   103s    v1.25.4
      
    # We now deploy the microservices in our cluster
    
    # First we create a namespace
    kubectl create ns microservices
    
    kubectl apply -f {path-to-config-file-in-this-repo} -n microservices
    
    ❯ kubectl get pod -n microservices
      # NAME                                     READY   STATUS         RESTARTS   AGE
      # adservice-5f5548d7df-q6qj2               0/1     ErrImagePull   0          36s
      # cartservice-7c456ddbb-p9pz9              1/1     Running        0          36s
      # checkoutservice-7ffd856f7-brw7d          1/1     Running        0          35s
      # currencyservice-c7658f7-2tb7h            1/1     Running        0          36s
      # emailservice-5cb9d6869c-rdjp4            1/1     Running        0          36s
      # frontend-675676fdb9-w6mjz                1/1     Running        0          35s
      # paymentservice-67c5f5c999-q9ln9          1/1     Running        0          36s
      # productcatalogservice-5db77bc769-9gm2v   1/1     Running        0          36s
      # recommendationservice-677f9c56c9-2ldd7   1/1     Running        0          36s
      # redis-cart-544975fd89-r89ww              1/1     Running        0          35s
      # shippingservice-7c798f9559-969qr         1/1     Running        0          36s
      
    ❯ kubectl get svc -n microservices
      # NAME                    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
      # adservice               ClusterIP   10.128.238.46    <none>        9555/TCP       2m11s
      # cartservice             ClusterIP   10.128.20.157    <none>        7070/TCP       2m11s
      # checkoutservice         ClusterIP   10.128.85.175    <none>        5050/TCP       2m11s
      # currencyservice         ClusterIP   10.128.94.12     <none>        7000/TCP       2m12s
      # emailservice            ClusterIP   10.128.73.187    <none>        5000/TCP       2m12s
      # frontend                ClusterIP   10.128.141.188   <none>        80/TCP         2m11s
      # frontend-external       NodePort    10.128.99.81     <none>        80:30007/TCP   2m11s
      # paymentservice          ClusterIP   10.128.176.248   <none>        50051/TCP      2m12s
      # productcatalogservice   ClusterIP   10.128.99.17     <none>        3550/TCP       2m12s
      # recommendationservice   ClusterIP   10.128.143.17    <none>        8080/TCP       2m12s
      # redis-cart              ClusterIP   10.128.34.200    <none>        6379/TCP       2m10s
      # shippingservice         ClusterIP   10.128.216.245   <none>        50051/TCP      2m11s
      
    # The 30007 port can be accessed via any of the created nodes on Linode
    
![image](https://user-images.githubusercontent.com/18715119/228047322-dd85926e-870f-4532-99c3-94e5d097ac1a.png)

    
