[![Build Status](https://travis-ci.org/stuarticus/UISearchControllerWithSwift.svg)](https://travis-ci.org/stuarticus/UISearchControllerWithSwift)

# UISearchController

This app uses the new `UISearchController` API to manage the display of search results. 

There are two main classes: `ViewController.swift` and `ViewControllerExtensions.swift`.

### ViewController.swift
This class sets up a sample `Array` of different countries (`countryArray`), an `Array` for holding search results (`searchArray`), and finally a `UISearchController` for which manages the searching (`countrySearchController`).

The `countrySearchController` is setup in `viewDidLoad` as follows:

`let controller = UISearchController(searchResultsController: nil)`

>*nil* is passed as the argument in the method parameter as it has the effect of presenting the search results in the current view.

`controller.searchResultsUpdater = self`

The ViewController is responsible for updating the contents of search controller. This means that the ViewController must conform to the new `UISearchResultsUpdating` protocol. 

`controller.hidesNavigationBarDuringPresentation = false`

This is set to `false` as a workaround. When `true` the navBar hides, but the first search result is hidden.

`controller.searchBar.sizeToFit()`     
`self.countryTable.tableHeaderView = controller.searchBar`

Finally, you need to set the frame of the searchBar (otherwise it has a height of 0). 

There is no need to add a search bar in Interface Builder.

### ViewControllerExtensions.swift
The important protocol included the extensions file is `extension ViewController: UISearchResultsUpdating`.

When this delegate method is called:
- `searchArray`is cleared.
- a search of `countryArray` is performed based on `searchController.searchBar.text`.
- any matches are added to `searchArray`
- `self.countryTable` is reloaded.

In `cellForRowAtIndexPath`, the `countryTable` is populated based on whether or not the `searchController` is active. 

---

Build 2 includes an alternate setup that allows you to use a separate view controller to present search results:

A standalone tableViewController is created in Storyboard and then instantiated as part of the `UISearchController` setup.

`let alternateController:AlternateTableViewController = storyBoard.instantiateViewControllerWithIdentifier("aTV") as AlternateTableViewController`
`let controller = UISearchController(searchResultsController: alternateController)`

`AlternateTableViewController` conforms to `UISearchResultsUpdating` and is responsible for displaying search results from the original view controller's search results array.

That's it. 