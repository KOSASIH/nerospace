developer.bring.com
===================

This repository contains the code behind https://developer.bring.com/

Our developer site is based on [Jekyll](https://jekyllrb.com/) and
[RAML](https://raml.org/). Jekyll is a static site generator, and  RAML
is a a YAML-based modeling language for APIs. We also use the lovely
[inuitcss](https://github.com/inuitcss/getting-started) CSS framework.

We currently use RAML 0.8 and [ePages' RAML parser gem](https://github.com/ePages-de/raml_parser)
to parse the RAML files, and have a  [custom Jekyll plugin](_plugins/raml_generator.rb)
for converting the  RAML files to HTML files.

The plugin reads a RAML->HTML map from [config](_config.yml) and outputs
HTML files accordingly. RAML 0.8 has a couple of quirks, for example it
doesn't have support for multiple examples, resulting in
[neat hacks](_layouts/api.html#L185-L192) here and there to string the
page together. We're hoping to migrate to the newly launched RAML 1.0
spec soon.


Run locally
-----------

Jekyll can serve the site locally and auto-watch for changes:

    brew install node
    bundle install
    bundle exec rake serve
    
    # or if you want to run for a specific environment:
    bundle exec rake serve[<env>] # test, qa, or production
    
This will clean the build, install necessary SASS dependencies
and launch the site at http://127.0.0.1:4000/


Release and deploy
------------------

Merging will automatically build and deploy to test, QA and production. 💥

You can still build it (populate the `_site` dir) manually by running

    bundle exec rake build
    
    # or if you want to build for a specific environment:
    bundle exec rake build [<env>] # test, qa, or production

and then deploy as normal with 

    b deploy [test|qa|production]

Documentation profiles
----------------------

When building, two different profiles is defined. These are:

1) `test` or `qa`
    * Uses file: `_config.yml` 
2) `production`
    * Uses file: `_config_production.yml`

As defined, production has it's own configuration, which essentially means that the build can differ from test and QA. This is useful if, for instance, some documentation is work-in-progress.
