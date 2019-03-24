# NSX-T Panel App
![alt text](https://www.dropbox.com/s/0tyapibwz0xfo4m/Screenshot%202019-03-22%2017.31.01.png)

# Quick highlights:
- This is a lightweight app running on Express/NodeJS (backend) and React (frontend).
- User authentication (HTTP basicAuth).
- Reporting various NSX-T health stats like CPU, Mem, Disk and cluster status.
- Reporting some logical config (e.g. Fabric Nodes, Logical Routers, T0 SNAT Translations), etc.
- The app is dockerized with all its dependencies and can be deployed on any K8s cluster with the sample YAML config in this repo.
- The config is external (nothing is hardcoded). Passwords are encoded in a K8s Secret.
- The app is stateless and can run via multiple pods for load balancing or high-availability.

# Usage
You can set the config from within the YAML file itself as follows:

**K8 ConfigMap:**
- NSX-T Manager IP Address
- NSS-T Username
- Tier-0 Router ID
- Update interval
- App Brand (e.g. ACME)

**K8s Secret:**
- NSX-T Manager Password
- App Password
 
**Deployment:**
- Create first a k8s namespace: ```kubectl create namespace nsxtpanel-app```
- Apply the k8s config: ```kubectl apply -f nsxt-panel-app.yaml -n nsxtpanel-app```
 
**Troubleshooting:**

I added some tools to help here like net-tools (e.g. traceroute) and cURL. There is also a shell script called tshoot that you can run after you connect to the container as follows:

```kubectl exec -it <pod-name> -n nsxtpanel-app sh```

then:

```sh tshoot.sh```
 
