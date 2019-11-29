### Search the grafana hem chart ###
helm inspect values stable/grafana >> /tmp/values.yaml


### Create the pv for grafana 
cat <<EOF > grafana-pv.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: grafana
spec:
  capacity:
    storage: 6Gi
  accessModes:
  - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: my-local-pv
  volumeMode: Filesystem
  hostPath:
    path: "/mnt/data5"
EOF

kubectl apply -f grafana-pv.yaml


### Modify the values.yml file by nodePort & Storage class name

- service: 
    type: NodePort
    nodePort: 30325

- persistent:
     enabled: true ( To keep data available if pod restarted)
     storageclassname: my-local-pv
- name: admin
  password: *******

######Deploy the grafana

helm install stable/grafana --values values.yaml --name grafana --namespace grafana

#####Expose the grafana
curl -k http://<<node-Ip>>:30325

####configure the data source with Prometheous
http://<<node-Ip>>:30325
 and Access to browser from default

###For dashboards import from grafanadashboard
For sample  use: 8588 number
