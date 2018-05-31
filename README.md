# busyapi

A sample API server for use as an optimization subject.

## Setup

  *  Clone this repository
  *  Install dependencies `npm install`
  *  Start the server `npm start`
  *  Go to [http://localhost:3000/](http://localhost:3000/) to confirm the server is running

## API

The API consists of a single endpoint which receives data when a patient uses their inhaler.

### Add Usage

  *  **method**: POST
  *  **endpoint**: /api/usages
  *  **data**: JSON usage object
  *  **result**: JSON object containing the usageId, HTTP Status 201, 200, 500

#### Example

**Data**
````
{
    "patientId":100,
    "timestamp":"Tue Nov 01 2016 09:11:51 GMT-0500 (CDT)",
    "medication":"Albuterol",
}
````

**Request**

     curl -X POST -H "Content-Type: application/json" --data '{"patientId":"100","timestamp":"Tue Nov 01 2016 09:11:51 GMT-0500 (CDT)","medication":"Albuterol"}' http://localhost:3000/api/usages

**Response**
````
{
    "id":22954
}
````




Did my first curl test that sends 16000 requests to the server and return the time it took

first run results

{"id":17001}
real	0m9.133s
user	0m1.056s
sys	0m1.428s

Went through and cleaned it up a little bit adding constraints on uneccessary console logs and calls, started to remove middleware that I couldnt see being used
after removing ran another curl test on 16000 post

second results

{"id":16000}
real	0m7.727s
user	0m1.044s
sys	0m1.512s

took the time down so that is awesome still not what I need my goal is 16k in a sencond or two

Removed some more middleware that wasnt really important only because this is api test and not a functional website
I am still trying to keep the integrity while increasing its speed..

third results
{"id":48000}
real	0m5.815s
user	0m0.912s
sys	0m1.608s

Since I am not that familiar with express applications I started doing some research on scaling node apps using express

found I could use Express Cluster to create multiple nodes
or just use Node.js built in Node cluster module

Given the right enviroment a single node server can process 16000 POST for this I think will have to add clusters and split up the
processes to better handle all of the requests
After adding a master and slave cluster framework and chand out the start up script to use master and worker the speed increased

fourth result
{"id":16000}
real	0m4.778s
user	0m0.772s
sys	0m1.492s


