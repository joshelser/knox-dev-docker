## Example scenario for Apache Knox and Apache Avatica

Apache Avatica, a part of Apache Calcite, is a service for exposing relational databases
over HTTP. Apache Knox provides authentication, auditing, authorizing and federation via
a centralized proxy service.

This project is a docker-compose system which launches LDAP-back authentication for
Knox in front of an instance of Avatica using HSQLDB.


## Prerequisites

The process of generating certificates for the Knox gateway presently cannot be automated
which requires that the user generate the certificate and the master secret when the default
certificate of "localhost" is insufficient.

```
$ cd knox-0.10.0
$ ./bin/knoxcli.sh create-master --force
$ ./bin/knoxcli.sh create-cert --hostname $(hostname -f)
```

## Executing the cluster

```
$ docker-compose up -d
```

## Connecting via sqlline

```
$ docker run -v .../truststores/gateway.jks:/truststores/gateway.jks -it joshelser/sqlline https://$(hostname -f):8443/gateway/sandbox/avatica /truststores/gateway.jks <your_keystore_password>
```
