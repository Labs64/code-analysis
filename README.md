<a href="https://netlicensing.io"><img src="https://netlicensing.io/img/netlicensing-stage-twitter.jpg" alt="Innovative License Management Solution"></a>

# [Labs64](https://www.labs64.com) Java Code Style

[![NetLicensing Client - CI](https://github.com/Labs64/code-analysis/actions/workflows/code-analysis-ci.yml/badge.svg)](https://github.com/Labs64/code-analysis/actions/workflows/code-analysis-ci.yml)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.labs64.commons/code-analysis/badge.svg?style=flat)](https://maven-badges.herokuapp.com/maven-central/com.labs64.commons/code-analysis)
[![Apache License 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/Labs64/code-analysis/blob/master/LICENSE)
[![Labs64 @ LinkedIn](https://img.shields.io/badge/NetLicensing-0077B5.svg?logo=LinkedIn)](https://www.linkedin.com/company/labs64-gmbh/)

## Quick Start

The recommended way to get started using [`code-analysis`](https://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.labs64.commons%22) in your project is with a dependency management system – the snippets below can be copied and pasted into your build configuration.

Maven:
```xml
  <!-- https://maven.apache.org/plugins/maven-checkstyle-plugin/usage.html -->
  <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-checkstyle-plugin</artifactId>
      <version>${maven-checkstyle-plugin.version}</version>
      <dependencies>
          <dependency>
              <groupId>com.labs64.commons</groupId>
              <artifactId>code-analysis</artifactId>
              <version>${labs64.code-analysis.version}</version>
          </dependency>
      </dependencies>
  </plugin>

  <!-- https://spotbugs.github.io -->
  <plugin>
      <groupId>com.github.spotbugs</groupId>
      <artifactId>spotbugs-maven-plugin</artifactId>
      <version>${spotbugs-maven-plugin.version}</version>
      <dependencies>
          <dependency>
              <groupId>com.labs64.commons</groupId>
              <artifactId>code-analysis</artifactId>
              <version>${labs64.code-analysis.version}</version>
          </dependency>
      </dependencies>
  </plugin>

  <!-- https://maven.apache.org/plugins/maven-pmd-plugin/usage.html -->
  <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-pmd-plugin</artifactId>
      <version>${maven-pmd-plugin.version}</version>
      <dependencies>
          <dependency>
              <groupId>com.labs64.commons</groupId>
              <artifactId>code-analysis</artifactId>
              <version>${labs64.code-analysis.version}</version>
          </dependency>
      </dependencies>
  </plugin>
```
