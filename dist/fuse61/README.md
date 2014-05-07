# Fuse 6.x Distribution of the BPEL Console UI

## How To Install in Fuse 6.1

First you obviously have to build and install all of gwt-console using maven.  Once
that is done you can simply fire up Fuse and run the following commands:

    features:addurl mvn:org.jboss.bpm/gwt-console-dist-fuse61/${project.version}/xml/features
    features:install bpm-console

## Configuration

Currently you will also need a bpel-console.properties configuration file located in 
fuse/etc:

    bpel-console.rest-proxy.proxy-url=SCHEME://HOST:PORT/bpel-console-server/rs
    bpel-console.rest-proxy.authentication.provider=org.jboss.bpm.console.server.RestProxyBasicAuthProvider
    gwt-console.rest-proxy.authentication.basic.username=admin
    gwt-console.rest-proxy.authentication.basic.password=admin
