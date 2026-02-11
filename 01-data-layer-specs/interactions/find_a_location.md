# Find a Location

Fire this event the user enters a City, State or Zip Code and the list of results is displayed. 

NOTE: Fire this event on both sites(.com and .jobs) where this feature is available(Find location widget, hamburger menu(responsive)


## Javascript Code
```js
window.dataLayer = window.dataLayer || [];
dataLayer.push({ event_data: null });  // Clear the previous event_data object.
dataLayer.push({
  event: "find_a_location",
  event_data: {
    search_term: "<search_term>",
    search_type: "<search_type>",
    number_of_items: "<number_of_items>"
  }
});
```

## Variable Definitions
|Field|Type|Required|Description|Example|Pattern|Min Length|Max Length|Minimum|Maximum|Multiple Of|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|search_term|string|required|The final search term submitted after any correction has been performed|Atlanta|
|search_type|string|required|The type of search performed|franchise,job,global|
|number_of_items|integer|recommended|The total number of search results found|324|












