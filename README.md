ey-cloud-recipes/mongodb v1.8.1+
--------

A chef recipe for enabling mongodb v1.8.1+ on Engine Yard AppCloud.  This recipe downloads the latest version binary from 10gen and sets up a basic MongoDB instance, or an Replica Set.

It makes a few assumptions:

  * You will be running MongoDB on utility instance(s).
  * You will want [journaling][3] enabled.
  * If you want to use replication you will it will be using Replica
    sets.

The only thing (currently) lacking from this recipe is the ability to setup
scheduled backups of your MongoDB database.

Dependencies
--------

None so far.


Using it
--------

  * add the following to main/recipes/default.rb,

``require_recipe "mongodb"``  

  * Upload recipes to your environment

``ey recipes upload -e <environment>``  

  * Add an utility instance with the following naming scheme(s)
    * For a stand alone instance,
      * mongodb_#{app.name}
    * For an replica set,
      * mongodb_#{app.name}repl_1
      * mongodb_#{app.name}repl_2
      * mongodb_#{app.name}repl_3
      * ...
    * Sharding? TODO

  * Drops /data/#{app.name}/shared/config/mongo.yml with all the
    information needed to connect to MongoDB.


Caveats
--------

Replica sets should normally be in a size of 3 or greater.  However, if you prefer to only have 2 nodes, this recipe will configure the solo|db_master instance as an Arbiter to ensure that if there is a failover that the vote can pass.  There is and will be 0 support for 32-bit in this recipe.


Legend
--------

  * The usage of #{app.name} is an indicator of the application name set on the [Applications][1] Section on the [Dashboard][2].

TODO
--------

Get backups running. With 1.4.x the idea is to be able to take backups without
the need to shutdown a slave, but issues regarding that have not been fully 
worked out yet.

Sharding?

Use MongoS on each app instance instead of dropping mongo.yml?

Known Bugs
--------

This recipe likely is broken on 32-bit.  You should not be running
mongodb this way normally so I am not inclined to fix this.

Warranty
--------

This cookbook is provided as is, there is no offer of support for this
recipe by Engine Yard in any capacity.  If you find bugs, please open an
issue and submit a pull request.

Credits
--------

Thanks to [Edward Muller][4] and [Dan Peterson][5] for the original awesome
recipe to begin with.  I just updated it so it worked with 1.8 and will
continue to attempt to add more to it and accept patches/pulls.

[1]: https://cloud.engineyard.com/apps
[2]: https://cloud.engineyard.com
[3]: https://github.com/engineyard/ey-cloud-recipes/blob/master/cookbooks/mongodb/attributes/recipe.rb#L13
[4]: https://github.com/freeformz
[5]: https://github.com/dpiddy
