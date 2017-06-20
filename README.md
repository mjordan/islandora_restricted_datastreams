# Islandora Restricted Datastreams

Utilty module that controls access to specific datastreams by role.

## Overview




## Requirements

* [Islandora](https://github.com/Islandora/islandora)

## Configuration and usage

Enable this module as you would any other, and configure it at `admin/islandora/tools/islandora_restricted_datastreams`.


## Discoverability of restricted datastreams

Most Islandora sites, particularly ones that use the DGI Basic Solr Configs, index all text and XML datastreams by default. They do this by calling the following XSLT stylesheets from within `foxmlToSolr.xslt`:

* `XML_text_nodes_to_solr.xslt`
* `text_to_solr.xslt`

This means that if your restricted datastreams are indexed by either of these two stylesheets, or by any other that are used by GSearch, all users will be able to query the content of those datastreams, resulting in hits in a search (for example) based on keywords in a restricted datastream even if the user does not have permission to view the contents of the datastream. Giving your restricted datastreams a non-standard datastream ID is not sufficient to exclude them from indexing, since these two XSLT stylesheets use datastreams' MIME TYPE to decide it the datastream is indexable.

Solr does not provide a mechanism to for excluding specific fields from a query. This means that the best strategy for not allowing users to accidently find objects based on the contents of a restricted datastream is to not index the content at all. To exclude indexing by Gseaerch, this module assigns the MIME type `application/octet-stream` to all datastreams that have a DSID that is configured to be "restricted", regardless of the content of the datastream.

## Maintainer

* [Mark Jordan](https://github.com/mjordan)

## Development and feedback

Bug reports, use cases and suggestions are welcome. If you want to open a pull request, please open an issue first.

## License

 [GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
