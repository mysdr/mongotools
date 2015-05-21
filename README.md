Wish MongoDB Tools
==================

A collection of tools for managing MongoDB deployments.

So far, it's:

* **MongoMem**: Get memory usage breakdown by collection

# MongoMem

MongoMem is a tool to give a breakdown of current memory usage by collection on a mongo server. It's easy to use and safe to run on a live production database. 

For more information, see http://eng.wish.com/mongomem-memory-usage-by-collection-in-mongodb/

## Installation

`sudo pip install mongomem`

### Ubuntu 14.04.1 LTS specific
- `sudo apt-get install git`
- `sudo apt-get install python-pip`
- `sudo apt-get install build-essential python-dev`
- `sudo python setup.py install`

If you run into any troubles here, feel free to ping me at adam@wish.com.

## Usage

MongoMem is pretty simple to use. You have to run it on the same server as your `mongod` since it needs to be able to read the mongo data files directly (so you may need to run it as root or your mongodb user, depending on how your permissions are setup). It's safe to run against a live production site (just makes a few cheap syscalls, doesn't actually touch data).

With that out of the way, usage is:

    mongomem --dbpath DBPATH [--num NUM] [--directoryperdb] [--connection CONN] [--username USER --password PASS]
    

*   `DBPATH`: path to your mongo data files (`/var/lib/mongodb/` is mongo's default location for this). 
*   `NUM`: show stats for the top N collections (by current memory usage)
*   Add `--directoryperdb` if you're using that option to start `mongod`. 
*   `CONN`: pymongo connection string ("localhost" is the default which should pretty much always work, unless you're running a port other than 27017) 
*   `USER` / `PASS`: Credentials for your `admin` DB, if authentication is enabled

It'll take up to a couple minutes to run depending on your data size then it'll print a report of the top collections. Don't worry if you see a few warnings about some lengths not being multiples of page size. Unless there are thousands of those warnings, it won't really impact your results.

For each collection, it prints:

*   Number of MB in memory
*   Number of MB total
*   Percentage *of the collection* that's in memory 
