`<titanium-grid>`
===================


[&lt;titanium-grid&gt;](https://github.com/M-Mansoor-Ali/titanium-grid/) is a free, high quality data grid / data table [Polymer](http://polymer-project.org) element.

----------

<img src="https://raw.githubusercontent.com/M-Mansoor-Ali/titanium-grid/master/griddemo.gif">


<!--
```
<custom-element-demo>
  <template>
    <script src="../bower_components/webcomponentsjs/webcomponents-lite.js"></script>

    <link rel="import" href="../bower_components/iron-demo-helpers/demo-pages-shared-styles.html">
    <link rel="import" href="../bower_components/iron-demo-helpers/demo-snippet.html">
    <link rel="import" href="../bower_components/iron-ajax/iron-ajax.html">
    <link rel="import" href="../titanium-grid.html">

    <custom-style>
      <style is="custom-style" include="demo-pages-shared-styles">
      </style>
    </custom-style>
    
    <div class="maindiv">
                <iron-ajax
                  id="iron-ajax"
                  auto
                  url="{{url}}"
                  handle-as="json"
                  on-response="handleResponse"></iron-ajax>
              <div>
                <titanium-grid 
                  id="titaniumGrid"
                  elevation="2" 
                  rowstyle="striped"
                  columns='{{columns}}'
                  gridtitle="{{title}}"
                  content='{{content}}'
                  totalrows='{{totalElements}}'
                  firstrow='{{firstRow}}'
                  lastrow='{{lastRow}}'
                  pagesize='{{pageSize}}'
                  pagenumber='{{pageNumber}}'
                  islastpage='{{isLastPage}}'
                  isfirstpage='{{isFirstPage}}'
                  sizeselectorlist = '{{sizeSelectorList}}'
                  sortkey='{{sortBy}}'
                  sortdirection="{{sortDirection}}"
                  on-page-size-updated = "updatePageSize"
                  on-previous-clicked="previousPage"
                  on-next-clicked="nextPage"
                  on-sort-clicked="sortClicked"
                  >
                  </titanium-grid>
                </div>
              </div>
  </template>
  <script>
              /*
              * I am using this spring boot rest api for demo
              * https://fast-eyrie-45591.herokuapp.com/user
              * Following is a sample response that i get
              * {"content":[{"name":"Aubry Alberti","email":"aalberti9p@squarespace.com","job":"Senior Editor"},
                {"name":"Adolf Bagg","email":"abagg1@state.gov","job":"Engineer II"},
                {"name":"Arleen Bearns","email":"abearnsdd@salon.com","job":"Structural Analysis Engineer"},
                {"name":"Alexis Bode","email":"abode2o@printfriendly.com","job":"Community Outreach Specialist"},
                {"name":"Alfreda Brockhouse","email":"abrockhouse3n@youtube.com","job":"Engineer II"}],
                "last":false,"totalPages":100,"totalElements":500,"first":true,
                "sort":[{"direction":"ASC","property":"email","ignoreCase":false,"nullHandling":"NATIVE","descending":false,"ascending":true}],"numberOfElements":5,"size":5,"number":0}
              */
              HTMLImports.whenReady(function() {
                class TitaniumGridDemo extends Polymer.Element {
                  static get is() { return 'titanium-grid-demo'; }
                  static get properties() {
                    return {
                      attached: {
                        type: Boolean,
                        value: false
                      },
                      content: Object,
                      totalElements: {
                        type: Number
                      },
                      firstRow: {
                        type: Number
                      },
                      lastRow: {
                        type: Number
                      },
                      pageSize: {
                        type: Number,
                        observer: '_updateUrl'
                      },
                      pageNumber: {
                        type: Number,
                        observer: '_updateUrl'
                      },
                      isLastPage: Boolean,
                      isFirstPage: Boolean,
                      title:{
                        type: String,
                        value: 'User Details'
                      },
                      baseurl:{
                        type: String,
                        value: 'https://fast-eyrie-45591.herokuapp.com/user'
                      },
                      url:{
                        type: String,
                        value: 'https://fast-eyrie-45591.herokuapp.com/user'
                      },
                      columns: {
                        type: Object,
                        value: [{"key":"name","value":"Name"},{"key":"email","value":"Email Address"},{"key":"job","value":"Job Title"}],
                      },
                      sizeSelectorList: {
                        type: Object,
                        value: [{"key":"5","value":"5"},{"key":"10","value":"10"},{"key":"15","value":"15"}],
                      },
                      sortBy:{
                        type: String,
                        observer: '_updateUrl'
                      },
                      sortDirection:{
                        type: String,
                        observer: '_updateUrl'
                      }
                    };
                  }
                  handleResponse(event){
                    //console.log(event.detail.response);
                    this.attached = false;
                    var response = event.detail.response;
                    this.totalElements = response.totalElements;
                    this.content = response.content;
                    this.pageNumber = response.number + 1;
                    this.isLastPage = response.last;
                    this.isFirstPage = response.first;
                    this.pageSize = response.size;
                    this.lastRow = response.size * (response.number + 1);
                    this.firstRow = (this.lastRow - response.size) + 1;
                    this.sortBy = response.sort? response.sort[0].property : '';
                    this.sortDirection = (response.sort? response.sort[0].direction : '').toLowerCase();
                    this.attached = true;
                  }
                  _updateUrl(){
                    if(this.attached){
                      this.url = this.baseurl + "?size=" + this.pageSize + "&number=" + (this.pageNumber - 1) + "&sort=" + this.sortBy + "," + this.sortDirection;
                      //console.log(this.url);
                    }
                  }
                  updatePageSize(event){
                    this.pageSize = event.detail.newPageSize;
                  }
                  previousPage(){
                    if(!this.isFirstPage){
                      this.pageNumber -= 1;
                    }
                  }
                  nextPage(){
                    if(!this.isLastPage){
                      this.pageNumber += 1;
                    }
                  }
                  sortClicked(event){
                    if(this.sortDirection != event.detail.sortDirection){
                      this.sortDirection = event.detail.sortDirection;
                    }
                    this.sortBy = event.detail.sortKey;
                  }
                }
                window.customElements.define(TitaniumGridDemo.is, TitaniumGridDemo);
              });
            </script>
            <titanium-grid-demo></titanium-grid-demo>
</custom-element-demo>
```
-->

## Running demos and tests in browser

1. Fork the `titanuim-grid` repository and clone it locally.

1. Make sure you have [npm](https://www.npmjs.com/) installed.

1. When in the `titanuim-grid` directory, run `npm install` and then `bower install` to install dependencies.

1. Run `polymer serve --open`, browser will automatically open the component API documentation.
