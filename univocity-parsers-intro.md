[uniVocity-parsers](http://www.univocity.com/pages/about-parsers): A powerful CSV/TSV/Fixed-width file parser library for Java
======

[uniVocity-parsers](http://www.univocity.com/pages/about-parsers) is an open-source project CSV/TSV/Fixed-width file parser library in Java, providing many capabilities to read/write files with simplified API, and powerful features as shown below.

Unlike other libraries out there, uniVocity-parsers built its own architecture for parsing text files, which
focuses on maximum performance and flexibility while making it easy to extend and build new parsers.

###Contents###

* [Overview](#1-overview)
* [Installation](#2-installation)
* [Features Overview](#3-features-overview)
* [Reading CSV/TSV/Fixed-width Files](#4-features-in-reading-tabular-presentations-data)
* [Writing CSV/TSV/Fixed-width Files](#5-features-in-writing-tabular-presentations-data)
* [Performance](#6-performance-and-flexibility)
* [Design and Implementations](#7-design-and-implementations)

### 1. Overview

I'm a Java developer working on a web-based system to evaluate telecommunication carriers' network and work out reports.
In the system, the CSV format was heavily involved for the network-related data,
such as real-time network status (online/offline) for the broadband subscribers, and real-time traffic for each subscriber.
Generally the size of a single CSV file would exceed 1GB, with millions of rows included. And we were using the library
[JavaCSV](http://sourceforge.net/projects/javacsv/) as the CSV file parser.

As growth in the capacity of carrier' network and the time duration our system monitors, the size of data in CSV increased so much.
My team and I have to work out a solution to achieve better performance (even in seconds) in CSV files processing,
and better extendability to provide much more customized functionality.

We came across this library [uniVocity-parsers](http://www.univocity.com/pages/about-parsers) as a final solution
after a lot of testing and analysis, and we found it great. In addition of better performance and extendability, the library
provides developers with simplified APIs, detailed documents & tutorials and commercial support for highly customized functionality.

This project is hosted at [Github](https://github.com/uniVocity/univocity-parsers) with 62 stars & 8 forks (at the time of writing).
Tremendous documents & tutorials are provided at [here](http://www.univocity.com/pages/parsers-tutorial)
and [here](http://www.univocity.com/pages/parsers-features).
You can find more examples and news [here](http://www.univocity.com/blogs/news) as well.

In addition, the well-known open-source project Apache Camel integrates uniVocity-parsers for reading and writing CSV/TSV/Fixed-width files.
Find more details [here](http://camel.apache.org/univocity-parsers-formats.html).

### 2. Installation
Install the parser library in the following 2 ways as you prefer:
* Directly download the jar [here](http://oss.sonatype.org/content/repositories/releases/com/univocity/univocity-parsers/1.5.1/univocity-parsers-1.5.1.jar).
* Add the following to your `pom.xml` if you are using Maven:

```xml
<dependency>
    <groupId>com.univocity</groupId>
    <artifactId>univocity-parsers</artifactId>
    <version>1.5.1</version>
    <type>jar</type>
</dependency>
```

### 3. Features Overview
uniVocity-parsers provides a list of powerful features, which can fulfill all requirements you might have for processing tabular presentations of data.
Check the following overview chart for the features:

![Features of uniVocity-parsers](img/univocity-features.png "features of uniVocity-parsers")

### 4. Reading Tabular Presentations Data

__Read all rows of a csv__

```java
// creates a CSV parser
CsvParser parser = new CsvParser(new CsvParserSettings());
List<String[]> allRows = parser.parseAll(getReader("/examples/example.csv"));
```

For full list of demos in reading features, refer to: [https://github.com/uniVocity/univocity-parsers#reading-csv](https://github.com/uniVocity/univocity-parsers#reading-csv)

### 5. Writing Tabular Presentations Data

__Write data in CSV format with just 3 lines of code:__

```java
CsvWriter writer = new CsvWriter(outputWriter, new CsvWriterSettings());
writer.writeHeaders("Year", "Make", "Model", "Description", "Price");
List<String[]> rows = someMethodToCreateRows();
writer.writeRowsAndClose(rows);
```

For full list of demos in writing features, refer to: [https://github.com/uniVocity/univocity-parsers/blob/master/README.md#writing](https://github.com/uniVocity/univocity-parsers/blob/master/README.md#writing)

### 6. Performance and Flexibility

Here is the performance comparison we tested for uniVocity-parsers and JavaCSV in our system:

| file size         | Duration for JavaCSV parsing | Duration for uniVocity-parsers parsing |
|-----|-----:|-----:|
|10MB, 145453 rows  |1138ms |836ms|
|100MB, 809008 rows |23s    |6s|
|434MB, 4499959 rows|91s    |28s|
|1GB, 23803502 rows |245s   |70s|

Here are some [performance comparison tables for almost all CSV parsers libraries in existence](https://github.com/uniVocity/csv-parsers-comparison#csv-parsers).
And you can find that uniVocity-parsers got significantly ahead of other libraries in performance.

uniVocity-parsers achieved its purpose in performance and flexibility with the following mechanisms:

* __Read input on separate thread (enable by invoking `CsvParserSettings.setReadInputOnSeparateThread()`)__
* __Caching during reading and writing (set buffer size by invoking `CsvParserSettings.setInputBufferSize()`)__
* __Concurrent row processor with package java.util.concurrent (refer to `ConcurrentRowProcessor` which implements `RowProcessor`)__
* __Extend `ColumnProcessor` to process columns with your own business logic__
* __Extend `RowProcessor` to read rows with your own business logic__

### 7. Design and Implementations
A bunch of processors in uniVocity-parsers are core modules, which are responsible for reading/writing data in
rows and columns, and execute data conversions.
Here is the diagram of processors:

![Reading and Writing processors](img/diagram-processors.png "Reading and Writing processors")

You can create your own processors easily by implementing the RowProcessor interface or extending the provided implementations. In the following example I simply used an anonymous class:

```java
CsvParserSettings settings = new CsvParserSettings();

settings.setRowProcessor(new RowProcessor() {

    /**
    * initialize whatever you need before processing the first row, with your own business logic
    **/
    @Override
    public void processStarted(ParsingContext context) {
        System.out.println("Started to process rows of data.");
    }

    /**
    * process the row with your own business logic
    **/
    StringBuilder stringBuilder = new StringBuilder();
    
    @Override
    public void rowProcessed(String[] row, ParsingContext context) {
        System.out.println("The row in line #" + context.currentLine() + ": ");
        for (String col : row) {
            stringBuilder.append(col).append("\t");
        }
    }

    /**
    * After all rows were processed, perform any cleanup you need
    **/
    @Override
    public void processEnded(ParsingContext context) {
        System.out.println("Finished processing rows of data.");
        System.out.println(stringBuilder);
    }
});

CsvParser parser = new CsvParser(settings);
List<String[]> allRows = parser.parseAll(new FileReader("/myFile.csv"));
```

The library offers a whole lot more of features. I recommend you to have a look. It really made a difference in our project.
