# Data Commons Architecture

## Overview

This document will cover the overall architecture of the AgLaunch Data Commons.  Where possible 
we will follow the best practices for the given technologies/methodologies listed. The purpose of the
Data Commons is to provide a centralized storage and processing system for farmers, startups, and other
affiliates of AgLaunch.  

## Physical Architecture

The Data Commons leverages Amazon Web Services (AWS) as the cloud provider for our technology stack.  While we leverage open 
source technologies as much as possible, some technologies will be based on the provided capabilities
with AWS.

![](https://github.com/AgLaunch/data-commons-documentation/blob/master/diagrams/architecture/data_commons_data_movement_architecture_security.png)

### Data Ingestion

Data may be processed one of two ways, as noted in the above diagram.

#### Bulk Processing

For initial data loads that are larger than API processing can handle, we have a bulk loading process.
Once the data has been dumped into the Data Commons' landing S3 bucket, a lambda process registers the file(s)
with [Process Tracker](https://process-tracker.readthedocs.io/en/latest/).  Those files are then provided a
light data cleansing to ensure the data conforms to the data quality standards required of the Data Commons
and processed into the Data Commons' cleaned S3 bucket.  Once cleaned, the data is the processed into a raw data vault.

All data processing is currently managed with [Pentaho Data Integration](https://help.pentaho.com/Documentation/).

#### API Processing

A REST API built using the [Django REST Framework](https://www.django-rest-framework.org/) is available for 
stream processing and data retrieval.  This allows for near real time data processing from startups into the 
Data Commons and allows those same startups to retrieve data from the Data Commons.  Graphql was not utilized due
to a bug in query generation between [Graphene](https://graphene-python.org/) and [Apache Hive](https://hive.apache.org/).

The REST API provides data from the various layers (raw data vault, conformed dimensional model, etc.) as required
for data consumers/providers.  Data provided thru the REST API will be stored in the raw data vault, provided it
follows the data quality standards of the Data Commons.

### Data Storage

All data within the Data Commons is stored within AWS and follows best practices on data encryption (encryption at rest,
in flight, etc.).  Each data layer has specific technologies used and will be described there.

### Landing and Cleaned Data

Data that is processed in bulk will initially be stored in an S3 bucket.  As the data is processed for conforming to the 
basic data quality standards, it gets moved into a new S3 bucket.  Raw and cleansed data files will eventually be cleaned
out after processing.

### Raw Data Vault - Central Storage

The raw data vault is based on [data vault 2.0](https://datavaultalliance.com/about/what-is-datavault/).  The
raw data vault acts as the central storage mechanism  for the Data Commons.  The data is kept as close to 
the provided format as possible, but is stored in an integrated fashion.  For instance, all farm entities are
stored in a farm table set.  Any entities related to a farm entity will be associated to the given farm record.
If multiple data sets have the same farm entity there will be only the one farm entity record.  Data for the raw data
vault is stored in S3 and an Elastic Map Reduce cluster with Apache Hive is positioned to query the data for further
data integration and processing.

### Conformed Dimensional Model

For basic reporting and analysis, data from the raw data vault will be processed into a conformed dimensional model (aka
star schema).  The reason for this processing is due to the fact that most reporting tools know how to work with star 
schema and not necessarily other data architectures.  This format is also easier for users to utilize.  The star schema
will be built within a columnar data store (i.e. Redshift).

### Other data storage

As new use cases arise, we will process data from the raw data vault into other data formats.  For instance, for use
cases that require better understanding of relationships within data we could leverage graph data stores (i.e. Neo4j)
to assist in answering those types of questions.  The Data Commons is never intended to be limited in what downstream
technologies are leveraged.
