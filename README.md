# Ruleset for versions-maven-plugin

Ref: https://www.mojohaus.org/versions-maven-plugin/version-rules.html

For all applications, we should periodically check for new versions of third party libraries.

This can be automated like this:

    mvn versions:property-updates-report
    mvn versions:update-properties

The first command creates an html report with new versions. The second command replaces the values of the version
properties inside pom.xml.

However, the versions plugin will find *all* new versions, including alphas, betas, milestones, candidate releases
and so on.

To avoid manual inspection and filtering every time a repository is updated, we can instead use a *ruleset* which
excludes versions based on regular expressions.

The ruleset is an xml file. It must be available via URL or classpath. It is placed in this repository, and so it
can be pointed to directly at its repository location (main branch) or it can be depended on as a normal dependency.

## Usage

### Direct url
In the top pom for an app, add a URL pointer to the ruleset like this:

    <plugin>
        <!-- Exclude non-release versions when running mvn versions:property-updates-report && mvn versions:update-properties -->
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>versions-maven-plugin</artifactId>
        <version>${version.versions-maven-plugin}</version>
        <configuration>
            <rulesUri>https://github.com/dsb-norge/maven-versions-dsb-ruleset/blob/main/ruleset.xml</rulesUri>
        </configuration>
        <dependencies>
            <dependency>
                <groupId>no.dsb</groupId>
                <artifactId>maven-versions-dsb-ruleset</artifactId>
                <version>1.0.0</version>
            </dependency>
        </dependencies>
    </plugin>

### Dependency
In the top pom for an app, add a classpath pointer to the ruleset like this:

    <plugin>
        <!-- Exclude non-release versions when running mvn versions:property-updates-report && mvn versions:update-properties -->
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>versions-maven-plugin</artifactId>
        <version>${version.versions-maven-plugin}</version>
        <configuration>
            <rulesUri>classpath:///maven-versions-rules.xml</rulesUri>
        </configuration>
        <dependencies>
            <dependency>
                <groupId>no.dsb</groupId>
                <artifactId>maven-versions-dsb-ruleset</artifactId>
                <version>1.0.0</version>
            </dependency>
        </dependencies>
    </plugin>
