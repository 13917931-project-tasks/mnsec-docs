# Activation

Using Kytos-ng SDN controller:

1. Activate Kytos;

```
source test_env/bin/activate
cd teste
cd kytos
sudo ./docker/scripts/add-etc-hosts.sh 
export MONGO_USERNAME=mymongouser
export MONGO_PASSWORD=mymongopass
docker compose up -d
docker ps 
kytosd -f --database mongodb
```
2. Iniciate mnsec;

```
cd mininet-sec
cd examples
python3 firewall.py
```

3. Activation of NOS;

```
for sw in $(curl -s http://127.0.0.1:8181/api/kytos/topology/v3/switches | jq -r '.switches[].id'); do curl -H 'Content-type: application/json' -X POST http://127.0.0.1:8181/api/kytos/topology/v3/switches/$sw/enable; curl -H 'Content-type: application/json' -X POST http://127.0.0.1:8181/api/kytos/topology/v3/interfaces/switch/$sw/enable; done

for l in $(curl -s http://127.0.0.1:8181/api/kytos/topology/v3/links | jq -r '.links[].id'); do curl -H 'Content-type: application/json' -X POST http://127.0.0.1:8181/api/kytos/topology/v3/links/$l/enable; done

curl -H 'Content-type: application/json' -X POST http://127.0.0.1:8181/api/kytos/mef_eline/v2/evc/ -d '{"name": "my evc1", "dynamic_backup_path": true, "enabled": true, "uni_a": {"interface_id": "00:00:00:00:00:00:00:01:1"}, "uni_z": {"interface_id": "00:00:00:00:00:00:00:01:2"}}'
```

