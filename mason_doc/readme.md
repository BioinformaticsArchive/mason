# Mason Documentation
This documentation is geared towards those wishing to build their own module or wishing to understand how Mason is architected. Developing your own module does require knowledge of JavaScript, and the steps for doing so are outlined below.

## Demos and Examples
<a href="http://www.yeastrc.org/mason/">Go to the demo page</a> for examples, demos, and download files.

## Generic JSON module
You probably do not need to build your own module! We have developed a module that requires no knowledge of JavaScript to implement and is capable of parsing simple JSON containing data to be displayed, which supports overlapping annotations, user-specified coloring, user-specified per-block tooltips, and user-specified URLs for linking from annotations. 

<a href="http://www.yeastrc.org/mason/Pre_Built_Modules/Generic_JSON/example_generic_json.html">Go to the generic JSON module page</a> for documentation, demos, and download files.

## Pre-built modules
In addition to the <a href="http://www.yeastrc.org/mason/Pre_Built_Modules/Generic_JSON/example_generic_json.html">generic JSON module</a>, we have built several modules that support the native output from several popular protein annotations programs. These include Paircoil2 (coiled-coil), DISOPRED (disordered regions), PSIPRED (secondary structure), and Philius (transmembrane and signal peptide).

<a href="http://www.yeastrc.org/mason/">Go to the pre-built module page</a> for documentation, demos, and download files.

## Installation
Installing Mason on a page is a simple matter of including the JavaScript files on the page:

```html
<script type="text/javascript" src="js/libraries/jquery-1.8.0.min.js" ></script>
<script type="text/javascript" src="js/libraries/modernizr.v2.7.1__custom.39924_min.js"></script>
<script type="text/javascript" src="js/libraries/svg.min.js"></script>
<script type="text/javascript" src="js/libraries/wz_tooltip-min.js"></script>
<script type="text/javascript" src="js/mason_viewer/mason_viewer-min-munged.js"></script>
```

You can download these files <a href="../mason_download/">here</a>.

## Creation of a Viewer
Creating a viewer on the page is done via a JavaScript call in the form of:

```javascript
MasonViewer.createMasonViewer( $rootDiv, requestParams, configParams, callbackFunctionsObj );
```

Where:
  * `$rootDiv` is the jQuery variable representing the DIV in which to place the viewer on the page.
  * `requestParams` represents the data to be shown. `requestParams` is described in more detail <a href="request_params.md">here</a>.
  * `configParams` represents the configuration parameters for the viewer. `configParams` is described in more detail <a href="config_params.md">here</a>.
  * `callbackFunctionsObj` contains the core code of a module as a set of callback functions designed to handle tooltip generation, colors and shading and click handling. If you are writing your own module, this is where most of your work will be. `callbackFunctionsObj` is described in much more detail <a href="callback_functions.md">here</a>.

## Inter-viewer Communication
To enable inter-viewer communication that supports indicating where annotations in one viewer align with annotations in other viewers annotating the same sequence, please see our <a href="http://www.yeastrc.org/mason/Example_Applications/Multiple_Viewers_on_One_Page_Example/Multiple_Viewers_on_One_Page_Example.html">"Multiple Viewers on One Page"</a> at our demo site.


## Building a Module
Building a module comprises two major steps:

### 1. Build a data parser to generate `requestParams`
Typically the source data will define start and end points for sequence annotations, confidence scores, and possibly URLs to visit for more information. All of these data must be converted into the <a href="request_params.md">`requestParams` format that Mason is expecting</a>.

This conversion may act on files containing native output of sequence annotation programs, or JavaScript code may connect directly with a sequence annotation server via AJAX and convert the data returned from the remote server.  Examples of converters for both scenarios may be found at <a href="http://www.yeastrc.org/mason/">our pre-built module demo page</a>. Note: The Philius pre-built module connect directly via AJAX to a Philius prediction server.

### 2. Define the callback functions passed into `callbackFunctionsObj`
The colors, tooltips, and click events are defined by a set of callback functions passed into the Mason creator function via an object we refer to as `callbackFunctionsObj`. Only two callback functions are required (the set corresponding to the blocks (annotated segments of the sequence) and set corresponding to the labels that appear to the left of the rows), though more may be provided that correspond to optional features of the viewer (such as an optional "combined" row, summary blocks, or lines noting positions of interest).

Detailed informatoin about the callback functions may be found on <a href="callback_functions.md">callback functions page</a>.

### 3. Done.
At this point your module should be done. You can instantiate a new Mason viewer using the syntax above, passing in your data, callback functions, and configuration parameters for the viewer.
