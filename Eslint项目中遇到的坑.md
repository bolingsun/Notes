# Notes
1、报错`Do not access Object.prototype method 'hasOwnProperty' from target object no-prototype-builtins`
原因：大概的意思是不要使用对象原型上的方法，因为原型上的方法可能被重写了。
````javascript
// bad
if (obj.hasOwnProperty('name')) {
}

// good
if (Object.prototype.hasOwnProperty.call(obj, 'name')) {
}
````
