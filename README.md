# utiljs

```js
export default {
  randomBoolean () {
    return Math.random() >= 0.5
  },
  // 获取最长11位随机字符串
  randomStr (size) {
    if (size < 0 || size > 11) size = 11
    return Math.random().toString(36).substring(2, size + 2)
  },
  // 返回指定范围内的随机整数。
  // randomIntegerInRange(0, 5) -> 2
  randomIntegerInRange (min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
  },
  // 返回指定范围内的随机数。
  // randomNumberInRange(2,10) -> 6.0211363285087005
  randomNumberInRange (min, max) {
    return Math.random() * (max - min) + min
  },
  // 将数字四舍五入到指定的位数。
  // round(1.005, 2) -> 1.01
  round (n, decimals = 0) {
    return Number(`${Math.round(`${n}e${decimals}`)}e-${decimals}`)
  },
  // 生成 UUID
  UUIDGenerator () {
    return ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, c => (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16))
  },
  // 是否是工作日
  isWeekday () {
    return date.getDay() % 6 !== 0
  },
  // 反转字符串
  reverse (str) {
    return [...str].reverse().join('')
  },
  // 截取小数位
  toFixed (n, fixed) {
    return ~~(Math.pow(10, fixed) * n) / Math.pow(10, fixed)
  },
  // 摄氏度 转 华氏度
  celsiusToFahrenheit (celsius) {
    return celsius * 9 / 5 + 32
  },
  // 华氏度 转 摄氏度
  fahrenheitToCelsius (fahrenheit) {
    return (fahrenheit - 32) * 5 / 9
  },
  // 从数组中移除 falsey 值
  arrCompact (arr) {
    return arr.filter(Boolean)
  },
  // 深拼合数组
  // arrDeepFlatten([1,[2],[[3],4],5]) -> [1,2,3,4,5]
  arrDeepFlatten (arr) {
    return [].concat(...arr.map(v => Array.isArray(v) ? deepFlatten(v) : v))
  },
  // 移除数组中的元素, 直到传递的函数返回true。返回数组中的其余元素。
  // arrDropElements([1, 2, 3, 4], n => n >= 3) -> [3,4]
  arrDropElements (arr, func) {
    while (arr.length > 0 && !func(arr[0])) arr.shift();
    return arr;
  },
  // 返回两个数组之间差异
  // arrDiff([1,2,3], [1,2,4]) -> [3]
  arrDiff (a, b) {
    const s = new Set(b);
    return a.filter(x => !s.has(x));
  },
  // 返回一个包含当前 URL 参数的对象。
  // getURLParameters('http://url.com/page?name=Adam&surname=Smith') -> {name: 'Adam', surname: 'Smith'}
  getURLParameters (url) {
    return url.match(/([^?=&]+)(=([^&]*))/g).reduce((a, v) => (a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1), a), {})
  },
  // 获取指定的 URL 参数
  GetQueryString (name) {
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
    var r = window.location.search.substring(1).match(reg);
    //        if(r!=null)return  unescape(r[2]); return null;
    if (r != null) return decodeURIComponent(r[2]); return null;
  },
  // 返回数字数组的平均值。
  arrayAverage (arr) {
    return arr.reduce((acc, val) => acc + val, 0)
  },
  // 移除从 JSON 对象指定的属性之外的任何特性。
  /*
    const testObj = {a: 1, b: 2, children: {a: 1, b: 2}}
    cleanObj(testObj, ["a"],"children")
    console.log(testObj)// { a: 1, children : { a: 1}}
  */
  cleanObj (obj, keysToKeep = [], childIndicator) {
    Object.keys(obj).forEach(key => {
      if (key === childIndicator) {
        cleanObj(obj[key], keysToKeep, childIndicator);
      } else if (!keysToKeep.includes(key)) {
        delete obj[key]
      }
    })
  },
  // 生成字符串的所有字谜 (包含重复项)。
  // anagrams('abc') -> ['abc','acb','bac','bca','cab','cba']
  anagrams (str) {
    if (str.length <= 2) return str.length === 2 ? [str, str[1] + str[0]] : [str];
    return str.split('').reduce((acc, letter, i) => acc.concat(anagrams(str.slice(0, i) + str.slice(i + 1)).map(val => letter + val)), []);
  },
  // 将字符串的第一个字母大写。
  // capitalize('myName') -> 'MyName'
  // capitalize('myName', true) -> 'Myname'
  capitalize ([first, ...rest], lowerRest = false) {
    return first.toUpperCase() + (lowerRest ? rest.join('').toLowerCase() : rest.join(''));
  },
  // 将字符串中每个单词的首字母大写。
  // capitalizeEveryWord('hello world!') -> 'Hello World!'
  capitalizeEveryWord (str) {
    return str.replace(/\b[a-z]/g, char => char.toUpperCase())
  },
  // 将驼峰转为指定连接符
  fromCamelCase (str, separator = '_') {
    return str.replace(/([a-z\d])([A-Z])/g, '$1' + separator + '$2').replace(/([A-Z]+)([A-Z][a-z\d]+)/g, '$1' + separator + '$2').toLowerCase();
  },
  // 将下划线连接符转为驼峰形式
  toCamelCase (str) {
    return str.replace(/^([A-Z])|[\s-_]+(\w)/g, (match, p1, p2, offset) => p2 ? p2.toUpperCase() : p1.toLowerCase());
  },
  // 16进制转为rgb
  hexToRGB (hex) {
    const extendHex = shortHex => '#' + shortHex.slice(shortHex.startsWith('#') ? 1 : 0).split('').map(x => x + x).join('');
    const extendedHex = hex.slice(hex.startsWith('#') ? 1 : 0).length === 3 ? extendHex(hex) : hex;
    return `rgb(${parseInt(extendedHex.slice(1), 16) >> 16}, ${(parseInt(extendedHex.slice(1), 16) & 0x00ff00) >> 8}, ${parseInt(extendedHex.slice(1), 16) & 0x0000ff})`;
  },
  // rgb转为16进制
  RGBToHex (r, g, b) {
    return ((r << 16) + (g << 8) + b).toString(16).padStart(6, '0')
  },
  // 正则校验 邮箱
  validateEmail (str) {
    return /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(str)
  },
  // 正则校验 密码，必须包含一个大写，一个小写，一个符号
  validPassword (str) {
    return /^((?=.*[a-z])(?=.*[A-Z])(?=.*[0-9]))[0-9a-zA-Z]{8,16}$/.test(str)
  },
  // 正则校验 手机号
  validPhone (str) {
    return /^1[356789][0-9]{9}$/.test(str)
  },
  // 正则校验 生日 1900-2-2 | 1900-02-02
  validBirthday (str) {
    return /^((19[0-9]{2})|(20[0-9]{2}))[\/\-](((0[13578]|1[02])[\/\-](0[1-9]|[12][0-9]|3[01]))|((0[469]|11)[\/\-](0[1-9]|[12][0-9]|30))|(02[\/\-](0[1-9]|1[0-9]|2[0-9])))$/.test(str) || /^((19[0-9]{2})|(20[0-9]{2}))[\/\-]((([13578]|1[02])[\/\-]([1-9]|[12][0-9]|3[01]))|(([469]|11)[\/\-]([1-9]|[12][0-9]|30))|(2[\/\-]([1-9]|1[0-9]|2[0-9])))$/
      .test(str)
  },


}
```