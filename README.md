# AgLaunch Data Commons

## Introduction

Welcome to the AgLaunch Data Commons platform documentation.  Our intent with this repository is to have a
landing space to coordinate information about the platform as well as showing how others how to utilize, share,
and integrate with the platform.

The purpose of the Data Commons is to provide a data platform where the data being collected is owned by the
farmers the data is about while also allowing companies within the AgLaunch network to further enhance the data.
Startups that are part of AgLaunch who are just starting out will be able to leverage the data collected within the Data Commons to kickstart
their efforts.

## Architecture

The Data Commons is designed with the latest best practices in mind.  By integrating the data starting with standard
ontologies to building a raw data vault, and finally deriving data and combining it in such a way that farmers and companies 
will be able to gain insights into the information they are providing across platforms/companies.

While we briefly discuss the components of the Data Commons below, further architecture information can be viewed in 
the [architecture](https://github.com/AgLaunch/data-commons-documentation/blob/master/architecture.md) section.

### Ontologies

While this is not the place to exhaustively discuss ontological modelling, we are leveraging ontologies (either open or 
constructing our own) that allow us to have a unified vocabulary for talking about farmer/company data.  Our ontologies 
are available in our [ontology git repo](https://github.com/AgLaunch/data-commons-ontologies) and are licensed as <insert license here>.

The ontologies allow us to take a standardized vocabulary and internationalize it so that even if we're not directly speaking
the same language, we are able to interpret to the same meanings.  Our ontologies will grow and evolve over time to reflect
new standard models that come out.

Our ontologies are built using [Protégé](https://protege.stanford.edu/) but any ontology tool that can work with OWL files
will be able to read the files. 

### Models

Our data models are built with the ontological models as a foundation.  This forces the models to maintain the standardized
nomenclature developed within the ontologies.  All data model documentation can be found in our [model git repo](https://github.com/AgLaunch/data-commons-models).
As with our ontological models, the data models will grow and evolve over time to reflect modifications in standard models.

Our data models are built with [pgModeller](https://pgmodeler.io/).  Due to the nature of the Data Commons, we do not 
necessarily use a relational database for the given data model.  These models are mainly used as reference.

### Data Pipeline

There are two methods of data being brought into the Data Commons.  The first method is for initial or bulk loading and
that is our data pipeline.  This allows us to process the initial set of data from a source without overloading our API
(the second method for processing data into the Commons).  

Our data pipeline is build using [Pentaho Data Integration](https://www.hitachivantara.com/en-us/products/data-management-analytics/pentaho-data-integration.html).

### API

Our API is the second primary method for processing data into/out of the Data Commons.  

Our API is built leveraging the [Django REST Framework](https://www.django-rest-framework.org/).