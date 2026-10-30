<!--
sword:
  schema: 1
  name: soft.acceleo
  family: ecore-esync
  kind: stub
  generation: stub
  shipped: false
  languages: [Markdown]
  build: [none]
  ng: []
  apps: []
-->

For the previous documentation of the project (v1), see [README.LEGACY.md](README.LEGACY.md).

# soft.acceleo

> Stub repository that holds no code: it backs a Maven registry on GitHub Packages for Eclipse Acceleo releases that are missing from the Eclipse Maven repository.

| | |
|---|---|
| **Family** | Ecore / esync |
| **Languages** | none (Markdown only) |
| **Build systems** | none (manual Maven deploy procedure described in `README.md`) |

**[Architecture](ARCHITECTURE.md)** · [Contributing](CONTRIBUTING.md) · [previous README (v1)](README.LEGACY.md) · [known issues and open questions](https://github.com/masagroup/wiki/wiki/known-issues#soft.acceleo) · [repository on GitHub](https://github.com/masagroup/soft.acceleo)

## Overview

The repository exists to back the Maven repository `https://maven.pkg.github.com/masagroup/soft.acceleo` (GitHub Packages). That registry stores releases of [Eclipse Acceleo](https://www.eclipse.org/acceleo/) (the MTL model-to-text template engine) that are not available in the Eclipse Maven repository (`https://repo.eclipse.org`). Releases are taken from the p2 update site `https://download.eclipse.org/acceleo/updates/releases/`, converted to Maven artifacts (groupId `org.eclipse.acceleo`) and deployed by hand by a maintainer. The artifacts live in the GitHub Packages registry, not in the working tree. License: Mozilla Public License 2.0 (`LICENSE`). Git history: 3 commits (`Initial commit`, `Add readme`, `Format readme`).

The diagram of this overview is in [Architecture](ARCHITECTURE.md#overview).

## Build

No `build.xml`, `Dockerfile`, `docker-compose.yaml`, `CMakeLists.txt`, `go.mod`, `Makefile` or `package.json`: nothing to build.

### Publication

Procedure from the previous README (`Readme.md` on `masa/main`, section "How to install new version"):

   ```xml
   <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 https://maven.apache.org/xsd/settings-1.0.0.xsd">
     <servers>
       <server>
         <id>github</id>
         <username>[GITHUB USERNAME]</username>
         <password>[GENERATED ACCESS TOKEN]</password>
       </server>
     </servers>
   </settings>
   ```

<details>
<summary>Details</summary>

1. Install Maven.
2. Create a GitHub personal access token with read/write packages rights and put it in `%User%/.m2/settings.xml` under a `<server><id>github</id>` entry (`<username>` is the GitHub user name, `<password>` the generated token):

3. Then either:
   - Windows only: download `p2mvn` (`https://github.com/KinoriTech/p2mvn`), run `p2maven.exe` with the p2 update site URL, `d` (deploy), the repository URL `https://maven.pkg.github.com/masagroup/soft.acceleo` and repository id `github`, then run `%User%/p2mvn.org.eclipse.acceleo/deploy.bat`.
   - All platforms: mirror the p2 metadata and artifact repositories with `eclipse -nosplash -verbose -application org.eclipse.equinox.p2.metadata.repository.mirrorApplication` and `...artifact.repository.mirrorApplication` (`-source <update site>`, `-destination file:/tmp/Acceleo-Mirror/`), then for each jar run:
     `mvn deploy:deploy-file -DgroupId=org.eclipse.acceleo -DartifactId=<id> -Dversion=<v> -Dpackaging=jar -Dfile=<id>_<v>.jar -DrepositoryId=github -Durl=https://maven.pkg.github.com/masagroup/soft.acceleo`

The example in the previous README uses Acceleo 3.7 (`.../releases/3.7/R202102190929/`).

</details>

## Interfaces and access methods

No executable is produced by this repository (library, contract, data or tooling): see the build and the dependencies.

## Documentation and guides

- [Architecture](ARCHITECTURE.md): position in the ecosystem, repository layout, internal structure, how it works (diagrams), dependencies.
- [Contributing](CONTRIBUTING.md): development environment, Git and Jira workflow, conventions, CI/CD.
- [Previous README (v1)](README.LEGACY.md): the documentation of the repository before the wiki rewrite.
- [Known issues and open questions](https://github.com/masagroup/wiki/wiki/known-issues#soft.acceleo): the known defects and open points of the repository, kept in the SWORD wiki.
- Transverse pages: [Build on Windows](https://github.com/masagroup/wiki/wiki/build-windows), [Build on Linux](https://github.com/masagroup/wiki/wiki/build-linux), [CI/CD pipelines](https://github.com/masagroup/wiki/wiki/cicd-pipelines), [Ecosystem today](https://github.com/masagroup/wiki/wiki/ecosystem-current), [Ecosystem next gen](https://github.com/masagroup/wiki/wiki/ecosystem-nextgen), [Glossary](https://github.com/masagroup/wiki/wiki/glossary).
- Repository on GitHub: [masagroup/soft.acceleo](https://github.com/masagroup/soft.acceleo).
- The same documentation in the SWORD wiki: [soft.acceleo](https://github.com/masagroup/wiki/wiki/soft.acceleo), [architecture](https://github.com/masagroup/wiki/wiki/soft.acceleo-architecture), [contributing](https://github.com/masagroup/wiki/wiki/soft.acceleo-contributing), [previous README](https://github.com/masagroup/wiki/wiki/soft.acceleo-legacy).
