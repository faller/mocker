Mocker
======

Mocking dataSource by javascript, providing save, remove, query and sort

create eg.
  ```javascript
    var fruitsDataSource = Mocker.mock({
        template: { say: 'hello' },
        amount: 100,
        
        // or json full format: { key: 'id', prefix: 'fruit_' }
        idAttribute: 'id',
        
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
        
        // item on create callback
        onCreate: function( item ) {                            
            item.say = 'hi';
        },
        
        // simulate request & response delay
        delay: 1000                                            
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
            
            // or json format: [ { key: 'type', operator: 'like', value: 'apple' } ]
            query: 'type gte {a}, type lte {z}, time gt {0}',   
            
            // or json format: [ { key: 'time', order: 'desc' } ]
            sort: 'type asc, time desc',
            
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
            
            // or string format: "{ 'type': 'orange', 'taste': 'bad' }"
            save: {                                             
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
            // or json format: [ { key: 'taste', operator: 'nq', value: 'awesome' } ]
            remove: 'taste eq {shit smells}'                    
        },
        success: function( count ) {
            console.log( 'data count of remove: ' + count );
        }
    });
  ```
