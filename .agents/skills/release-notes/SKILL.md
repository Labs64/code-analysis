---
name: release-notes
description: Draft the GitHub release page text for a code-analysis release (Markdown). Use when asked for release notes, a release page or a changelog for a new version of com.labs64.commons:code-analysis.
---

# Release notes for code-analysis

The release workflow (`code-analysis-release.yml`) only uploads to Maven Central and pushes the `X.Y.Z` tag. The GitHub release page is written by hand, so output the release title and the body text for the user to paste, the body as one Markdown block.

## Before writing

- Take `<VERSION>` from the tag or the user's request and `<PREVIOUS>` from the previous tag (`git tag --sort=-v:refname`).
- Read what changed since the previous tag (`git log <PREVIOUS>..<VERSION> --oneline`, and the diff of `src/main/resources`) and describe only that.
- Compare with the latest release page (`gh release view <PREVIOUS>`); if its structure has moved on from the template below, follow the release page.
- Check that the commands work against the artifact. For an unpublished version, test with the staging profile and server the user configured in their settings.xml. Never put that profile or `-s` in the release text.
- Use the current `maven-dependency-plugin` release (check Maven Central) in the Option 1 command, and the current `maven-checkstyle-plugin` and `spotless-maven-plugin` releases in Option 3.

## Release title

The title field of the release page is the bare version, `<VERSION>` (e.g. `3.1.0`), with no summary: the Releases list truncates long titles. Always give it next to the body. The change summary belongs in "What changed"; the body keeps its own `# code-analysis <VERSION>` heading.

## Template

Keep the section order, headings and wording; replace the version and the change list.

```markdown
# code-analysis <VERSION>

The artifact is published on Maven Central as [`com.labs64.commons:code-analysis:<VERSION>`](https://central.sonatype.com/artifact/com.labs64.commons/code-analysis/<VERSION>).

## What changed

- <one bullet per change since <PREVIOUS>>

**Full Changelog**: https://github.com/Labs64/code-analysis/compare/<PREVIOUS>...<VERSION>

## Option 1: unpack from the command line (no POM needed)

Unpacks every file in the jar into `./code-analysis`:

    mvn org.apache.maven.plugins:maven-dependency-plugin:3.11.0:unpack \
      -Dartifact=com.labs64.commons:code-analysis:<VERSION> \
      -DoutputDirectory=code-analysis

## Option 2: unpack as part of the build (POM)

    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
        <executions>
            <execution>
                <id>unpack-code-analysis</id>
                <phase>verify</phase>
                <goals>
                    <goal>unpack</goal>
                </goals>
                <configuration>
                    <artifactItems>
                        <artifactItem>
                            <groupId>com.labs64.commons</groupId>
                            <artifactId>code-analysis</artifactId>
                            <version><VERSION></version>
                            <outputDirectory>${project.build.directory}/code-analysis</outputDirectory>
                        </artifactItem>
                    </artifactItems>
                </configuration>
            </execution>
        </executions>
    </plugin>

The files are then available at `${project.build.directory}/code-analysis/labs64-code-formatting/` and `${project.build.directory}/code-analysis/labs64-code-analysis/`.

## Option 3: add as a dependency of the tool that uses the file (Checkstyle and Spotless examples)

The tools read the files from the plugin's classpath, so no unpacking is needed. Checkstyle with the analysis rules:

    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-checkstyle-plugin</artifactId>
        <version><CHECKSTYLE_PLUGIN_VERSION></version>
        <configuration>
            <configLocation>labs64-code-analysis/checkstyle.xml</configLocation>
        </configuration>
        <dependencies>
            <dependency>
                <groupId>com.labs64.commons</groupId>
                <artifactId>code-analysis</artifactId>
                <version><VERSION></version>
            </dependency>
        </dependencies>
    </plugin>

SpotBugs and PMD work the same way: `<includeFilterFile>labs64-code-analysis/spotbugs.xml</includeFilterFile>` for `spotbugs-maven-plugin` and `<ruleset>labs64-code-analysis/pmd.xml</ruleset>` for `maven-pmd-plugin`, each with the same dependency.

Spotless with the formatting files:

    <plugin>
        <groupId>com.diffplug.spotless</groupId>
        <artifactId>spotless-maven-plugin</artifactId>
        <version><SPOTLESS_PLUGIN_VERSION></version>
        <dependencies>
            <dependency>
                <groupId>com.labs64.commons</groupId>
                <artifactId>code-analysis</artifactId>
                <version><VERSION></version>
            </dependency>
        </dependencies>
        <configuration>
            <java>
                <importOrder>
                    <file>labs64-code-formatting/labs64.importorder</file>
                </importOrder>
                <eclipse>
                    <version>4.41</version>
                    <file>labs64-code-formatting/labs64_eclipse_formatter.xml</file>
                </eclipse>
            </java>
        </configuration>
    </plugin>
```

Notes on the template:

- The Maven Central sentence stays under the title, outside "What changed": earlier versions were already on Central.
- Option 3 shows the tools known to use the files (Checkstyle, SpotBugs, PMD, Spotless). If other consumers are identified, add them there.
- Code is indented by four spaces, not fenced, so it survives being pasted into the release form.
- The "Full Changelog" line is the compare link GitHub generates; keep it right after "What changed".

## Leave out

- Words like "migrate" or "migration"; just say how to get the files.
- Artifacts that were never public (the old `code-formatting` zip).
- Internal repositories, credentials, runner or CI details.
