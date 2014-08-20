# Fuse 6.x Distribution of the BPEL Console UI


## How To Install in Fuse 6.1

First you obviously have to build and install all of gwt-console using maven.

After unpacking Fuse 6.1, you need to perform the following steps before launching the fuse console:

* Add the following element in the etc/jetty.xml file, under the top level <Configure> element:

```
    <Call name="addBean">
        <Arg>
            <New class="org.eclipse.jetty.plus.jaas.JAASLoginService">
                <Set name="name">Overlord</Set>
                <Set name="loginModuleName">karaf</Set>
                <Set name="roleClassNames">
                    <Array type="java.lang.String">
                        <Item>org.apache.karaf.jaas.boot.principal.RolePrincipal</Item>
                    </Array>
                </Set>
            </New>
        </Arg>
    </Call>
```

* Add the following additional line at the end of the etc/org.ops4j.pax.url.mvn.cfg file:

```
org.ops4j.pax.url.mvn.repositories= \
        .... , \
        https://repository.jboss.org/nexus/content/groups/public@id=jboss.public
```

* Unpack the xmlseczip attachment, found on this forum post (https://community.jboss.org/wiki/S-RAMPDeploymentOnFuse61WithAndWithoutFabric), over the fuse installation

* Create a file called etc/bpel-console.properties with contents:

```
    bpel-console.rest-proxy.proxy-url=${overlord.baseUrl}/bpel-console-server/rs/
    bpel-console.rest-proxy.authentication.provider=org.jboss.bpm.console.server.RestProxySAMLBearerTokenAuthProvider
    bpel-console.rest-proxy.authentication.saml.issuer=/bpel-console
    bpel-console.rest-proxy.authentication.saml.service=/bpel-console-server
    bpel-console.rest-proxy.authentication.saml.sign-assertions=true
    bpel-console.rest-proxy.authentication.saml.keystore=${overlord.auth.saml-keystore}
    bpel-console.rest-proxy.authentication.saml.keystore-password=${overlord.auth.saml-keystore-password}
    bpel-console.rest-proxy.authentication.saml.key-alias=${overlord.auth.saml-key-alias}
    bpel-console.rest-proxy.authentication.saml.key-password=${overlord.auth.saml-key-alias-password}
```

* Create a file called etc/overlord.properties with contents:

```
overlord.port=8181
overlord.baseUrl=http\://localhost\:${overlord.port}
```

Once that is done you can simply fire up Fuse and run the following commands:

* esb:create-admin-user

Enter the admin username and password.

Once completed, you will need to edit the etc/users.properties file to add ",overlorduser" to the end of the property line created for the new admin user.

* Install the BPEL console

```
    features:addurl mvn:org.jboss.bpm/gwt-console-dist-fuse61/${project.version}/xml/features
    features:install -v bpm-console
```

Note: Make sure you replace ${project.version} above with the appropriate version string.

* Create SAML Keystore

```
    overlord:generatesamlkeystore <password>
```

At this point, you will need to exit the console and restart fuse. Once restarted, navigate to the console at the URL: http://localhost:8181/bpel-console


NOTE: If switchyard has not been installed, then the console will log in, but will show an exception.

