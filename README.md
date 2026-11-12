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

soft.acceleo holds no code. It backs the Maven registry `https://maven.pkg.github.com/masagroup/soft.acceleo` (GitHub Packages), which stores the releases of [Eclipse Acceleo](https://www.eclipse.org/acceleo/) (groupId `org.eclipse.acceleo`) that are missing from the Eclipse Maven repository. A maintainer converts a release of the p2 update site `https://download.eclipse.org/acceleo/updates/releases/` into Maven artifacts and deploys it by hand.

## Where to start

| You want to... | Read |
|---|---|
| publish a new Acceleo release | [Build](#build) below |
| understand how it is made | [ARCHITECTURE.md](ARCHITECTURE.md) |
| contribute | [CONTRIBUTING.md](CONTRIBUTING.md) |

## Build

Nothing to build: the repository has no build file. The only task is to publish a new Acceleo release to the registry. UNVERIFIED: the procedure below comes from the previous README and was not run again.

### Publish a new Acceleo release

Install Maven, create a GitHub personal access token with read and write packages rights, and declare it in `%USERPROFILE%\.m2\settings.xml` (`~/.m2/settings.xml` on Linux):

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

Then use one of the two methods below, with the p2 update site of the release as source (for example `https://download.eclipse.org/acceleo/updates/releases/3.7/R202102190929/`).

On Windows, with [p2mvn](https://github.com/KinoriTech/p2mvn): run `p2maven.exe` and give it the update site URL, `d` (deploy), the repository URL `https://maven.pkg.github.com/masagroup/soft.acceleo` and the repository id `github`, then run the generated script.

```
%USERPROFILE%\p2mvn.org.eclipse.acceleo\deploy.bat
```

On every platform, mirror the p2 repositories with Eclipse, then deploy each jar with Maven:

```
eclipse -nosplash -verbose -application org.eclipse.equinox.p2.metadata.repository.mirrorApplication -source <update site> -destination file:/tmp/Acceleo-Mirror/
eclipse -nosplash -verbose -application org.eclipse.equinox.p2.artifact.repository.mirrorApplication -source <update site> -destination file:/tmp/Acceleo-Mirror/
```

```
mvn deploy:deploy-file -DgroupId=org.eclipse.acceleo -DartifactId=<id> -Dversion=<version> -Dpackaging=jar -Dfile=<id>_<version>.jar -DrepositoryId=github -Durl=https://maven.pkg.github.com/masagroup/soft.acceleo
```

## Documentation and guides

- [Architecture](ARCHITECTURE.md): position in the ecosystem, repository layout, internal structure, how it works (diagrams), dependencies.
- [Contributing](CONTRIBUTING.md): development environment, Git and Jira workflow, conventions, CI/CD.
- [Previous README (v1)](README.LEGACY.md): the documentation of the repository before the wiki rewrite.
- [Known issues and open questions](https://github.com/masagroup/wiki/wiki/known-issues#soft.acceleo): the known defects and open points of the repository, kept in the SWORD wiki.
- Transverse pages: [CI/CD pipelines](https://github.com/masagroup/wiki/wiki/cicd-pipelines), [Ecosystem today](https://github.com/masagroup/wiki/wiki/ecosystem-current), [Ecosystem next gen](https://github.com/masagroup/wiki/wiki/ecosystem-nextgen), [Glossary](https://github.com/masagroup/wiki/wiki/glossary).
- Repository on GitHub: [masagroup/soft.acceleo](https://github.com/masagroup/soft.acceleo).
- The same documentation in the SWORD wiki: [soft.acceleo](https://github.com/masagroup/wiki/wiki/soft.acceleo), [architecture](https://github.com/masagroup/wiki/wiki/soft.acceleo-architecture), [contributing](https://github.com/masagroup/wiki/wiki/soft.acceleo-contributing), [previous README](https://github.com/masagroup/wiki/wiki/soft.acceleo-legacy).
