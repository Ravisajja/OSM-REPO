# K8s-At-Home PowerDNS NS

Descriptors that installs a PowerDNS chart from K8s-At-Home repo.

There is one VNF (powerdns\_vnf) with only one KDU.

There is one NS that connects the VNF to a mgmt network

## Onboarding and instantiation

```bash
osm nfpkg-create powerdns_knf.tar.gz
osm nspkg-create powerdns_ns.tar.gz
osm ns-create --ns_name powerdns --nsd_name powerdns_ns --vim_account <VIM_ACCOUNT_NAME>|<VIM_ACCOUNT_ID> --ssh_keys ${HOME}/.ssh/id_rsa.pub
```

### Instantiation option

Some parameters could be passed during the instantiation.

```bash
osm ns-create --ns_name powerdns --nsd_name powerdns_ns --vim_account <VIM_ACCOUNT_NAME>|<VIM_ACCOUNT_ID> --config '{additionalParamsForVnf: [{"member-vnf-index": "openldap", "additionalParams": {"replicaCount": "2"}}]}'
osm ns-create --ns_name powerdns --nsd_name powerdns_ns --vim_account <VIM_ACCOUNT_NAME>|<VIM_ACCOUNT_ID> --config_file params.yaml
```

## Test deployment

```bash
helm -n <PROJECT_ID> list
kubectl -n <PROJECT_ID> get all
kubectl -n <PROJECT_ID> get service
```

## Testing PowerDNS server

```bash
PROJECT_ID="$(osm project-list | grep "_43" | awk '{ print $4 }')"
UDP_NODE_PORT="$(k -n "${PROJECT_ID}" get svc | grep udp | awk '{ print $5 }' | sed 's#53\:\([0-9]*\)/UDP#\1#g')"
dig @<NODE_IP> -p "${UDP_NODE_PORT}" google.com
```
