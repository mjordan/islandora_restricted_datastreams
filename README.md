# Islandora Restricted Datastreams

Utilty module that controls access to specific datastreams by role.

## Overview

The original use case for this module is that site admins wanted to store potentially sensitive information in a special datastream. They did, however, want to allow users of a particular role to access the datastream.

The module provides a relatively light-weight way to control access to specific datastreams. It does this by relying on Drupal's access control mechanisms and does not use XACML policies.

## Requirements

* [Islandora](https://github.com/Islandora/islandora)

## Configuration and usage

Enable this module as you would any other, and configure it at `admin/islandora/tools/islandora_restricted_datastreams`. Settings that you will want to configure include the list of datastream IDs to restrict and the list of which roles should be granted access to those datastreams.

## Discoverability of restricted datastreams

Most Islandora sites, particularly those that use the [DGI Basic Solr Configs](https://github.com/discoverygarden/basic-solr-config), index the content of all text and XML datastreams by default. They do this by calling the following XSLT stylesheets from within `foxmlToSolr.xslt`:

* `XML_text_nodes_to_solr.xslt`
* `text_to_solr.xslt`

The implication of this is if your restricted datastreams are indexed by either of these two stylesheets, or by any other that are used by GSearch, all users will be able to query the content of those datastreams, resulting in hits in a search (for example) based on keywords in a restricted datastream, even if the user does not have permission to view the contents of the datastream.

Giving your restricted datastreams a non-standard datastream ID is not sufficient to exclude them from indexing, since these two XSLT stylesheets use datastreams' MIME TYPE to decide it the datastream is indexable. The only practical strategy for not allowing users to accidently find objects based on the contents of a restricted datastream is to not index the content at all. The easiest way to do this is to assign the restricted datastreams a MIME type that is not indexed. By default, this module assigns the MIME type `application/octet-stream` to all datastreams that have a DSID that is configured to be "restricted", regardless of the content of the datastream. You probably should not change this setting unless you have a good reason to.

## Maintainer

* [Mark Jordan](https://github.com/mjordan)

## Development and feedback

Bug reports, use cases and suggestions are welcome. If you want to open a pull request, please open an issue first.

## License

 [GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
