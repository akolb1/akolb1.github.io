---
layout: post
comments: true
title: Using gRPC with Java
---
My notes on using gRPC with Java.

## File Locations

Place your gRPC spec in _src/main/proto_ directory.

## POM file settings

Modify the _pom.xml_ file to have the following:

### Dependencies

      <dependencies>
        <dependency>
          <groupId>io.grpc</groupId>
          <artifactId>grpc-netty</artifactId>
          <version>1.10.0</version>
        </dependency>
        <dependency>
          <groupId>io.grpc</groupId>
          <artifactId>grpc-protobuf</artifactId>
        </dependency>
        <dependency>
          <groupId>io.grpc</groupId>
          <artifactId>grpc-stub</artifactId>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.google.protobuf/protobuf-java -->
        <dependency>
          <groupId>com.google.protobuf</groupId>
          <artifactId>protobuf-java</artifactId>
        </dependency>
        <dependency>
          <groupId>com.google.guava</groupId>
          <artifactId>guava</artifactId>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-log4j12 -->
        <dependency>
          <groupId>org.slf4j</groupId>
          <artifactId>slf4j-log4j12</artifactId>
        </dependency>
        <dependency>
          <groupId>org.apache.hive.hcatalog</groupId>
          <artifactId>hive-hcatalog-server-extensions</artifactId>
        </dependency>
      </dependencies>

### Plugin versions.

This changes all the time, here is the current set of versions:

      <properties>
        <protobuf.version>3.5.1</protobuf.version>
        <guava.version>19.0</guava.version>
        <maven.compiler.plugin.version>3.6.1</maven.compiler.plugin.version>
    
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <grpc.version>1.11.0</grpc.version>
        <os.plugin.version>1.5.0.Final</os.plugin.version>
        <protobuf.plugin.version>0.5.0</protobuf.plugin.version>
        <protoc.version>3.5.1</protoc.version>
        <hive.version>2.3.0</hive.version>
      </properties>

#### Dependency versions

      <dependencyManagement>
        <dependencies>
          <dependency>
            <groupId>io.grpc</groupId>
            <artifactId>grpc-netty</artifactId>
            <version>${grpc.version}</version>
          </dependency>
          <dependency>
            <groupId>io.grpc</groupId>
            <artifactId>grpc-protobuf</artifactId>
            <version>${grpc.version}</version>
          </dependency>
          <dependency>
            <groupId>io.grpc</groupId>
            <artifactId>grpc-stub</artifactId>
            <version>${grpc.version}</version>
          </dependency>
          <!-- https://mvnrepository.com/artifact/com.google.protobuf/protobuf-java -->
          <dependency>
            <groupId>com.google.protobuf</groupId>
            <artifactId>protobuf-java</artifactId>
            <version>${protobuf.version}</version>
          </dependency>
          <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>${guava.version}</version>
          </dependency>
          <dependency>
            <groupId>org.apache.hive.hcatalog</groupId>
            <artifactId>hive-hcatalog-server-extensions</artifactId>
            <version>${hive.version}</version>
          </dependency>
        </dependencies>
      </dependencyManagement>

### Build section:

      <build>
        <extensions>
          <extension>
            <groupId>kr.motd.maven</groupId>
            <artifactId>os-maven-plugin</artifactId>
            <version>${os.plugin.version}</version>
          </extension>
        </extensions>
        <plugins>
          <plugin>
            <groupId>org.xolstice.maven.plugins</groupId>
            <artifactId>protobuf-maven-plugin</artifactId>
            <version>${protobuf.plugin.version}</version>
            <configuration>
              <protocArtifact>com.google.protobuf:protoc:${protoc.version}:exe:${os.detected.classifier}</protocArtifact>
              <pluginId>grpc-java</pluginId>
              <pluginArtifact>io.grpc:protoc-gen-grpc-java:${grpc.version}:exe:${os.detected.classifier}</pluginArtifact>
            </configuration>
            <executions>
              <execution>
                <goals>
                  <goal>compile</goal>
                  <goal>compile-custom</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
        </plugins>
      </build>
