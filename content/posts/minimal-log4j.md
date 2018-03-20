---
title: "Minimal logging configuration with Java, SLF4J & Gradle"
date: 2018-03-09T13:46:15+01:00
draft: false
tags: [ "java" ]
---
This is mainly here for my copying and pasting for a new Java project.

The gradle deps bring the [SLF4J](https://www.slf4j.org/) API in to the compile time classpath and the LOG4J implementation in to the runtime.  If you wanted to compile the log4j implementation in to your code base (e.g. building an application and not a library), you would need to move it into the compile list.
<!--more-->

## buid.gradle

    dependencies {
        compile (
            "org.slf4j:slf4j-api:1.7+"
        )
        testCompile (
            "junit:junit:4+"
        )
        runtime(
            "org.slf4j:slf4j-log4j12:1.7+"
        )
    }


## log4j.properties

    log4j.rootLogger = info, CONSOLE
    log4j.appender.CONSOLE = org.apache.log4j.ConsoleAppender
    log4j.appender.CONSOLE.layout = org.apache.log4j.PatternLayout
    log4j.appender.CONSOLE.layout.conversionPattern = [%-5p] %m%n

