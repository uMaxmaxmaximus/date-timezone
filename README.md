I WRITE THIS POLYFILL FOR YOU!
BEST SOLUTION ONLY FOR YOU <3 MY FRIEND!1

Adding timezone control to javascript Date object. Date.setTimezoneOffset()

Using:
```javascript
    // TO ALL dates
    Date.timezoneOffset(-240) // +4 UTC

    // Override offset only for this date
    new Date().timezoneOffset(-180) // +3 UTC
```


Coffee version:

```coffeescript
    Date.prototype.timezoneOffset = new Date().getTimezoneOffset() # default offset
    
    
    Date.setTimezoneOffset = (timezoneOffset)->
    	return @prototype.timezoneOffset = timezoneOffset
    
    
    Date.getTimezoneOffset = (timezoneOffset)->
    	return @prototype.timezoneOffset


    Date.prototype.setTimezoneOffset = (timezoneOffset)->
    	return @timezoneOffset = timezoneOffset
    
    
    Date.prototype.getTimezoneOffset = ->
    	return @timezoneOffset
    
    
    Date.prototype.toString = ->
    	offsetTime = @timezoneOffset * 60 * 1000
    	offsetDate = new Date(@getTime() - offsetTime)
    	return offsetDate.toUTCString()
    
    
    [
    	'Milliseconds', 'Seconds', 'Minutes', 'Hours',
    	'Date', 'Month', 'FullYear', 'Year', 'Day'
    ]
    .forEach (key)=>
    	Date.prototype["get#{key}"] = ->
    		offsetTime = @timezoneOffset * 60 * 1000
    		offsetDate = new Date(@getTime() - offsetTime)
    		return offsetDate["getUTC#{key}"]()
    
    	Date.prototype["set#{key}"] = (value)->
    		offsetTime = @timezoneOffset * 60 * 1000
    		offsetDate = new Date(@getTime() - offsetTime)
    		offsetDate["setUTC#{key}"](value)
    		time = offsetDate.getTime() + offsetTime
    		@setTime(time)
    		return time
```


Compiled version:

```javascript
    Date.prototype.timezoneOffset = new Date().getTimezoneOffset();
    
    Date.setTimezoneOffset = function(timezoneOffset) {
      return this.prototype.timezoneOffset = timezoneOffset;
    };
    
    Date.getTimezoneOffset = function(timezoneOffset) {
      return this.prototype.timezoneOffset;
    };
    
    Date.prototype.setTimezoneOffset = function(timezoneOffset) {
      return this.timezoneOffset = timezoneOffset;
    };

    Date.prototype.getTimezoneOffset = function() {
      return this.timezoneOffset;
    };
    
    Date.prototype.toString = function() {
      var offsetDate, offsetTime;
      offsetTime = this.timezoneOffset * 60 * 1000;
      offsetDate = new Date(this.getTime() - offsetTime);
      return offsetDate.toUTCString();
    };
    
    ['Milliseconds', 'Seconds', 'Minutes', 'Hours', 'Date', 'Month', 'FullYear', 'Year', 'Day'].forEach((function(_this) {
      return function(key) {
      
        Date.prototype["get" + key] = function() {
          var offsetDate, offsetTime;
          offsetTime = this.timezoneOffset * 60 * 1000;
          offsetDate = new Date(this.getTime() - offsetTime);
          return offsetDate["getUTC" + key]();
        };
        
        return Date.prototype["set" + key] = function(value) {
          var offsetDate, offsetTime, time;
          offsetTime = this.timezoneOffset * 60 * 1000;
          offsetDate = new Date(this.getTime() - offsetTime);
          offsetDate["setUTC" + key](value);
          time = offsetDate.getTime() + offsetTime;
          this.setTime(time);
          return time;
        };
        
      };
    })(this));
```
