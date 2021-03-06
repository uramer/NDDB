# NDDB

NDDB provides a simple, lightweight NO-SQL database for node.js and the browser.

---

NDDB allows to define any number of comparator functions, which are
associated to any of the dimensions (i.e. properties) of the objects
stored in the database. Whenever a comparison is needed, the
corresponding comparator function is called, and the database is
updated.


Additional features are: methods chaining, tagging, and iteration 
through the entries.

NDDB is work in progress. Currently, the following methods are
implemented:

 1. Sorting and selecting:

     - select, sort, reverse, last, first, limit, shuffle*
 
 2. Custom callbacks
 
     - map, forEach, filter
 
 3. Deletion
 
     - delete, clear
 
 4. Advanced operations
 
     - split*, join, concat
 
 5. Fetching
 
     - fetch, fetchArray, fetchKeyArray
 
 6. Statistics operator
 
     - size, count, max, min, mean
 
 7. Diff
 
     - diff, intersect
 
 8. Iterator
 
     - previous, next, first, last
     *

 9. Tagging*
 
     - tag
        
 10. Updating
  
     - Update must be performed manually after a selection.
    
* = experimental

## Usage

Create an instance of NDDB

    var NDDB = require('NDDB').NDDB;
    var db = new NDDB();

Insert an item into the database

    db.insert({
        painter: "Picasso",
        title: "Les Demoiselles d'Avignon",
        year: 1907
    });

Import a collection of items

    var items = [
        {
            painter: "Dali",
            title: "Portrait of Paul Eluard",
            year: 1929,
            portrait: true
        },
        {
            painter: "Dali",
            title: "Barcelonese Mannequin",
            year: 1927
        },
        {
            painter: "Monet",
            title: "Water Lilies",
            year: 1906
        },
        {
            painter: "Monet",
            title: "Wheatstacks (End of Summer)",
            year: 1891
        },
        {
            painter: "Manet",
            title: "Olympia",
            year: 1863
        }
    ];
    
    db.import(items);
    
Retrieve the database size

    var db_size = db.size(); // 6
    
Select all paintings from Dali

    db.select('painter', '=', 'Dali'); // 2 items
    
Select all portraits

    db.select('portrait'); // 1 item
    
Select all paintings from Dali that are before 1928

    db.select('painter', '=', 'Dali')
      .select('year', '<', 1928); // 1 item

Select all paintings of the beginning of XX's century

    db.select('year', '><', [1900, 1910]); // 2 items    

## Advanced commands

Define a global comparator function that sorts all the entries chronologically

    db.globalCompator = function (o1, o2) {
        if (o1.year < o2.year) return 1;
        if (o1.year < o2.year) return 2;
        return 0;
    };

Sort all the items (global comparator function is automatically used)

    db.sort(); // Order: Manet, Monet, Monet, Picasso, Dali, Dali

Reverse the order of the items

    db.reverse(); // Order: Dali, Dali, Picasso, Monet, Monet, Manet

Define a custom comparator function for the name of the painter, which gives highest priorities to the canvases of Picasso;
    
    db.d('painter', function (o1, o2) {
        if (o1.painter === 'Picasso') return -1;
        if (o2.painter === 'Picasso') return 1;
    }
    
Sort all the paintings by painter

    db.sort('painter'); // Picasso is always listed first


# Test

NDDB relies on [mocha](http://visionmedia.github.com/mocha/) and [should.js](http://github.com/visionmedia/should.js) for testing.

    $ npm install # will load all necessary dependencies
    $ npm test # will run the test suite against nddb.js

## License

Copyright (C) 2012 Stefano Balietti

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
