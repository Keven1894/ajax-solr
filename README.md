# AJAX Solr

AJAX Solr is a JavaScript library for creating user interfaces to Solr.

It is JavaScript framework-agnostic, but requires an AJAX implementation to communication with Solr. As such, you may use the library whether you develop using JQuery, MooTools, Prototype, Dojo, or any other framework. You need only define a Manager object that extends the provided AbstractManager object, and define the function executeRequest() on that object. A JQuery-compatible Manager is provided at widgets/managers/Manager.jquery.js.

## Advantages over SolrJS

[SolrJS][1] is ["a JavaScript client library that was initially developed as a 2008 Google Summer of Code project"][2]. However, it is nearly inactive, averaging one (trivial) commit per month for the past year. This would be acceptable if the library were mature, but it isn't.

### More efficient

SolrJS executes an AJAX request for each of its widgets. This is hugely inefficient, as you may have many widgets for faceting on date, language, location, price, category, etc. and at least one widget for displaying results. AJAX Solr performs a single AJAX request. It can do this, because Solr can retrieve the result set and the data for each facet in a single query. AJAX Solr takes advantage of this feature; SolrJS doesn't.

### Better browser support

AJAX Solr supports the back button and bookmarking (using the fragment identifier). You can add a filter or change your query, and click the back button to undo your changes. If you want to save your filtered result set, you just need to bookmark the page. SolrJS doesn't do either.

As of writing, [SolrJS][3] is still not IE 6, IE 7, and IE 8-compatible. AJAX Solr is.

### More secure

SolrJS requires that your Solr instance be publicly accessible. AJAX Solr was built under the assumption that you may want to proxy requests to your Solr instance, e.g. to limit what information the user has access to. AJAX Solr gives you the option.

### More Solr features

SolrJS doesn't provide support for Solr's [highlighting][4] or sorting parameters. AJAX Solr does.

### API improvements

SolrJS's functions for selecting/unselecting filters require that you pass in the full list of selected filters. AJAX Solr's functions just require that you pass in the list of filters that you want to add or remove from the list of selected filters. In other words, with AJAX Solr, you don't need to know the state of the current list of selected filters; you just add/remove what you want.

AJAX Solr's manager adds useful functions, such as: `deselectWidget`, to deselect all of a given widget's filters; `selectOnlyItems`, to deselect all except the given filters; `selectOnlyWidget`, to deselect all except the given widget's filters. Thanks to these and other improvements, your widgets will never need access to the manager's or other widgets' private fields.

### More customizable

SolrJS defines only a few hooks for modifying a widget's behaviour. AJAX Solr adds: `afterChangeSelection`, which allows a widget to run some code if one of its filters was set/unset; `startAnimation`/`endAnimation`, which allow you to customize what happens while the AJAX request is being performed, e.g. replace the result set with a loading spinner; `displayQuery`, which allows you to display the current query, filters, sort, etc. to the user.

### Better isolation of logic from presentation

In the widgets we've written for AJAX Solr, we separate the HTML from the JavaScript by putting all the HTML into "theme" functions using the AjaxSolr.theme API call (inspired by Drupal). Examples will be provided on the wiki shortly.

### Cleaner code

After working with SolrJS for a month, I can assure you AJAX Solr is <abbrev title="Don't Repeat Yourself">DRY</abbrev>-er, more loosely coupled, and easier to read than SolrJS. SolrJS's coupling between the manager and its widgets, for example, is egregious. If you require detailed justification, contact [james@evolvingweb.ca][5]. AJAX Solr's API is also more of a pleasure to work with. Consider this example:

Inheritance in SolrJS:

    jQuery.solrjs.AbstractClientSideWidget = jQuery.solrjs.createClass ("AbstractWidget", {
      /* key/value pairs */
    });

Inheritance in AJAX Solr:

    AjaxSolr.AbstractFacetWidget = AjaxSolr.AbstractWidget.extend({
      /* key/value pairs */
    });

## How do I get started?

Dig around the code for now. Further documentation and a demo site are pending.

We have currently written widgets that use JQuery and integrate with Drupal. Release pending.

[1]: http://solrjs.solrstuff.org/
[2]: http://wiki.apache.org/solr/SolrJS "SolrJS Wiki"
[3]: https://issues.apache.org/jira/browse/SOLR-1294 "SolrJS issue tracker"
[4]: http://wiki.apache.org/solr/HighlightingParameters
[5]: mailto:james@evolvingweb.ca
[6]: http://wiki.apache.org/solr/CommonQueryParameters