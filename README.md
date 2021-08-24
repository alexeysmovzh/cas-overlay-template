## Test build of CAS Server with github actions

1. Fork CAS overlay repository
   https://github.com/apereo/cas-overlay-template

2. Add dependencies if needed in to [build.gradle](build.gradle)
```
  dependencies { 
    ...
    # LDAP support
    classpath "org.apereo.cas:cas-server-support-ldap:${project.'cas.version'}" 
    ...
  }
```

3. Set CAS version to build in [gradle.properties](gradle.properties)
```
  # The version of this overlay project
  version=6.4.0

  #CAS server version
  cas.version=6.4.0
```

4. Create Git CI file [.github/workflows/build_cas.yml](.github/workflows/build_cas.yml)

5. Push changes into repository
```
  git remote set-url origin git@github.com:alexeysmovzh/cas-overlay-template.git
  git commit -am 'new CAS configuration'
  git push
```

6. When Github actions completes build download and unzip artifact


## CAS configuration
1. Copy and edit configuration files from 
 [etc/cas/config](etc/cas/config) to system /etc folder

2. Create keystore from existing certificates and place it in /etc/cas folder
```
  # create PCKS12
  sudo openssl pkcs12 -export -out cas.p12 -inkey ca-key.pem -in ca.pem -certfile ca-root.pem -password pass:secret
  # create JKS Keystore
  sudo keytool -importkeystore -srckeystore cas.p12 -storetype pkcs12 -destkeystore cas.jks -deststoretype jks -storepass secret -srcstorepass secret
```

3. Run CAS
```
  java -Xms256m -Xmx512m -jar cas.war 
```
