Traffic Control (Linux)
=======================




In the default configuration Traffic Control (tc) consist in a single queue `qdisk`. The default qdisk is `pfifo_fast`.


Vocabulary:

- shaping: `class`
- Scheduling: `qdisc`
- Classifying: `filter`
- policing: `policer`
- dropping: `drop`
- marking: `dsmark` in `qdisc`


- outbound traffic: `egress` or `root`
- inbound traffic: `ingress`
