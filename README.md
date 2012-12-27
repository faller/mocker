Mocker
======

Mocking dataSource by javascript, providing save, remove, query and sort

create eg.
  ```javascript
    var fruitsDataSource = Mocker.mock({
        template: { say: 'hello' },
        amount: 100,
        idAttribute: 'id',                                      // or json full format: { key: 'id', prefix: 'fruit_' }
        randomAttributes: [{
            key: 'type',
            values: [ 'apple', 'banana', 'orange', 'grapes' ]
        },{
            key: 'taste',
            values: [ 'awesome', 'good', 'shit smells' ]
        },{
            key: 'time',
            values: { min: 1325347200000, max: 1356883200000 }
        }],
        onCreate: function( item ) {                            // item on create callback
            item.say = 'hi';
        },
        delay: 1000                                             // simulate request & response delay
    });
  ```

or with AMD.
  ```javascript
    require( [ 'mocker' ], function( Mocker ) { } );
  ```
    
query eg.
  ```javascript
    fruitsDataSource({
        params: {
            query: 'type gte {a}, type lte {z}, time gt {0}',   // or json format: [ { key: 'type', operator: 'like', value: 'apple' } ]
            sort: 'type asc, time desc',                        // or json format: [ { key: 'time', order: 'desc' } ]
            skip: 0,
            limit: 100,
            count: true
        },
        success: function( data ) {
            console.log( 'data count: ' + data.count );
            console.log( 'data list: ' );
            console.log( data.list );
        },
        error: function() {
            console.error( 'shit happens!' );
        }
    });
  ```
    
save eg.
  ```javascript
    fruitsDataSource({
        params: {
            save: {                                             // or string format: "{ 'type': 'orange', 'taste': 'bad' }"
                type: 'apple',
                taste: 'awesome',
                time: new Date().getTime()
            }
        },
        success: function( data ) {
            console.log( 'data id: ' + data.id );
        }
    });
  ```

remove eg.
  ```javascript
    fruitsDataSource({
        params: {
            remove: 'taste eq {shit smells}'                    // or json format: [ { key: 'taste', operator: 'nq', value: 'awesome' } ]
        },
        success: function( count ) {
            console.log( 'data count of remove: ' + count );
        }
    });
  ```
