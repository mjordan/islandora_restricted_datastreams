# Islandora Restricted Datastreams

Utilty module that controls access to specific datastreams by role.

## Overview

The original use case for this module was that site admins wanted to store potentially sensitive information in a specific datastream, and they wanted to allow only users who had a particular Drupal role to access the datastream.

This module restricts access to the datastreams by relying on Drupal's access control mechanisms and does not use XACML policies.

## Requirements

* [Islandora](https://github.com/Islandora/islandora)

## Configuration and usage

Enable this module as you would any other, and configure it at `admin/islandora/tools/islandora_restricted_datastreams`. List the IDs of the datastreams you want to restrict, select the roles allowed to access them, and you're good to go.

## Discoverability of restricted datastreams

Most Islandora sites index the content of all text and XML datastreams by default. For example, sites that use the [DGI Basic Solr Configs](https://github.com/discoverygarden/basic-solr-config) do this by having GSearch call the following XSLT stylesheets from within `foxmlToSolr.xslt`:

* `XML_text_nodes_to_solr.xslt`
* `text_to_solr.xslt`

The implication of this is if your restricted datastreams are indexed by these two stylesheets or by any other stylesheets are used by GSearch, searches performed by all users can potentially find objects with restricted datastreams. So even if the user cannot view the content of a datastream, their searches may find objects because of hits in the content of the restricted datastreams.

Site admins have several options for dealing with this:

1. Do nothing, and allow the content of the restricted datastreams to be discovered in searches by all users. This is the least desirable option from a permissions (and user experience) perspective.
1. Modify their site's GSearch stylesheets to not index the restricted datastreams. This is the most complicated option.
1. Apply a MIME type to restricted datastreams that will prevent them from being indexed by GSearch. This is probably the least disruptive option.

This module provides an easy way to implement the third option. It will assign the MIME type `application/octet-stream` to all datastreams that have a DSID configured to be restricted, regardless of what the real MIME type should be. Note that the default file extension that Islandora will give datastream files of this MIME type if downloaded (which can only be done by authorized users, of course) is `.bin`.

Whatever approach you take, you should perform some tests to see if queries for known restricted datastreams return the results that you expect.

## Maintainer

* [Mark Jordan](https://github.com/mjordan)

## Development and feedback

Bug reports, use cases and suggestions are welcome. If you want to open a pull request, please open an issue first.

## License

 [GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
