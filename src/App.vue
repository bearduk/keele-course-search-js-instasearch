<template>
  <div class="container">

  <h1>Course Finder</h1>
      <!-- <div id="search-box"> -->
        <!-- SearchBox widget will appear here -->
      <!-- </div> -->

      <div id="custom-searchbox"></div>
      
      <div id="courseLevel"></div>
      <div id="clear-all"></div>

      <div id="hits-per-page-selector"></div>

      <div id="hits">
        <!-- Hits widget will appear here -->
      </div>

      <div id="pagination-container"></div>
  </div> <!-- .container -->
</template>

<script>
// installed in main.js, let's use it here
import instantsearch from 'instantsearch.js';

export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted(){

    const search = instantsearch({
          appId: 'HT7VYJG3KU',
          apiKey: 'd37bbf3291b226676c9f3f1937e865d3',
          indexName: 'keele_main_COURSES',
          urlSync: true,
          searchParameters: {
            hitsPerPage: 5, // best to match an option in Pagination array
            attributesToSnippet: [
            'snippet:35'
            ],
            snippetEllipsisText: '...'
          }
        });



/* start custom search box */ /* REQUIRES LODASH AND JQUERY */
/* see docs for info https://community.algolia.com/instantsearch.js/v2/connectors/connectSearchBox.html */
// custom `renderFn` to render the custom SearchBox widget
  function renderFn(SearchBoxRenderingOptions, isFirstRendering) {
    if (isFirstRendering) {
      SearchBoxRenderingOptions.widgetParams.containerNode.html('Throttled: <input name="name" id="search" type="Search" placeholder="Search" />');
      SearchBoxRenderingOptions.widgetParams.containerNode
        .find('input')
        .on('keyup', _.debounce(function() {
          SearchBoxRenderingOptions.refine($(this).val());
        }, 300));
      SearchBoxRenderingOptions.widgetParams.containerNode
        .find('input')
        .val(SearchBoxRenderingOptions.query);
    }
  }

  // connect `renderFn` to SearchBox logic
  var customSearchBox = instantsearch.connectors.connectSearchBox(renderFn);

  // mount widget on the page
  search.addWidget(
    customSearchBox({
      containerNode: $('#custom-searchbox'),
    })
  );

/* end custom search box */




    // initialize SearchBox
    // search.addWidget(
    //   instantsearch.widgets.searchBox({
    //     container: '#search-box',
    //     placeholder: 'Search for courses'
    //   })
    // );
    
    // refinement list
    search.addWidget(
      instantsearch.widgets.refinementList({
          container: '#courseLevel',
          attributeName: 'courseLevelName',
          cssClasses: { // these may be helpful when adding Absolute styling?
            root: 'cb-TESTER-ROOT',
            list: 'cb-TESTER-LIST',
            item: 'cb-TESTER-ITEM form__field',
            body: 'cb-TESTER-BODY'
          },
          templates: {
          header: 'Course Level',
          // cbChecked class added in {{#isRefined}}cbChecked{{/isRefined}}, use this to hook into Absolute code?
          // item: '<div class="form__field"><input name="name-study-level" id="" type="checkbox" class="{{#isRefined}}cbChecked{{/isRefined}}"> <label for="id-undergraduate">{{ value }}</label> ( {{count}} )</div>',
          // item: '<div class="form__field"><input name="name-study-level" id="id-undergraduate" type="checkbox" value="undergraduate"> <label for="id-undergraduate">{{value}}</label></div>'
          
          // item: '<input type="checkbox">{{value}}</input> (this is a custom template but the checkboxes don\'t work) {{#isRefined}} - is Checked{{/isRefined}} - ( hits {{count}} )'          
          // ## I've tried using a single string for the value i.e. courseLevel from the Algolia set rather than the courseLevelName which has spaces. Not even sure if this is the way to go for checkboxes at all.

          // 1 
          // item: '{{value}} {{#isRefined}}Selected{{/isRefined}} ({{count}})',
          // OR
          // 2
          // item: function(param) {
          //  return '👉 ' + param.value; // end 2
          },
          sortBy: ["name:desc"] // count should almost always order by ug, pg, pgt
      })
    );

    // clear all 
    search.addWidget(
      instantsearch.widgets.clearAll({
        container: '#clear-all',
        templates: {
          link: 'Clear filter'
        },
        autoHideContainer: true,
        clearsQuery: false,
    })
  );

    // hits per page
    search.addWidget(
      instantsearch.widgets.hitsPerPageSelector({
        container: '#hits-per-page-selector',
        items: [
          {value: 5, label: '5 per page'},
          {value: 10, label: '10 per page'},
          {value: 20, label: '20 per page'},
          {value: 30, label: '30 per page'}
        ]
      })
    );    

    // initialize hits widget
    search.addWidget(
      instantsearch.widgets.hits({
        container: '#hits',
        templates: {
        empty: 'No courses found',
        item: '<div class="course-result"><!-- Hit {{ objectID }}: --><a href="{{ urlPath }}">{{{ _highlightResult.courseTitle.value }}}</a> <span class="course-level">{{courseLevelName}}</span>\
        <p class="needs_bem_snippet">{{{_snippetResult.snippet.value}}}</p></div>'
      }
      })
    );

    search.addWidget(
      instantsearch.widgets.pagination({
        container: '#pagination-container',
        maxPages: 20,
        // default is to scroll to 'body', here we disable this behavior
        scrollTo: false,
        showFirstLast: false,
    })
  );
  
    search.start();
  }
}
</script>

<style lang="scss">

    .ais-clear-all--link-disabled {
      display: none;
    }
    .container {
      width:90%;
      margin: auto;
    }

    .ais-refinement-list--count{
      background: black;
      color:white;
    }
    .ais-refinement-list--label {
      text-transform: capitalize;
    }



    /* throw some basic CSS in */
    body {
      font-family: sans-serif;
    }
    em {
      // removing as it is misleading for the user as the re
      font-weight: bold;
      color: #FFA500;
    }
    .needs_bem_snippet em {
      font-weight: initial;
      color: initial;
      font-style: normal;
    }
    .logo {
      margin-bottom: 10px;
    }
    /* and for bootstrap nav */
    body { padding-top: 70px; }
      @media screen and (max-width: 768px) {
          body { padding-top: 70px; }
    }
    .course-level {
    border: 1px solid #e0e0e0;
    padding: 3px 5px;
    text-align: center;
    text-transform: capitalize;
    }
    .course-result{
      margin-bottom: 4px;
    }
</style>
