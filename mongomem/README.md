# MongoMem

MongoMem is a tool to give a breakdown of current memory usage by collection on a mongo server. It's easy to use and safe to run on a live production database. 

For more information, see http://eng.wish.com/mongomem-memory-usage-by-collection-in-mongodb/

# Installation

1.  Download the code from GitHub
2.  `pip install -r pip-requirements` (it's just `argparse` and `pymongo`; any version of either is probably fine)
3.  Run `sudo python setup.py install` (makes `mongomem` in `/usr/local/bin`, so you'll probably need `sudo` here)

If you run into any troubles here, leave a comment or ping me at adam@contextlogic.com. I've only tried installing this on a couple machines here, so there could be problems I missed.

# Usage

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
