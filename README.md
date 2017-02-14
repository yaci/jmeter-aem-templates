![Cognifide logo](http://cognifide.github.io/images/cognifide-logo.png)
![JMeter logo](http://jmeter.apache.org/images/logo.svg)

# Introduction
AEM JMeter template is a predefined Test Plan template, ready to use. Template accelerates performance tests implementation for web applications with JMeter tool and introduces set of good practices.

# Main features
* Parametrization - re-used, preconfigured User Variables,
* Tuned for Adobe AEM - preconfigured thread that activates page on author (to replicate page via author-publish-dispatcher path). Thread also presents how to authenticate to AEM Author instance and reuse CSRF token (works for AEM 6.2),
* Preconfigured loggers - preconfigured Simple Data Writers for .csv, .xml and Influx database (for Live monitoring).
* Other features:
    * Think times with with parametrized Gaussian Random Timer,
    * Test Profiles with User Defined Variables for different test environments,
    * HTTP Request configured for download embedded resources, parallel download and filtering out calls to 3rd parties domains,
    * Exemplary HTTP Requests (GET, POST) and Config Elements (Transaction and Throughput Controllers).

# Prerequisites
* JMeter 3.1 (http://jmeter.apache.org/download_jmeter.cgi)
* Plug-in Manager (https://jmeter-plugins.org/wiki/PluginsManager/)
* Install plug-ins:
  * 3 Basic Graphs,
  * 5 Additional Graphs,
  * Distribution/Percentile Graphs,
  
# Installing template  
Copy .xml snippet from "templates-snippet.xml" and  paste into JMeter templates file "\bin\templates\templates.xml". Remember to preserve correct XML syntax.

# Running
We assume that you are in JMETER_HOME/bin directory.

To run the tests from the console, execute  
`jmeter -q [path/to]environment.properties -q [path/to]loadparams.properties`

We advise that you make a copy of environment.properties file, one for every of your environments, so then you can run  
`jmeter -n -q integration.properties -q loadparams.properties` or  
`jmeter -n -q staging.properties -q loadparams.properties`  
Similarily you can make copies of loadparams.properties so you can easily run the same test with different loads.

For opening tests in GUI, simply omit `-n` parameter from above example, i.e.  
`jmeter -q environment.properties -q loadparams.properties`
 
# All Features
* Test Plan element - with ${project} variable to be changed.
* "Domains" User Defined Variables - Server related variables for each environment. Enable or disable appropriate one (e.g.: Domains STAGING to test staging).
* "Variables" User Defined Variables - test profiles for LOAD, SOAK, STRESS tests with Thread and Time related variables. Enable or disable appropriate one. Data is exemplary and to be change in a project.
* "Publish Tread" Group:
    * ${thread} and ${rampup} variables used for Forever run.
    * HTTP Request Defaults element
        * with ${domain} and ${protocol} variables used
        * Embedded Resources are downloaded
        * Parallel download of embedded resources = 6 (simulate browser behaviour)
        * Filtering out 3rd party domains - Tab Advanced ".*${domain}.*" regular expression. No analitycs will be send.
    * Cookie Manager
        * Clears cookie each iteration
    * Header Manager
        * adds Host header with ${domain} variable
    * Gaussian Random Timer
        * adds Think Time before EVERY sample
        * uses ${think-time-deviation} and ${think-time-constant} variables
    * Throughput Controller
        * demonstrates hot to split users in percentage manner e.g.: New users vs Returning users
        * HTTP Cache manager - simulating caching assets in browser
        * HTTP request Sampler
            * Response Assertion
        * Transaction Controller
            * Demonstrates how to embed multiple samples into one Transaction.
            * "Log in" POST request
* "Activate Page EXAMPLE" Thread group
    * Activates content from ${content-demo} variable using AEM etc/replication/treeactivation.html servlet
    * Hits Author to push correct flow of invalidation: Author->Publish->Dispatcher
* Set of useful graphs that can be used to import .cvs result file and analyze test results.
* Simple Data Writer CSV
    * Configured to write samples only to .csv file
* Simple Data Writer XML
    * Configured to write samples and sub samples to xml file. Use this one if you want to know Server Hits not only Transactions per second.
* Backend Listener for Live monitoring
    * Preconfigured to push summary only data to localhost Influx database.
    * uses ${project} variable to add prefix for database measurements.

# References
* JMeter http://jmeter.apache.org/

# Contact
lukasz.morawski@cognifide.com
