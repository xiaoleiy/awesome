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
Generally the size of a single CSV file would exceed 1GB, with millions of rows included.

I came across this library when searching for a better way to solve <your problem here> and it's been great

![The uniVocity-parsers library](img/uniVocity-logo.png "uniVocity-parsers library")

The project uniVocity-parsers was started by [uniVocity Software](http://www.univocity.com/) during the
development of uniVocity, a commercial data integration API for Java. It focuses on
flexibility, performance, and reliability for processing tabular representation of data.
[Commercial support](http://www.univocity.com/products/parsers-support) is also provided for building new parsers.

The project is hosted at [Github](https://github.com/uniVocity/univocity-parsers) with 60 stars & 7 forks.
Tremendous document and tutorial are provided at [here](http://www.univocity.com/pages/parsers-tutorial)
and [here](http://www.univocity.com/pages/parsers-features).
You can find more examples and news [here](http://www.univocity.com/blogs/news) also.

__The well-known open-source project Apache Camel integrates uniVocity-parsers for reading and writing CSV/TSV/Fixed-width files.
Find more details [here](http://camel.apache.org/univocity-parsers-formats.html).__

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
uniVocity-parsers provides a list of powerful features, which can fulfill virtually all requirements you might have for processing tabular presentations of data. Check the following overview chart for the features:

![Features of uniVocity-parsers](img/univocity-features.png "features of uniVocity-parsers")

### 4. Features in Reading Tabular Presentations Data

### 5. Features in Writing Tabular Presentations Data

### 6. Performance and Flexibility

Here are some [performance comparison tables  for almost all CSV parsers libraries in existence](https://github.com/uniVocity/csv-parsers-comparison#csv-parsers)
And you can find that uniVocity-parsers got significantly ahead of other libraries in performance.

uniVocity-parsers achieved its purpose in performance and flexibility with the following mechanisms:

* __Read input on separate thread (enable by invoking `CsvParserSettings.setReadInputOnSeparateThread()`)__

> When enabled, a reading thread (in `input.concurrent.ConcurrentCharInputReader`) will be started and load characters from the input,
> while the parser is processing its input buffer. This yields better performance, especially when reading from big input (greater than 100 mb)
>
> When disabled, the parsing process will briefly pause so the buffer can be replenished every time
> it is exhausted (in `DefaultCharInputReader` it is not as bad or slow as it sounds, and can even be (slightly) more efficient if your input is small)

* __Caching during reading and writing (set buffer size by invoking `CsvParserSettings.setInputBufferSize()`)__

> For concurrency reading with `input.concurrent.ConcurrentCharInputReader`, the size of "bucket" and quantity of "buckets"
> are specified to for the cache pool. The `ConcurrentCharInputReader` will load "buckets" of characters in a separate thread
> and provides them sequentially to the buffer defined by instance of  `CharBucket`.
>
> For sequential reading, input reader `DefaultCharInputReader` owns buffer with array of characters with initial size 1024 * 1024 bytes.

* __Concurrent row processor__

> As an entry to process rows of data, the `ConcurrentRowProcessor` implements `RowProcessor` to perform row processing tasks in parallel.
> It wraps another `RowProcessor` and collects rows read from the input. The actual row processing is performed in by wrapped RowProcessor in a separate thread.
>
> The row processing task is submitted to the thread pool implemented with `java.util.concurrent` package.
> The thread pool is initialized with `new Executors.FinalizableDelegatedExecutorService(new ThreadPoolExecutor(1, 1, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue()));`

* __Extend `RowProcessor` to read rows with your own business logic)__

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
    @Override
    public void rowProcessed(String[] row, ParsingContext context) {
        System.out.println("The row in line #" + context.currentLine() + ": ");
        StringBuffer stringBuffer = new StringBuffer();
        for (String col : row) {
            stringBuffer.append(col).append("\t");
        }
        System.out.println(stringBuffer);
    }

    /**
    * After all rows were processed, perform any cleanup you need
    **/
    @Override
    public void processEnded(ParsingContext context) {
        System.out.println("Finished processing rows of data.");
    }
});

CsvParser parser = new CsvParser(settings);
List<String[]> allRows = parser.parseAll(new FileReader("/examples/example.csv"));
```

* __Extend `RowProcessor` to write rows with your own business logic__

```java
// setup the settings for witting rows
StringWriter strWriter = new StringWriter();
CsvWriterSettings settings = new CsvWriterSettings();

CsvWriter writer = new CsvWriter(strWriter, settings);

// write the row headers
writer.writeHeaders("Year", "Model", "Description", "Price");

// write the column values in current row
Collection<Object[]> rows = new ArrayList<Object[]>();
rows.add(new String[]{"2015", "Inspiron 1420", "DELL laptop", "4000"});
rows.add(new String[]{"2014", "iPhone 5s", "The best phone ever", "4000"});

// write rows of data and close the I/O to finish
writer.commentRow("This is a comment");
writer.writeRowsAndClose(rows);
```

* __Extend `ColumnProcessor` to process columns with your own business logic__

### 7. Design and Implementations
* The bunch of processors in uniVocity-parsers are core modules, which are responsible for reading/writing data in
rows and columns, and execute data conversions.
Here is the diagram of processors:

![Reading and Writing processors](img/diagram-processors.png "Reading and Writing processors")

You can create your own processors easily by implementing the RowProcessor interface or extending the provided implementations.
