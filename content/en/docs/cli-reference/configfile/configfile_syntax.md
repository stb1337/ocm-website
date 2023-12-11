---
title: Config File Syntax
name: Config File Syntax
url: /docs/cli/configfile/sytnax/
date: 2023-12-11T10:43:19Z
draft: false
images: []
menu:
  docs:
    parent: configfile
toc: true
isCommand: false
---

### Configuration File Syntax

The command line client supports configuration using a configuration file.
If existent, by default, the file <code>$HOME/.ocmconfig</code> will be read.
Using the option <code>--config</code> an alternative file can be specified.

The file format is yaml. It uses the same type mechanism used for all
kinds of typed specification in the ocm area. The file must have the type of
a configuration specification. Instead, the command line client supports
a generic configuration specification able to host a list of arbitrary configuration
specifications. The type for this spec is <code>generic.config.ocm.software/v1</code>.

The following configuration types are supported:

- <code>attributes.config.ocm.software</code>
  The config type <code>attributes.config.ocm.software</code> can be used to define a list
  of arbitrary attribute specifications:

  <pre>
      type: attributes.config.ocm.software
      attributes:
         &lt;name>: &lt;yaml defining the attribute>
         ...
  </pre>
- <code>credentials.config.ocm.software</code>
  The config type <code>credentials.config.ocm.software</code> can be used to define a list
  of arbitrary configuration specifications:

  <pre>
      type: credentials.config.ocm.software
      consumers:
        - identity:
            &lt;name>: &lt;value>
            ...
          credentials:
            - &lt;credential specification>
            ... credential chain
      repositories:
         - repository: &lt;repository specification>
           credentials:
            - &lt;credential specification>
            ... credential chain
      aliases:
         &lt;name>:
           repository: &lt;repository specification>
           credentials:
            - &lt;credential specification>
            ... credential chain
  </pre>
- <code>downloader.ocm.config.ocm.software</code>
  The config type <code>downloader.ocm.config.ocm.software</code> can be used to define a list
  of pre-configured download handler registrations (see [ocm ocm-downloadhandlers](ocm_ocm-downloadhandlers.md)):

  <pre>
      type: downloader.ocm.config.ocm.software
      descrition: "my standard download handler configuration"
      handlers:
        - name: oci/artifact
          artifactType: ociImage
          mimeType:
          config: ...
        ...
  </pre>
- <code>generic.config.ocm.software</code>
  The config type <code>generic.config.ocm.software</code> can be used to define a list
  of arbitrary configuration specifications and named configuration sets:

  <pre>
      type: generic.config.ocm.software
      configurations:
        - type: &lt;any config type>
          ...
        ...
      sets:
         standard:
            description: my selectable standard config
            configurations:
              - type: ...
                ...
              ...
  </pre>

  Configurations are directly applied. Configuration sets are
  just stored in the configuration context and can be applied
  on-demand. On the CLI, this can be done using the main command option
  <code>--config-set &lt;name></code>.
- <code>hasher.config.ocm.software</code>
  The config type <code>hasher.config.ocm.software</code> can be used to define
  the default hash algorithm used to calculate digests for resources.
  It supports the field <code>hashAlgorithm</code>, with one of the following
  values:
    - <code>NO-DIGEST</code>
    - <code>SHA-256</code> (default)
    - <code>SHA-512</code>
- <code>keys.config.ocm.software</code>
  The config type <code>keys.config.ocm.software</code> can be used to define
  public and private keys. A key value might be given by one of the fields:
  - <code>path</code>: path of file with key data
  - <code>data</code>: base64 encoded binary data
  - <code>stringdata</code>: data a string parsed by key handler

  <pre>
      type: keys.config.ocm.software
      privateKeys:
         &lt;name>:
           path: &lt;file path>
         ...
      publicKeys:
         &lt;name>:
           data: &lt;base64 encoded key representation>
         ...
  </pre>
- <code>logging.config.ocm.software</code>
  The config type <code>logging.config.ocm.software</code> can be used to configure the logging
  aspect of a dedicated context type:

  <pre>
      type: logging.config.ocm.software
      contextType: attributes.context.ocm.software
      settings:
        defaultLevel: Info
        rules:
          - ...
  </pre>

  The context type attributes.context.ocm.software is the root context of a
  context hierarchy.

  If no context type is specified, the config will be applies to any target
  acting as logging context provider, which is not a non-root context.
- <code>memory.credentials.config.ocm.software</code>
  The config type <code>memory.credentials.config.ocm.software</code> can be used to define a list
  of arbitrary credentials stored in a memory based credentials repository:

  <pre>
      type: memory.credentials.config.ocm.software
      repoName: default
      credentials:
        - credentialsName: ref
          reference:  # refer to a credential set stored in some other credential repository
            type: Credentials # this is a repo providing just one explicit credential set
            properties:
              username: mandelsoft
              password: specialsecret
        - credentialsName: direct
          credentials: # direct credential specification
              username: mandelsoft2
              password: specialsecret2
  </pre>
- <code>merge.config.ocm.software</code>
  The config type <code>merge.config.ocm.software</code> can be used to set some
  assignments for the merging of (label) values. It applies to a value
  merge handler registry, either directly or via an OCM context.

  <pre>
      type: merge.config.ocm.software
      labels:
      - name: acme.org/audit/level
        merge:
          algorithm: acme.org/audit
          config: ...
      assignments:
         label:acme.org/audit/level@v1:
            algorithm: acme.org/audit
            config: ...
            ...
  </pre>
- <code>oci.config.ocm.software</code>
  The config type <code>oci.config.ocm.software</code> can be used to define
  OCI registry aliases:

  <pre>
      type: oci.config.ocm.software
      aliases:
         &lt;name>: &lt;OCI registry specification>
         ...
  </pre>
- <code>ocm.cmd.config.ocm.software</code>
  The config type <code>ocm.cmd.config.ocm.software</code> can be used to
  configure predefined aliases for dedicated OCM repositories and
  OCI registries.

  <pre>
     type: ocm.cmd.config.ocm.software
     ocmRepositories:
         &lt;name>: &lt;specification of OCM repository>
     ...
     ociRepositories:
         &lt;name>: &lt;specification of OCI registry>
     ...
  </pre>
- <code>ocm.config.ocm.software</code>
  The config type <code>ocm.config.ocm.software</code> can be used to set some
  configurations for an OCM context;

  <pre>
      type: ocm.config.ocm.software
      aliases:
         myrepo:
            type: &lt;any repository type>
            &lt;specification attributes>
            ...
      resolvers:
        - repository:
            type: &lt;any repository type>
            &lt;specification attributes>
            ...
          prefix: ghcr.io/open-component-model/ocm
          priority: 10
  </pre>

  With aliases repository alias names can be mapped to a repository specification.
  The alias name can be used in a string notation for an OCM repository.

  Resolvers define a list of OCM repository specifications to be used to resolve
  dedicated component versions. These settings are used to compose a standard
  component version resolver provided for an OCM context. Optionally, a component
  name prefix can be given. It limits the usage of the repository to resolve only
  components with the given name prefix (always complete name segments).
  An optional priority can be used to influence the lookup order. Larger value
  means higher priority (default 10).

  All matching entries are tried to lookup a component version in the following
  order:
  - highest priority first
  - longest matching sequence of component name segments first.

  If resolvers are defined, it is possible to use component version names on the
  command line without a repository. The names are resolved with the specified
  resolution rule.
  They are also used as default lookup repositories to lookup component references
  for recursive operations on component versions (<code>--lookup</code> option).
- <code>plugin.config.ocm.software</code>
  The config type <code>plugin.config.ocm.software</code> can be used to configure a
  plugin.

  <pre>
      type: plugin.config.ocm.software
      plugin: &lt;plugin name>
      config: &lt;arbitrary configuration structure>
      disableAutoRegistration: &lt;boolean flag to disable auto registration for handlers>
  </pre>
- <code>scripts.ocm.config.ocm.software</code>
  The config type <code>scripts.ocm.config.ocm.software</code> can be used to define transfer scripts:

  <pre>
      type: scripts.ocm.config.ocm.software
      scripts:
        &lt;name>:
          path: &lt;>file path>
        &lt;other name>:
          script: &lt;>nested script as yaml>
  </pre>
- <code>transport.ocm.config.ocm.software</code>
  The config type <code>transport.ocm.config.ocm.software</code> can be used to define transfer scripts:

  <pre>
      type: transport.ocm.config.ocm.software
      recursive: true
      overwrite: true
      localResourcesByValue: false
      resourcesByValue: true
      sourcesByValue: false
      keepGlobalAccess: false
      stopOnExistingVersion: false
      omitAccessTypes:
      - s3
  </pre>
- <code>uploader.ocm.config.ocm.software</code>
  The config type <code>uploader.ocm.config.ocm.software</code> can be used to define a list
  of pre-configured download handler registrations (see [ocm ocm-downloadhandlers](ocm_ocm-downloadhandlers.md)):

  <pre>
      type: uploader.ocm.config.ocm.software
      descrition: "my standard download handler configuration"
      handlers:
        - name: oci/artifact
          artifactType: ociImage
          mimeType:
          config: ...
        ...
  </pre>


### Examples

Pointing to an existing Docker config json:

```yaml
type: generic.config.ocm.software/v1
configurations:
  - type: credentials.config.ocm.software
    repositories:
      - repository:
          type: DockerConfig/v1
          dockerConfigFile: "~/.docker/config.json"
          propagateConsumerIdentity: true
```

Pointing to an existing Docker config json and configure two additional consumers, one
for a Github repository and another for a Helm chart repository.
Caching for OCM component versions is switched on.
A key pair for signing / verifiying OCM component versions has been configured, too.

```yaml
type: generic.config.ocm.software/v1
configurations:
  - type: credentials.config.ocm.software
    consumers:
      - identity:
          type: HelmChartRepository
          hostname: my.repository.mycomp.com
          pathprefix: artifactory/myhelm-repo
          port: "443"
        credentials:
          - type: Credentials
            properties:
              username: myuser
              password: 8eYwL5Ru44L6ZySyLUcyP
      - identity:
          type: Github
          hostname: github.com
        credentials:
          - type: Credentials
            properties:
              token: ghp_QRP489abcd1234A9q3x17a8BlD42kabv65
    repositories:
      - repository:
          type: DockerConfig/v1
          dockerConfigFile: ~/.docker/config.json
          propagateConsumerIdentity: true
  - type: attributes.config.ocm.software
    attributes:
      cache: ~/.ocm/cache
  - type: keys.config.ocm.software
    privateKeys:
      sap.com:
        path: /Users/myuser/.ocm/keys/mycomp.com.key
    publicKeys:
      sap.com:
        path: /Users/myuser/.ocm/keys/mycomp.com.pub
```