---
title: artifacts
name: get artifacts
url: /docs/cli/get/artifacts/
date: 2022-10-19T11:39:28+01:00
draft: false
images: []
menu:
  docs:
    parent: get
toc: true
isCommand: true
---
### Usage

```
ocm get artifacts [<options>] {<artifact-reference>}
```

### Options

```
  -a, --attached           show attached artifacts
  -h, --help               help for artifacts
  -o, --output string      output mode (JSON, json, tree, wide, yaml)
  -r, --recursive          follow index nesting
      --repo string        repository name or spec
  -s, --sort stringArray   sort fields
```

### Description


Get lists all artifact versions specified, if only a repository is specified
all tagged artifacts are listed.
	
If the repository/registry option is specified, the given names are interpreted
relative to the specified registry using the syntax

<center>
    <pre>&lt;OCI repository name>[:&lt;tag>][@&lt;digest>]</pre>
</center>

If no <code>--repo</code> option is specified the given names are interpreted 
as extended OCI artifact references.

<center>
    <pre>[&lt;repo type>::]&lt;host>[:&lt;port>]/&lt;OCI repository name>[:&lt;tag>][@&lt;digest>]</pre>
</center>

The <code>--repo</code> option takes a repository/OCI registry specification:

<center>
    <pre>[&lt;repo type>::]&lt;configured name>|&lt;file path>|&lt;spec json></pre>
</center>

For the *Common Transport Format* the types <code>directory</code>,
<code>tar</code> or <code>tgz</code> are possible.

Using the JSON variant any repository type supported by the 
linked library can be used:
- `ArtifactSet`
- `CommonTransportFormat`
- `DockerDaemon`
- `Empty`
- `OCIRegistry`
- `oci`
- `ociRegistry`

With the option <code>--recursive</code> the complete reference tree of a index is traversed.

With the option <code>--output</code> the output mode can be selected.
The following modes are supported:
 - JSON
 - json
 - tree
 - wide
 - yaml


### Examples

```

$ ocm get artifact ghcr.io/mandelsoft/kubelink
$ ocm get artifact --repo OCIRegistry:ghcr.io mandelsoft/kubelink

```

### See Also

* [ocm get](/docs/cli/get)	 &mdash; Get information about artifacts and components
