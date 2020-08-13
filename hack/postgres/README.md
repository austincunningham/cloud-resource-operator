# Postgres 

Scripts to load postgres (AWS) with data to easily allow and test alerting

## Usage 
### Prerequisites
```
export ns=cloud-resources-load-test
# Spin up workshop `postgres` instance
make cluster/prepare NAMESPACE=$ns
make cluster/seed/workshop/postgres NAMESPACE=$ns
make run NAMESPACE=$ns


# In another terminal window run
export ns=cloud-resources-load-test
export aws_rds_db_host=<your-db-host.rds.amazonaws.com>
export aws_rds_db_password=<password>
# Size to fill in GiB
export load_data_size=<number>

# Get postgres workshop pod name
export pod_name=$(oc get pods -n $ns -o jsonpath='{.items[0].metadata.name}')
```
### Load Script

Copy `load.sh` file to provisioned postgres workshop pod  
```
oc cp load.sh $ns/$pod_name:/var/lib/pgsql
```
Run command
``` 
oc exec $pod_name sh /var/lib/pgsql/load.sh $aws_rds_db_host $aws_rds_db_password $load_data_size -n $ns
```

### Clean Script
Copy `clean.sh` file to provisioned postgres workshop pod  
```
 oc cp clean.sh $ns/$pod_name:/var/lib/pgsql
```
Run command
``` 
oc exec $pod_name sh /var/lib/pgsql/clean.sh $aws_rds_db_host $aws_rds_db_password -n $ns
```