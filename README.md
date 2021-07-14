I WRITE THIS POLYFILL FOR YOU!
BEST SOLUTION ONLY FOR YOU <3 MY FRIEND!1

Adding timezone control to javascript Date object. Date.setTimezoneOffset()

Using:
```javascript
// Timezone to ALL dates
Date.setTimezoneOffset(-240) // +4 UTC

// Override timezone only for this date
new Date().timezoneOffset(-180) // +3 UTC
```


Coffee version:

```coffeescript
do ->	
	# tmp date
	offsetDate = new Date()
	
	# default timezone
	Date.prototype.timezoneOffset = offsetDate.getTimezoneOffset()
	
	
	Date.setTimezoneOffset = (timezoneOffset)->
		return @prototype.timezoneOffset = timezoneOffset
	
	
	Date.getTimezoneOffset = (timezoneOffset)->
		return @prototype.timezoneOffset
	
	
	Date.prototype.getTimezoneOffset = ->
		return @timezoneOffset
	
	
	Date.prototype.setTimezoneOffset = (timezoneOffset)->
		return @timezoneOffset = timezoneOffset
	
	
	Date.prototype.toString = ->
		offsetTime = @timezoneOffset * 60 * 1000
		offsetDate.setTime(@getTime() - offsetTime)
		return offsetDate.toUTCString()
	
	
	[
		'Milliseconds', 'Seconds', 'Minutes', 'Hours',
		'Date', 'Month', 'FullYear', 'Year', 'Day'
	]
	.forEach (key)=>
		Date.prototype["get#{key}"] = ->
			offsetTime = @timezoneOffset * 60 * 1000
			offsetDate.setTime(@getTime() - offsetTime)
			return offsetDate["getUTC#{key}"]()
	
		Date.prototype["set#{key}"] = (value)->
			offsetTime = @timezoneOffset * 60 * 1000
			offsetDate.setTime(@getTime() - offsetTime)
			offsetDate["setUTC#{key}"](value)
			time = offsetDate.getTime() + offsetTime
			@setTime(time)
			return time
```


Compiled version:

```javascript
(function() {
  var offsetDate;
  offsetDate = new Date();
  Date.prototype.timezoneOffset = offsetDate.getTimezoneOffset();
  Date.setTimezoneOffset = function(timezoneOffset) {
    return this.prototype.timezoneOffset = timezoneOffset;
  };
  Date.getTimezoneOffset = function(timezoneOffset) {
    return this.prototype.timezoneOffset;
  };
  Date.prototype.getTimezoneOffset = function() {
    return this.timezoneOffset;
  };
  Date.prototype.setTimezoneOffset = function(timezoneOffset) {
    return this.timezoneOffset = timezoneOffset;
  };
  Date.prototype.toString = function() {
    var offsetTime;
    offsetTime = this.timezoneOffset * 60 * 1000;
    offsetDate.setTime(this.getTime() - offsetTime);
    return offsetDate.toUTCString();
  };
  return ['Milliseconds', 'Seconds', 'Minutes', 'Hours', 'Date', 'Month', 'FullYear', 'Year', 'Day'].forEach((function(_this) {
    return function(key) {
      Date.prototype["get" + key] = function() {
        var offsetTime;
        offsetTime = this.timezoneOffset * 60 * 1000;
        offsetDate.setTime(this.getTime() - offsetTime);
        return offsetDate["getUTC" + key]();
      };
      return Date.prototype["set" + key] = function(value) {
        var offsetTime, time;
        offsetTime = this.timezoneOffset * 60 * 1000;
        offsetDate.setTime(this.getTime() - offsetTime);
        offsetDate["setUTC" + key](value);
        time = offsetDate.getTime() + offsetTime;
        this.setTime(time);
        return time;
      };
    };
  })(this));
})();
```
