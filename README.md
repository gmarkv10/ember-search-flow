# Ember-search-flow

This is a dependency free multi-faceted search based on many of the "visual search" packages out there designed for ember.
See the demo at [https://alechirsch.github.io/ember-search-flow/](https://alechirsch.github.io/ember-search-flow/)

## Installation
`ember install ember-search-flow`

## Usage
Once installed, include the component in any template
```
{{search-flow parameters=parameters query=query onQueryUpdated=(action 'onQueryUpdated') onValueUpdated=(action 'onValueUpdated')}}
```

### Query
The query is an object of filters that you can use in an ember store query
This is the output of the component, you can also use it to initialize the filters for the component.
For example, if you want to filter a list of users by the name "Bob" and age of "25", the query object will look like this
```
{
	name: "Bob",
	age: 25
}
```
A query for filtering by "Bob" or "Bill" AND age 25:
```
{
	name: ["Bob", "Bill"],
	age: 25
}
```

There is also an option to query on a partial string using the "contains" key in the query.
A query for filterting all on names that contain the letter "b":
```
{
	contains: {
		name: "b"
	}
}
```

### Parameters
Parameters is an array of objects that define what filters can be added to the query.
Each object in the array is defined with the following options

| Key | Required | Description |
|-------|----------|-------------|
| name | yes | The key of the filter in the query |
| title | yes | The text to be displayed for the name of filter. |
| placeholder | no | The displayed string for a parameter when no value is typed into the input box. |
| allowMultiple | no | Default is true. Setting to false will allow only one filter of this parameter in the query. |
| contains | no | Default is false. This enables a filter to use the 'contains' key in the query. |
| options | no | This is an array of strings that represents the available options for the parameter. || options | no | This is an array of strings that represents the available options for the parameter. |
| remoteOptions | no | Default is false. If set to true, the options are remotely obtained and the onValueUpdated action will be called whenever the value of an input changes. Refer to the section about the onValueUpdated action. |

Here is an example of a parameter that does not use remote options and where a user can only input one of:
```
{
	name: 'status',
	title: 'Status',
	options: ['New', 'Open', 'Pending', 'Closed'],
	placeholder: 'Enter status',
	allowMultiple: false
}
```

### onValueUpdated
This action is called only if remoteOptions is set to true. Whenever a user changes the value of an input box, this action is called with two parameters:
- value
	The value that was entered into the input box
- parameter
	The parameter that was defined in the parameters array.

This action should set the 'options' on the parameter to an array of string that are the available options.
```
onValueUpdated(value, parameter){
	// do remote call
	let options = // results from remote call
	Ember.set(parameter, 'options', options);
}
```

### onQueryUpdated
This action is called each time ember-search-flow makes a new query from a newly selected key-value pair. Pointing this attribute to an action in a route or component enables you to listen and define behavior when filters are created or deleted.
```
onQueryUpdated(){
	let newFilter = Ember.get(this.currentModel, 'query'); //the property passed to search-flow's query parameter
	this.set('routeFilter', newFilter);
	//refresh the route, newFilter is used by a this.store.query call to make  JSONAPI call to a resource server
	this.refresh();
}
```

## Development

* `git clone https://github.com/alechirsch/ember-search-flow.git`
* `cd ember-search-flow`
* `npm link`
* `cd ../dummy-app`
* `npm link ember-search-flow`
* `npm install`
* `bower install`

## Running

* `ember serve`
* Visit your app at [http://localhost:5000](http://localhost:5000).


For more information on using ember-cli, visit [http://ember-cli.com/](http://ember-cli.com/).
