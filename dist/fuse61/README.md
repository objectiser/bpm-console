# Fuse 6.x Distribution of the BPEL Console UI


## How To Install in Fuse 6.1

First you obviously have to build and install all of gwt-console using maven.  Once
that is done you can simply fire up Fuse and run the following commands:

    features:addurl mvn:org.jboss.bpm/gwt-console-dist-fuse61/${project.version}/xml/features
    features:install -v bpm-console

Note: Make sure you replace ${project.version} above with the appropriate version string.


## Configuration

Currently you will also need a bpel-console.properties configuration file located in 
fuse/etc with properties set something like this:

    bpel-console.rest-proxy.proxy-url=http://localhost:8181/bpel-console-server/rs/
    bpel-console.rest-proxy.authentication.provider=org.jboss.bpm.console.server.RestProxySAMLBearerTokenAuthProvider
    bpel-console.rest-proxy.authentication.saml.issuer=/bpel-console
    bpel-console.rest-proxy.authentication.saml.service=/bpel-console-server
    bpel-console.rest-proxy.authentication.saml.sign-assertions=true
    bpel-console.rest-proxy.authentication.saml.keystore=${karaf.home}/etc/overlord-saml.keystore
    bpel-console.rest-proxy.authentication.saml.keystore-password=samlkeystore77
    bpel-console.rest-proxy.authentication.saml.key-alias=overlord
    bpel-console.rest-proxy.authentication.saml.key-password=overlord99
