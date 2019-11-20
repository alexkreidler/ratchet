# Ratchet

[![GoDoc](https://godoc.org/github.com/alexkreidler/ratchet?status.svg)](https://godoc.org/github.com/alexkreidler/ratchet)
 
### A Golang ETL/pipeline library

The Go programming language's simplicity, execution speed, and concurrency support make it a great choice for building data pipeline systems that can perform custom ETL (Extract, Transform, Load) tasks. Ratchet is a library that is written 100% in Go, and let's you easily build custom data pipelines by writing your own Go code.

Ratchet provides a set of built-in, useful data processors, while also providing
an interface to implement your own. Conceptually, data processors are organized
into stages, and those stages are run within a pipeline. It looks something like:

![Pipeline Drawing](http://assets1.dailyburn.com/random/ratchet-pipeline-concept.png)

Each data processor is receiving, processing, and then sending data to the next stage in the pipeline. All data processors are running in their own goroutine, so all processing is happening concurrently. Go channels are connecting each stage of processing, so the syntax for sending data will be intuitive for anyone familiar with Go. All data being sent and received is JSON, which provides for a nice balance of flexibility and consistency.

## Getting Started

- Get Ratchet:
    - `go get github.com/alexkreidler/ratchet`
    - or just include it in your `import`s, and Go Modules will do the rest  

While not necessary, it may be helpful to understand
some of the pipeline concepts used within Ratchet's internals: https://blog.golang.org/pipelines

## Why would I use this?

Ratchet could be used anytime you need to perform some type of custom ETL. At DailyBurn we use Ratchet mainly to handle extracting data from our application databases, transforming it into reporting-oriented formats, and then loading it into our dedicated reporting databases.

Another good use-case is when you have data stored in disparate locations that can't be easily tied together. For example, if you have some CSV data stored on S3, some related data in a SQL database, and want to combine them into a final CSV or SQL output.

In general, Ratchet tends to solve the type of data-related tasks that you end up writing a bunch of custom and difficult to maintain scripts to accomplish.

# Future work

In the future, we may change the underlying data model that is passed between processors. JSON has a lot of overhead, especially for a solely in memory data representation. 

We may use Apache Arrow instead, as it has great support for ETL-like functionality

Additionally, we may extend the library to work in a distributed manner by using great libraries like [raft](https://github.com/hashicorp/raft), and other libs for building clusters. We would then need a scheduling algorithm, but data passing between tasks would happen with Apache Arrow's IPC framework.