## Data Scheduling

### Batch and Stream Processing
- batch
  - cheap
  - in groups
- streams
  - send individual records right away
- tools
  - Apache Airflow
  - Luigi

## Parallel Computing
- important for memory concerns and processing power
- splitting up subtasks and distributed over several computers

### Benefits
- extra processing power
- reduced memory footprint

### Risks
- moving data incurs a cost
- communication time
- might not be worth the disadvantage

## Cloud Computing
- rented
- don't need space
- less latency
- reliability by replication
- risks of sensitive data
- three big players
  - AWS
  - Azure
  - GCP
- File storage (respectively)
  - S3
  - Azure blob service
  - Google cloud storage
- Computation
  - EC2
  - Azure virtual machines
  - Google compute engine
- Databases
  - RDS
  - Azure SQL
  - Google cloud SQL
- multicloud
